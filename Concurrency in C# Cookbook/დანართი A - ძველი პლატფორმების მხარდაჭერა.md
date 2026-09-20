ამ წიგნში განხილული ტექნოლოგიების ბევრს **ძველი პლატფორმების** გარკვეული მხარდაჭერაც აქვს. თუ იმ სამწუხარო სიტუაციაში ხართ, რომ ასეთი პლატფორმების მხარდაჭერა გჭირდებათ, ამ დანართის ინფორმაცია დაგეხმარებათ, გაარკვიოთ, რომელი ტექნოლოგიებია ხელმისაწვდომი.

> [!WARNING] ეს რეკომენდაცია არ არის
> ამ ტექნოლოგიების ძველ პლატფორმებზე გამოყენება **იდეალური არ არის**. და მაშინაც კი, თუ ამუშავებთ, გახსოვდეთ, რომ ერთადერთი გრძელვადიანი გადაწყვეტა თქვენი კოდის პლატფორმის მიზნის **განახლებაა**. ეს დანართი ძირითადად **ისტორიული ცნობარია** და არა რეკომენდაცია; თუმცა ძველი კოდის მხარდამჭერებს ის შეიძლება გამოადგეთ.

## ცხრილი A-1: ძველი პლატფორმების მხარდაჭერა

| პლატფორმა | async | Parallel | Reactive | Dataflow | Concurrent კოლექციები | უცვლელი კოლექციები |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| .NET 4.5 | ✔ | ✔ | NuGet | NuGet | ✔ | NuGet |
| .NET 4.0 | NuGet | ✔ | NuGet | | ✔ | |
| Windows Phone Apps 8.1 | ✔ | ✔ | NuGet | NuGet | ✔ | NuGet |
| Windows Phone SL 8.0 | ✔ | | NuGet | NuGet | | NuGet |
| Windows Phone SL 7.1 | NuGet | | NuGet | | | |
| Silverlight 5 | NuGet | | NuGet | | | |

<svg viewBox="0 0 780 210" xmlns="http://www.w3.org/2000/svg" style="max-width:100%; height:auto; font-family: sans-serif;">
  <text x="390" y="22" text-anchor="middle" font-size="12" font-weight="bold" fill="currentColor">დროის ხაზი: რა როდის გახდა ხელმისაწვდომი</text>

  <line x1="60" y1="120" x2="740" y2="120" stroke="currentColor" stroke-width="2"/>

  <line x1="130" y1="112" x2="130" y2="128" stroke="currentColor"/>
  <text x="130" y="145" text-anchor="middle" font-size="9" fill="currentColor">.NET 4.0</text>
  <text x="130" y="160" text-anchor="middle" font-size="8" fill="currentColor">2010</text>
  <text x="130" y="100" text-anchor="middle" font-size="8" fill="currentColor">Parallel, PLINQ</text>
  <text x="130" y="86" text-anchor="middle" font-size="8" fill="currentColor">Concurrent კოლექციები</text>

  <line x1="330" y1="112" x2="330" y2="128" stroke="currentColor"/>
  <text x="330" y="145" text-anchor="middle" font-size="9" fill="currentColor">.NET 4.5</text>
  <text x="330" y="160" text-anchor="middle" font-size="8" fill="currentColor">2012</text>
  <text x="330" y="100" text-anchor="middle" font-size="8" fill="currentColor">async/await</text>
  <text x="330" y="86" text-anchor="middle" font-size="8" fill="currentColor">Dataflow (NuGet)</text>

  <line x1="530" y1="112" x2="530" y2="128" stroke="currentColor"/>
  <text x="530" y="145" text-anchor="middle" font-size="9" fill="currentColor">.NET Core 3.0 / C# 8</text>
  <text x="530" y="160" text-anchor="middle" font-size="8" fill="currentColor">2019</text>
  <text x="530" y="100" text-anchor="middle" font-size="8" fill="currentColor">ასინქრონული ნაკადები</text>
  <text x="530" y="86" text-anchor="middle" font-size="8" fill="currentColor">Channels, IAsyncDisposable</text>

  <line x1="700" y1="112" x2="700" y2="128" stroke="currentColor"/>
  <text x="700" y="145" text-anchor="middle" font-size="9" fill="currentColor">.NET 6+</text>
  <text x="700" y="100" text-anchor="middle" font-size="8" fill="currentColor">ForEachAsync</text>
  <text x="700" y="86" text-anchor="middle" font-size="8" fill="currentColor">PeriodicTimer</text>
</svg>

*ნახაზი A-1: კონკურენტულობის ტექნოლოგიების გამოჩენა.*

## Async-ის მხარდაჭერა ძველ პლატფორმებზე

თუ `async`-ის მხარდაჭერა ძველ, მემკვიდრეობით მიღებულ პლატფორმებზე გჭირდებათ, დააინსტალირეთ NuGet პაკეტი **`Microsoft.Bcl.Async`**.

