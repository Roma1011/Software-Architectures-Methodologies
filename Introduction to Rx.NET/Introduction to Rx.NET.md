# Introduction to Rx.NET

**ავტორები:** Ian Griffiths და Lee Campbell
**გამოცემა:** მე-2, `System.Reactive` 6.0-ზე დაყრდნობით
**გამომცემელი:** .NET Foundation (ღია კოდის წიგნი)

ქართული კონსპექტი. ყოველი ჩანაწერი ერთ თემას ეთმობა, დიაგრამებით, გაფრთხილებებითა და საკუთარი მაგალითებით.

> [!TIP] როგორ წავიკითხოთ
> წიგნი სამ ნაწილად იყოფა: **დაწყება** (ძირითადი ტიპები), **მოვლენებიდან დასკვნებამდე** (ოპერატორები) და **პრაქტიკა** (ნაკადები, დრო, ტესტირება). ყოველ ჩანაწერს ბოლოში `წინა`/`შემდეგი` ნავიგაცია აქვს.

> [!NOTE] ტერმინოლოგია
> კონსპექტში ინგლისური ტექნიკური ტერმინები შენარჩუნებულია იქ, სადაც ქართული შესატყვისი აზრს ამახინჯებს: `hot`/`cold`, `push`/`pull`, `Subscribe`, `observable`. „Thread" ითარგმნება როგორც **ნაკადი**, „thread pool" როგორც **ნაკადების აუზი**.

---

## რა არის Rx ერთი ნახაზით

<svg viewBox="0 0 900 300" xmlns="http://www.w3.org/2000/svg" style="max-width:100%; height:auto; font-family: sans-serif;">
  <text x="450" y="30" text-anchor="middle" font-size="17" font-weight="bold" fill="currentColor">ნედლი მოვლენებიდან ღირებულ დასკვნამდე</text>

  <rect x="30" y="60" width="160" height="80" fill="#4A90D9" fill-opacity="0.18" stroke="currentColor" rx="10"/>
  <text x="110" y="92" text-anchor="middle" font-size="14" font-weight="bold" fill="currentColor">წყარო</text>
  <text x="110" y="115" text-anchor="middle" font-size="12" fill="currentColor">IObservable&lt;T&gt;</text>

  <line x1="190" y1="100" x2="234" y2="100" stroke="currentColor" stroke-width="2"/>
  <polygon points="240,100 228,94 228,106" fill="currentColor"/>

  <rect x="242" y="60" width="150" height="80" fill="#27AE60" fill-opacity="0.18" stroke="currentColor" rx="10"/>
  <text x="317" y="92" text-anchor="middle" font-size="14" font-weight="bold" fill="currentColor">ფილტრაცია</text>
  <text x="317" y="115" text-anchor="middle" font-size="12" fill="currentColor">Where, Take</text>

  <line x1="392" y1="100" x2="436" y2="100" stroke="currentColor" stroke-width="2"/>
  <polygon points="442,100 430,94 430,106" fill="currentColor"/>

  <rect x="444" y="60" width="150" height="80" fill="#E67E22" fill-opacity="0.18" stroke="currentColor" rx="10"/>
  <text x="519" y="92" text-anchor="middle" font-size="14" font-weight="bold" fill="currentColor">გარდაქმნა</text>
  <text x="519" y="115" text-anchor="middle" font-size="12" fill="currentColor">Select, GroupBy</text>

  <line x1="594" y1="100" x2="638" y2="100" stroke="currentColor" stroke-width="2"/>
  <polygon points="644,100 632,94 632,106" fill="currentColor"/>

  <rect x="646" y="60" width="150" height="80" fill="#8E44AD" fill-opacity="0.18" stroke="currentColor" rx="10"/>
  <text x="721" y="92" text-anchor="middle" font-size="14" font-weight="bold" fill="currentColor">დასკვნა</text>
  <text x="721" y="115" text-anchor="middle" font-size="12" fill="currentColor">Subscribe</text>

  <rect x="120" y="185" width="660" height="80" fill="#C0392B" fill-opacity="0.08" stroke="currentColor" stroke-dasharray="5 4" rx="10"/>
  <text x="450" y="213" text-anchor="middle" font-size="14" font-weight="bold" fill="currentColor">ყველაფერი ამ ჯაჭვში კომპოზიციურია</text>
  <text x="450" y="240" text-anchor="middle" font-size="13" fill="currentColor">ნებისმიერი ოპერატორი ნებისმიერს უკავშირდება, და შედეგი ისევ IObservable&lt;T&gt;-ია</text>
