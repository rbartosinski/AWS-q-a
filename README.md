# AWS Associate Architect - Questions and Answers 
Knowledge base about architecture standards in AWS (in Polish).

## EC2

#### EC2 Metadata
Musisz pobrać identyfikator instancji, klucze publiczne i publiczny adres IP serwera EC2, który utworzyłeś do oznaczania i grupowania atrybutów

    metadane instancji

#### Dostęp dla operacji wykonanych przez EC2

Instancja EC2, która do codziennych operacji wymaga dostępu do różnych usług AWS

    EC2 role attached

#### Services with underlying OS
Unikaj używania w pełni zarządzanych usług AWS, a zamiast tego używaj tylko określonych usług, które umożliwiają dostęp do bazowego systemu operacyjnego dla zasobu.

    EC2 (OS) & EMR (hadoop)

#### Kopiowanie obrazu AMI pozmiędzy regionami
W tym scenariuszu aktualnie używane instancje EC2 zależą od wstępnie zbudowanego interfejsu AMI. Ten AMI nie jest dostępny dla innego regionu, dlatego musisz skopiować go do regionu us-west-2, aby poprawnie ustanowić instancję odzyskiwania po awarii.

Możesz skopiować obraz maszyny Amazon (AMI) w obrębie regionu AWS, używając:

- konsoli zarządzania AWS,

- narzędzi wiersza polecenia AWS lub zestawów SDK lub

- interfejsu `API Amazon EC2`, z których wszystkie obsługują akcję `CopyImage`.

#### Koszty transferu danych pomiędzy zasobami w regionie
You deployed an On-Demand EC2 instance that is transferring large amounts of data to an Amazon S3 bucket in the same region.

    Przesyłanie danych z instancji EC2 do Amazon S3, Amazon Glacier, Amazon DynamoDB, Amazon SES, Amazon SQS lub Amazon SimpleDB w tym samym regionie AWS nie wiąże się z żadnymi kosztami.

### Reserved Instances
Instancje zarezerwowane (RI) zapewniają znaczną zniżkę (do 75%) w porównaniu z cenami instancji na żądanie. Masz swobodę zmieniania rodzin, typów systemów operacyjnych i najmu, a jednocześnie korzystasz z cen RI, kiedy korzystasz z Convertible RIs. Należy tutaj pamiętać, że Instancje zarezerwowane nie są instancjami fizycznymi, lecz zniżką rozliczeniową stosowaną w przypadku korzystania z instancji na żądanie na koncie.

    You can also sell your unused instance on the Reserved Instance Marketplace.

#### RI #2    
Podczas planowania zostało omówione, że będziesz potrzebować dwóch instancji EC2, które powinny działać nieprzerwanie przez TRZY LATA. Oczekuje się, że wykorzystanie procesora przez instancje EC2 będzie stabilne i przewidywalne.

    Reserved Instances

Instancje zarezerwowane zapewniają znaczną zniżkę (do 75%) w porównaniu z cenami instancji na żądanie. Ponadto, gdy Zarezerwowane Instancje są przypisane do konkretnej Strefy Dostępność, zapewniają rezerwację pojemności, co daje dodatkową pewność co do możliwości uruchamiania instancji, gdy są one potrzebne.

W przypadku aplikacji, które mają stały stan lub przewidywalne użycie, wystąpienia zarezerwowane mogą zapewnić znaczne oszczędności w porównaniu do korzystania z wystąpień na żądanie.

- Aplikacje ze stałym stanem użytkowania
- Aplikacje, które mogą wymagać zarezerwowanej pojemności
- Klienci, którzy mogą zobowiązać się do korzystania z EC2 przez okres 1 lub 3 lat w celu zmniejszenia swoich całkowitych kosztów obliczeniowych

#### Spoty
EC2 non-priority batch loads, which can be interrupted at any time.   

    Spot

Jeśli ten proces zostanie przerwany, wideo zostanie transkodowane przez inną instancję opartą na systemie kolejkowania. Ta aplikacja ma dużą liczbę zaległych filmów, które należy transkodować. Twój menedżer chciałby zmniejszyć ten zaległość, dodając więcej wystąpień EC2, jednak są one potrzebne tylko do momentu zmniejszenia zaległości.

W tym scenariuszu, który typ instancji Amazon EC2 jest najbardziej opłacalny w użyciu?

    Spot

Wymagana jest instancja, która nie będzie używana jako serwer główny, ale jako zapasowy zasób obliczeniowy, aby usprawnić proces transkodowania aplikacji. Instancje te powinny również zostać zakończone po znacznym zmniejszeniu zaległości. Ponadto scenariusz wspomina, że jeśli bieżący proces zostanie przerwany, wideo może zostać transkodowane przez inną instancję opartą na systemie kolejkowania.

By simply selecting Spot when launching EC2 instances, you can save up-to 90% on On-Demand prices. 
Take note that there is no "bid price" anymore for Spot EC2 instances since March 2018. You simply have to set your maximum price instead.


#### Security groups modifying

Modified the security group - changes immediately

    you can assign up to five security groups to the instance.
    
#### Statyczny adresu IP
Starsza aplikacja wymaga statycznego adresu IP na stałe zakodowanego w wewnętrznej bazie danych, co blokuje korzystanie z modułu równoważenia obciążenia aplikacji.
Jakie kroki podjąłbyś, aby zastosować wysoką dostępność i odporność na uszkodzenia w tej aplikacji bez ELB?

    script to check health isntance - script to swithc elstic IP to instance + elastic IP

Skrypt niestandardowy umożliwia jednej instancji Amazon Elastic Compute Cloud (EC2) monitorowanie innej instancji Amazon EC2 i przejmowanie prywatnego „wirtualnego” adresu IP w przypadku awarii instancji. W przypadku użycia z dwoma instancjami skrypt włącza scenariusz wysokiej dostępności, w którym instancje monitorują się nawzajem i przejmują współdzielony wirtualny adres IP, jeśli druga instancja zawiedzie. Można go łatwo zmodyfikować, aby działał na zewnętrznym serwerze monitorowania lub serwerze świadków w celu wykonania wymiany VIP w imieniu dwóch monitorowanych węzłów.

Opcja 4 jest niepoprawna, ponieważ chociaż grupa automatycznego skalowania zapewnia wysoką dostępność i skalowalność, !!!!!!!!!!nadal zależy od ELB, który nie jest dostępny w tym scenariuszu. Pamiętaj, że musisz mieć statyczny adres IP, który może mieć postać elastycznego adresu IP. Chociaż grupa automatycznego skalowania może zostać skalowana, jeśli jedno z wystąpień EC2 stanie się niezdrowe, nadal nie można bezpośrednio przypisać EIP do grupy automatycznego skalowania.

#### Placement Groups
Grupy miejsc docelowych mogą być typu „klaster”, „spread” lub „partycja”. Wybierz opcje poniżej, które są specyficzne tylko dla grup miejsc docelowych.

    grupa instancji umieszczona w odrębnym sprzęcie bazowym

Jest tylko jedna odpowiedź, która jest specyficzna dla grup miejsc docelowych Spread i jest to ostatnia opcja. Chociaż niektóre z tych odpowiedzi są poprawne tylko dla obu Grup rozmieszczania klastrów lub zarówno dla Grup rozmieszczania klastrów, jak i grup rozmieszczania rozproszonego, w pytaniu stwierdzono, że należy wybrać tylko opcje specyficzne dla grup rozmieszczania klastrów. Wykluczałoby to dwie opcje, ponieważ są one prawdziwe zarówno dla grup miejsc docelowych typu Spread & Cluster. Logiczne grupowanie wystąpień w obrębie jednej Strefy dostępności jest prawdziwe tylko w przypadku Grup rozmieszczania klastrów i jest również nieprawidłowe.

#### Klaster aplikacji
Działają w klastrze aplikacji wielowarstwowych, które obejmują wiele serwerów dla modelu symulacji wiatru i jego wpływu na najnowocześniejszy projekt skrzydła. Obecnie w aplikacjach występuje spowolnienie, a po dalszym badaniu okazało się, że jest to spowodowane opóźnieniami.

Które z poniższych funkcji EC2 należy użyć do zoptymalizowania wydajności klastra obliczeniowego wymagającego niskiego opóźnienia sieci?

    GRUPY MIEJSCA
    - klaster

Możesz uruchamiać instancje EC2 w grupie miejsc docelowych, która określa sposób umieszczania instancji na podstawowym sprzęcie. Podczas tworzenia grupy miejsc docelowych określasz jedną z następujących strategii dla grupy:

- Klaster - klastruje instancje w grupę o niskim opóźnieniu w jednej strefie dostępności
- SPREAD - Rozprzestrzenianie - rozkłada wystąpienia na podstawowy sprzęt


**** Instancje dedykowane EC2 to instancje EC2, które działają w VPC na sprzęcie dedykowanym jednemu klientowi i są fizycznie odizolowane na poziomie sprzętu hosta od instancji należących do innych kont AWS. Nie służy do zmniejszania opóźnień.



#### Limit 20 instancji

Pomyślnie utworzono 20 instancji, ale pozostałe 20 żądań nie powiodło się

    limit dla każdego nowego konta do 20 instancji na region (Amazon EC2 instance request form to increase)

#### PuTTY
If you use PuTTY to connect to your instance via SSH and get either of the following errors, Error: Server refused our key or Error: No supported authentication methods available, verify that you are connecting with the appropriate user name for your AMI. Enter the user name in the User name box in the PuTTY Configuration window.

    The appropriate user names are as follows:
    
    -For an Amazon Linux AMI, the user name is ec2-user.
    -For a RHEL AMI, the user name is ec2-user or root.
    -For an Ubuntu AMI, the user name is ubuntu or root.
    -For a Centos AMI, the user name is centos.
    -For a Debian AMI, the user name is admin or root.
    -For a Fedora AMI, the user name is ec2-user.
    -For a SUSE AMI, the user name is ec2-user or root.
    -Otherwise, if ec2-user and root don't work, check with the AMI provider.
    20 questions within a 30 minute

You should also verify that your private key (.pem) file has been correctly converted to the format recognized by PuTTY (.ppk). 






## EBS

### Types of EBS

#### High-throughput workloads performing small, random I/O operations - the most suitable EBS type to use for your database?

    PROVISIONED IOPS
    
**single EBS volume that can support up to 30,000 IOPS.

    PROVISIONED


*SSD - small, random (transactional-best)

*HDD for large, sequential DBs(large streaming)

#### Wolumen, który może obsługiwać duże, sekwencyjne operacje I/O, najbardziej opłacalne miejsce do przechowywania
    
    COLD HDD

*Throughput Optimized HDD is primarily używany głównie do częstych operacji


#### Snapshots without downtime of EC2
Tworzenie snapshota woluminu EBS dołączonej do instancji EC2 wspieranej przez sklep i jest obecnie w toku

Snapshoty EBS występują asynchronicznie, co sprawia, że opcja 1 jest poprawną odpowiedzią.

    Wolumin EBS może być używany podczas tworzenia snapshota.


