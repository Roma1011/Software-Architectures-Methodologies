ეს არის სასწავლო კონსპექტების კრებული წიგნზე **Professional .NET 2.0 Generics** (Tod Golding, Wrox, 2005). მასალა ქართულად, საკუთარი სიტყვებით არის გადმოცემული (და არა სიტყვასიტყვით თარგმნილი), წიგნის იდეების, არგუმენტებისა და სტრუქტურის დაცვით. კოდის მაგალითები წიგნის კოდის ასლი არ არის: ისინი საკუთარი C# მაგალითებია, რომლებიც იმავე პრინციპებს ასახავს. დიაგრამები ორიგინალური SVG ნახაზებია.

წიგნი .NET 2.0-ს აღწერს. სადაც მას შემდეგ რამე შეიცვალა (C#-ის ახალი ვერსიები, LINQ, ახალი კოლექციები, მოძველებული ტექნოლოგიები), ეს `NOTE` ბლოკებშია აღნიშნული. სადაც წიგნის ტექსტში ან კოდში შეცდომაა, ის `WARNING` ბლოკით არის მონიშნული და გასწორებულია.

<svg viewBox="0 0 780 200" xmlns="http://www.w3.org/2000/svg" style="max-width:100%; height:auto; font-family: sans-serif;">
  <rect x="10" y="20" width="180" height="160" fill="#4A90D9" fill-opacity="0.12" stroke="currentColor" rx="8"/>
  <text x="100" y="45" text-anchor="middle" font-size="13" font-weight="bold" fill="currentColor">საფუძვლები</text>
  <text x="100" y="75" text-anchor="middle" font-size="11" fill="currentColor">1. Generics 101</text>
  <text x="100" y="95" text-anchor="middle" font-size="11" fill="currentColor">2. ტიპების უსაფრთხოება</text>
  <text x="100" y="115" text-anchor="middle" font-size="11" fill="currentColor">3. Generics და Templates</text>

  <rect x="200" y="20" width="180" height="160" fill="#27AE60" fill-opacity="0.12" stroke="currentColor" rx="8"/>
  <text x="290" y="45" text-anchor="middle" font-size="13" font-weight="bold" fill="currentColor">ენის კონსტრუქციები</text>
  <text x="290" y="75" text-anchor="middle" font-size="11" fill="currentColor">4. Generic კლასები</text>
  <text x="290" y="95" text-anchor="middle" font-size="11" fill="currentColor">5. Generic მეთოდები</text>
  <text x="290" y="115" text-anchor="middle" font-size="11" fill="currentColor">6. Generic დელეგატები</text>
  <text x="290" y="135" text-anchor="middle" font-size="11" fill="currentColor">7. შეზღუდვები</text>

  <rect x="390" y="20" width="180" height="160" fill="#E67E22" fill-opacity="0.15" stroke="currentColor" rx="8"/>
  <text x="480" y="45" text-anchor="middle" font-size="13" font-weight="bold" fill="currentColor">პლატფორმა</text>
  <text x="480" y="75" text-anchor="middle" font-size="11" fill="currentColor">8. BCL Generics</text>
  <text x="480" y="95" text-anchor="middle" font-size="11" fill="currentColor">9. Reflection, სერიალიზაცია</text>
  <text x="480" y="115" text-anchor="middle" font-size="11" fill="currentColor">10. რეკომენდაციები</text>
  <text x="480" y="135" text-anchor="middle" font-size="11" fill="currentColor">11. CLR-ის შიგნით</text>

  <rect x="580" y="20" width="190" height="160" fill="#8E44AD" fill-opacity="0.12" stroke="currentColor" rx="8"/>
  <text x="675" y="45" text-anchor="middle" font-size="13" font-weight="bold" fill="currentColor">ენები და ბიბლიოთეკები</text>
  <text x="675" y="75" text-anchor="middle" font-size="11" fill="currentColor">12. C++/CLI</text>
  <text x="675" y="95" text-anchor="middle" font-size="11" fill="currentColor">13. J#</text>
  <text x="675" y="115" text-anchor="middle" font-size="11" fill="currentColor">14. Power Collections</text>
</svg>

### თავი 1: Generics 101

[[თავი 1.1 რატომ გვჭირდება Generics]]
[[თავი 1.2 Generics-ის ტერმინოლოგია]]

---

### თავი 2: ტიპების უსაფრთხოება

[[თავი 2.1 ტიპების უსაფრთხოების ღირებულება]]

---

### თავი 3: Generics და Templates

[[თავი 3.1 Generics და Templates]]

---

### თავი 4: Generic კლასები

[[თავი 4.1 ტიპების პარამეტრიზაცია]]
[[თავი 4.2 მემკვიდრეობა Generic კლასებში]]
[[თავი 4.3 ველები და სტატიკური მონაცემები]]
[[თავი 4.4 მეთოდები Generic კლასებში]]
[[თავი 4.5 ჩადგმული კლასები და ხელმისაწვდომობა]]
[[თავი 4.6 default, Nullable და ტიპის ინფორმაცია]]
[[თავი 4.7 Property-ები, Event-ები, Struct-ები და ინტერფეისები]]

---

### თავი 5: Generic მეთოდები

[[თავი 5.1 Generic მეთოდების საფუძვლები]]
[[თავი 5.2 Generic მეთოდების გადატვირთვა და გადაფარვა]]
[[თავი 5.3 ტიპის გამოყვანა და დელეგატები]]

---

### თავი 6: Generic დელეგატები

[[თავი 6.1 Generic დელეგატების საფუძვლები]]
[[თავი 6.2 BCL-ის დელეგატები და დამატებითი თემები]]

---

### თავი 7: Generic შეზღუდვები

[[თავი 7.1 შეზღუდვების მიმოხილვა]]
[[თავი 7.2 შეზღუდვების ტიპები]]
[[თავი 7.3 მრავლობითი შეზღუდვები და ორაზროვნება]]

---

### თავი 8: BCL Generics

[[თავი 8.1 BCL Generics-ის სრული სურათი]]
[[თავი 8.2 Enumerator-ები და დელეგატები კოლექციებში]]
[[თავი 8.3 Collection, KeyedCollection და ReadOnlyCollection]]
[[თავი 8.4 List კლასი]]
[[თავი 8.5 Dictionary, SortedDictionary და SortedList]]
[[თავი 8.6 Queue, Stack და LinkedList]]

---

### თავი 9: Reflection, სერიალიზაცია და Remoting

[[თავი 9.1 Reflection და Generics]]
[[თავი 9.2 სერიალიზაცია და Remoting]]

---

### თავი 10: Generics-ის რეკომენდაციები

[[თავი 10.1 Generic შესაძლებლობების ამოცნობა]]
[[თავი 10.2 წაკითხვადობა და გამომსახველობა]]
[[თავი 10.3 BCL-ის Generic ტიპების გამოყენება]]
[[თავი 10.4 შეზღუდვების გამოყენება]]
[[თავი 10.5 სხვადასხვა რეკომენდაციები]]

---

### თავი 11: CLR-ის შიგნით

[[თავი 11.1 CLR-ის მიზნები და პრინციპები]]
[[თავი 11.2 Generic ტიპები IL-ში]]
[[თავი 11.3 სპეციალიზაცია და გაზიარება]]
[[თავი 11.4 ზუსტი ტიპები და წარმადობა]]

---

### თავი 12: Generics C++-ში

[[თავი 12.1 Generics C++-ში]]
[[თავი 12.2 შეზღუდვები, დელეგატები და Templates-თან შერევა]]

---

### თავი 13: Generics J#-ში

[[თავი 13.1 Generics J#-ში]]

---

### თავი 14: Power Collections

[[თავი 14.1 Power Collections-ის სრული სურათი]]
[[თავი 14.2 Algorithms კლასი]]
[[თავი 14.3 Power Collections-ის კონტეინერები]]

---

შემდეგი [[თავი 1.1 რატომ გვჭირდება Generics]]
