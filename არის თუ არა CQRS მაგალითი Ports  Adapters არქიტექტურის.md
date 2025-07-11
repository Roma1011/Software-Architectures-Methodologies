**Command-Query Responsibility Separation (CQRS)** — საინტერესო არქიტექტურაა, რომელიც შექმნა **Greg Young**-მა.  
იხილეთ:  
[https://cqrs.files.wordpress.com/2010/11/cqrs_documents.pdf](https://cqrs.files.wordpress.com/2010/11/cqrs_documents.pdf)

CQRS არქიტექტურა, მოწოდებული Mehmet Ozkaya-ს მიერ 
![[Pasted image 20250711005502.png]]
https://medium.com/design-microservices-architecture-with-patterns/cqrs-design-pattern-in-microservices-architectures-5d41e359768c

---

 **Lightning talk**-ის დროს (Mountain West RubyConf, 2010), **Alistair Cockburn**-მა წარადგინა CQRS არქიტექტურა და დასვა კითხვა:  
_„არის თუ არა ეს Hexagonal Architecture-ის მაგალითი?“_

პატრიკ მედოქსმა (Pat Maddox), რომელიც ასევე ესწრებოდა კონფერენციას, მოგვიანებით ჩამოჯდა ალისტერთან ერთად. მათ ერთად გააანალიზეს CQRS არქიტექტურის ევოლუცია და მასთან დაკავშირებული დიზაინის ნიმუშები.

**დასკვნა ასეთი იყო**:

> CQRS შეიძლება **გამოიყენებდეს** Ports & Adapters-ს,  
> მაგრამ **თავისთავად არ წარმოადგენს** Ports & Adapters არქიტექტურის მაგალითს.  
> 📺 იხილეთ: [https://www.youtube.com/watch?v=9kQ2veoeWZM](https://www.youtube.com/watch?v=9kQ2veoeWZM)

---

 როგორც Command, ისე Query სექციები CQRS-ში წარმოადგენს **დამოუკიდებელ ქვეკომპონენტებს (bounded subsystems)**.  
თუ ისინი **გაყოფილია** (decoupled) მართვის და მოწოდების ტექნოლოგიებისგან, მაშინ **წარმოადგენენ Ports & Adapters-ის მაგალითებს.**

**„თუ“  რადგან** CQRS არქიტექტურაში არაფერია, რაც შეგიშლით ხელს ამ სექციების პირდაპირი ტექნოლოგიებზე მიბმაში.  
ამგვარად, შესაძლებელია იგივე პრობლემები დაგიგროვდეთ, რაც ნებისმიერ სისტემას ექნება პირდაპირი დაკავშირებით.

---

ეს მაგალითები გვიჩვენებს, **როგორ შეიძლება Ports & Adapters-ის გამოყენება ფართო სისტემაში.**  
თუმცა, **მთლიანი CQRS სისტემა** შედგება არა მხოლოდ ბიზნეს ლოგიკის კომპონენტებისგან,  
არამედ **გარე ტექნოლოგიური ნაწილებისგანაც**, კერძოდ:

- **რეპოზიტორიებისგან**,
    
- **UI-ისგან**.
    

ამიტომ, **მთლიანი CQRS სისტემა არ არის** Ports & Adapters არქიტექტურის მაგალითი.