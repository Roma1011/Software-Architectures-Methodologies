ასინქრონული კოდის სარგებელი .NET-ის გამოგონებამდე ათწლეულებით ადრე კარგად იყო გააზრებული. .NET-ის ადრეულ დღეებში ასინქრონული კოდის რამდენიმე განსხვავებული სტილი განვითარდა, ესეც და ისიც გამოიყენეს და საბოლოოდ მიატოვეს. ეს ყველა ცუდი იდეა არ ყოფილა: ბევრმა მათგანმა თანამედროვე `async`/`await` მიდგომას გაუკვალა გზა. თუმცა იქ ბევრი მემკვიდრეობით მიღებული კოდია, რომელიც ძველ ასინქრონულ შაბლონებს იყენებს. ეს დანართი უფრო გავრცელებულ შაბლონებს განიხილავს.

### `Socket`: ერთი ტიპი, ხუთი შაბლონი

ზოგჯერ ერთი და იგივე ტიპი წლების განმავლობაში ახლდება და სულ უფრო მეტ წევრს იძენს, რადგან რამდენიმე ასინქრონულ შაბლონს უჭერს მხარს. ალბათ საუკეთესო მაგალითი `Socket` კლასია:

```csharp
class Socket
{
    // სინქრონული
    public int Send(byte[] buffer, int offset, int size, SocketFlags flags);

    // APM
    public IAsyncResult BeginSend(byte[] buffer, int offset, int size,
        SocketFlags flags, AsyncCallback callback, object state);
    public int EndSend(IAsyncResult result);

    // საკუთარი, APM-თან ძალიან ახლოს
    public IAsyncResult BeginSend(byte[] buffer, int offset, int size,
        SocketFlags flags, out SocketError error,
        AsyncCallback callback, object state);
    public int EndSend(IAsyncResult result, out SocketError error);

    // საკუთარი
    public bool SendAsync(SocketAsyncEventArgs e);

    // TAP (გაფართოების მეთოდად)
    public Task<int> SendAsync(ArraySegment<byte> buffer,
        SocketFlags socketFlags);

    // TAP (გაფართოების მეთოდად) უფრო ეფექტური ტიპებით
    public ValueTask<int> SendAsync(ReadOnlyMemory<byte> buffer,
        SocketFlags socketFlags, CancellationToken cancellationToken = default);
}
```

სამწუხაროდ, რაკი დოკუმენტაციის უმეტესობა ანბანურია და გადატვირთვები უამრავია, `Socket`-ის მსგავსი ტიპების გაგება რთულდება.