#### Snapy wielu woluminów EBS jednocześnie
najszybsze i najbardziej opłacalne rozwiązanie do automatycznego tworzenia kopii zapasowych wszystkich woluminów EBS

    Amazon Data Lifecycle Manager DLM to create snaps

#### "Ephermal" volumes

If this instance is stopped, what will happen to the data on the EPHERMAL store volumes?

    Deleted (ephemeral means "short-lived" or "temporary")

#### EBS jest w jednej AZ
Czy EBS może tolerować awarię strefy dostępności za każdym razem?

    NO - EBS VOLS STORED AND REPLICATED IN SINGLE AZ ONLY!!!

#### Incydenty z woluminami
Wystąpił incydent, który wymaga usunięcia woluminów EBS, a następnie ich ponownego utworzenia.

Co należy zrobić przed usunięciem woluminów EBS?

    Zrobić snapa



#### RAID and EBS

Musisz wykonać kopię zapasową bazy danych mySQL hostowanej w wystąpieniu Reserved EC2. Korzysta z woluminów EBS skonfigurowanych w macierzy RAID.

    1. Zatrzymać ruch RAID
    2. wyczyścić cache dysku
    3. upewnij się, że nie ma zapisu na RAID, odmontuj RAID
    4. po tym zrób snapy EBSów

#### EBS advantages
Szukasz magazynu blokowego do przechowywania wszystkich danych i zdecydowałeś się na woluminy EBS. Twój szef obawia się, że woluminy EBS nie są odpowiednie do twoich obciążeń z powodu wymagań zgodności, scenariuszy przestojów i wydajności IOPS.

    - Off-instance storage indepent from life of an instance
    - EBS provide ability to create snaps

Wolumin Amazon EBS to trwałe urządzenie pamięci masowej na poziomie bloku, które można podłączyć do pojedynczej instancji EC2. Woluminów EBS można użyć jako podstawowej pamięci dla danych wymagających częstych aktualizacji, takich jak dysk systemowy dla instancji lub pamięć dla aplikacji bazy danych. Możesz ich również użyć do aplikacji wymagających dużej przepustowości, które wykonują ciągłe skanowanie dysku. Woluminy EBS utrzymują się niezależnie od czasu działania instancji EC2.

Oto lista ważnych informacji o Wolumenach EBS:

- Gdy utworzysz wolumin EBS w strefie dostępności, jest on automatycznie replikowany w tej strefie, aby zapobiec utracie danych z powodu awarii jednego komponentu sprzętowego.
- Wolumin EBS można podłączyć tylko do jednej instancji EC2 na raz.
- Po utworzeniu woluminu można dołączyć go do dowolnej instancji EC2 w tej samej strefie dostępności
- Wolumin EBS to pamięć poza instancją, która może utrzymywać się niezależnie od czasu życia instancji. Można określić, aby nie przerywać woluminu EBS po zakończeniu instancji EC2 podczas tworzenia instancji.
- Woluminy EBS obsługują zmiany konfiguracji na żywo podczas produkcji, co oznacza, !!!!!!!!!!!!!!!!!!!!!że ​​można modyfikować typ woluminu, rozmiar woluminu i pojemność IOPS bez przerw w świadczeniu usług.!!!!!!!!!!!!!!!
- Szyfrowanie Amazon EBS wykorzystuje 256-bitowe algorytmy Advanced Encryption Standard (AES-256)
- EBS Wolumeny oferują 99,999% SLA.



## S3

`AVAILIBILITY`

    Dostępność - można opisać jako% okresu, w którym usługa będzie mogła w pewien sposób odpowiedzieć na Twoje zapytanie.


`DURABILITY `

    Trwałość - odnosi się do ciągłego istnienia obiektu lub zasobu. Pamiętaj, że nie oznacza to, że możesz uzyskać do niego dostęp, tylko że nadal istnieje.


#### Dostępność i koszt

#### S3 Use cases
EC2 przetwarza przesłany plik audio i generuje plik tekstowy jako wynik. Oba te często używane pliki należy przechowywać w tej samej trwałej pamięci, dopóki plik tekstowy nie zostanie pobrany przez przesyłającego. Ze względu na spodziewany wzrost popytu należy upewnić się, że pamięć jest skalowalna i można ją odzyskać w ciągu kilku minut.

    S3

#### Klasy przechowania
Aplikacja używa S3 do przechowywania obrazów, a spodziewasz się nagłego i znacznego wzrostu ruchu do S3, gdy nastąpi duże wydarzenie informacyjne (użytkownicy będą przesyłali duże ilości treści). Musisz ograniczyć koszty przechowywania do minimum , i z przyjemnością tymczasowo utracisz dostęp do 0,1% przesyłanych rocznie.

Kluczowymi czynnikami tutaj są dostępność i koszt, dlatego należy odpowiedzieć na to pytanie.

Pełna S3 jest dość droga i kosztuje około 0,023 USD za GB dla najniższego pasma.

Standard S3 IA wynosi 0,0125 USD na GB,

S3 OneZone-IA wynosi 0,01 USD na GB,

a starszy S3-RRS wynosi około 0,024 USD na GB dla najniższego pasma.

    Z oferowanych rozwiązań S3 One Zone-IA jest najtańszą odpowiednią opcją.

***Glacier nie może być brany pod uwagę, ponieważ nie jest przeznaczony do bezpośredniego dostępu, ale kosztuje około 0,004 USD za GB.

S3 ma dostępność 99,99%, S3-IA ma dostępność 99,9%, podczas gdy S3-1Zone-IA ma tylko 99,5%.

#### Kompromisy z użyciem S3 One Zone-IA zamiast S3 Standard-IA.

Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA) do klasy pamięci Amazon S3 do danych, do których dostęp jest rzadki, ale wymaga dostępu w razie potrzeby. W przeciwieństwie do innych klasycznych obiektów Amazon, które obsługują dane w co najmniej trzech Strefach aktywnych (AZ), S3 One Zone-IA przechowuje dane w jednym AZ

Jest to dobry wybór, na przykład, do zbioru wtórnych kopii zapasowych danych lokalnych lub danych, które można łatwo przekształcić, lub zrobić jako cel replikacji

#### Amazon S3 IA - less frequently + for data requires rapid access when needed

#### Glacier
Glacier storage service is primarily used for which use case

    INFREQUENTLY DATA STORING + DATA ARCHIVES STORING

#### Przyspieszone wyciąganie danych z Glacier
Aplikacja do handlu akcjami online przechowująca dane finansowe w segmencie S3 ma politykę cyklu życia, która co miesiąc przenosi starsze dane do Glacier. Istnieje ścisły wymóg zgodności, w którym kontrola niespodziewana może się zdarzyć w dowolnym momencie i powinieneś być w stanie odzyskać wymagane dane w mniej niż 15 minut we wszystkich okolicznościach. Twój menedżer poinstruował cię, aby upewnić się, że pojemność pobierania jest dostępna, gdy jej potrzebujesz, i powinna obsłużyć do 150 MB / s przepustowości pobierania.

    Expedited retrievals + Provisioned capacity purchase

#### Lifecycle Policies

Przesyłać przestarzałe dane ze swojego segmentu S3 do taniego systemu pamięci masowej w AWS.

    lifecycle policies + glacier


#### Prefix lub bez
Amazon S3 zapewnia teraz zwiększoną wydajność w zakresie obsługi co najmniej 3500 żądań na sekundę w celu dodania danych i 5500 żądań na sekundę w celu pobrania danych, co może zaoszczędzić znaczny czas przetwarzania bez dodatkowych opłat. Każdy prefiks S3 może obsługiwać te prędkości żądań, co znacznie zwiększa wydajność.

    W tym celu nie trzeba już dodawać losowego prefiksu, ponieważ S3 zwiększył wydajność w celu obsługi co najmniej 3500 żądań na sekundę w celu dodania danych i 5500 żądań na sekundę w celu 

****S3 przechowuje nazwy kluczy w kolejności alfabetycznej. Nazwa klucza określa, na której partycji jest przechowywany klucz. Użycie prefiksu sekwencyjnego zwiększa prawdopodobieństwo, że Amazon S3 będzie celował na określoną partycję dla dużej liczby twoich kluczy, przytłaczając pojemność I / O partycji.



#### Ochorna przed usunięciem objektów S3

What combination of the following options will protect the S3 objects in your bucket from both accidental deletion and overwriting?

    Versioning + MFA Deletion


#### Upload dużych plików (ponad 100 MB)

Their application uploads the user files, which can have a max file size of 1 TB, to an S3 Bucket.

In this scenario, what is the best way for the application to upload the large files in S3?

    Multipart Upload

W przypadku obiektów większych niż 100 megabajtów klienci powinni rozważyć użycie funkcji przesyłania wielu części. Interfejs API przesyłania wielu części umożliwia przesyłanie dużych obiektów w częściach.


#### S3 web hosting with Route 53
S3 bucket and a new web domain name which was registered using Route 53

    registered domain + S3 name of bucket the same as domain

NIE musi być CORS - gdy aplikacja kliencka w jednej domenie wchodzi w interakcje z zasobami w innej domenie 


#### Problem wydajności S3
Zmiany w celu zwiększenia szybkości aktualizacji danych przez aplikację. Istnieją doniesienia, że nieaktualne dane pojawiają się sporadycznie, gdy aplikacja uzyskuje dostęp do obiektów z segmentu S3. Zespół programistów zbadał logikę aplikacji i nie znalazł żadnych problemów.

Która z poniższych jest NAJBARDZIEJ prawdopodobną przyczyną tego problemu?

    równoległe requesty do bucketa

Obsługa Amazon S3 dla równoległych żądań oznacza, że ​​możesz skalować wydajność S3 ze względu na klaster obliczeniowy, bez dokonywania dostosowań w swojej aplikacji. Amazon S3 obecnie nie obsługuje blokowania obiektów. Jeśli dwa żądania PUT są jednocześnie wysyłane do tego samego klucza, wygrywa żądanie z najnowszym znacznikiem czasu. Jeśli jest to problem, będziesz musiał zbudować mechanizm blokujący obiekty w swojej aplikacji.

#### Wymóg Global Content Delivery Network

