# Concurrency in C# Cookbook

**ავტორი:** Stephen Cleary
**სრული სათაური:** Concurrency in C# Cookbook: Asynchronous, Parallel, and Multithreaded Programming (მე-2 გამოცემა)

ეს არის წიგნის სრული ქართული კონსპექტი: **14 თავი, 78 რეცეპტი და 2 დანართი**, თითოეული ცალკე ჩანაწერად, დიაგრამებით, გაფრთხილებებითა და თანამედროვე .NET-ის შენიშვნებით.

> [!TIP] როგორ გამოვიყენოთ ეს კონსპექტი
> ყოველი რეცეპტი ერთსა და იმავე სტრუქტურას მიჰყვება: **პრობლემა → გადაწყვეტა → განხილვა → იხილეთ ასევე**. შეგიძლიათ თანმიმდევრულად წაიკითხოთ (ყოველ ჩანაწერს ბოლოში `წინა`/`შემდეგი` ნავიგაცია აქვს), ან პირდაპირ იმ რეცეპტზე გადახვიდეთ, რომელიც თქვენს პრობლემას ეხება.

---

## კონკურენტულობის სახეობები

<svg viewBox="0 0 780 250" xmlns="http://www.w3.org/2000/svg" style="max-width:100%; height:auto; font-family: sans-serif;">
  <text x="390" y="24" text-anchor="middle" font-size="13" font-weight="bold" fill="currentColor">კონკურენტულობის ოთხი სახე და ამ წიგნის თავები</text>

  <rect x="25" y="50" width="175" height="105" fill="#4A90D9" fill-opacity="0.16" stroke="currentColor" rx="6"/>
  <text x="112" y="74" text-anchor="middle" font-size="11" font-weight="bold" fill="currentColor">ასინქრონული</text>
  <text x="112" y="96" text-anchor="middle" font-size="11" fill="currentColor">I/O-ს ლოდინი ნაკადის</text>
  <text x="112" y="110" text-anchor="middle" font-size="9" fill="currentColor">დაკავების გარეშე</text>
  <text x="112" y="136" text-anchor="middle" font-size="9" font-weight="bold" fill="currentColor">თავები 2, 3</text>

  <rect x="215" y="50" width="175" height="105" fill="#27AE60" fill-opacity="0.16" stroke="currentColor" rx="6"/>
  <text x="302" y="74" text-anchor="middle" font-size="11" font-weight="bold" fill="currentColor">პარალელური</text>
  <text x="302" y="96" text-anchor="middle" font-size="15" fill="currentColor">CPU-ს ბევრი ბირთვის</text>
  <text x="302" y="110" text-anchor="middle" font-size="15" fill="currentColor">ერთდროული გამოყენება</text>
  <text x="302" y="136" text-anchor="middle" font-size="9" font-weight="bold" fill="currentColor">თავი 4</text>

  <rect x="405" y="50" width="175" height="105" fill="#E67E22" fill-opacity="0.16" stroke="currentColor" rx="6"/>
  <text x="492" y="74" text-anchor="middle" font-size="11" font-weight="bold" fill="currentColor">რეაქტიული</text>
  <text x="492" y="96" text-anchor="middle" font-size="9" fill="currentColor">მოვლენების ნაკადებზე</text>
  <text x="492" y="110" text-anchor="middle" font-size="9" fill="currentColor">რეაგირება</text>
  <text x="492" y="136" text-anchor="middle" font-size="9" font-weight="bold" fill="currentColor">თავი 6</text>

  <rect x="595" y="50" width="175" height="105" fill="#8E44AD" fill-opacity="0.16" stroke="currentColor" rx="6"/>
  <text x="682" y="74" text-anchor="middle" font-size="11" font-weight="bold" fill="currentColor">Dataflow</text>
  <text x="682" y="96" text-anchor="middle" font-size="15" fill="currentColor">მილსადენები და ბადეები</text>
  <text x="682" y="110" text-anchor="middle" font-size="15" fill="currentColor">მონაცემების დამუშავებისთვის</text>
  <text x="682" y="136" text-anchor="middle" font-size="9" font-weight="bold" fill="currentColor">თავი 5</text>

  <rect x="80" y="180" width="620" height="50" fill="#C0392B" fill-opacity="0.08" stroke="currentColor" stroke-dasharray="4 3" rx="6"/>
  <text x="390" y="202" text-anchor="middle" font-size="10" fill="currentColor">საერთო საფუძვლები: კოლექციები (9), გაუქმება (10), სინქრონიზაცია (12), დაგეგმვა (13)</text>
  <text x="390" y="220" text-anchor="middle" font-size="10" fill="currentColor">და მათი გაერთიანება: თავსებადობა (8), OOP (11), სცენარები (14)</text>