<svg viewBox="0 0 780 270" xmlns="http://www.w3.org/2000/svg" style="max-width:100%; height:auto; font-family: sans-serif;">
  <text x="390" y="22" text-anchor="middle" font-size="12" font-weight="bold" fill="currentColor">ხუთი შაბლონი და მათი ამომცნობი ნიშნები</text>

  <rect x="30" y="42" width="720" height="38" fill="#27AE60" fill-opacity="0.16" stroke="currentColor" rx="5"/>
  <text x="120" y="60" text-anchor="middle" font-size="10" font-weight="bold" fill="currentColor">TAP</text>
  <text x="120" y="74" text-anchor="middle" font-size="8" fill="currentColor">თანამედროვე</text>
  <text x="440" y="66" text-anchor="middle" font-size="9" font-family="monospace" fill="currentColor">Task&lt;T&gt; FooAsync(...)  →  await</text>

  <rect x="30" y="88" width="720" height="38" fill="#4A90D9" fill-opacity="0.14" stroke="currentColor" rx="5"/>
  <text x="120" y="106" text-anchor="middle" font-size="10" font-weight="bold" fill="currentColor">APM</text>
  <text x="120" y="120" text-anchor="middle" font-size="8" fill="currentColor">Begin/End</text>
  <text x="440" y="112" text-anchor="middle" font-size="9" font-family="monospace" fill="currentColor">IAsyncResult BeginFoo(...) + EndFoo(...)  →  FromAsync</text>

  <rect x="30" y="134" width="720" height="38" fill="#E67E22" fill-opacity="0.14" stroke="currentColor" rx="5"/>
  <text x="120" y="152" text-anchor="middle" font-size="10" font-weight="bold" fill="currentColor">EAP</text>
  <text x="120" y="166" text-anchor="middle" font-size="8" fill="currentColor">მოვლენა</text>
  <text x="440" y="158" text-anchor="middle" font-size="9" font-family="monospace" fill="currentColor">void FooAsync(...) + FooCompleted  →  TaskCompletionSource</text>

  <rect x="30" y="180" width="720" height="38" fill="#8E44AD" fill-opacity="0.14" stroke="currentColor" rx="5"/>
  <text x="120" y="198" text-anchor="middle" font-size="10" font-weight="bold" fill="currentColor">CPS</text>
  <text x="120" y="212" text-anchor="middle" font-size="8" fill="currentColor">callback</text>
  <text x="440" y="204" text-anchor="middle" font-size="9" font-family="monospace" fill="currentColor">void Foo(..., Action&lt;Exception, T&gt; done)  →  TaskCompletionSource</text>

  <rect x="30" y="226" width="720" height="38" fill="#C0392B" fill-opacity="0.14" stroke="currentColor" rx="5"/>
  <text x="120" y="244" text-anchor="middle" font-size="10" font-weight="bold" fill="currentColor">საკუთარი</text>
  <text x="120" y="258" text-anchor="middle" font-size="8" fill="currentColor">უნიკალური</text>
  <text x="440" y="250" text-anchor="middle" font-size="9" font-family="monospace" fill="currentColor">რაც გინდა  →  TaskCompletionSource</text>
</svg>

*ნახაზი B-1: ასინქრონული შაბლონების რუკა.*

---

## Task-Based Asynchronous Pattern (TAP)

**TAP** თანამედროვე ასინქრონული API შაბლონია, რომელიც `await`-თან გამოსაყენებლად მზადაა. ყოველი ასინქრონული ოპერაცია წარმოდგენილია **ერთი მეთოდით**, რომელიც **await-ადს** აბრუნებს. „await-ადი" არის ნებისმიერი ტიპი, რომელიც `await`-ს შეუძლია მოიხმაროს: ჩვეულებრივ `Task` ან `Task<T>`, მაგრამ შეიძლება იყოს `ValueTask`, `ValueTask<T>`, ფრეიმვორკის მიერ განსაზღვრული ტიპი (მაგალითად, `IAsyncAction` ან `IAsyncOperation<T>`), ან ბიბლიოთეკის საკუთარი ტიპიც.

TAP მეთოდებს ჩვეულებრივ **`Async` სუფიქსი** აქვს. თუმცა ეს მხოლოდ **კონვენციაა**: ყველა TAP მეთოდს `Async` სუფიქსი არ აქვს. ის შეიძლება გამოტოვდეს, თუ API-ის ავტორს მიაჩნია, რომ ასინქრონული კონტექსტი საკმარისადაა ნაგულისხმევი (მაგალითად, `Task.WhenAll`-სა და `Task.WhenAny`-ს `Async` სუფიქსი არ აქვს). გარდა ამისა, გახსოვდეთ, რომ `Async` სუფიქსი შეიძლება **არა-TAP** მეთოდებსაც ჰქონდეს (მაგალითად, `WebClient.DownloadStringAsync` TAP მეთოდი **არ არის**). ასეთ შემთხვევაში ჩვეული შაბლონია, რომ TAP მეთოდს **`TaskAsync`** სუფიქსი ჰქონდეს (მაგალითად, `WebClient.DownloadStringTaskAsync`).