Cost-efficient and scalable storage option that you should use for this scenario?

    S3 + CFront (jeśłi jest wymóg Global Content Delivery Network (CDN)

***ElastiCache is only used for caching and not specifically as CDN

#### S3 Transfer Acceleration

Terabajty danych transakcyjnych do scentralizowanego segmentu S3. Jakiej funkcji AWS należy użyć w obecnym systemie, aby poprawić przepustowość i zapewnić konsekwentnie szybki transfer danych do segmentu Amazon S3, niezależnie od lokalizacji użytkownika?

    S3 Transfer Acceleration (używa CFront)

Amazon S3 Transfer Acceleration umożliwia szybkie, łatwe i bezpieczne przesyłanie plików na duże odległości między klientem a wiadrem Amazon S3. Transfer Acceleration wykorzystuje globalnie dystrybuowane lokalizacje AWS Edge firmy Amazon CloudFront. Gdy dane docierają do lokalizacji AWS Edge, dane są kierowane do segmentu Amazon S3 przez zoptymalizowaną ścieżkę sieciową.

Jest to funkcja, która zapewnia, że tylko CloudFront może obsługiwać zawartość S3. Nie zwiększa przepustowości i nie zapewnia szybkiego dostarczania treści do klientów.

#### S3 Multiple facilities
Jeden z Twoich klientów korzysta z Amazon S3 w regionie ap-southeast-1, aby przechowywać filmy szkoleniowe dotyczące procesu dołączania pracowników. Klient przechowuje filmy przy użyciu klasy Standard Storage.

Gdzie są replikowane filmy szkoleniowe Twojego klienta?

    Multiple facilities in ap-southeast-1

Dane są automatycznie dystrybuowane w co najmniej trzech obiektach fizycznych, które są geograficznie oddzielone w regionie AWS, a Amazon S3 może również automatycznie replikować dane do dowolnego innego regionu AWS.

#### CloudFront Signed URLs/cookies
Firma niedawno wprowadziła nowy dostęp tylko dla członków do niektórych swoich wysokiej jakości plików multimedialnych. Istnieje wymóg zapewnienia dostępu do wielu prywatnych plików multimedialnych tylko ich płatnym subskrybentom bez konieczności zmiany ich obecnych adresów URL.

CloudFront Signed URLs i podpisane pliki cookie CloudFront zapewniają tę samą podstawową funkcjonalność: pozwalają kontrolować, kto może uzyskać dostęp do twoich treści. Jeśli chcesz wyświetlać prywatne treści za pośrednictwem CloudFront i próbujesz zdecydować, czy użyć podpisanych adresów URL, czy podpisanych plików cookie, rozważ następujące kwestie:

Użyj podpisanych adresów URL w następujących przypadkach:

-Chcesz użyć dystrybucji RTMP. Podpisane pliki cookie nie są obsługiwane w przypadku dystrybucji RTMP.

Opcja 1 jest nieprawidłowa, ponieważ Match Viewer jest zasadą protokołu Origin, która konfiguruje CloudFront do komunikowania się z twoim źródłem za pomocą HTTP lub HTTPS, w zależności od protokołu żądania przeglądarki. CloudFront buforuje obiekt tylko raz, nawet jeśli przeglądający przesyłają żądania przy użyciu zarówno protokołów HTTP, jak i HTTPS.


#### Zastrzeganie dostępu do S3 z wyłączenie określonych grup użytkowników publicznych
Po kilku dniach dowiedziałeś się, że istnieją inne witryny turystyczne łączące i wykorzystujące Twoje zdjęcia.

    not public access + URLs specified

***NIE CloudFront, ponieważ jest to usługa sieciowa dostarczania treści, która przyspiesza dostarczanie treści do twoich klientów.

#### Integracja S3 z serverless
Dowiedziałeś się, że S3 ma powiadamiać o zdarzeniu, ilekroć operacja usuwania lub zapisu ma miejsce w segmencie S3. Jak to zrobić?

    SQS + Lambda
Schemat postępowania:

- Utwórz kolejkę, topic lub funkcję Lambda (którą nazywam celem dla zwięzłości).
- Udziel uprawnienia S3 do publikowania w celu lub wywołania funkcji Lambda. W przypadku SNS lub SQS robisz to, stosując odpowiednie zasady do tematu lub kolejki. W przypadku Lambda musisz utworzyć i podać rolę IAM, a następnie powiązać ją z funkcją Lambda.
- Umów się, aby twoja aplikacja była wywoływana w odpowiedzi na aktywność w celu. Jak zobaczysz za chwilę, masz tutaj kilka opcji.
- Ustaw konfigurację powiadomień segmentu, aby wskazywało cel.

#### Integracja S3 z serverless #2
Za każdym razem, gdy użytkownik załaduje obraz, zostanie on przesłany do Kinesis w celu przetworzenia, zanim zostanie zapisany w wiadrze S3. Następnie, jeśli przesyłanie się powiodło, aplikacja zwróci komunikat informujący użytkownika, że przesyłanie się powiodło.

ASYNCHORNICZNIE? + powaidomienia z S3 przesyłane dalej: Lambda triggered

    LAMBDA

#### Encryption S3

data being backed up or stored on Amazon S3 must be encrypted

    encrypt locally + enable SSE AES-256


## Elastic File System


#### EFS as a POSIX-compliant
Your manager noticed that the system performance is quite slow and he has instructed you to improve the architecture of the system.

In this scenario, what will you do to implement a scalable, high throughput POSIX-compliant file system?

    EFS





## RDS

Czy po wdrożeniu bazy danych RDS w wielu strefach dostępności można użyć dodatkowej bazy danych jako niezależnego węzła odczytu?

    Nie


Czy mogę „wymusić” przełączenie awaryjne dla dowolnej instancji RDS, która ma skonfigurowaną funkcję Multi-AZ?

    tak

you have to ensure that your RDS database can only be accessed using the profile credentials specific to your EC2 instances via an authentication token.

    NIE STS - ALE IAM DB auth

#### Replikacje synchroniczne i asynchroniczne  
Option 2 is incorrect as a Read Replica provides an asynchronous replication instead of synchronous. 

    SYNCHORNICZNA - MULTI AZ

#### Failover in RDS - prevention

Database server failure in the future?

    multiAZ

W przypadku awarii infrastruktury Amazon RDS wykonuje automatyczne przełączenie awaryjne do trybu gotowości (lub do repliki odczytu w przypadku Amazon Aurora).


#### RDS - primary database instance fails

What would happen to RDS if the primary database instance fails?

    CNAME is switched from primary to secondary

#### Automatic failover of RDS - causes

Jakie zdarzenia spowodują, że Amazon RDS automatycznie wykona przełączenie awaryjne do repliki rezerwowej?

Usługa Amazon RDS automatycznie wykonuje przełączenie awaryjne w przypadku jednej z poniższych sytuacji:

- Utrata dostępności w podstawowej strefie dostępności
- Utrata połączenia sieciowego z podstawowym
- Awaria jednostki obliczeniowej na podstawowym
- Błąd przechowywania na podstawowym

#### MultiAZ nie jako bufor odczytu
Czy podczas uruchamiania podstawowej instancji Amazon RDS jako wdrożenia Multi-AZ można używać instancji rezerwowej do operacji odczytu i zapisu?

Nie

****Wdrożenia Multi-AZ dla silników MySQL, MariaDB, Oracle i PostgreSQL wykorzystują synchroniczną replikację fizyczną, aby utrzymywać dane w trybie gotowości na bieżąco z podstawowym. Wdrożenia Multi-AZ dla silnika SQL Server używają synchronicznej replikacji logicznej, aby osiągnąć ten sam wynik, wykorzystując natywną technologię kopii lustrzanych SQL Server. Oba podejścia chronią twoje dane w przypadku awarii wystąpienia bazy danych lub utraty strefy dostępności.


#### RDS - Enhanced monitoring
musisz ściśle monitorować, w jaki sposób różne procesy lub wątki w instancji DB wykorzystują procesor, w tym procent przepustowości procesora (percentage of the CPU bandwidth) i całkowitej pamięci zużywanej przez każdy proces (total memory consumed).

    RDS - Enhanced monitoring


## DynamoDB, Redshift and other NoSQL DBs

#### Frequent schema changes

One of their systems requires a database that can scale globally and can handle !!!!!frequent schema changes (<- NOSQL) . It should also provide low-latency response to high-traffic queries.

    Dynamo

#### Key-value data
store both key-value store and document models like band ID, album ID, song ID and composer ID

    Dynamo


#### Poprawianie wydajności DynamoDB
DynamoDB. Poinstruowano Cię, aby poprawić wydajność bazy danych przez równomierne rozłożenie obciążenia i efektywne wykorzystanie przydzielonej przepustowości

    PARTITION KEY

Przydzielona pojemność I/O dla tabeli jest równomiernie podzielona między te partycje fizyczne. Dlatego projekt klucza partycji, który nie rozdziela równomiernie żądań I/O, może tworzyć „gorące” partycje, które powodują dławienie i nieefektywnie wykorzystują przydzieloną pojemność I/O.

Oznacza to, że im bardziej wyraźne są wartości klucza partycji, do których uzyskuje dostęp twoje obciążenie, tym bardziej żądania te zostaną rozłożone na partycjonowane miejsce. Zasadniczo bardziej efektywnie wykorzystasz zainicjowaną przepływność, ponieważ wzrasta stosunek wartości kluczy partycji dostępnych do całkowitej liczby wartości kluczy partycji

#### Valid use cases for DynamoDB

    + Managing Web Sessions (TTL)
    + Storing metadata for Amazon S3

Mechanizm **DynamoDB Time-to-Live (TTL)** umożliwia łatwe zarządzanie sesjami sieciowymi aplikacji. Pozwala ustawić określony znacznik czasu, aby usunąć wygasłe elementy ze swoich tabel. Po upływie znacznika czasu odpowiedni element jest oznaczany jako wygasł, a następnie jest usuwany z tabeli. Korzystając z tej funkcji, nie musisz śledzić wygasłych danych i usuwać je ręcznie. TTL może pomóc w zmniejszeniu zużycia pamięci i obniżeniu kosztów przechowywania danych, które nie są już istotne.

DynamoDB przechowuje uporządkowane dane indeksowane według klucza głównego (primary key) i umożliwia dostęp do odczytu i zapisu z niewielkim opóźnieniem w elementach od 1 bajtu do 400 KB. Amazon S3 przechowuje nieuporządkowane obiekty BLOB i nadaje się do przechowywania dużych obiektów do 5 TB. Aby zoptymalizować koszty w ramach usług AWS, duże obiekty lub zestawy danych, do których nie ma dostępu, powinny być przechowywane w Amazon S3, podczas gdy mniejsze elementy danych lub wskaźniki plików (prawdopodobnie do obiektów Amazon S3) najlepiej zapisywać w Amazon DynamoDB.


#### Shard Table for Kinesis Throughput
Strumienie danych Amazon Kinesis. Zauważyłeś jednak, że w wielu przypadkach iterator niezależnego fragmentu wygasa nieoczekiwanie. Po sprawdzeniu okazało się, że tabela DynamoDB używana przez Kinesis nie ma wystarczającej pojemności do przechowywania danych dzierżawy

    INCREASE Write capacity assigned to shard table

Nowy iterator niezależnego fragmentu jest zwracany przy każdym żądaniu GetRecords (jako NextShardIterator), którego następnie używasz w następnym żądaniu GetRecords (jako ShardIterator). Zazwyczaj ten iterator niezależnego fragmentu nie wygasa przed jego użyciem. Może się jednak okazać, że iteratory niezależnego fragmentu wygasają, ponieważ nie wywołano GetRecords przez więcej niż 5 minut lub zrestartowano aplikację konsumencką.

Jeśli iterator niezależnego fragmentu wygasa bezpośrednio przed użyciem, może to oznaczać, że tabela DynamoDB używana przez Kinesis nie ma wystarczającej pojemności do przechowywania danych dzierżawy. Ta sytuacja jest bardziej prawdopodobna, jeśli masz dużą liczbę odłamków. Aby rozwiązać ten problem, zwiększ pojemność zapisu przypisaną do tabeli niezależnego fragmentu. Dlatego opcja 1 jest poprawna.

#### Wymóg: w wysoce skalowalny i łatwo dostępny magazyn danych 
Miliony użytkowników z całego świata będą głosować za pomocą swoich telefonów komórkowych. Głosy te muszą być gromadzone i przechowywane w wysoce skalowalnym i łatwo dostępnym magazynie danych, który zostanie zapytany o ranking w czasie rzeczywistym.

    Dynamo + AppSync

Kiedy pojawia się słowo trwałość, pierwszą usługą, która powinna przyjść ci do głowy, jest Amazon S3. Ponieważ ta usługa nie jest dostępna w opcjach odpowiedzi, możemy spojrzeć na inny dostępny magazyn danych, którym jest Amazon DynamoDB.

DynamoDB to trwały, skalowalny i wysoce dostępny magazyn danych, którego można używać do tabelowania w czasie rzeczywistym. Możesz także użyć AppSync z DynamoDB, aby ułatwić tworzenie aplikacji do współpracy, które aktualizują udostępniane dane w czasie rzeczywistym. Wystarczy podać dane dla swojej aplikacji za pomocą prostych instrukcji kodowych, a AWS AppSync zarządza wszystkim, co jest potrzebne do aktualizowania danych aplikacji w czasie rzeczywistym. Umożliwi to Twojej aplikacji dostęp do danych w Amazon DynamoDB, wyzwalanie funkcji AWS Lambda lub uruchamianie zapytań Amazon Elasticsearch i łączenie danych z tych usług w celu zapewnienia dokładnych danych potrzebnych dla Twojej aplikacji.

#### DAX use cases

Tabela DynamoDB i zasoby statyczne są dystrybuowane przez CloudFront. Istnieje jednak wiele skarg, że zapisywanie i pobieranie informacji o graczach zajmuje dużo czasu.

    DAX

#### Poprawa wydajności Lambdy i DAX
Tabela DynamoDB, która została uruchomiona za pomocą interfejsu AWS CLI do przechowywania wszystkich danych użytkownika i informacji zebranych od graczy oraz funkcji Lambda do pobierania danych z DynamoDB. Z gry korzystają miliony użytkowników każdego dnia do odczytu i przechowywania danych.

Jak zaprojektowałbyś aplikację, aby poprawić jej ogólną wydajność i uczynić ją bardziej skalowalną przy jednoczesnym utrzymaniu niskich kosztów?

    DAX + zmiana write and read CU + LAMBDA z API GW do cacheowania często żądanych danych

!!!!!Auto Scaling of DynamoDB is not enabled in a DynamoDB table which is created using the AWS CLI.


#### Analiza ogromnych zestawów danych + szybkie zapytania
Szukasz usługi do wykonywania superszybkich analiz ogromnych zestawów danych w czasie prawie rzeczywistym

**zdolność do przechowywania ogromnych ilości danych i wykonywania szybkich i elastycznych zapytań na ich temat**

    REDSHIFT

#### Generować codzienne i miesięczne raporty finansowe i kolumnowy sposób przechowania
Dane muszą być przechowywane w sposób kolumnowy, aby zmniejszyć liczbę żądań I/O dysku i zmniejszyć ilość danych potrzebnych do załadowania z dysku. Bank ma istniejącą aplikację do analizy danych biznesowych innej firmy, która połączy się z usługą magazynowania, a następnie będzie generować codzienne i miesięczne raporty finansowe dla swoich klientów na całym świecie.

    Amazon Redshift 


#### Redis AUTH
Wprowadzić hasło, zanim otrzymają pozwolenie na wykonywanie poleceń Redis.

    Using Redis AUTH command can improve data security by requiring the user to enter a password 
    +
    --auth-token

#### Redshift workload
Redshift do aplikacji do przetwarzania analitycznego online (OLAP), która przetwarza złożone zapytania na duże zestawy danych. Istnieje wymóg, w którym należy zdefiniować liczbę dostępnych kolejek zapytań oraz sposób kierowania zapytań do tych kolejek w celu przetworzenia.

    REDSHIFT WORKLOAD MANAGEMENT PARAMETER

Podczas tworzenia grupy parametrów domyślna konfiguracja WLM zawiera jedną kolejkę, w której można uruchomić do pięciu zapytań jednocześnie. Możesz dodać dodatkowe kolejki i skonfigurować właściwości WLM w każdej z nich, jeśli chcesz mieć większą kontrolę nad przetwarzaniem zapytań. Każda dodana kolejka ma tę samą domyślną konfigurację WLM, dopóki nie skonfigurujesz jej właściwości. Po dodaniu kolejnych kolejek ostatnia kolejka w konfiguracji jest kolejką domyślną. O ile zapytanie nie jest kierowane do innej kolejki na podstawie kryteriów w konfiguracji WLM, jest przetwarzane przez kolejkę domyślną. Nie można określić grup użytkowników ani grup zapytań dla domyślnej kolejki.

Podobnie jak w przypadku innych parametrów, nie można modyfikować konfiguracji WLM w domyślnej grupie parametrów. Klastry powiązane z domyślną grupą parametrów zawsze używają domyślnej konfiguracji WLM. Jeśli chcesz zmodyfikować konfigurację WLM, musisz utworzyć grupę parametrów, a następnie powiązać tę grupę parametrów z dowolnymi klastrami wymagającymi niestandardowej konfiguracji WLM.



#### ElastiCache - in-memory data store
Przechowywanie najczęściej używanych danych w magazynie danych w pamięci, aby skrócić czas pobierania i odpowiedzi
    
    Amazon ElastiCache

Amazon ElastiCache to usługa internetowa, która ułatwia wdrażanie, obsługę i skalowanie magazynu danych lub pamięci podręcznej w chmurze.

    
### Storing session state data needed    
build a highly available web application using stateless web servers. - suitable for storing session state data? 

    DynamoDB + ElastiCache

## load streaming data from Kinesis into

A traffic monitoring and reporting application uses Kinesis to accept real-time data. In order to process and store the data, they used Amazon Kinesis Data Firehose to load the streaming data to various AWS resources.  Which of the following services can you load streaming data into?

    ELASTICSEARCH






## Auto Scaling Groups & Elastic Load Balancers

#### AS - Schedule Scaling Policies
Znasz już dokładne godziny szczytu aplikacji. Zanim procesor lub pamięć osiągną wartość szczytową, aplikacja ma już problemy z wydajnością

    AUTO SCALING GROUPS - need to configure a SCHEDULE SCALING POLICY

Chcesz, aby grupa automatycznego skalowania zachowywała się w taki sposób, aby działała zgodnie ze wstępnie zdefiniowanym zestawem parametrów, zanim zmniejszy liczbę instancji EC2, co chroni system przed niezamierzonym spowolnieniem lub niedostępnością.

    default val 300 seconds
    +
    AS group nie włączy instancji lub nie terminuje przed sprawdzeniem efektu

- Zapewnia, że grupa automatycznego skalowania nie uruchomi ani nie zakończy dodatkowych instancji EC2 przed wejściem w życie poprzedniej operacji skalowania.
- Jego domyślna wartość to 300 sekund.
- Jest to konfigurowalne ustawienie dla twojej grupy automatycznego skalowania.

#### AS - Domyślne zasady zakończenia
Która instancja EC2 będzie pierwszą, która zostanie zakończona przez twoją grupę automatycznego skalowania?
Domyślne zasady zakończenia mają na celu zapewnienie równomiernego rozłożenia architektury sieci w strefach dostępności.

    the oldest launch configuration.



#### ELB Health Checks failed
EC2 instance behind an ELB fails a health check

    Stop sending traffic to this instance


#### Perfect Forward Secrecy

Perfect Forward Secrecy służy do oferowania pakietów szyfrów SSL/TLS. Dostępny jest dla usług:

    ELoad Ballancers, Cfront


#### Shake ELB with SSL Certs 

System jest używany na całym świecie, jest hostowany w grupie automatycznych skalowań instancji EC2 za CLASSIC modułem równoważenia obciążenia. Musisz zabezpieczyć aplikację, umożliwiając wielu domenom obsługę ruchu SSL na tym samym adresie IP.

    Generowanie SSL w AWS Cert Manager + CFront distro + enable support for SNI (Server Name Indication)

SNI Niestandardowy SSL opiera się na rozszerzeniu SNI (SERVER NAME INDICATION) protokołu Transport Layer Security, który pozwala wielu domenom obsługiwać ruch SSL na tym samym adresie IP, włączając nazwę hosta, z którą widzowie próbują się połączyć.

Amazon CloudFront dostarcza Twoje treści z każdej lokalizacji brzegowej i oferuje takie same bezpieczeństwo, jak funkcja Dedykowanego IP Custom SSL.

#### Path-based routing in ELB
will be used by an online accounting application which requires path-based routing, host-based routing, and bi-directional communication channels using WebSockets.

TYPE OF ELB?

    Application Load Balancer




## Users, encryption, security

Both the master keys and the unencrypted data should never be sent to AWS

    Client side encryption + client side master key
    
#### Autoryzacja początku trasy (ROA)
migrate these applications to AWS without requiring your partners and customers to change their IP address whitelists.

    ROA + X509  

Autoryzacja początku trasy (ROA) to dokument, który można utworzyć za pomocą regionalnego rejestru internetowego (RIR), takiego jak amerykański rejestr numerów internetowych (ARIN) lub Réseaux IP Européens Network Coordination Centre (RIPE). Zawiera zakres adresów, numery ASN, które mogą reklamować zakres adresów oraz datę ważności. Dlatego opcja 3 jest prawidłową odpowiedzią.

ROA upoważnia Amazon do reklamowania zakresu adresów pod określonym numerem AS. Jednak nie upoważnia twojego konta AWS do przeniesienia zakresu adresów do AWS. Aby autoryzować swoje konto AWS w celu wprowadzenia zakresu adresów do AWS, musisz opublikować samopodpisany certyfikat X509 w uwagach RDAP dotyczących zakresu adresów. Certyfikat zawiera klucz publiczny, którego AWS używa do weryfikacji podanego przez siebie podpisu kontekstu autoryzacji. Należy zabezpieczyć swój klucz prywatny i używać go do podpisywania wiadomości kontekstu autoryzacji.


#### Following can you manage in the IAM dashboard

    grupy + identity providers


#### Active Directory + on prem creds = SAML
Access resources on both environments using their on-premises credentials, which is stored in Active Directory.

    SAML

#### Active Directory + ID federation & role based 
Company policy mandates the use of identity federation and role-based access control. Currently, the roles are already assigned using groups in the corporate Active Directory.

    AD Connector + IAM Roles
    
#### Active Directory + S3
1200 pracowników otrzymają dostęp do korzystania z Amazon S3 do przechowywania swoich dokumentów osobistych.

Które z poniższych należy wziąć pod uwagę, aby można było skonfigurować rozwiązanie, które zawiera funkcję pojedynczego logowania z firmowego katalogu AD lub LDAP, a także ogranicza dostęp dla każdego użytkownika do wyznaczonego folderu użytkownika w segmencie S3?

- Setup a Federation proxy or an Identity provider

- Setup an AWS Security Token Service to generate temporary tokens (Option 2)

- Configure an IAM role (Option 4) 

#### Szyfrowanie danych przed zapisaniem ich na dysku

polityka bezpieczeństwa wymaga, aby aplikacja hostowana w EC2 najpierw szyfrowała dane przed zapisaniem ich na dysku w celu przechowywania

    AWS KMS API

#### EC2 Roles
Instancje EC2, które wymagają dostępu do różnych usług AWS, takich jak S3 i Redshift + muszą zapewnić dostęp administratorom systemu

    IAM role attached to EC2 instance + Enable MFA
    
system obsługuje transakcje finansowe, musisz upewnić się, że architektura chmury jest bezpieczna.

#### Access to one service for many IAM users
300 użytkowników IAM. Mają nową politykę firmy, która zmieni dostęp 100 użytkowników IAM w celu uzyskania szczególnego rodzaju dostępu do segmentów Amazon S3.

    Stworzyć IAM grupę - dodać userów - zastosować zasadę dla grupy

#### Encryption at rest - services
Które z poniższych usług AWS DOMYŚLNIE szyfrują dane w spoczynku?

    Glacier + AWS Storage Gateway
    
    ****RDS, Lambda czy ECS NIE domyślnie

Wszystkie dane przesyłane między dowolnym rodzajem urządzenia bramy a pamięcią AWS są szyfrowane przy użyciu protokołu SSL. Domyślnie wszystkie dane przechowywane przez AWS Storage Gateway w S3 są szyfrowane po stronie serwera za pomocą zarządzanych kluczy szyfrowania Amazon S3 (SSE-S3). Ponadto podczas korzystania z bramy plików można opcjonalnie skonfigurować każdy udział pliku, aby obiekty były szyfrowane za pomocą kluczy zarządzanych przez AWS KMS za pomocą SSE-KMS. To jest powód, dla którego opcja 1 jest poprawna.

#### Tymczasowe poświadczenia

Umożliwia wydawanie krótkoterminowych tokenów dostępu, które działają jako tymczasowe poświadczenia bezpieczeństwa

    STS

Odpowiedzi błędne - opis uług:

- Usługa Amazon Cognito służy przede wszystkim do uwierzytelniania użytkowników, a nie do zapewniania dostępu do zasobów AWS.

- JSON Web Token (JWT) służy do uwierzytelniania użytkowników i zarządzania sesjami.

- chociaż usługa SSO AWS korzysta z STS, sama nie wydaje krótkoterminowych poświadczeń. AWS Single Sign-On (SSO) to usługa chmurowego logowania jednokrotnego, która ułatwia centralne zarządzanie dostępem SSO do wielu kont AWS i aplikacji biznesowych.


#### Cognito ID

Startujący w San Francisco technologiczny startup tworzy wieloplatformową aplikację mobilną, która może powiadamiać użytkownika o nadchodzących wydarzeniach astronomicznych, takich jak zaćmienia, niebieski księżyc, nowe lub deszcz meteorów. Twoja aplikacja mobilna uwierzytelnia się za pomocą dostawcy tożsamości (IdP) przy użyciu zestawu SDK dostawcy i usługi Amazon Cognito. Gdy użytkownik końcowy zostanie uwierzytelniony za pomocą IdP, **token OAuth lub OpenID Connect zwrócony z IdP** jest przekazywany przez twoją aplikację do Amazon Cognito.

Które z poniższych zwrócono użytkownikowi w celu zapewnienia zestawu tymczasowych poświadczeń AWS o ograniczonych uprawnieniach?

    COGNITO ID

Za pomocą Amazon Cognito można dostarczać do aplikacji tymczasowe poświadczenia o ograniczonych uprawnieniach, aby użytkownicy mieli dostęp do zasobów AWS. Pule tożsamości Amazon Cognito obsługują zarówno tożsamości uwierzytelnione, jak i nieuwierzytelnione. Możesz natychmiast pobrać unikalny identyfikator Amazon Cognito (identyfikator tożsamości) dla użytkownika końcowego, jeśli zezwalasz nieuwierzytelnionym użytkownikom lub po ustawieniu tokenów logowania w dostawcy poświadczeń, jeśli uwierzytelniasz użytkowników.

#### Dane wrażliwe - szyforwanie w EBS i S3
systemu cyfrowego portfela. Aplikacje będą działać na wielu instancjach EC2 wspieranych przez EBS, które będą przechowywać dzienniki, transakcje i wyciągi rozliczeniowe użytkownika w segmencie S3. Ze względu na surowe wymagania dotyczące bezpieczeństwa i zgodności badasz opcje bezpiecznego przechowywania poufnych danych w woluminach EBS i S3.

    do wrażliwych danych: S3 - SSE + CSE ++ Enable EBS enryption

Możesz teraz domyślnie włączyć szyfrowanie Amazon Elastic Block Store (EBS) , upewniając się, że wszystkie nowe woluminy EBS utworzone na Twoim koncie są szyfrowane. Domyślne ustawienia szyfrowania są specyficzne dla poszczególnych regionów AWS na Twoim koncie. Dzięki coraz bardziej rygorystycznym przepisom dotyczącym danych ta funkcja ułatwia szyfrowanie danych w systemie EBS, dzięki czemu osiągane są cele dotyczące zgodności i bezpieczeństwa. 








## CloudWatch, Cloud Trail, Monitoring

#### Custom metrics
You need to prepare a custom metric using CloudWatch Monitoring Scripts which is written in Perl. You can also install CloudWatch Agent to collect more system-level metrics from Amazon EC2 instances. Here's the list of custom metrics that you can set up:

    - Memory utilization
    - Disk swap utilization
    - Disk space utilization
    - Page file utilization
    - Log collection
    
#### Cloud Watch Alarms
Instancje EC2 zgłaszają **failed heatlh checks** stanu systemu. Zespół operacyjny szuka łatwiejszego sposobu monitorowania i naprawy tych instancji, niż naprawiania ich ręcznie.

    Akcje alarmowe Amazon CloudWatch, możesz tworzyć alarmy, które automatycznie: 
    zatrzymają, zakończą, uruchomią ponownie lub odzyskają instancje EC2.
    
### API calls to your Redshift instance
Which service will allow you to monitor all API calls to your Redshift instance and can also provide secured data for auditing and compliance purposes?

    Cloud Trail

#### Audits logging of AWS users activities
Zewnętrzna firma audytorska pod kątem zgodności. Które z poniższych usług dostępnych w AWS można wykorzystać, aby zapewnić, że odpowiednie informacje są dostępne do celów kontrolnych?
    
    CTrail

#### Cloud Watch Agent installed on EC2    
Aby zapewnić wysoką dostępność systemów, musisz monitorować wykorzystanie pamięci i dysku we wszystkich instancjach.

Które z poniższych jest najbardziej odpowiednim rozwiązaniem do monitorowania?

    INSTALL CWATCH AGENT + which gathers memory and disk utilization
    AWS Systems Manager

***Enhanced Monitoring is a feature of RDS and not of CloudWatch.

#### Logi audytu z usług globalnych
Architekt ma za zadanie skonfigurować system rejestrowania, który będzie śledził wszystkie zmiany dokonane w ich zasobach AWS we wszystkich regionach, w tym konfiguracje dokonane w IAM, CloudFront, AWS WAF i Route 53.

    - Ctrail CLI `--is-multi-region-trail` + `--include-global-service-events`
    - MFA Auth on del S3

CloudWatch będzie jednak obejmował wyłącznie usługi regionalne (EC2, S3, RDS itp.), A nie usługi globalne, takie jak IAM, CloudFront, AWS WAF i Route 53.

Można włączyć kilka regionów

***Funkcja rejestrowania audytu służy przede wszystkim do uzyskiwania informacji o połączeniu, zapytaniach i działaniach użytkowników w klastrze Redshift.

#### Cloud Watch & Lambda integration
Wiesz, że Lambda automatycznie monitoruje funkcje w Twoim imieniu i raportuje dane za pośrednictwem Amazon CloudWatch.

W tym scenariuszu, jakie typy danych monitorują te metryki?

    INVOCATIONS + DEADLETTERS ERROR

The throttles, Dead Letter Queues errors and Iterator age for stream-based invocations are also monitored.



#### Integracja Cloud Watch z SNS przy alarmach
Monitorujesz kilka wskaźników bazy danych, a następnie wyślij powiadomienia e-mail do zespołu operacyjnego na wypadek wystąpienia problemu

    Cwatch + SNS (nie SES)

Pamiętaj, że powinieneś używać SNS zamiast SES (Simple Email Service), gdy chcesz monitorować swoje wystąpienia EC2.

CloudWatch gromadzi dane monitorowania i operacyjne w postaci dzienników, metryk i zdarzeń, zapewniając ujednolicony widok zasobów, aplikacji i usług AWS działających na AWS i serwerach lokalnych.

SNS to wysoce dostępna, trwała, bezpieczna, w pełni zarządzana usługa przesyłania wiadomości pub / sub, która umożliwia oddzielenie mikrousług, systemów rozproszonych i aplikacji bezserwerowych.

(Nie SES, bo to oparta na chmurze usługa wysyłania wiadomości e-mail przeznaczona do wysyłania powiadomień i transakcji e-mail.)

    
    
    
    
    
    
    
## AWS Lambda, API Gateway


- AWS Lambda - AWS Lambda to usługa obliczeniowa, w której można przesłać kod, a usługa może uruchomić kod w Twoim imieniu przy użyciu infrastruktury AWS. Podczas tworzenia funkcji Lambda pakujesz i przesyłasz swój niestandardowy kod do AWS Lambda

#### Szyforwanie Lambdy i zmiennych
Jeśli chcesz używać pomocników szyfrowania i KMS do szyfrowania zmiennych środowiskowych po utworzeniu funkcji Lambda, musisz utworzyć własny klucz KMS AWS i wybrać go zamiast klucza domyślnego.

Domyślne szyforwanie lambdy - Wrażliwe informacje byłyby nadal widoczne dla innych użytkowników, którzy mają dostęp do konsoli Lambda.


#### Lambda + API Getaway
W tym scenariuszu, w jaki sposób można chronić systemy zaplecza platformy przed skokami ruchu?

    API Throttling

Usługa Amazon API Gateway zapewnia ograniczanie przepustowości na wielu poziomach, w tym na poziomie globalnym i serwisowym. Limity dławienia można ustawić dla standardowych stawek i serii. Na przykład właściciele API mogą ustawić limit prędkości 1000 żądań na sekundę dla określonej metody w swoich interfejsach API REST, a także skonfigurować bramę API Amazon, aby obsługiwała serię 2000 żądań na sekundę przez kilka sekund. Usługa Amazon API Gateway śledzi liczbę żądań na sekundę. Każde żądanie przekraczające limit otrzyma odpowiedź 429 HTTP. Pakiety SDK klienta generowane przez Amazon API Gateway ponawiają automatycznie połączenia po spełnieniu tej odpowiedzi. Dlatego opcja 2 jest prawidłową odpowiedzią.

Możesz dodać buforowanie do wywołań API, udostępniając pamięć podręczną bramy API Amazon i określając jej rozmiar w gigabajtach. Pamięć podręczna jest udostępniana na określonym etapie interfejsów API. Poprawia to wydajność i zmniejsza ruch wysyłany do zaplecza. Ustawienia pamięci podręcznej pozwalają kontrolować sposób budowania klucza pamięci podręcznej oraz czas życia (TTL) danych przechowywanych dla każdej metody. Usługa Amazon API Gateway udostępnia także interfejsy API do zarządzania, które pomagają unieważnić pamięć podręczną dla każdego etapu.


#### Lambda @Edge (integracja z Cloud Front)
Lambda @Edge pozwala uruchamiać funkcje Lambda w celu dostosowania treści dostarczanych przez CloudFront, wykonując funkcje w lokalizacjach AWS bliższych przeglądarce

- Po otrzymaniu przez CloudFront żądania od przeglądarki (żądanie przeglądarki)
- Przed CloudFront przekazuje żądanie do źródła (żądanie pochodzenia)
- Po otrzymaniu odpowiedzi CloudFront od źródła (odpowiedź pochodzenia)
- Przed CloudFront przekazuje odpowiedź do przeglądarki (odpowiedź przeglądarki)

W podanym scenariuszu można użyć Lambda @Edge, aby umożliwić funkcjom Lambda dostosowanie treści dostarczanych przez CloudFront

Ponadto można skonfigurować przełączanie awaryjne źródła, tworząc grupę początkową z dwoma źródłami (origin failover by creating an origin group with two origins), z których jedno jest pierwotnym źródłem, a drugie jako drugim źródłem, do którego CloudFront automatycznie przełącza się w przypadku awarii pierwotnego źródła. Pozwoli to złagodzić występujące czasami błędy HTTP 504.


#### Key features of API Gateway 

    API Gw without servers

    Pay only form API calls and amount of transfered data





## VPCs


Musisz ustanowić połączenie VPN między lokacjami (site-to-site VPN connection).

Domyślne instancje uruchomane w wirtualnej chmurze prywatnej (VPC) nie można komunikować się z własną używaną. Możesz włączyć dostęp do swojej sieci z VPC, podłączyć wirtualną aplikację do VPC, dostosować niestandardową tabelę tras, zaktualizować grupę zabezpieczeń i skonfigurować połączenie VPN przez AWS.

#### CIDR Block ranges

CIDR block of 172.0.0.0/27, with a netmask of /27, has an equivalent of 27 usable IP addresses. Take note that a netmask of /27 originally provides you with 32 IP addresses but in AWS, there are 5 IP addresses that are reserved which you cannot use

#### CIDR - IP

Wymagane jest, aby zezwolić tylko na indywidualny adres IP klienta, a nie na całą sieć. Dlatego należy zastosować poprawną notację CIDR. / 32 oznacza jeden adres IP, a / 0 odnosi się do całej sieci. Dlatego opcja 3 jest niepoprawna, ponieważ zezwala na całą sieć zamiast jednego adresu IP.


### Subnets ideal-solutions
In which subnet should you launch the new database server

    Private 

#### SSH connection NOT needed
Establish an SSH connection to a Linux instance hosted in your VPC via the Internet

    DONT NEED secondary Private IP adress

#### Publiczne IP w nie-deomyślnej publicznej VPC



#### VPC & Highly Available Architecture 
Aby zapewnić wysoką dostępność i odporność na awarie, zarówno aplikacje INTERNETOWE, jak i WEWNĘTRZNE muszą być w stanie wykorzystać co najmniej dwa AZ w celu zapewnienia wysokiej dostępności.
    
    4

!!!!!!!!!!!
Pamiętaj, że jedna podsieć jest odwzorowana na jedną konkretną strefę dostępności.
!!!!!!!!!!!

!!!!!!!!!!!!!!!!!!!!!!
VPC obejmuje wszystkie Strefy Dostępność w regionie. Po utworzeniu VPC możesz dodać jedną lub więcej podsieci w każdej strefie dostępności. Podczas tworzenia podsieci określa się blok CIDR dla podsieci, która jest podzbiorem bloku CIDR VPC. Każda podsieć musi znajdować się całkowicie w obrębie jednej strefy dostępności i nie może obejmować stref. 

Uruchamiając instancje w osobnych strefach dostępności, możesz chronić swoje aplikacje przed awarią jednej lokalizacji. 

    1 VPC = 1 AZ = 3 regiony = 1 blok CIDR dla SUBNET = 1 subnet = 1 AZ

#### Komunikacja EC2 wewnątrz VPC - 2 subnets!
EC2 są wewnątrz wirtualnej chmury prywatnej w tej samej strefie dostępności, ale są wdrażane w różnych podsieciach
Jakie rzeczy musisz sprawdzić, aby te instancje EC2 mogły komunikować się w VPC?

    N.ACLs + Security Groups allow traffic

- Po pierwsze, ACL sieci powinien być odpowiednio ustawiony, aby umożliwić komunikację między dwiema podsieciami (subnets).
- Grupa zabezpieczeń powinna również być odpowiednio skonfigurowana, aby serwer WWW mógł komunikować się z serwerem bazy danych. 




### ACLs

Zauważyłeś, że przychodzi wiele skanów portów z określonego bloku adresu IP, które próbują połączyć się z !!KILKOMA zasobami AWS w twoim VPC.

    Network ACLs

***NIE Security Group - na poziomie instancji, a nie na całym VPC.

#### Numeracja reguł ACL
Sieciowe listy ACL w twoim VPC. Jak oceniane są reguły dostępu

    od najniższej do najwyższej liczby + działają natychmiast

#### Public subnet + Internet GW

skonfigurowałeś również instancję EC2 z publicznym adresem IP. Jednak nadal nie możesz połączyć się z instancją przez Internet.

    MAIN ROUTE TABLE + wpis tam, gdzie skonfigurowano IGW

Jeśli nie możesz skorzystać z EC2, nawet jeśli w VPC jest już dostępna strona internetowa i nie ma problemu w grupie zabezpieczeń, musisz sprawdzić, czy wpisy w tabelach tras są obsługiwane

#### Otwarcie portu

Jesteś architektem rozwiązań pracującym dla dużej firmy ubezpieczeniowej, która wdrożyła swoje środowisko produkcyjne w niestandardowej wirtualnej chmurze prywatnej w AWS z domyślną konfiguracją. VPC składa się z dwóch prywatnych podsieci i jednej publicznej podsieci. Wewnątrz podsieci publicznej znajduje się grupa wystąpień EC2, które są tworzone przez grupę automatycznego skalowania, a wszystkie wystąpienia znajdują się w tej samej grupie zabezpieczeń. Twój zespół programistów stworzył nową aplikację, do której będą dostępne urządzenia mobilne za pośrednictwem niestandardowego portu. Ta aplikacja została wdrożona w środowisku produkcyjnym i musisz otworzyć ten port globalnie w Internecie.
Które z poniższych jest prawidłową procedurą spełniającą ten wymóg?

    SECURITY GROUP!!

Aby zezwolić na niestandardowy port, musisz zmienić reguły ruchu przychodzącego w grupie zabezpieczeń, aby zezwolić na ruch przychodzący z urządzeń mobilnych. Grupy zabezpieczeń zwykle kontrolują listę portów, które mogą być używane przez instancje EC2, a NACL kontrolują, która sieć lub lista adresów IP może łączyć się z całym komputerem VPC.

#### VPC + IGW, no connection with new instance
Jesteś architektem rozwiązań dla globalnej firmy prasowej. Konfigurujesz flotę instancji EC2 w podsieci, która obecnie znajduje się w VPC z podłączoną bramą internetową. Dostęp do wszystkich tych instancji EC2 można uzyskać z Internetu.

**Następnie URUCHAMIASZ INNĄ PODSIEĆ** i uruchamiasz w niej instancję EC2, jednak nie możesz uzyskać dostępu do instancji EC2 z Internetu.

Jakie mogą być przyczyny tego problemu?

W przypadkach, gdy do Twojej instancji EC2 nie można uzyskać dostępu z Internetu (lub odwrotnie), zwykle musisz sprawdzić dwie rzeczy:

    Czy ma EIP lub publiczny adres IP?

    Czy tabela tras jest odpowiednio skonfigurowana?


    Amazon EC2 instance does not have a public IP address associated with it.

    The route table is not configured properly to send traffic from the EC2 instance to the Internet through the Internet gateway.

#### Another issues
VPC issue connection throught the Internet. Twoje środowisko jest skonfigurowane z czterema instancjami EC2, które wszystkie należą do publicznej podsieci innej niż domyślna. Instancje EC2 również należą do tej samej grupy zabezpieczeń. Wszystko działa zgodnie z oczekiwaniami, z wyjątkiem jednej instancji EC2, która nie jest w stanie wysyłać ani odbierać ruchu przez Internet.

    CHODZI O PUBLICZNE IP instancji musi być przypisane

***nie chodzi o Route Table czy SG bo pozostałe trzy wystąpienia, które są powiązane z tą samą tabelą tras (Route Table) i grupą zabezpieczeń, nie mają żadnych problemów.

#### EC2 by SSH issue
Scenariusz jest taki, że można już połączyć się z instancją EC2 przez SSH. Oznacza to, że nie ma problemu w tabeli tras twojego VPC. Aby rozwiązać ten problem, wystarczy **zaktualizować grupę zabezpieczeń i dodać regułę ruchu przychodzącego, aby zezwolić na ruch HTTP**.

### VPC Peering
Masz dwa VPC, które mają ze sobą połączenia równorzędne (peering). Należy zauważyć, że połączenie równorzędne VPC nie obsługuje routingu od krawędzi do krawędzi. Oznacza to, że jeśli którykolwiek VPC w relacji równorzędnej ma jedno z następujących połączeń, nie można rozszerzyć relacji równorzędnej na to połączenie:

- Połączenie VPN lub połączenie AWS Direct Connect z siecią korporacyjną
- Połączenie internetowe przez bramę internetową
- Połączenie internetowe w prywatnej podsieci za pośrednictwem urządzenia NAT
- Punkt końcowy VPC do usługi AWS; na przykład punkt końcowy dla Amazon S3.
- (IPv6) Połączenie ClassicLink. Można włączyć komunikację IPv4 między połączoną instancją EC2-Classic a instancjami w VPC po drugiej stronie połączenia równorzędnego VPC. Jednak IPv6 nie jest obsługiwany w EC2-Classic, więc nie można przedłużyć tego połączenia dla komunikacji IPv6.


    - Establish a hardware VPN over the Internet between the VPC and the on-premises network.
    - Establish another AWS Direct Connect connection and private virtual interface in the same AWS region. 

#### Peering - add new entry
Duża firma ubezpieczeniowa ma konto AWS, które zawiera trzy VPC (DEV, UAT i PROD) w tym samym regionie. UAT jest monitorowany zarówno do PROD, jak i DEV za pomocą połączenia równorzędnego VPC. Wszystkie VPC mają nie nakładające się bloki CIDR. Firma chce przesunąć drobne wydania kodu z Dev na Prod, aby przyspieszyć wprowadzanie na rynek.

    Nowy wpis dla PRODa w tabeli DEV obsługi połączenia VPC peering jako cel

Połączenie równorzędne VPC to połączenie sieciowe między dwoma komputerami VPC, które umożliwia prywatne kierowanie ruchem między nimi. Instancje w obu VPC mogą komunikować się ze sobą, tak jakby były w tej samej sieci. Możesz utworzyć połączenie równorzędne VPC między własnymi VPC, z VPC na innym koncie AWS lub z VPC w innym regionie AWS.

AWS wykorzystuje istniejącą infrastrukturę VPC do utworzenia połączenia równorzędnego VPC; nie jest bramą ani połączeniem VPN i nie polega na osobnym fizycznym sprzęcie. Nie ma jednego punktu awarii komunikacji lub wąskiego gardła przepustowości.



#### Bastion
bastion in Amazon VPC and it should only be accessed from the corporate data center via SSH
    
    small EC2 with opened port 22 + use private keys pem
    
#### Bastion RDP

Bastion Remote Desktop Protocol (RDP) dostęp do grup zabezpieczeń instancji aplikacji

wdróż hosta Bastion systemu Windows z elastycznym adresem IP w publicznej podsieci i zezwól na dostęp RDP do bastionu tylko z korporacyjnych adresów IP.

#### Elastyczny interfejs sieciowy (ENI) - dołaczanie

When it comes to the ENI attachment to an EC2 instance, what does 'warm attach'

    Instance Stopped

Elastyczny interfejs sieciowy (ENI) to logiczny element sieci w VPC, który reprezentuje wirtualną kartę sieciową. Możesz podłączyć interfejs sieciowy do instancji EC2 na następujące sposoby:

- hot attach - runned;
- warm attach - stopped;
- cold - is being launched.



### Redshift Enhanced VPC Routing
Amazon Redshift Enhanced VPC Routing, Amazon Redshift forces all COPY and UNLOAD traffic between your cluster and your data repositories through your Amazon VPC.

#### Nie-domyślna sieć publiczna VPC
VPC ma inną niż domyślna podsieć publiczną, która ma cztery instancje EC2 na żądanie, do których można uzyskać dostęp przez Internet. Za pomocą interfejsu AWS CLI uruchomiono piąte wystąpienie korzystające z tej samej podsieci, Amazon Machine Image (AMI) i grupy zabezpieczeń, z których korzystają inne wystąpienia. Podczas testowania nie można uzyskać dostępu do nowej instancji.

Domyślnie „domyślna podsieć” VPC jest w rzeczywistości publiczną podsiecią, ponieważ główna tablica tras wysyła ruch podsieci przeznaczony do Internetu do bramy internetowej. Możesz zmienić domyślną podsieć w prywatną podsieć, usuwając trasę z miejsca docelowego 0.0.0.0/0 do bramy internetowej. Jednak jeśli to zrobisz, każda instancja EC2 działająca w tej podsieci nie będzie mogła uzyskać dostępu do Internetu.

Instancje uruchamiane w domyślnej podsieci otrzymują zarówno publiczny adres IPv4, jak i prywatny adres IPv4 oraz publiczne i prywatne nazwy hostów DNS. Instancje uruchamiane w podsieci innej niż domyślna w domyślnym VPC nie otrzymują publicznego adresu IPv4 ani nazwy hosta DNS. Możesz zmienić domyślne zachowanie publicznego adresowania IP swojej podsieci

**Domyślnie podsieci inne niż domyślne mają ustawiony publiczny adres IPv4 na wartość false, a podsieci domyślne mają ten atrybut ustawiony na true. Wyjątkiem jest podsieć inna niż domyślna utworzona przez kreatora instancji uruchomienia Amazon EC2 - kreator ustawia atrybut na true.**


Opcja 4 jest poprawna, ponieważ piąta instancja nie ma publicznego adresu IP, ponieważ została wdrożona w podsieci innej niż domyślna. Pozostałe 4 instancje są dostępne przez Internet, ponieważ każdy z nich ma dołączony elastyczny adres IP, w przeciwieństwie do ostatniej instancji, która ma tylko prywatny adres IP. Elastyczny adres IP to publiczny adres IPv4, dostępny z Internetu. Jeśli Twoja instancja nie ma publicznego adresu IPv4, możesz powiązać elastyczny adres IP z instancją, aby umożliwić komunikację z Internetem.

Opcja 1 jest niepoprawna, ponieważ grupy miejsc docelowych są używane przede wszystkim do określania, w jaki sposób instancje są umieszczane na podstawowym sprzęcie, podczas gdy z drugiej strony Enhanced Networking zapewnia wydajne funkcje sieciowe przy użyciu wirtualizacji we / wy z jednym rootem (SR-IOV) na obsługiwanych typach instancji EC2.

Opcja 2 jest niepoprawna, ponieważ nie potrzebujesz bramy NAT ani instancji NAT w tym scenariuszu, biorąc pod uwagę, że instancje są już w publicznej podsieci. Pamiętaj, że brama NAT lub instancja NAT jest używana przede wszystkim w celu umożliwienia instancjom w prywatnej podsieci łączenia się z Internetem lub innymi usługami AWS, ale uniemożliwiają zainicjowanie połączenia internetowego z tymi instancjami.

Opcja 3 jest nieprawidłowa, ponieważ wszystkie cztery instancje EC2, które znajdują się w tej samej podsieci, tym samym AMI i tej samej grupie zabezpieczeń, są dostępne przez Internet. Oznacza to, że nie ma problemu z tabelą routingu.

#### VPC advantages in context of connections with on-premise
Menedżer biznesu poinstruował cię, aby zintegrować lokalne centrum danych i VPC. Wyjaśniłeś listę zadań, które będziesz wykonywać, i wspomniałeś o połączeniu z wirtualną siecią prywatną (VPN). Menedżer biznesowy nie jest obeznany z technologią, ale interesuje go, czym jest VPN i jakie są jej zalety.

Jakie są główne zalety posiadania VPN w AWS?

    VPN = tunele TLS + IPsec (i brama klienta)

Amazon VPC oferuje elastyczność pełnego zarządzania obiema stronami połączenia Amazon VPC poprzez utworzenie połączenia VPN między twoją siecią zdalną a programowym urządzeniem VPN działającym w sieci Amazon VPC. Ta opcja jest zalecana, jeśli musisz zarządzać obydwoma końcami połączenia VPN w celu zachowania zgodności lub wykorzystania bramek, które nie są obecnie obsługiwane przez rozwiązanie VPN Amazon VPC.

Możesz utworzyć połączenie IPsec VPN między VPC a siecią zdalną. Po stronie AWS połączenia VPN wirtualna brama prywatna zapewnia dwa punkty końcowe VPN (tunele) do automatycznego przełączania awaryjnego. Skonfigurujesz bramę klienta po zdalnej stronie połączenia VPN. Jeśli masz więcej niż jedną sieć zdalną (na przykład wiele oddziałów), możesz utworzyć wiele połączeń VPN zarządzanych przez AWS za pośrednictwem wirtualnej bramy prywatnej, aby umożliwić komunikację między tymi sieciami.

Dzięki AWS Site-to-Site VPN możesz łączyć się z Amazon VPC w chmurze w taki sam sposób jak z oddziałami. AWS Site-to-Site VPN ustanawia bezpieczne i prywatne sesje z tunelami IP Security (IPSec) i Transport Layer Security (TLS).




    
    
## SQS, SNS, MQ and messaging

- Amazon SNS - usługa internetowa koordynująca dostarczanie lub wysyłanie wiadomości do subskrybujących punktów końcowych lub klientów.

- Amazon SQS - Oferuje niezawodne i skalowalne hostowane kolejki do przechowywania wiadomości podczas podróży między komputerem.

- SES służy głównie do wysyłania wiadomości e-mail zaprojektowanych, aby pomóc sprzedawcom cyfrowym i twórcom aplikacji w wysyłaniu wiadomości marketingowych, powiadomień i transakcji, a nie do wysyłania powiadomień o zdarzeniach z S3. Musisz użyć SNS, SQS lub Lambda.

- SWF służy głównie do tworzenia aplikacji wykorzystujących chmurę Amazon do koordynowania pracy między rozproszonymi komponentami i nie jest wykorzystywany jako sposób wyzwalania powiadomień o zdarzeniach z S3. Musisz użyć SNS, SQS lub Lambda.


#### Deleting SQS messages by consumer-component
EC2 that consumes messages from an SQS queue and is integrated with SNS to send out an email to you once the process is complete. You received 5 orders but after a few hours, you saw more than 20 email notifications in your inbox.

    Web app not deleting messages

Zawsze pamiętaj, że komunikaty w kolejce SQS będą istniały nawet po przetworzeniu go przez instancję EC2, dopóki nie usuniesz tego komunikatu. 

Refer to the third step of the SQS Message Lifecycle:

1. Component 1 sends Message A to a queue, and the message is distributed across the Amazon SQS servers redundantly.

2. When Component 2 is ready to process a message, it consumes messages from the queue, and Message A is returned. While Message A is being processed, it remains in the queue and isn't returned to subsequent receive requests for the duration of the visibility timeout. 

3. Component 2 deletes Message A from the queue to prevent the message from being received and processed again once the visibility timeout expires.

#### SQS - Multiple queues

Strona internetowa oferuje darmowe konto i konto premium, które gwarantuje szybsze przetwarzanie. Wszystkie żądania członków bezpłatnych i premium przechodzą przez jedną kolejkę SQS, a następnie przetwarzane przez grupę instancji EC2, które generują filmy

    dwie kolejki SQS + std kolejka premium a jeśli pusta to z free

#### SQS Retention Period
SQS skonfigurowany z maksymalnym okresem przechowywania wiadomości. Po TRZYNASTU DNIACH działania aplikacja internetowa nagle uległa awarii i w kolejce nadal czeka 10 000 nieprzetworzonych wiadomości.

w tym scenariuszu stwierdzono, że kolejka SQS jest skonfigurowana z maksymalnym okresem przechowywania wiadomości. Maksymalny czas przechowywania wiadomości w SQS wynosi 14 dni, dlatego opcja 3 jest prawidłową odpowiedzią, tj. Nie będzie brakujących wiadomości.

W Amazon SQS możesz skonfigurować okres przechowywania wiadomości na wartość od 1 minuty do 14 dni. Domyślnie są to 4 dni. Po osiągnięciu limitu przechowywania wiadomości są automatycznie usuwane.

Pojedyncza kolejka komunikatów Amazon SQS może zawierać nieograniczoną liczbę wiadomości. Istnieje jednak limit 120 000 dla liczby wiadomości przychodzących dla standardowej kolejki i 20 000 dla kolejki FIFO. Wiadomości są rozświetlane po odebraniu ich z kolejki przez odbiorcę, ale nie zostały jeszcze usunięte z kolejki.

SQS zauważyłeś, że ty i twój oficer otrzymacie 50 e-maili z aplikacji z tą samą wiadomością.

Twoja aplikacja nie wydaje polecenia usuwania do kolejki SQS po przetworzeniu komunikatu, dlatego ten komunikat wrócił do kolejki i został przetworzony wiele razy



#### Decoupled architecture serices

Create a decoupled architecture?
    
    SWF + SQS

***there is no such thing as Amazon Simple Decoupling Service!!

#### SWF
rejestracja online dla zdarzeń, która wykorzystuje Simple Workflow do pełnej kontroli logiki aranżacji. Decydent przyjmuje nazwę klienta, adres, numer kontaktowy i adres e-mail, podczas gdy pracownicy aktywności aktualizują klienta o statusie swojej aplikacji online za pośrednictwem poczty elektronicznej. Ostatnio miałeś problemy ze swoją internetową platformą rejestracyjną, która została rozwiązana przez sprawdzenie zadania decyzyjnego twojego przepływu pracy.

    zadanie decyzyjne - decydent może być postrzegany jako specjalny rodzaj pracownika. Podobnie jak pracownicy, może być napisany w dowolnym języku i prosi Amazon SWF o zadania. Jednak obsługuje specjalne zadania zwane zadaniami decyzyjnymi.

definicja przepływu pracy w SWF - wszystkie działania w przepływie pracy

definicja zadania aktywności - informuje pracownika o wykonaniu funkcji

definicja zadania SWF - reprezentuje pojedyncze zadanie w przepływie pracy


Amazon SWF wydaje zadania decyzyjne za każdym razem, gdy wykonanie przepływu pracy ma przejścia, takie jak ukończenie zadania aktywności lub przekroczenie limitu czasu. Zadanie decyzyjne zawiera informacje o danych wejściowych, wyjściowych i bieżącym stanie wcześniej zainicjowanych zadań związanych z aktywnością. Decydent korzysta z tych danych, aby zdecydować o kolejnych krokach, w tym o nowych zadaniach związanych z aktywnością, i zwraca je do Amazon SWF. Amazon SWF z kolei wprowadza te decyzje, inicjując nowe zadania związane z aktywnością w stosownych przypadkach i monitorując je.

Odpowiadając na bieżące zadania decyzyjne, decydent kontroluje kolejność, harmonogram i współbieżność zadań związanych z działaniem, a tym samym wykonywanie kroków przetwarzania w aplikacji. SWF wydaje pierwsze zadanie decyzyjne po rozpoczęciu wykonywania. Odtąd Amazon SWF wprowadza w życie decyzje podejmowane przez osobę podejmującą decyzję dotyczące kierowania egzekucją. Egzekucja trwa, dopóki osoba podejmująca decyzję nie podejmie decyzji o jej zakończeniu.


#### Amazon MQ

Aplikacja ma usługę brokera komunikatów, która wykorzystuje standardowe w branży interfejsy API i protokoły przesyłania komunikatów, które również wymagają migracji, bez przepisywania kodu wiadomości w aplikacji.

Które z poniższych jest najbardziej odpowiednią usługą, której należy użyć, aby przenieść usługę przesyłania wiadomości do AWS?

    AWS MQ - dla apek już istniejących

Jeśli budujesz zupełnie nowe aplikacje w chmurze, zdecydowanie zalecamy rozważenie Amazon SQS i Amazon SNS

SNS is more suitable as a pub/sub 

SQS is a fully managed message queuing service

SWF is a fully-managed state tracker and task coordinator service

***Korzystanie z Amazon SQS wymaga wprowadzenia dodatkowych zmian w kodzie aplikacji, aby był kompatybilny.







## ECS

ECS. Jako architekt rozwiązań musisz upewnić się, że poświadczenia są bezpieczne i że nie można ich wyświetlić w postaci zwykłego tekstu w samym klastrze.

Amazon ECS umożliwia wstrzykiwanie wrażliwych danych do kontenerów poprzez przechowywanie wrażliwych danych w tajemnicach AWS Secrets Manager lub AWS Systems Manager Parameter Store, a następnie przywoływanie ich w definicji kontenera. Ta funkcja jest obsługiwana przez zadania korzystające zarówno z typów uruchamiania EC2, jak i Fargate.


## Cloud Formation, Elastic Beanstalk

Instancje EC2 i serwer relacyjnych baz danych Oracle. Wymagana jest wysoka dostępność bazy danych i pełna kontrola nad jej systemem operacyjnym.

Instancje Amazon EC2 z replikacją danych między dwiema różnymi strefami dostępności

    Cloudformation i QuickStart

Szybki start QuickStart wdraża podstawową dostępną bazę danych Oracle (przy użyciu zestawu danych bazowych dostępnych usług Oracle) w instancji Amazon EC2 w pierwszej grupie działań. Następnie skonfiguruj drugą instancję EC2 w drugiej liście operacji, skopiuj podstawową zawartość danych do drugiej instancji za pomocą instrukcji DUPLICATE i konfiguruj Oracle Data Guard


## Route 53

#### Route 53 failover

Stos MEAN, który zostanie wprowadzony w przyszłym miesiącu. Istnieje prawdopodobieństwo, że ruch będzie dość wysoki w ciągu pierwszych kilku tygodni. W przypadku niepowodzenia ładowania, w jaki sposób można ustawić przełączenie awaryjne DNS na statyczną stronę internetową

    R53 failover to static S3 lub CFront

#### Weighted Routing Policy
  
Oczekuje się, że liczba użytkowników wzrośnie w nadchodzących miesiącach, dlatego należy zwiększyć elastyczność i skalowalność architektury AWS, aby sprostać wymaganiom.

    2xEC2 i ELB + EC2 use R53 Weighted Routing Policy
    

#### Geolocation Routing Policy

Twoich użytkowników można znaleźć na całym świecie, ale większość z nich pochodzi z Japonii i Szwecji. Ze względu na wymagania dotyczące zgodności w tych dwóch lokalizacjach chcesz, aby Twoi japońscy użytkownicy łączyli się z serwerami w regionie Azji Południowo-Pacyfiku ap-północno-wschodni-1, a szwedzcy użytkownicy powinni być połączeni z serwerami w Europie Zachodniej 1 region UE (Irlandia).

Która z poniższych usług pozwoli ci łatwo spełnić ten wymóg?

    GEOLOCATION ROUTING

(ISTNIEJE COŚ TAKIEGO JAK GEORESTRICTION) Funkcja ograniczenia geograficznego CloudFront służy przede wszystkim do uniemożliwienia użytkownikom w określonych lokalizacjach geograficznych dostępu do treści dystrybuowanych za pośrednictwem dystrybucji internetowej CloudFront. Nie pozwala wybrać zasobów, które obsługują ruch na podstawie położenia geograficznego użytkowników, w przeciwieństwie do zasad routingu geolokalizacji na Route 53.)
    
    
#### Kinesis

