| ელემენტი                      | როლი                                                   |
| ----------------------------- | ------------------------------------------------------ |
| 1️⃣ **App (აპლიკაცია)**       | ბიზნეს ლოგიკის "გული", ტექნოლოგიების გარეშე            |
| 2️⃣ **Ports (პორტები)**       | გარესამყაროსთან საკომუნიკაციო წერტილები (ინტერფეისები) |
| 3️⃣ **Actors (აქტორები)**     | გარე სუბიექტები, რომლებიც კომუნიკაციას ამყარებენ აპთან |
| 4️⃣ **Adapters (ადაპტერები)** | პორტთან დასაკავშირებლად აუცილებელი თარგმანის კოდი      |


### [[აპლიკაცია (Application)]]

„აპი“ ან „სისტემა“ ნიშნავს იმ ბიზნეს ლოგიკას, რომელიც საჭიროა სასურველი ფუნქციონალის შესასრულებლად — მაგრამ არა **გარე ტექნოლოგიების ჩათვლით**.  
აპი არის ტექნოლოგიურად ნეიტრალური (technology-agnostic). იგი დაწერილია მხოლოდ ბიზნესის ტერმინებით, **გარეშე კონკრეტული ტექნოლოგიის ან პლატფორმის მიმართ ნებისმიერი მითითებისა**.

**Ports & Adapters** ნიმუში გთავაზობს განსაზღვრო ის წერტილები, სადაც შენი აპლიკაცია გარე სამყაროს ეჯახება.  
„გარე სამყაროს“ იდენტიფიცირება მარტივია: როგორც წესი, ის იქნება ისეთი ფიზიკური ელემენტი, როგორიცაა **მონაცემთა ბაზა, ადამიანური მომხმარებელი ან ქსელური კავშირი**.

შეიძლება აგრეთვე შეგხვდეს სიტუაცია, როდესაც გარე სამყარო არა ტექნოლოგიური, არამედ **სოციალური საზღვარია** — ანუ იქ, სადაც შენი პროექტის გუნდის გავლენა მთავრდება. ასეთ შემთხვევაში, შენ ქმნი მკაცრ, ტექნიკურ საზღვარს გამოცხადებული ინტერფეისებითა და პროტოკოლებით — და რა თქმა უნდა, ტესტებით, რათა დაიცვა ეს საზღვარი.

ორივე შემთხვევაში შენ ქმნი **კომპონენტს**. როგორც ეს აღწერილია თავის [[Component Strategy]], კომპონენტი აღწერს ორ ინტერფეისს:

- **მოცემული ინტერფეისი (Provided Interface)** — იმ სერვისების ნაკრები, რომელსაც აპი სთავაზობს გარე სამყაროს.
    
- **მოთხოვნილი ინტერფეისი (Required Interface)** — სერვისები, რომელთა მიწოდებაც სხვა ერთეულს ევალება აპისთვის, როგორც თავისი მოცემული ინტერფეისი.
    

პირველი — **მოცემული ინტერფეისი** — არის ის, რასაც ნებისმიერ აპლიკაციისგან ან სისტემისგან ველით: API ან გამოძახების ინტერფეისი.  
ხოლო **მოთხოვნილი ინტერფეისი** უფრო მოულოდნელია და სწორედ ის ხდის Ports & Adapters ნიმუშს ამდენად ძლიერად.

ამრიგად, **ყველა გარე ერთეულის**  იქნება ის პირველადი თუ მეორეული  მიმართ, აპი აცხადებს:

> „მე მხოლოდ მაშინ გესაუბრები, თუ ჩემი ენით მელაპარაკები“  
> და ამ ენას მკაფიოდ განსაზღვრავს.

ეს კი მას ხდის **კომპონენტად**, ისეთ ელემენტად, რომელიც შეგიძლია აიღო კატალოგიდან და ჩასვა ნებისმიერ სისტემაში რადგან ყველა საზღვრის პროტოკოლი სრულადაა განსაზღვრული.