ასინქრონული ნაკადების დამბრუნებელი მეთოდებიც TAP-ისებურ შაბლონს მიჰყვება, `Async` სუფიქსით. მართალია, ისინი await-ადს არ აბრუნებს, მაგრამ **await-ად ნაკადს** აბრუნებს: ტიპებს, რომლებიც `await foreach`-ით მოიხმარება.

**ამომცნობი ნიშნები:**

1. ოპერაცია წარმოდგენილია **ერთი მეთოდით**.
2. მეთოდი აბრუნებს **await-ადს ან await-ად ნაკადს**.
3. მეთოდი ჩვეულებრივ `Async`-ით მთავრდება.

```csharp
class ExampleHttpClient
{
    public Task<string> GetStringAsync(Uri requestUri);

    // სინქრონული ეკვივალენტი, შესადარებლად
    public string GetString(Uri requestUri);
}
```

**მოხმარება:** `await`-ით. მთელი ეს წიგნი ამას ეხება.

---

## Asynchronous Programming Model (APM)

TAP-ის შემდეგ **APM** ალბათ შემდეგი ყველაზე გავრცელებული შაბლონია. ეს იყო პირველი შაბლონი, სადაც ასინქრონულ ოპერაციებს **პირველი კლასის ობიექტური წარმოდგენა** ჰქონდათ. მისი გამომჟღავნებელი ნიშანია `IAsyncResult` ობიექტები `Begin`-ითა და `End`-ით დაწყებული მეთოდების წყვილთან ერთად.

`IAsyncResult`-ზე ძლიერ იმოქმედა **ნატიურმა overlapped I/O**-მ. APM შაბლონი მომხმარებელ კოდს საშუალებას აძლევს, სინქრონულადაც მოიქცეს და ასინქრონულადაც:

- **დაბლოკოს** ოპერაციის დასრულებამდე (`End` მეთოდის გამოძახებით);
- **გამოკითხოს** ოპერაციის დასრულება სხვა რამის კეთებისას;
- **callback დელეგატი** მიაწოდოს, რომელიც დასრულებისას გამოიძახება.

ყველა შემთხვევაში მომხმარებელმა კოდმა საბოლოოდ **აუცილებლად უნდა გამოიძახოს `End`** ასინქრონული ოპერაციის შედეგის მისაღებად. თუ ოპერაცია `End`-ის გამოძახებისას დასრულებული არ არის, ის გამომძახებელ ნაკადს **დაბლოკავს**.

`Begin` მეთოდი `AsyncCallback` პარამეტრსა და `object` პარამეტრს (ჩვეულებრივ `state`) იღებს ბოლო ორ პარამეტრად. `object` პარამეტრი შეიძლება იყოს რაც გინდათ: ეს .NET-ის ძალიან ადრეული დღეების გადმონაშთია, ლამბდა მეთოდებისა და ანონიმური მეთოდების არსებობამდე.

> [!NOTE] რატომ იშვიათია APM ეკოსისტემაში
> APM საკმაოდ გავრცელებულია Microsoft-ის ბიბლიოთეკებში, მაგრამ არა ფართო .NET ეკოსისტემაში. ეს იმიტომ, რომ `IAsyncResult`-ის ხელახლა გამოსაყენებელი იმპლემენტაცია არასდროს გამოქვეყნებულა, და ამ ინტერფეისის სწორად განხორციელება საკმაოდ რთულია. გარდა ამისა, APM-ზე დაფუძნებული სისტემების **კომპოზიცია რთულია**.

**ამომცნობი ნიშნები:**

1. ოპერაცია წარმოდგენილია **მეთოდების წყვილით**: ერთი `Begin`-ით იწყება, მეორე `End`-ით.
2. `Begin` აბრუნებს `IAsyncResult`-ს და იღებს ყველა ჩვეულებრივ შემავალ პარამეტრს, პლუს დამატებით `AsyncCallback` და `object` პარამეტრებს.
3. `End` იღებს მხოლოდ `IAsyncResult`-ს და აბრუნებს შედეგს, თუ ასეთია.