Jakiej usługi możesz użyć do łatwego przechwytywania, przekształcania i ładowania danych przesyłanych strumieniowo do Amazon S3, Amazon Elasticsearch Service i Splunk?

    Kinesis FIREHOSE

Amazon Kinesis Data Firehose to najprostszy sposób na załadowanie strumieniowych danych do magazynów danych i narzędzi analitycznych. Może przechwytywać, przekształcać i ładować dane przesyłane strumieniowo do Amazon S3, Amazon Redshift, Amazon Elasticsearch Service i Splunk, umożliwiając analizę w czasie rzeczywistym za pomocą istniejących narzędzi analizy biznesowej i pulpitów nawigacyjnych, których już używasz.

#### Data Streams - Shards
Jakiej usługi AWS możesz użyć do gromadzenia i przetwarzania dużych strumieni rekordów danych w czasie rzeczywistym?

    Kinezis Data Strims ;-)


#### Okres, od którego rekord jest dodawany w Kinesis
Zainstalowano czujniki do śledzenia liczby odwiedzających, którzy udają się do parku. Dane przesyłane są codziennie do strumienia Amazon Kinesis z domyślnymi ustawieniami przetwarzania, w których konsument jest skonfigurowany do przetwarzania danych co drugi dzień. Zauważyłeś, że twój segment S3 nie odbiera wszystkich danych wysyłanych do strumienia Kinesis. Sprawdziłeś czujniki, czy prawidłowo wysyłają dane do Amazon Kinesis i potwierdziłeś, że dane są wysyłane codziennie.

    Okres, od którego rekord jest dodawany, kiedy nie jest już dostępny, nazywany jest okresem przechowywania.
    Strumień danych Kinesis przechowuje zapisy od domyślnie 24 godzin do maksymalnie 168 godzin.


    

