ყველა წინა დიაგრამასა და განხილვაში გამოტოვებული იყო მნიშვნელოვანი კითხვა:

**როგორ იგებენ ეს ობიექტები ერთმანეთის არსებობის შესახებ?**

**Strategy**-ს დიაგრამა არ აჩვენებს, თუ როგორ გაიგო **Context** ობიექტმა რომელი კონკრეტული სტრატეგია გამოეყენებინა. ეს ნიმუშის ფარგლებს გარეთაა. იგივე ეხება როგორც **Component + Strategy**-ს, ასევე **Ports & Adapters**-ს.

თუმცა, ადრე თუ გვიან უნდა არსებობდეს რაიმე მოდული ან კოდი, რომელიც იცნობს ყველა მონაწილეს და აცნობს მათ ერთმანეთს. სწორედ იქ იდება **source-code დამოკიდებულებები**. ეს არის **Configurator** ობიექტი.  
არსებობს რამდენიმე სხვა გადაწყვეტაც, მაგრამ ყველაზე გავრცელებული სწორედ ესაა.

![[Pasted image 20250801233454.png]]
Configurator აწყობს ცოდნის ბილიკებს  

როდესაც სისტემურ დონეზე ტესტირებას აკეთებთ, არ არსებობს UI და არ არსებობს მონაცემთა ბაზები. თქვენ გყავთ **test harness**, რომელიც მართავს Provided Interface-ს, და **test double**, რომელიც ამუშავებს მოთხოვნებს Required Interface-ზე.

თქვენ წერთ მოდულს, სადაც ქმნით სამივეს: test harness-ს, test double-ს და კომპონენტს.  
მიჰმართავთ test harness-ს, რომ გამოიყენოს კომპონენტი, და უშვებთ test double-ს კომპონენტში როგორც კონკრეტულ სტრატეგიას Required Interface-ზე. შემდეგ ეუბნებით test harness-ს, რომ დაიწყოს, და ყველაფერი მუშაობს.

განვითარების ადრეულ ეტაპებზე, თითოეული ტესტის ქეისი აკეთებს ამ wiring-ს და შემდეგ უშვებს კონკრეტულ ტესტს. აქ configurator თითოეულ ტესტ ქეისშია.

შემდეგ, **პროდუქციულ გამოყენებაში**, ყველა იგივე ინსტანციაცია ხდება პროგრამის აგებისა და გაშვების დროს.  
**Startup მოდული** შექმნის კომპონენტს, UI-ს, და ადაპტერებს შესაბამის მონაცემთა ბაზებთან და სხვა აქტორებთან.  
თქვენი დიზაინის მიხედვით, configurator-მა შეიძლება გადასცეს სტრატეგიის ობიექტები კომპონენტს, ან ეს შეიძლება იყოს UI-ს ან სხვა მოდულის დავალება. ყველა შემთხვევაში, configurator იცნობს ყველა მონაწილეს და მათ საჭიროებებს.

რადგან **Configurator ნიმუშის განსაზღვრების ფარგლებს გარეთაა** — ზუსტად როგორ და სად ხდება ეს ცოდნის მიღება — ზოგადად მასზე არ საუბრობენ. თუმცა, იმისათვის რომ ნიმუშები სასარგებლო იყოს, უნდა გავხადოთ იგი აშკარა.


@Configuration
public class SpringDiscounterAppConfigurator {

    @Bean
    @ConditionalOnProperty(name = "for-managing-discounts",
                           havingValue = "test-cases")
    public Driver testCasesDriver(ForDiscounting discounterApp) {
        return new TestCases(discounterApp);
    }

    @Bean
    @ConditionalOnProperty(name = "for-managing-discounts",
                           havingValue = "cli")
    public Driver cliDriver(ForDiscounting discounterApp) {
        return new Console(discounterApp);
    }

    @Bean
    public ForDiscounting discounterApp(ForObtainingRates rateRepository) {
        return new DiscounterApp(rateRepository);
    }

    @Bean
    @ConditionalOnProperty(name = "for-obtaining-rates",
                           havingValue = "test-double")
    public ForObtainingRates testDoubleRateRepository() {
        return new StubRateRepository();
    }

    @Bean
    @ConditionalOnProperty(name = "for-obtaining-rates",
                           havingValue = "file")
    public ForObtainingRates fileRateRepository() {
        return new FileRateRepository();
    }
}


შემდეგი [[The article Configurable Receiver]]