```csharp
class MyHttpClient
{
    public IAsyncResult BeginGetString(Uri requestUri,
        AsyncCallback callback, object state);

    public string EndGetString(IAsyncResult asyncResult);

    // სინქრონული ეკვივალენტი, შესადარებლად
    public string GetString(Uri requestUri);
}
```

**მოხმარება:** გადააქციეთ TAP-ად `Task.Factory.FromAsync`-ით ([[თავი 8.2 Begin-End მეთოდების async გარსები|8.2]]).

> [!TIP] როცა APM „თითქმის" APM-ია
> ზოგჯერ კოდი შაბლონს **თითქმის** მიჰყვება, მაგრამ არა სრულად. მაგალითად, ძველ `Microsoft.TeamFoundation` ბიბლიოთეკებში `Begin` მეთოდებს `object` პარამეტრი არ ჰქონდათ. ასეთ შემთხვევაში `Task.Factory.FromAsync` არ იმუშავებს, და ორი ვარიანტი გრჩებათ: **ნაკლებად ეფექტური** (გამოიძახოთ `Begin` და მიღებული `IAsyncResult` გადასცეთ `FromAsync`-ს) და **ნაკლებად ელეგანტური** (უფრო მოქნილი `TaskCompletionSource<T>`, [[თავი 8.3 ნებისმიერი შეტყობინების async გარსი|8.3]]).

---

## Event-Based Asynchronous Programming (EAP)

**EAP** განსაზღვრავს **მეთოდის/მოვლენის** შესაბამის წყვილს. მეთოდი ჩვეულებრივ `Async`-ით მთავრდება, და ის საბოლოოდ იწვევს მოვლენას, რომელიც `Completed`-ით მთავრდება.

EAP-თან მუშაობისას რამდენიმე სირთულეა:

1. უნდა გახსოვდეთ, რომ **დამმუშავებელი მეთოდის გამოძახებამდე** დაამატოთ მოვლენას. სხვაგვარად **რბოლის მდგომარეობა** გექნებათ, სადაც მოვლენა ხელმოწერამდე მოხდება და ვერასდროს დაინახავთ დასრულებას.
2. EAP შაბლონით დაწერილი კომპონენტები ჩვეულებრივ რაღაც მომენტში იჭერს მიმდინარე `SynchronizationContext`-ს და შემდეგ მოვლენას ამ კონტექსტში აღძრავს. ზოგი მას **კონსტრუქტორში** იჭერს, ზოგი კი **მეთოდის გამოძახებისას**.

**ამომცნობი ნიშნები:**

1. ოპერაცია წარმოდგენილია **მოვლენითა და მეთოდით**.
2. მოვლენა `Completed`-ით მთავრდება.
3. `Completed` მოვლენის არგუმენტების ტიპი შეიძლება `AsyncCompletedEventArgs`-დან იყოს წარმოებული.
4. მეთოდი ჩვეულებრივ `Async`-ით მთავრდება.
5. მეთოდი **`void`-ს აბრუნებს**.

> [!IMPORTANT] EAP vs TAP განსხვავება
> `Async`-ით დამთავრებული EAP მეთოდები TAP-ისგან იმით განსხვავდება, რომ EAP მეთოდები **`void`-ს აბრუნებს**, TAP მეთოდები კი **await-ად ტიპს**.

```csharp
class GetStringCompletedEventArgs : AsyncCompletedEventArgs
{
    public string Result { get; }
}

class MyHttpClient
{
    public void GetStringAsync(Uri requestUri);
    public event Action<object, GetStringCompletedEventArgs> GetStringCompleted;

    // სინქრონული ეკვივალენტი, შესადარებლად
    public string GetString(Uri requestUri);
}
```

