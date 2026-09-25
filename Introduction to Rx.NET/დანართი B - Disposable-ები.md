Rx გამოწერებს არსებული `IDisposable` ინტერფეისით წარმოადგენს, რაც საშუალებას აძლევს, ენის ჩაშენებული საშუალებები (`using`) გამოვიყენოთ. გარდა ამისა, Rx `System.Reactive.Disposables` namespace-ში `IDisposable`-ის რამდენიმე მზა იმპლემენტაციას გვთავაზობს. `ScheduledDisposable`-ის გარდა, ისინი Rx-ს პირდაპირ არ უკავშირდება და ნებისმიერ `IDisposable`-თან მომუშავე კოდში გამოგადგებათ.

## ცხრილი

| ტიპი | დანიშნულება |
|---|---|
| **`Disposable.Empty`** | `IDisposable`, რომელიც `Dispose`-ზე **არაფერს** აკეთებს |
| **`Disposable.Create(Action)`** | `Dispose`-ზე გადაცემულ მოქმედებას იძახებს (იდემპოტენტურად, მხოლოდ პირველ ჯერზე) |
| **`BooleanDisposable`** | `IsDisposed` თვისებას ავლენს (`true` ხდება `Dispose`-ის შემდეგ) |
| **`CancellationDisposable`** | ხიდი `CancellationTokenSource`-სა და `IDisposable`-ს შორის: `Dispose` იძახებს `Cancel`-ს |
| **`CompositeDisposable`** | ბევრ რესურსს ერთად ამუშავებს |
| **`ContextDisposable`** | რესურსს კონკრეტულ `SynchronizationContext`-ზე ხურავს |
| **`MultipleAssignmentDisposable`** | `Disposable` თვისების **მრავალჯერ** გადაწერა შესაძლებელია |
| **`SerialDisposable`** | ახლის მინიჭებისას **ძველს ხურავს** |
| **`SingleAssignmentDisposable`** | ერთი მინიჭება; მეორეზე `InvalidOperationException` |
| **`RefCountDisposable`** | ქვედა რესურსს ხურავს მხოლოდ მას შემდეგ, რაც ყველა დამოკიდებული დაიხურა |
| **`ScheduledDisposable`** | რესურსს მითითებულ scheduler-ზე ხურავს |

## ხშირად გამოსადეგი

> [!TIP] `Disposable.Empty` და `Disposable.Create`
> `Disposable.Empty` გამოგადგებათ, როცა `IDisposable`-ის დაბრუნება გავალდებულებთ (მაგალითად, `Observable.Create`-ში), მაგრამ დახურვისას არაფრის გაკეთება არ გჭირდებათ. `Disposable.Create(action)` კი პირიქით: დახურვისას რესურსის გასათავისუფლებელ მოქმედებას აწვდით.

```csharp
return Disposable.Create(() =>
{
    listener.Stop();
    Console.WriteLine("გამოწერა დაიხურა");
});
```

> [!IMPORTANT] `CompositeDisposable`: ბევრი ერთში
> `CompositeDisposable` საშუალებას გაძლევს, მრავალი რესურსი ერთ ერთეულად მართო. `Dispose`-ზე ისინი **დამატების რიგით** იხურება. ის `ICollection<IDisposable>`-საც იმპლემენტირებს, ამიტომ რესურსების დამატება/ამოღება შეიძლება. თუ კოლექცია უკვე დახურულია, ახლად დამატებული რესურსი **მაშინვე** იხურება; ამოღებული რესურსიც იხურება, კოლექციის მდგომარეობის მიუხედავად.

<svg viewBox="0 0 860 210" xmlns="http://www.w3.org/2000/svg" style="max-width:100%; height:auto; font-family: sans-serif;">
  <text x="430" y="26" text-anchor="middle" font-size="16" font-weight="bold" fill="currentColor">Serial vs MultipleAssignment vs SingleAssignment</text>

  <rect x="20" y="50" width="270" height="140" fill="#4A90D9" fill-opacity="0.08" stroke="currentColor" rx="10"/>
  <text x="155" y="74" text-anchor="middle" font-size="13" font-weight="bold" fill="currentColor">Serial</text>
  <text x="155" y="102" text-anchor="middle" font-size="11" fill="currentColor">ახალი მინიჭება</text>
  <text x="155" y="124" text-anchor="middle" font-size="11" fill="#C0392B">→ ძველი იხურება</text>
  <text x="155" y="160" text-anchor="middle" font-size="10" fill="currentColor">ერთი აქტიური რესურსი</text>

  <rect x="300" y="50" width="270" height="140" fill="#27AE60" fill-opacity="0.08" stroke="currentColor" rx="10"/>
  <text x="435" y="74" text-anchor="middle" font-size="13" font-weight="bold" fill="currentColor">MultipleAssignment</text>
  <text x="435" y="102" text-anchor="middle" font-size="11" fill="currentColor">ახალი მინიჭება</text>
  <text x="435" y="124" text-anchor="middle" font-size="11" fill="#27AE60">→ ძველი რჩება</text>
  <text x="435" y="160" text-anchor="middle" font-size="10" fill="currentColor">მრავალჯერ, ძველის დახურვის გარეშე</text>

  <rect x="580" y="50" width="260" height="140" fill="#8E44AD" fill-opacity="0.08" stroke="currentColor" rx="10"/>
  <text x="710" y="74" text-anchor="middle" font-size="13" font-weight="bold" fill="currentColor">SingleAssignment</text>
  <text x="710" y="102" text-anchor="middle" font-size="11" fill="currentColor">მეორე მინიჭება</text>
  <text x="710" y="124" text-anchor="middle" font-size="11" fill="#C0392B">→ Exception</text>
  <text x="710" y="160" text-anchor="middle" font-size="10" fill="currentColor">მხოლოდ ერთხელ</text>
</svg>

*ნახაზი B.1: სამი მინიჭებადი Disposable-ის განსხვავება.*

> [!NOTE] `RefCountDisposable`
> `RefCountDisposable` შიდა მთვლელს ინახავს: ყოველი `GetDisposable` ზრდის, ყოველი დამოკიდებულის დახურვა ამცირებს. ქვედა რესურსი მხოლოდ მაშინ იხურება, როცა მთვლელი 0-ს მიაღწევს. ეს [[თავი 13.2 RefCount და AutoConnect|RefCount ოპერატორის]] დაბალი დონის ანალოგია.

## იხილეთ ასევე

- [[თავი 2.4 გამოწერის სიცოცხლის ციკლი|2.4]]: `IDisposable` როგორც გამოწერა.
- [[თავი 3.1 საკუთარი წყაროს აგება|3.1]]: `Observable.Create` და `Disposable.Create`.
- [[თავი 12.2 Finally და Using|12.2]]: რესურსის სიცოცხლის მიბმა თანმიმდევრობაზე.

---

წინა [[დანართი A - IO Stream-ების პრობლემა]]
შემდეგი [[დანართი C - გამოყენების სახელმძღვანელო]]