## CI/CD

#### Canary
Jeśli korzystasz z platformy obliczeniowej Lambda AWS, musisz wybrać jeden z następujących typów konfiguracji wdrażania, aby określić sposób przenoszenia ruchu z oryginalnej wersji funkcji AWS Lambda na nową wersję funkcji AWS Lambda:

    Canary

Canary: ruch jest przesuwany w dwóch krokach. Możesz wybierać spośród predefiniowanych opcji kanarka, które określają procent ruchu przesuniętego do zaktualizowanej wersji funkcji Lambda w pierwszym kroku i interwału, w minutach, zanim pozostały ruch zostanie przesunięty w drugim kroku.


#### Pilot Light
Którą z poniższych architektur odzyskiwania po awarii jest najbardziej opłacalnym typem do zastosowania w tym scenariuszu?

    Światło pilota

Termin lampka kontrolna jest często używany do opisania scenariusza DR, w którym minimalna wersja środowiska zawsze działa w chmurze. Idea lampki kontrolnej jest analogią pochodzącą z nagrzewnicy gazowej. W grzejniku gazowym mały płomień, który jest zawsze włączony, może szybko zapalić cały piec, aby ogrzać dom. Ten scenariusz jest podobny do scenariusza tworzenia kopii zapasowych i przywracania.