**მოხმარება:** გადააქციეთ TAP-ად `TaskCompletionSource<T>`-ით ([[თავი 8.1 EAP მეთოდების async გარსები|8.1]]).

---

## Continuation Passing Style (CPS)

ეს შაბლონი გაცილებით უფრო გავრცელებულია სხვა ენებში, განსაკუთრებით JavaScript-სა და TypeScript-ში (Node.js). ამ შაბლონში ყოველი ასინქრონული ოპერაცია **callback დელეგატს** იღებს, რომელიც ოპერაციის დასრულებისას გამოიძახება, წარმატებით ან შეცდომით. ვარიანტი ორ callback დელეგატს იყენებს: ერთს წარმატებისთვის, მეორეს შეცდომისთვის. ასეთ callback-ს **„გაგრძელება" (continuation)** ჰქვია, და ის პარამეტრად გადაეცემა, აქედან სახელი „გაგრძელების გადაცემის სტილი". ეს შაბლონი .NET-ის სამყაროში არასდროს ყოფილა გავრცელებული, მაგრამ რამდენიმე ძველი ღია კოდის ბიბლიოთეკა მას იყენებდა.

**ამომცნობი ნიშნები:**

1. ოპერაცია წარმოდგენილია ერთი მეთოდით.
2. მეთოდი იღებს დამატებით პარამეტრს, რომელიც **callback დელეგატია**; ეს დელეგატი ორ არგუმენტს იღებს: ერთს შეცდომებისთვის, მეორეს შედეგებისთვის.
3. ალტერნატივად, მეთოდი ორ დამატებით callback დელეგატს იღებს: ერთს მხოლოდ შეცდომებისთვის, მეორეს მხოლოდ შედეგებისთვის.
4. callback დელეგატებს ჩვეულებრივ `done` ან `next` ჰქვია.

```csharp
class MyHttpClient
{
    public void GetString(Uri requestUri, Action<Exception, string> done);

    // სინქრონული ეკვივალენტი, შესადარებლად
    public string GetString(Uri requestUri);
}
```

**მოხმარება:** გადააქციეთ TAP-ად `TaskCompletionSource<T>`-ით, callback დელეგატების გადაცემით, რომლებიც უბრალოდ `TaskCompletionSource<T>`-ს ასრულებს ([[თავი 8.3 ნებისმიერი შეტყობინების async გარსი|8.3]]).

---

## საკუთარი ასინქრონული შაბლონები

ძალიან სპეციალიზებული ტიპები ზოგჯერ **საკუთარ** ასინქრონულ შაბლონებს განსაზღვრავს. ყველაზე ცნობილი მაგალითია `Socket` ტიპი, რომელმაც შაბლონი განსაზღვრა, სადაც `SocketAsyncEventArgs` ეგზემპლარები გადაეცემოდა და ოპერაციას წარმოადგენდა. ეს შაბლონი იმიტომ შემოვიდა, რომ `SocketAsyncEventArgs` **ხელახლა გამოსაყენებელი** იყო და ამგვარად ამცირებდა მეხსიერების „ბრუნვას" იმ აპლიკაციებში, რომლებიც ინტენსიურ ქსელურ აქტივობას ეწევიან. თანამედროვე აპლიკაციებს მსგავსი მოგებისთვის `ValueTask<T>` და `ManualResetValueTaskSourceCore<T>` შეუძლიათ გამოიყენონ ([[თავი 2.10 ValueTask-ის შექმნა|2.10]]).

საკუთარ შაბლონებს **საერთო ნიშნები არ აქვთ**, ამიტომ ისინი ყველაზე რთული ამოსაცნობია. საბედნიეროდ, ისინი იშვიათია.

```csharp
class MyHttpClient
{
    public void GetString(Uri requestUri,
        MyHttpClientAsynchronousOperation operation);

    // სინქრონული ეკვივალენტი, შესადარებლად
    public string GetString(Uri requestUri);
}
```

