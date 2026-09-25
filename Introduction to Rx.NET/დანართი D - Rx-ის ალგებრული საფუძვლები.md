> [!INFO] რატომ იკრიბება Rx-ის ოპერატორები ასე თავისუფლად
> Rx-ის ოპერატორები თითქმის ნებისმიერი წარმოსახვადი წესით ერთდება, ჩვეულებრივ პრობლემების გარეშე. ეს **შემთხვევითი არაა**: Rx თეორიაზეა დაფუძნებული. ამის ცოდნა Rx-ის გამოსაყენებლად **საჭირო არაა**, მაგრამ ცნობისმოყვარე დეველოპერს ხშირად აინტერესებს.

## სამი კონცეფცია კატეგორიების თეორიიდან

LINQ-ის (და, მაშასადამე, Rx-ის) დიზაინი სამ მათემატიკურ იდეას ეყრდნობა:

- **მონადები** (monads);
- **კატამორფიზმები** (catamorphisms);
- **ანამორფიზმები** (anamorphisms).

ეს ცნებები **კატეგორიების თეორიიდან** მოდის (მათემატიკის აბსტრაქტული დარგი). 1980-იანების ბოლოს მკვლევრებმა შენიშნეს, რომ ისინი პროგრამების ქცევის აღწერას კარგად ერგება; ეს იდეები Haskell-ში აისახა. მნიშვნელობა ის არის, რომ ეს საფუძვლები სწორედ ის მიზეზია, რის გამოც LINQ-ის ოპერატორები თავისუფლად იკრიბება.

## მონადა: კონტეინერის აბსტრაქცია

LINQ მონადას **ელემენტების კონტეინერის** ზოგად წარმოდგენად ეპყრობა. მასივი, `IList<T>`, ბაზის ცხრილი — ყველა ერთი აზრით მსგავსია. LINQ-ის ძირითადი შემეცნება ისაა, რომ არსებობს აბსტრაქცია, რომელიც ამ საერთოს იჭერს.

`IEnumerable<T>`, `IQueryable<T>` და — Rx-ისთვის გადამწყვეტად — `IObservable<T>` ყველა **მონადაა**. მონადა ორ ოპერაციას მოითხოვს:

| ოპერაცია | მათემატიკაში | LINQ/Rx-ში | რას აკეთებს |
|---|---|---|---|
| **`return`** | unit | `Observable.Return` | მნიშვნელობას მონადაში ახვევს |
| **`bind`** | bind | `SelectMany` | ჩალაგებულ კონტეინერს ბრტყელდება |

> [!NOTE] `Observable.Return`-ის სახელი აქედან მოდის
> `return` მნიშვნელობას მონადაში ახვევს: `int`-იდან `IObservable<int>`. სწორედ აქედან მიიღო `Observable.Return`-მა სახელი. `Enumerable`-ს `Return` მეთოდი არ აქვს, მაგრამ `new[] { value }` საკმარისია.

> [!IMPORTANT] `bind` (SelectMany) და სამი კანონი
> `SelectMany` კონტეინერს, სადაც ყოველი ელემენტი **ჩალაგებული კონტეინერია** (მაგალითად, `IEnumerable<IEnumerable<T>>`), ერთ ბრტყელ კონტეინერად აქცევს. მონადად რომ ჩაითვალოს, `return`-მა და `bind`-მა **სამ კანონს** უნდა დაემორჩილონ. სწორედ ეს კანონებია ოპერაციების კომპოზიციურობის საფუძველი.

`SelectMany` და `Return` იმდენად ფუნდამენტურია, რომ სხვა ოპერატორები მათგან აიგება:

```csharp
// Where მხოლოდ Return + SelectMany-თ
public static IObservable<T> Where<T>(
    this IObservable<T> source, Func<T, bool> predicate)
    => source.SelectMany(item =>
        predicate(item) ? Observable.Return(item) : Observable.Empty<T>());

// Select მხოლოდ Return + SelectMany-თ
public static IObservable<TResult> Select<TSource, TResult>(
    this IObservable<TSource> source, Func<TSource, TResult> f)
    => source.SelectMany(item => Observable.Return(f(item)));
```

## კატამორფიზმი: ჩაშლა

**კატამორფიზმი** არის ნებისმიერი დამუშავება, რომელიც კონტეინერის **ყველა** ელემენტს ითვალისწინებს და ჩვეულებრივ **ერთ** მნიშვნელობას აწარმოებს — ანუ ნებისმიერი აგრეგირება. LINQ-ის ფუნდამენტური კატამორფიზმი `Aggregate`-ია, საიდანაც სხვები აიგება:

```csharp
public static IObservable<int> MyCount<T>(this IObservable<T> items)
    => items.Aggregate(0, (total, _) => total + 1);

public static IObservable<T> MySum<T>(this IObservable<T> items)
    where T : INumber<T>
    => items.Aggregate(T.Zero, (total, x) => x + total);
```