</svg>

---

## თავი 1: კონკურენტულობის მიმოხილვა

- [[თავი 1.1 კონკურენტულობის ცნებები]]: კონკურენტულობა, მრავალნაკადიანობა, პარალელიზმი, ასინქრონულობა, რეაქტიულობა.
- [[თავი 1.2 ასინქრონული პროგრამირების შესავალი]]: `async`/`await`, კონტექსტის დაჭერა, deadlock-ები.
- [[თავი 1.3 პარალელური პროგრამირების შესავალი]]: მონაცემთა და ამოცანების პარალელიზმი.
- [[თავი 1.4 რეაქტიული პროგრამირება და Dataflow]]: `System.Reactive` და TPL Dataflow.
- [[თავი 1.5 ნაკადები, კოლექციები და თანამედროვე დიზაინი]]: მრავალნაკადიანობა, კონკურენტული კოლექციები, თანამედროვე მიდგომები.

## თავი 2: async-ის საფუძვლები

- [[თავი 2.1 დროებითი პაუზა]]: `Task.Delay`, ხელახალი ცდები, ექსპონენციური უკუსვლა.
- [[თავი 2.2 დასრულებული Task-ების დაბრუნება]]: `Task.FromResult`, `FromException`, `CompletedTask`.
- [[თავი 2.3 პროგრესის შესახებ შეტყობინება]]: `IProgress<T>` და `Progress<T>`.
- [[თავი 2.4 Task-ების ნაკრების დასრულების ლოდინი]]: `Task.WhenAll`.
- [[თავი 2.5 ნებისმიერი Task-ის დასრულების ლოდინი]]: `Task.WhenAny`.
- [[თავი 2.6 Task-ების დამუშავება დასრულების მიხედვით]]: `OrderByCompletion`.
- [[თავი 2.7 კონტექსტის არიდება გაგრძელებებისთვის]]: `ConfigureAwait(false)`.
- [[თავი 2.8 async Task-ის გამონაკლისები]]: გამონაკლისების დაჭერა და გავრცელება.
- [[თავი 2.9 async void-ის გამონაკლისები]]: რატომ არის `async void` საშიში.
- [[თავი 2.10 ValueTask-ის შექმნა]]: როდის ღირს `ValueTask<T>`.
- [[თავი 2.11 ValueTask-ის მოხმარება]]: `ValueTask`-ის მოხმარების მკაცრი წესები.

## თავი 3: ასინქრონული ნაკადები

- [[თავი 3.1 ასინქრონული ნაკადების შესავალი]]: `IAsyncEnumerable<T>` და `await foreach`.
- [[თავი 3.2 ასინქრონული ნაკადების მოხმარება]]: მოხმარება და ადრეული გამოსვლა.
- [[თავი 3.3 LINQ ასინქრონულ ნაკადებთან]]: `System.Linq.Async`.
- [[თავი 3.4 ასინქრონული ნაკადები და გაუქმება]]: `WithCancellation`, `[EnumeratorCancellation]`.

## თავი 4: პარალელური საფუძვლები