**მოხმარება:** `TaskCompletionSource<T>` ერთადერთი გზაა ([[თავი 8.3 ნებისმიერი შეტყობინების async გარსი|8.3]]).

---

## `ISynchronizeInvoke`

ყველა წინა შაბლონი ეხება ასინქრონულ ოპერაციებს, რომლებიც **იწყება და ერთხელ სრულდება**. ზოგიერთი კომპონენტი **გამოწერის (subscription)** მოდელს მიჰყვება: ისინი მოვლენების **უბიძგებად ნაკადს** წარმოადგენს და არა ერთჯერად ოპერაციას. კარგი მაგალითია `FileSystemWatcher`: ფაილური სისტემის ცვლილებების დასაკვირვებლად მომხმარებელი კოდი ჯერ რამდენიმე მოვლენას აწერს ხელს და შემდეგ `EnableRaisingEvents` თვისებას `true`-ზე აყენებს.

ზოგიერთი კომპონენტი თავისი მოვლენებისთვის **`ISynchronizeInvoke`** შაბლონს იყენებს. ისინი ერთ `ISynchronizeInvoke` თვისებას ამჟღავნებს, და მომხმარებლები ამ თვისებას ისეთ იმპლემენტაციაზე აყენებს, რომელიც კომპონენტს სამუშაოს დაგეგმვის საშუალებას აძლევს. ეს ყველაზე ხშირად სამუშაოს **UI ნაკადზე** დასაგეგმად გამოიყენება. კონვენციით, თუ `ISynchronizeInvoke` არის `null`, მოვლენების სინქრონიზაცია არ ხდება და ისინი ფონურ ნაკადებზე შეიძლება აღიძრას.

**ამომცნობი ნიშნები:**

1. არსებობს `ISynchronizeInvoke` ტიპის თვისება.
2. თვისებას ჩვეულებრივ `SynchronizingObject` ჰქვია.

```csharp
class MyHttpClient
{
    public ISynchronizeInvoke SynchronizingObject { get; set; }
    public void StartListening();
    public event Action<string> StringArrived;
}
```

**მოხმარება:** რაკი `ISynchronizeInvoke` გამოწერის მოდელში **მრავალ მოვლენას** გულისხმობს, ამ კომპონენტების მოხმარების სწორი გზა ამ მოვლენების **observable ნაკადად თარგმნაა**: `FromEvent`-ით ([[თავი 6.1 .NET მოვლენების გარდაქმნა|6.1]]) ან `Observable.Create`-ით.

---

## შეჯამების ცხრილი

| შაბლონი | ამომცნობი ნიშანი | გარდაქმნა TAP-ად |
|---|---|---|
| **TAP** | `Task<T> FooAsync(...)` | უკვე TAP-ია |
| **APM** | `BeginFoo`/`EndFoo` + `IAsyncResult` | `Task.Factory.FromAsync` ([[თავი 8.2 Begin-End მეთოდების async გარსები\|8.2]]) |
| **EAP** | `void FooAsync(...)` + `FooCompleted` | `TaskCompletionSource<T>` ([[თავი 8.1 EAP მეთოდების async გარსები\|8.1]]) |
| **CPS** | `Action<Exception, T> done` პარამეტრი | `TaskCompletionSource<T>` ([[თავი 8.3 ნებისმიერი შეტყობინების async გარსი\|8.3]]) |
| **საკუთარი** | უნიკალური | `TaskCompletionSource<T>` ([[თავი 8.3 ნებისმიერი შეტყობინების async გარსი\|8.3]]) |
| **`ISynchronizeInvoke`** | `SynchronizingObject` თვისება | `Observable.FromEvent` ([[თავი 6.1 .NET მოვლენების გარდაქმნა\|6.1]]) |

---

წინა [[დანართი A - ძველი პლატფორმების მხარდაჭერა]]
შემდეგი [[Concurrency in C# Cookbook]]