</svg>

---

## ნაწილი 1: დაწყება

### თავი 1: რატომ Rx

- [[თავი 1.1 რატომ Rx]]: ცოცხალი მონაცემები, `push` და `pull`, რატომ LINQ.
- [[თავი 1.2 როდის გამოვიყენოთ Rx]]: სად ჯდება კარგად, სად არა, და შედარება `Task`/`IAsyncEnumerable`/Channels-თან.
- [[თავი 1.3 Rx მოქმედებაში]]: პირველი მოქმედი მაგალითი.

### თავი 2: ძირითადი ტიპები

- [[თავი 2.1 IObservable ინტერფეისი]]: ფუნდამენტური აბსტრაქცია, `hot` და `cold` წყაროები.
- [[თავი 2.2 IObserver ინტერფეისი]]: სამი მეთოდი და მათი კავშირი `IEnumerator<T>`-თან.
- [[თავი 2.3 Rx თანმიმდევრობების ფუნდამენტური წესები]]: დასაშვები „გრამატიკა" და უკუწნევა.
- [[თავი 2.4 გამოწერის სიცოცხლის ციკლი]]: `IDisposable`, გაუქმება და მისი ზღვრები.

### თავი 3: observable თანმიმდევრობების შექმნა

- [[თავი 3.1 საკუთარი წყაროს აგება]]: `Observable.Create` და `Defer`, რატომ არ ვწერთ ინტერფეისს ხელით.
- [[თავი 3.2 მარტივი ფაბრიკის მეთოდები]]: `Return`, `Empty`, `Throw`, `Never`.
- [[თავი 3.3 თანმიმდევრობების გენერატორები]]: `Range` და `Generate`, უსასრულო წყაროები.
- [[თავი 3.4 დროზე დაფუძნებული გენერატორები]]: `Interval`, `Timer`, ცვლადი ინტერვალი.
- [[თავი 3.5 არსებული ტიპების ადაპტაცია]]: მოვლენები, `Task`, `IEnumerable<T>`.
- [[თავი 3.6 Subject-ები]]: `Subject`, `ReplaySubject`, `BehaviorSubject`, `AsyncSubject`.

---

## ნაწილი 2: მოვლენებიდან დასკვნებამდე

### თავი 4: ფილტრაცია

- [[თავი 4.1 Where და ფილტრაციის საფუძვლები]]: `Where`, `IgnoreElements`, `OfType`, `Distinct`.
- [[თავი 4.2 პოზიციური ფილტრაცია]]: `Take`, `Skip`, `FirstAsync`, `LastAsync`, `ElementAt`.
- [[თავი 4.3 დროითი ფილტრაცია]]: `TakeWhile`, `TakeUntil`, `SkipUntil`.

### თავი 5: გარდაქმნა

- [[თავი 5.1 Select]]: ერთი-ერთზე გარდაქმნა, `Cast` და `OfType`.
- [[თავი 5.2 SelectMany]]: ერთი-მრავალზე, პარალელურობა, `Task`-თან მუშაობა.
- [[თავი 5.3 Materialize და Dematerialize]]: შეტყობინებები მონაცემებად.

### თავი 6: აგრეგირება