- [[თავი 4.1 მონაცემების პარალელური დამუშავება]]: `Parallel.ForEach`.
- [[თავი 4.2 პარალელური აგრეგირება]]: `localInit`/`localFinally`.
- [[თავი 4.3 პარალელური გამოძახება]]: `Parallel.Invoke`.
- [[თავი 4.4 დინამიკური პარალელიზმი]]: `Task.Factory.StartNew`, მშობელი/შვილი task-ები.
- [[თავი 4.5 Parallel LINQ]]: `AsParallel`, `AsOrdered`.

## თავი 5: TPL Dataflow

- [[თავი 5.1 ბლოკების დაკავშირება]]: `LinkTo`, `PropagateCompletion`.
- [[თავი 5.2 შეცდომების გავრცელება]]: `AggregateException` ბადეში.
- [[თავი 5.3 ბლოკების გათიშვა]]: დინამიკური ბადეები.
- [[თავი 5.4 ბლოკების დროსელირება]]: `BoundedCapacity`.
- [[თავი 5.5 პარალელური დამუშავება Dataflow-ით]]: `MaxDegreeOfParallelism`.
- [[თავი 5.6 საკუთარი ბლოკების შექმნა]]: `DataflowBlock.Encapsulate`.

## თავი 6: System.Reactive

- [[თავი 6.1 .NET მოვლენების გარდაქმნა]]: `FromEventPattern`, `SubscribeOn`.
- [[თავი 6.2 შეტყობინებების გაგზავნა კონტექსტში]]: `ObserveOn`.
- [[თავი 6.3 მოვლენების დაჯგუფება Window-თი და Buffer-ით]]: `Buffer` და `Window`.
- [[თავი 6.4 მოვლენების ნაკადის დამორჩილება]]: `Throttle` და `Sample`.
- [[თავი 6.5 Timeout-ები]]: `Timeout` ოპერატორი.

## თავი 7: ტესტირება

- [[თავი 7.1 async მეთოდების ერთეულოვანი ტესტირება]]: `async Task` ტესტები, mock-ები.
- [[თავი 7.2 მოსალოდნელი გამონაკლისების ტესტირება]]: `ThrowsAsync`.
- [[თავი 7.3 async void მეთოდების ტესტირება]]: რეფაქტორინგი და `AsyncContext`.
- [[თავი 7.4 Dataflow ბადეების ტესტირება]]: `Post`/`Receive`/`Completion`.
- [[თავი 7.5 System.Reactive Observable-ების ტესტირება]]: `Return`, `Throw`, `SingleAsync`.
- [[თავი 7.6 System.Reactive-ის ტესტირება scheduler-ით]]: `TestScheduler` და ვირტუალური დრო.

## თავი 8: თავსებადობა (Interop)

- [[თავი 8.1 EAP მეთოდების async გარსები]]: `TaskCompletionSource<T>` EAP-სთვის.
- [[თავი 8.2 Begin-End მეთოდების async გარსები]]: `Task.Factory.FromAsync`.
- [[თავი 8.3 ნებისმიერი შეტყობინების async გარსი]]: უნივერსალური `TaskCompletionSource<T>` შაბლონი.
- [[თავი 8.4 პარალელური კოდის async გარსი]]: `await Task.Run(() => Parallel...)`.
- [[თავი 8.5 System.Reactive Observable-ების async გარსი]]: `LastAsync`, `FirstAsync`, `ToList`.
- [[თავი 8.6 System.Reactive-ის Observable გარსები]]: `ToObservable`, `StartAsync`, `FromAsync`.
- [[თავი 8.7 ასინქრონული ნაკადები და Dataflow ბადეები]]: ხიდები ორივე მიმართულებით.
- [[თავი 8.8 System.Reactive და Dataflow ბადეები]]: `AsObservable` და `AsObserver`.
- [[თავი 8.9 Observable-ების ასინქრონულ ნაკადებად გარდაქმნა]]: push → pull და უკუწნევა.

## თავი 9: კოლექციები