## Other AWS services

#### Budgets
Service can help you manage the budgets for all your AWS resources

    AWS Budgets
    
#### DDoS attacks 

    AWS Shield

#### DDoS attacks #2

Które z poniższych NIE są wykonalnymi technikami łagodzenia?

    Użyj usługi Amazon CloudFront do dystrybucji zarówno treści statycznych, jak i dynamicznych.

    Użyj modułu równoważenia obciążenia aplikacji z grupami automatycznego skalowania dla instancji EC2, a następnie ogranicz bezpośredni ruch internetowy do bazy danych Amazon RDS poprzez wdrożenie w prywatnej podsieci.

    Skonfiguruj alerty w usłudze Amazon CloudWatch, aby wyszukać wysokie wskaźniki wejścia sieciowego i wykorzystania procesora.



#### Chef recipes in AWS

    OpsWorks

#### Apka korzysta z czujników i urządzeń inteligentnych

    AWS IoT Core
    

#### Amazon Macie

Amazon Macie jest głównie używany jako usługa bezpieczeństwa, która wykorzystuje uczenie maszynowe do automatycznego wykrywania, klasyfikowania i ochrony wrażliwych danych w AWS. 


#### Snowball

Remember that an 80 TB Snowball appliance and 100 TB Snowball Edge appliance only have 72 TB and 83 TB of usable capacity respectively

####AWS Shared Responsibility Model

which security aspects are the responsibilities of the customer

    OS Patching  on EC2 instances
    +
    IAM Policies Management

Customer:

- Amazon Machine Images (AMIs)
- Operating systems
- Applications
- Data in transit
- Data at rest
- Data stores
- Credentials
- Policies and configuration