> [!WARNING] Rx-ში კატამორფიზმი კონტეინერშივე რჩება
> ჩვეულებრივ კატამორფიზმი კონტეინერს „ხდის" (`Sum` `IEnumerable<int>`-ზე `int`-ს აბრუნებს). მაგრამ Rx-ში ასე **არ** ხდება: შედეგის ლოდინში ნაკადის დაბლოკვა (მაგალითად, ჯამის დასათვლელად ყველა ელემენტს უნდა დაელოდო) **deadlock**-ის რეცეპტია. ამიტომ Rx-ის კატამორფიზმები აგრეგირებას აკეთებენ, მაგრამ შედეგს ისევ `IObservable<T>`-ში ახვევენ.

## ანამორფიზმი: გაშლა

**ანამორფიზმი** კატამორფიზმის მიახლოებით საპირისპიროა: კატამორფიზმი სტრუქტურას **ჩაშლის**, ანამორფიზმი კი შესავალს სტრუქტურად **გაშლის**. Rx-ში ამის მთავარი მაგალითია `Observable.Generate` ([[თავი 3.3 თანმიმდევრობების გენერატორები|3.3]]): საწყისი მდგომარეობიდან თანმიმდევრობას აწარმოებს.

<svg viewBox="0 0 860 200" xmlns="http://www.w3.org/2000/svg" style="max-width:100%; height:auto; font-family: sans-serif;">
  <text x="430" y="26" text-anchor="middle" font-size="16" font-weight="bold" fill="currentColor">ანა vs კატა: გაშლა vs ჩაშლა</text>

  <text x="150" y="60" text-anchor="middle" font-size="13" font-weight="bold" fill="#27AE60">ანამორფიზმი (Generate)</text>
  <rect x="110" y="75" width="80" height="30" fill="#27AE60" fill-opacity="0.25" stroke="currentColor" rx="4"/>
  <text x="150" y="95" text-anchor="middle" font-size="11" fill="currentColor">seed</text>
  <line x1="190" y1="90" x2="250" y2="90" stroke="currentColor"/><polygon points="256,90 244,84 244,96" fill="currentColor"/>
  <g fill="#27AE60"><circle cx="290" cy="90" r="6"/><circle cx="330" cy="90" r="6"/><circle cx="370" cy="90" r="6"/><circle cx="410" cy="90" r="6"/></g>
  <text x="350" y="120" text-anchor="middle" font-size="10" fill="currentColor">ერთი მნიშვნელობა → ნაკადი</text>

  <text x="640" y="60" text-anchor="middle" font-size="13" font-weight="bold" fill="#E67E22">კატამორფიზმი (Aggregate)</text>
  <g fill="#E67E22"><circle cx="500" cy="90" r="6"/><circle cx="540" cy="90" r="6"/><circle cx="580" cy="90" r="6"/><circle cx="620" cy="90" r="6"/></g>
  <line x1="640" y1="90" x2="700" y2="90" stroke="currentColor"/><polygon points="706,90 694,84 694,96" fill="currentColor"/>
  <rect x="710" y="75" width="90" height="30" fill="#E67E22" fill-opacity="0.25" stroke="currentColor" rx="4"/>
  <text x="755" y="95" text-anchor="middle" font-size="11" fill="currentColor">შედეგი</text>
  <text x="620" y="120" text-anchor="middle" font-size="10" fill="currentColor">ნაკადი → ერთი მნიშვნელობა</text>

  <text x="430" y="170" text-anchor="middle" font-size="12" fill="currentColor">მონადა (SelectMany) კი ორივეს ერთდროულად აკავშირებს</text>
</svg>

*ნახაზი D.1: სამი კონცეფცია: გაშლა, ჩაშლა და კომპოზიცია.*

> [!TIP] რას ვსწავლობთ აქედან
> პრაქტიკულად საკმარისია იცოდე: Rx მუშაობს, რადგან `IObservable<T>` მონადაა, `Aggregate` კატამორფიზმი და `Generate` ანამორფიზმი. ეს მათემატიკური თვისებები (განსაკუთრებით კომპოზიცია) სწორედ ის მიზეზია, რის გამოც ოპერატორები თავისუფლად იკრიბება. დანარჩენი დეტალი ცნობისმოყვარეთათვისაა.

## იხილეთ ასევე

- [[თავი 5.2 SelectMany|5.2]]: `bind`/`SelectMany` პრაქტიკაში.
- [[თავი 6.3 საკუთარი აგრეგირება|6.3]]: `Aggregate` (კატამორფიზმი).
- [[თავი 3.3 თანმიმდევრობების გენერატორები|3.3]]: `Generate` (ანამორფიზმი).

---

წინა [[დანართი C - გამოყენების სახელმძღვანელო]]
შემდეგი [[Introduction to Rx.NET]]