- [[თავი 6.1 რიცხვითი აგრეგირება]]: `Count`, `Sum`, `Average`, `Min`, `MinBy`.
- [[თავი 6.2 ლოგიკური აგრეგირება]]: `Any`, `All`, `Contains`, `SequenceEqual`.
- [[თავი 6.3 საკუთარი აგრეგირება]]: `Aggregate` და `Scan`.

### თავი 7: დაყოფა

- [[თავი 7.1 GroupBy]]: ნაკადების ნაკადი გასაღების მიხედვით.
- [[თავი 7.2 Buffer]]: ნაწილები სიების სახით, მოძრავი ფანჯარა.
- [[თავი 7.3 Window]]: ნაწილები ნაკადების სახით.

### თავი 8: თანმიმდევრობების გაერთიანება

- [[თავი 8.1 თანმიმდევრული გაერთიანება]]: `Concat`, `StartWith`, `Repeat`.
- [[თავი 8.2 კონკურენტული გაერთიანება]]: `Merge`, `Switch`, `Amb`.
- [[თავი 8.3 წყვილებად შეერთება]]: `Zip`, `CombineLatest`, `WithLatestFrom`.
- [[თავი 8.4 Join და GroupJoin]]: დაწყვილება დროის ფანჯრებით.

## ნაწილი 3: პრაქტიკა

> [!INFO] მუშავდება
> დაგეგმვა და ნაკადები, დროზე დაფუძნებული თანმიმდევრობები, Rx-ის სამყაროდან გასვლა, შეცდომების დამუშავება, გამოქვეყნების ოპერატორები და ტესტირება.

## დანართები

> [!INFO] მუშავდება
> A: კლასიკური IO ნაკადების პრობლემები · B: `Disposable`-ები · C: გამოყენების რეკომენდაციები · D: Rx-ის ალგებრული საფუძვლები.

---

## სწრაფი ძებნა

| „მინდა…" | სად ვნახო |
|---|---|
| გავიგო, რა განსხვავებაა `IEnumerable`-სა და `IObservable`-ს შორის | [[თავი 1.1 რატომ Rx\|1.1]], [[თავი 2.1 IObservable ინტერფეისი\|2.1]] |
| გადავწყვიტო, Rx მჭირდება თუ `async`/`await` | [[თავი 1.2 როდის გამოვიყენოთ Rx\|1.2]] |
| გავიგო, რატომ ვერ ვიღებ ადრინდელ მოვლენებს | [[თავი 2.1 IObservable ინტერფეისი\|2.1]] (`hot` და `cold`) |
| ვიცოდე, როდის უნდა გამოვიძახო `Dispose` | [[თავი 2.4 გამოწერის სიცოცხლის ციკლი\|2.4]] |
| გავიგო, რატომ არ მუშაობს ჩემი `OnNext` სხვა ნაკადიდან | [[თავი 2.3 Rx თანმიმდევრობების ფუნდამენტური წესები\|2.3]] |
| შევქმნა საკუთარი წყარო | [[თავი 3.1 საკუთარი წყაროს აგება\|3.1]] |
| მოვლენა ან `Task` Rx-ში გადმოვიტანო | [[თავი 3.5 არსებული ტიპების ადაპტაცია\|3.5]] |
| გვიან მოსულ გამომწერს ისტორია ვაჩვენო | [[თავი 3.6 Subject-ები\|3.6]] |
| ძებნა კრეფისას, ძველი მოთხოვნის გაუქმებით | [[თავი 8.2 კონკურენტული გაერთიანება\|8.2]] (`Switch`) |
| მიმდინარე ჯამი ან მდგომარეობა ვაწარმოო | [[თავი 6.3 საკუთარი აგრეგირება\|6.3]] (`Scan`) |
| ჩანაწერები ჯგუფურად ჩავწერო ბაზაში | [[თავი 7.2 Buffer\|7.2]] |
| ორი მდგომარეობა გავაერთიანო | [[თავი 8.3 წყვილებად შეერთება\|8.3]] (`CombineLatest`) |