გინდაც არასდროს გქონდეს მიზანი აპის ხელახლა გამოყენებისა სხვა კონტექსტში ან კატალოგში მოთავსების, ის, რომ სისტემას მკაცრ საზღვარს უსვამ, გაძლევს შემდეგ სარგებელს:

- უკეთესი ტესტირებადობა,
    
- ბიზნეს ლოგიკის გარე სამყაროში გადაჭარბებისგან დაცვა,
    
- ტექნოლოგიების მარტივი შეცვლა დროთა განმავლობაში.



### პორტები

პორტები განსაზღვრავენ აპლიკაციის ნამდვილ საზღვარს.

ყველა ინტერაქცია აპლიკაციასა და გარე სამყაროს შორის ხდება პორტის ინტერფეისზე, იმ ენით, რომელსაც თავად აპი განსაზღვრავს. შესაბამისად, პორტები მარკირებენ ზღვარს იმასთან, რაც არის აპის შიგნით და რაც არის მის გარეთ.

აპლიკაციასა და გარე აქტორებს შორის ინტერაქციებს ვაჯგუფებთ იმის მიხედვით, თუ **რა მიზნით** ხდება ეს ინტერაქცია. ამ მოდელში, ინტერაქციების თითოეული ჯგუფი, რომელსაც საერთო მიზანი ან განზრახვა აქვს, წარმოადგენს პორტს.

იმის გამო, რომ ინტერაქციები მიზნის მიხედვით იჯგუფება, პორტს ვარქმევთ იმ მიზნის მიხედვით, **ფორმით: _"რაღაცის გასაკეთებლად"_**. სახელწოდება იწყება სიტყვით **"For"**, მისდევს ზმნა -ing დაბოლოებით, შემდეგ კი საჭირო სიტყვები, რათა მიზანი გამოკვეთილად გამოიხატოს. მაგალითად:

- "For managing the contents of the shopping cart"  
    (_საყიდლების კალათის შიგთავსის სამართავად_)
    
- "For configuring the system"  
    (_სისტემის კონფიგურაციისთვის_)
    
- "For sending notifications"  
    (_შეტყობინებების გასაგზავნად_)
    

ასეთი სახელდებით, აპლიკაციას წარმოდგენაც კი არ აქვს, რა ტექნოლოგია იმყოფება პორტის მიღმა. იგი სრულად იზოლირებულია გარე სამყაროსგან და ურთიერთობას მხოლოდ თავისი პორტებით ამყარებს.

ეს იზოლაცია საშუალებას იძლევა, რომ პორტები გაშვების დროს დაკონფიგურდეს და დაერთოს რაც საჭიროა იმ მომენტში. სწორედ ეს თვისება აძლევს ნიმუშს მის ძლიერებას.

ჩვენ ხშირად ვხვდებით მმართავი პორტების სამ ტიპს:

- სისტემის ინიციალიზაციისა და კონფიგურაციისთვის,
    
- ადმინისტრაციული სამუშაოს შესასრულებლად,
    
- ბიზნეს მიზნებისთვის სისტემის გამოყენებისთვის.
    

ასევე გვხვდება მართული პორტების სამ ტიპიც:

- მონაცემების მისაღებად რეპოზიტორიიდან,
    
- ვიღაცისთვის შეტყობინების გასაგზავნად,
    
- რომელიმე მოწყობილობის სამართავად.
    

ზოგიერთ პროგრამულ ენაში, როგორიცაა Java და C++, პორტი უნდა გამოცხადდეს ცალსახად (explcitly). სხვებში, როგორიცაა Ruby და Python, პორტები არ ცხადდება პირდაპირ და, შესაბამისად, კოდში აშკარად არ ჩანს.


### გარე აქტორები

**Ports & Adapters** ნიმუში **განზრახ სიმეტრიულია**. ის საუბრობს მხოლოდ იმაზე, რაც არის აპის შიგნით და რაც არის გარეთ, სადაც **აპის პორტები წარმოადგენენ ზღვარს** ამ ორს შორის.