- [[თავი 9.1 უცვლელი სტეკები და რიგები]]: უცვლელი კოლექციების პრინციპები + ცხრილი 9-1.
- [[თავი 9.2 უცვლელი სიები]]: `ImmutableList<T>` და მისი წარმადობა.
- [[თავი 9.3 უცვლელი სიმრავლეები]]: `ImmutableHashSet<T>`, `ImmutableSortedSet<T>`.
- [[თავი 9.4 უცვლელი ლექსიკონები]]: `ImmutableDictionary<TK,TV>`.
- [[თავი 9.5 Thread-safe ლექსიკონები]]: `ConcurrentDictionary<TK,TV>` და `AddOrUpdate`.
- [[თავი 9.6 მბლოკავი რიგები]]: `BlockingCollection<T>`.
- [[თავი 9.7 მბლოკავი სტეკები და ტომრები]]: `ConcurrentStack`/`ConcurrentBag`.
- [[თავი 9.8 ასინქრონული რიგები]]: Channels, `BufferBlock<T>`.
- [[თავი 9.9 რიგების დროსელირება]]: შეზღუდული ტევადობა და უკუწნევა.
- [[თავი 9.10 შერჩევითი რიგები]]: `BoundedChannelFullMode`.
- [[თავი 9.11 ასინქრონული სტეკები და ტომრები]]: `AsyncCollection<T>`.
- [[თავი 9.12 მბლოკავი და ასინქრონული რიგები]]: ორივე API ერთდროულად.

## თავი 10: გაუქმება

- [[თავი 10.1 გაუქმების მოთხოვნის გაცემა]]: `CancellationTokenSource`.
- [[თავი 10.2 გაუქმებაზე რეაგირება გამოკითხვით]]: `ThrowIfCancellationRequested`.
- [[თავი 10.3 გაუქმება timeout-ის გამო]]: `CancelAfter`.
- [[თავი 10.4 async კოდის გაუქმება]]: token-ის გადაცემა ფენებზე.
- [[თავი 10.5 პარალელური კოდის გაუქმება]]: `ParallelOptions`, `WithCancellation`.
- [[თავი 10.6 System.Reactive კოდის გაუქმება]]: `ToTask`, `CancellationDisposable`.
- [[თავი 10.7 Dataflow ბადეების გაუქმება]]: `DataflowBlockOptions`.
- [[თავი 10.8 გაუქმების მოთხოვნების ინექცია]]: დაკავშირებული token-ები.
- [[თავი 10.9 სხვა გაუქმების სისტემებთან თავსებადობა]]: `CancellationToken.Register`.

## თავი 11: ფუნქციურ-მეგობრული OOP

- [[თავი 11.1 ასინქრონული ინტერფეისები და მემკვიდრეობა]]: `Task`-ის დამბრუნებელი კონტრაქტები.
- [[თავი 11.2 ასინქრონული კონსტრუირება ფაბრიკებით]]: `CreateAsync` შაბლონი.
- [[თავი 11.3 ასინქრონული ინიციალიზაციის შაბლონი]]: `IAsyncInitialization`.
- [[თავი 11.4 ასინქრონული თვისებები]]: მეთოდი თუ `AsyncLazy`.
- [[თავი 11.5 ასინქრონული მოვლენები]]: deferral-ები, შეტყობინების vs ბრძანების მოვლენები.
- [[თავი 11.6 ასინქრონული განთავისუფლება]]: `IAsyncDisposable` და `await using`.

## თავი 12: სინქრონიზაცია

- [[თავი 12.1 ბლოკირებები]]: `lock` + როდის არის სინქრონიზაცია საჭირო.
- [[თავი 12.2 ასინქრონული ბლოკირებები]]: `SemaphoreSlim`, `AsyncLock`.
- [[თავი 12.3 მბლოკავი სიგნალები]]: `ManualResetEventSlim`.
- [[თავი 12.4 ასინქრონული სიგნალები]]: `TaskCompletionSource<T>`, `AsyncManualResetEvent`.
- [[თავი 12.5 დროსელირება]]: კონკურენტულობის შეზღუდვა ყველა ტექნოლოგიაში.

## თავი 13: დაგეგმვა