> [!WARNING] ASP.NET-ზე არა
> **არ გამოიყენოთ `Microsoft.Bcl.Async` .NET 4.0-ზე მომუშავე ASP.NET-ში `async` კოდის ჩასართავად!** ASP.NET-ის მილსადენი .NET 4.5-ში განახლდა async-ის გასაგებად, და `async` ASP.NET პროექტებისთვის **.NET 4.5 ან უფრო ახალი** უნდა გამოიყენოთ. `Microsoft.Bcl.Async` მხოლოდ **არა-ASP.NET** აპლიკაციებისთვისაა.

### ცხრილი A-2: async-ის მხარდაჭერა

| პლატფორმა | Async-ის მხარდაჭერა |
|---|---|
| .NET 4.5 | ჩაშენებული |
| .NET 4.0 | NuGet: `Microsoft.Bcl.Async` |
| Windows Phone Apps 8.1 | ჩაშენებული |
| Windows Phone SL 8.0 | ჩაშენებული |
| Windows Phone 7.1 | NuGet: `Microsoft.Bcl.Async` |
| Silverlight 5 | NuGet: `Microsoft.Bcl.Async` |

> [!IMPORTANT] `TaskEx`
> `Microsoft.Bcl.Async`-ის გამოყენებისას თანამედროვე `Task` ტიპის ბევრი წევრი სინამდვილეში **`TaskEx`** ტიპზეა, მათ შორის `Delay`, `FromResult`, `WhenAll` და `WhenAny`. ანუ `await Task.Delay(...)`-ის ნაცვლად `await TaskEx.Delay(...)` დაგჭირდებათ.

## Dataflow-ის მხარდაჭერა ძველ პლატფორმებზე

TPL Dataflow-ის გამოსაყენებლად დააინსტალირეთ NuGet პაკეტი **`System.Threading.Tasks.Dataflow`**.

> [!WARNING] ძველი პაკეტი
> **არ გამოიყენოთ ძველი `Microsoft.Tpl.Dataflow` პაკეტი.** ის აღარ ვითარდება.

### ცხრილი A-3: TPL Dataflow-ის მხარდაჭერა

| პლატფორმა | Dataflow-ის მხარდაჭერა |
|---|---|
| .NET 4.5 | NuGet: `System.Threading.Tasks.Dataflow` |
| .NET 4.0 | არ არის |
| Windows Phone Apps 8.1 | NuGet: `System.Threading.Tasks.Dataflow` |
| Windows Phone SL 8.0 | NuGet: `System.Threading.Tasks.Dataflow` |
| Windows Phone SL 7.1 | არ არის |
| Silverlight 5 | არ არის |

## System.Reactive-ის მხარდაჭერა ძველ პლატფორმებზე

`System.Reactive`-ის გამოსაყენებლად დააინსტალირეთ NuGet პაკეტი **`System.Reactive`**. `System.Reactive`-ს ისტორიულად ფართო პლატფორმული მხარდაჭერა ჰქონდა, თუმცა ძველი პლატფორმების უმეტესობა უკვე აღარ არის მხარდაჭერილი.

### ცხრილი A-4: System.Reactive-ის მხარდაჭერა

| პლატფორმა | Reactive-ის მხარდაჭერა |
|---|---|
| .NET 4.7.2 | NuGet: `System.Reactive` |
| .NET 4.5 | NuGet: `System.Reactive` v3.x |
| .NET 4.0 | NuGet: `Rx.Main` |
| Windows Phone Apps 8.1 | NuGet: `System.Reactive` v3.x |
| Windows Phone SL 8.0 | NuGet: `System.Reactive` v3.x |
| Windows Phone SL 7.1 | NuGet: `Rx.Main` |
| Silverlight 5 | NuGet: `Rx.Main` |

> [!WARNING] ძველი პაკეტი
> ძველი **`Rx.Main`** პაკეტი აღარ ვითარდება.

> [!NOTE] დღევანდელი მდგომარეობა
> ამ ცხრილებში ჩამოთვლილი პლატფორმების უმეტესობა (Silverlight, Windows Phone) **სრულიად მოძველებულია** და მათი მხარდაჭერა შეწყვეტილია. თანამედროვე კოდისთვის მიზნად **.NET 8 ან უფრო ახალი** აიღეთ, სადაც ამ ტექნოლოგიების უმეტესობა ჩაშენებულია:
> - `async`/`await`, `IAsyncEnumerable<T>`, `Channel<T>`, `IAsyncDisposable`: ჩაშენებული.
> - `System.Collections.Immutable`, `System.Threading.Tasks.Dataflow`: NuGet, მაგრამ Microsoft-ის მიერ მხარდაჭერილი.
> - `System.Reactive`: NuGet, საზოგადოების მიერ მხარდაჭერილი.

---

წინა [[თავი 14.7 პროგრესის განახლებების დროსელირება]]
შემდეგი [[დანართი B - ასინქრონული შაბლონების ამოცნობა]]