თუმცა, პრაქტიკაში არსებობს **მნიშვნელოვანი ასიმეტრია** — კერძოდ იმ მხრივ, **ვინ იცნობს ვის**, რათა მოხდეს ფუნქციის გამოძახება.

- აპს **არ სჭირდება იცოდეს**, თუ ვინ იძახებს მის მოცემულ ინტერფეისს (provided interface).  
    პირიქით: **მომთხოვნ ობიექტს** (გარე აქტორს) სჭირდება იცოდეს აპის შესახებ, რომ სერვისის გამოძახება შეძლოს.
    
- სამაგიეროდ, აპს **საჭიროა იცოდეს** ის ობიექტი, რომელსაც მიმართავს თავისი მოთხოვნილი ინტერფეისით (required interface).  
    ანუ, რომ სერვისის მოთხოვნა გაუგზავნოს გარე ერთეულს, აპს სჭირდება ამ ობიექტის „ჰენდლი“ (მისამართი, ბმული).
    

ამ განსხვავების უკეთ გასაგებად, ვიყენებთ ტერმინებს:

- **მმართავი აქტორები (driving actors)** – ისინი, ვინც აპს ამოძრავებს;
    
- **მართული აქტორები (driven actors)** – ისინი, ვინც აპის მიერ იძახებიან.
    

ასევე, **[[Use Case]]**-ების სამყაროში  უკვე არსებობს შესაბამისი ტერმინები:

- **პირველადი აქტორი (primary actor)** — ნებისმიერი ერთეული (ადამიანი ან ელექტრონული), რომელიც აპს ამოძრავებს, ანუ მისდამი სერვისის მოთხოვნას აგზავნის. ეს შეიძლება იყოს კომპლექსური ინტერაქციების დასაწყისი.
    
- **მეორეული აქტორი (secondary actor)** — ნებისმიერი ერთეული, რომელსაც აპი ამოძრავებს ანუ რომელსაც აპი აგზავნის სერვისის მოთხოვნას.
    

ეს ტერმინები იდეალურად ეწყობა პორტების კონცეფციას:

- **პირველადი აქტორები** = **მმართავი აქტორები**, რომლებიც მოქმედებენ **მმართავ პორტებზე** (driving/primary ports).
    
- **მეორეული აქტორები** = **მართული აქტორები**, რომლებიც მოქმედებენ **მართული პორტებით** (driven/secondary ports).
    

[[Use Case]]-ების ენის გამოყენებით, ვხედავთ, **რამდენი პორტი** უნდა შევიტანოთ აპში და როგორ დავაჯგუფოთ ინტერფეისები პორტებად:

- Use case-ის აქტორი განისაზღვრება იმით, **რა მიზნით** ურთიერთობს სისტემასთან.
    
- შესაბამისად, ვიწყებთ იმით, რომ თითოეულ აქტორზე **შევქმნათ პორტი**,  
    და თითოეულ ინტერფეისზე ვკითხოთ საკუთარ თავს:  
    _“ვისთვისაა ეს ინტერფეისი?”_
    

დროთა განმავლობაში შეიძლება გადაწყვიტო, რომ შეცვალო ან გააფართო ის საწყისი არქიტექტურა, მაგრამ **ეს არის ძალიან კარგი დასაწყისი**.








### ადაპტერები

| Driving Adapter | პორტის საშუალებით მიღებული მოთხოვნის კონვერტაცია ტექნოლოგიიდან → აპი |
| --------------- | -------------------------------------------------------------------- |
| Driven Adapter  | აპის მოთხოვნის გადაყვანა აპისთვის უცნობ ტექნოლოგიაზე (მაგ. SMTP)     |


შეიძლება მოხდეს ისე, რომ მმართავმა ან მართულმა აქტორმა უკვე იცოდეს და იყენებდეს აპის **მოცემულ (provided)** ან **მოთხოვნილ (required)** ინტერფეისს.  
ასეთ შემთხვევაში **ადაპტერი საჭირო არ არის**.

მაგალითად:

- როდესაც იწყებ სისტემის განვითარებას, როგორც წესი, წერ ტესტებს აპის სამართავად.  
    შეიძლება პირდაპირ ტესტ კეისში ჩაწერო აპის მოცემული ინტერფეისი, ან შექმნა მოყალბებული ბაზები (mock) ან ტესტ დუბლიკატები (test doubles) მართული მხარისთვის, რომლებიც აპის მოთხოვნილ ინტერფეისზე რეაგირებენ.
    
- შეიძლება ორი აპი თავიდანვე ისე დაპროექტდეს, რომ ერთმანეთს დაუკავშირდნენ.  
    ორივე გამოყენებს Ports & Adapters სტრუქტურას, და შენ თავიდანვე ისე ქმნი მათ ინტერფეისებს, რომ ისინი ემთხვევიან ერთმანეთს.
    

ასეთ სიტუაციებში ადაპტერი საჭირო არ არის  გარე აქტორები უკვე შეესაბამებიან საჭირო ინტერფეისებს.

---

სხვა შემთხვევებში, როცა გარე აქტორი არ ემთხვევა აპის ინტერფეისს, საჭიროა დაწერო კოდი, რომელიც ერთი მხარის ინტერფეისს გარდაქმნის მეორის ინტერფეისად.  
ეს კოდია **ადაპტერი**.

### ადაპტერი  ეს უკვე ნაცნობი ცნებაა

ადამიანი, რომელიც კომპიუტერთან ზის, ვერ იძახებს აპს პირდაპირ.  
ის მოქმედებს **მომხმარებლის ინტერფეისის** საშუალებით  იქნება ეს ტექსტური (CLI), ხმოვანი თუ გრაფიკული ინტერფეისი (GUI).  
ეს ინტერფეისები გარდაქმნიან ადამიანის ქმედებებს აპის ინტერფეისის შესაბამის მოქმედებად  რაც საშუალებას აძლევს ადამიანს და აპს, რომ ურთიერთობა დაამყარონ.

CLI ან GUI შეიძლება მართული მხარეზეც იყოს მოთავსებული.  
მაგალითად, თუ ქმნი ხელოვნური ინტელექტის (AI) ძრავს, ტესტირებისას შეიძლება რეაგირების მხარეს ადამიანი ჩააყენო  რომელიც პასუხებს აგზავნის აპისკენ.

### ადაპტერები თითქმის ყოველთვის საჭიროა რეალურ ტექნოლოგიებთან ინტეგრაციისთვის

სადაც ტექნოლოგია ჩნდება  იქნება ეს:

- ინტერნეტი
    
- მონაცემთა ბაზა
    
- პეიჯინგ სისტემა
    
- რეალურ დროში მომავალი სტრიმი
    
- ან ადამიანი —
    

შენ დაგჭირდება ადაპტერი.

---

**ადაპტერები მდებარეობენ აპის გარეთ.**  
ამის გამო, **როგორ დაალაგებ ან სტრუქტურირებ მათ, შენზეა დამოკიდებული**.

Ports & Adapters ნიმუში **არ განსაზღვრავს**, როგორ უნდა იყოს მოწყობილი აპის შიდა სტრუქტურა, ან გარე ტექნოლოგიების სტრუქტურა.  
მას მხოლოდ ერთი რამ აქვს ნათლად ჩამოყალიბებული:

> არსებობს შიდა და გარე ნაწილი, და ამ ორს შორის ზღვარს პორტები განსაზღვრავს  
> (მოცემული და მოთხოვნილი ინტერფეისები).

---

ამის მიუხედავად, ადაპტერების სტრუქტურირებასთან დაკავშირებული დაბნეულობა ხშირად ჩნდება.  
არასწორად ორგანიზებული ადაპტერები პროგრამისტებს აეჭვებთ იმაზე, სად განათავსონ კოდი ან რა ხდება სინამდვილეში აპისა და გარე სამყაროს ინტერაქციაში.

ამიტომ, ამ წიგნში საკმაო ყურადღებას დავუთმობთ ადაპტერების სწორი სტრუქტურირების მეთოდებს.


შემდეგი [[კონფიგურატორი(The Configurator)]]