- [[თავი 13.1 სამუშაოს დაგეგმვა thread pool-ზე]]: `Task.Run`.
- [[თავი 13.2 კოდის შესრულება Task Scheduler-ით]]: `TaskScheduler`, `ConcurrentExclusiveSchedulerPair`.
- [[თავი 13.3 პარალელური კოდის დაგეგმვა]]: `ParallelOptions.TaskScheduler`.
- [[თავი 13.4 Dataflow-ის სინქრონიზაცია scheduler-ებით]]: ბლოკები სხვადასხვა კონტექსტში.

## თავი 14: სცენარები

- [[თავი 14.1 გაზიარებული რესურსების ინიციალიზაცია]]: `Lazy<T>`, `AsyncLazy<T>`.
- [[თავი 14.2 System.Reactive-ის გადავადებული შეფასება]]: `Observable.Defer`.
- [[თავი 14.3 ასინქრონული მონაცემთა მიბმა]]: `NotifyTask`, `BindableTask<T>`.
- [[თავი 14.4 იმპლიციტური მდგომარეობა]]: `AsyncLocal<T>`.
- [[თავი 14.5 იდენტური სინქრონული და ასინქრონული კოდი]]: ბულევური არგუმენტის ხრიკი.
- [[თავი 14.6 Railway პროგრამირება Dataflow-ით]]: `Try<T>` და ორი ლიანდაგი.
- [[თავი 14.7 პროგრესის განახლებების დროსელირება]]: `Sample` + `ObserveOn`.

## დანართები

- [[დანართი A - ძველი პლატფორმების მხარდაჭერა]]: რა მუშაობს ძველ პლატფორმებზე.
- [[დანართი B - ასინქრონული შაბლონების ამოცნობა]]: TAP, APM, EAP, CPS და სხვა.

---

## სწრაფი ძებნა პრობლემის მიხედვით

| „მინდა..." | სად ვნახო |
|---|---|
| ასინქრონული ოპერაციის ხელახლა ცდა | [[თავი 2.1 დროებითი პაუზა\|2.1]] |
| deadlock-ის თავიდან აცილება ბიბლიოთეკაში | [[თავი 2.7 კონტექსტის არიდება გაგრძელებებისთვის\|2.7]] |
| ბევრი ელემენტის პარალელურად დამუშავება | [[თავი 4.1 მონაცემების პარალელური დამუშავება\|4.1]] |
| მონაცემების მილსადენის აგება | [[თავი 5.1 ბლოკების დაკავშირება\|5.1]] |
| ძებნის ველის „დამშვიდება" | [[თავი 6.4 მოვლენების ნაკადის დამორჩილება\|6.4]] |
| ასინქრონული კოდის დატესტვა | [[თავი 7.1 async მეთოდების ერთეულოვანი ტესტირება\|7.1]] |
| ძველი API-ის `await`-ადად გადაქცევა | [[თავი 8.3 ნებისმიერი შეტყობინების async გარსი\|8.3]] |
| მწარმოებელი/მომხმარებლის რიგი | [[თავი 9.8 ასინქრონული რიგები\|9.8]] |
| „გაუქმება" ღილაკის დამატება | [[თავი 10.1 გაუქმების მოთხოვნის გაცემა\|10.1]] |
| ასინქრონული ინიციალიზაცია კონსტრუქტორში | [[თავი 11.2 ასინქრონული კონსტრუირება ფაბრიკებით\|11.2]] |
| გაზიარებული მდგომარეობის დაცვა `async` კოდში | [[თავი 12.2 ასინქრონული ბლოკირებები\|12.2]] |
| UI-ის საპასუხოდ შენარჩუნება | [[თავი 13.1 სამუშაოს დაგეგმვა thread pool-ზე\|13.1]], [[თავი 14.7 პროგრესის განახლებების დროსელირება\|14.7]] |
| მოთხოვნის ID-ის ტარება მთელ სტეკზე | [[თავი 14.4 იმპლიციტური მდგომარეობა\|14.4]] |
