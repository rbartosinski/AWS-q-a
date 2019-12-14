# AWS-q-a
Knowledge base about architecture, basics and standards in AWS (in Polish).


### AWS general

`AVAILIBILITY`

    Dostępność - można opisać jako% okresu, w którym usługa będzie mogła w pewien sposób odpowiedzieć na Twoje zapytanie.


`DURABILITY `

    Trwałość - odnosi się do ciągłego istnienia obiektu lub zasobu. Pamiętaj, że nie oznacza to, że możesz uzyskać do niego dostęp, tylko że nadal istnieje.



#### Budgets
Service can help you manage the budgets for all your AWS resources

    AWS Budgets

## EC2

#### Placement Groups
Grupy miejsc docelowych mogą być typu „klaster”, „spread” lub „partycja”. Wybierz opcje poniżej, które są specyficzne tylko dla grup miejsc docelowych.

    grupa instancji umieszczona w odrębnym sprzęcie bazowym

Jest tylko jedna odpowiedź, która jest specyficzna dla grup miejsc docelowych Spread i jest to ostatnia opcja. Chociaż niektóre z tych odpowiedzi są poprawne tylko dla obu Grup rozmieszczania klastrów lub zarówno dla Grup rozmieszczania klastrów, jak i grup rozmieszczania rozproszonego, w pytaniu stwierdzono, że należy wybrać tylko opcje specyficzne dla grup rozmieszczania klastrów. Wykluczałoby to dwie opcje, ponieważ są one prawdziwe zarówno dla grup miejsc docelowych typu Spread & Cluster. Logiczne grupowanie wystąpień w obrębie jednej Strefy dostępności jest prawdziwe tylko w przypadku Grup rozmieszczania klastrów i jest również nieprawidłowe.

#### Services with underlying OS
avoid using fully-managed AWS services and instead, only use specific services which allows you to access the underlying operating system for the resource

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

#### "Ephermal" volumes

If this instance is stopped, what will happen to the data on the EPHERMAL store volumes?

    Deleted (ephemeral means "short-lived" or "temporary")


#### Security groups modyfing

Modified the security group - changes immediately

    you can assign up to five security groups to the instance.
    
#### Statyczny adresu IP
Starsza aplikacja wymaga statycznego adresu IP na stałe zakodowanego w wewnętrznej bazie danych, co blokuje korzystanie z modułu równoważenia obciążenia aplikacji.
Jakie kroki podjąłbyś, aby zastosować wysoką dostępność i odporność na uszkodzenia w tej aplikacji bez ELB?

    script to check health isntance - script to swithc elstic IP to instance + elastic IP

Skrypt niestandardowy umożliwia jednej instancji Amazon Elastic Compute Cloud (EC2) monitorowanie innej instancji Amazon EC2 i przejmowanie prywatnego „wirtualnego” adresu IP w przypadku awarii instancji. W przypadku użycia z dwoma instancjami skrypt włącza scenariusz wysokiej dostępności, w którym instancje monitorują się nawzajem i przejmują współdzielony wirtualny adres IP, jeśli druga instancja zawiedzie. Można go łatwo zmodyfikować, aby działał na zewnętrznym serwerze monitorowania lub serwerze świadków w celu wykonania wymiany VIP w imieniu dwóch monitorowanych węzłów.

Opcja 4 jest niepoprawna, ponieważ chociaż grupa automatycznego skalowania zapewnia wysoką dostępność i skalowalność, !!!!!!!!!!nadal zależy od ELB, który nie jest dostępny w tym scenariuszu. Pamiętaj, że musisz mieć statyczny adres IP, który może mieć postać elastycznego adresu IP. Chociaż grupa automatycznego skalowania może zostać skalowana, jeśli jedno z wystąpień EC2 stanie się niezdrowe, nadal nie można bezpośrednio przypisać EIP do grupy automatycznego skalowania.

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

## S3

#### Dostępność i koszt

Aplikacja używa S3 do przechowywania obrazów, a spodziewasz się nagłego i znacznego wzrostu ruchu do S3, gdy nastąpi duże wydarzenie informacyjne (użytkownicy będą przesyłali duże ilości treści). Musisz ograniczyć koszty przechowywania do minimum , i z przyjemnością tymczasowo utracisz dostęp do 0,1% przesyłanych rocznie.

Kluczowymi czynnikami tutaj są dostępność i koszt, dlatego należy odpowiedzieć na to pytanie.

Pełna S3 jest dość droga i kosztuje około 0,023 USD za GB dla najniższego pasma.

Standard S3 IA wynosi 0,0125 USD na GB,

S3 OneZone-IA wynosi 0,01 USD na GB,

a starszy S3-RRS wynosi około 0,024 USD na GB dla najniższego pasma.

    Z oferowanych rozwiązań S3 One Zone-IA jest najtańszą odpowiednią opcją.

Glacier nie może być brany pod uwagę, ponieważ nie jest przeznaczony do bezpośredniego dostępu, ale kosztuje około 0,004 USD za GB.

S3 ma dostępność 99,99%, S3-IA ma dostępność 99,9%, podczas gdy S3-1Zone-IA ma tylko 99,5%.

#### Ochorna przed usunięciem objektów S3

What combination of the following options will protect the S3 objects in your bucket from both accidental deletion and overwriting?

    Versioning + MFA Deletion

#### Logi audytu z usług globalnych
Architekt ma za zadanie skonfigurować system rejestrowania, który będzie śledził wszystkie zmiany dokonane w ich zasobach AWS we wszystkich regionach, w tym konfiguracje dokonane w IAM, CloudFront, AWS WAF i Route 53.

    - Ctrail CLI `--is-multi-region-trail` + `--include-global-service-events`
    - MFA Auth on del S3

CloudWatch będzie jednak obejmował wyłącznie usługi regionalne (EC2, S3, RDS itp.), A nie usługi globalne, takie jak IAM, CloudFront, AWS WAF i Route 53.

Ale można wlaczyć kilka regionów

***Funkcja rejestrowania audytu służy przede wszystkim do uzyskiwania informacji o połączeniu, zapytaniach i działaniach użytkowników w klastrze Redshift.

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

#### RDS - Enhanced monitoring
musisz ściśle monitorować, w jaki sposób różne procesy lub wątki w instancji DB wykorzystują procesor, w tym procent przepustowości procesora (percentage of the CPU bandwidth) i całkowitej pamięci zużywanej przez każdy proces (total memory consumed).

    RDS - Enhanced monitoring


## DynamoDB, Redshift and other NoSQL DBs

#### Frequent schema changes

One of their systems requires a database that can scale globally and can handle !!!!!frequent schema changes (<- NOSQL) . It should also provide low-latency response to high-traffic queries.

    DYNAMO


#### DAX use cases
DynamoDB table and the static assets are distributed by CloudFront. However, there are a lot of complaints that saving and retrieving player information is taking a lot of time.

    DAX

#### Redshift workload
Redshift do aplikacji do przetwarzania analitycznego online (OLAP), która przetwarza złożone zapytania na duże zestawy danych. Istnieje wymóg, w którym należy zdefiniować liczbę dostępnych kolejek zapytań oraz sposób kierowania zapytań do tych kolejek w celu przetworzenia.

    REDSHIFT WORKLOAD MANAGEMENT PARAMETER

Podczas tworzenia grupy parametrów domyślna konfiguracja WLM zawiera jedną kolejkę, w której można uruchomić do pięciu zapytań jednocześnie. Możesz dodać dodatkowe kolejki i skonfigurować właściwości WLM w każdej z nich, jeśli chcesz mieć większą kontrolę nad przetwarzaniem zapytań. Każda dodana kolejka ma tę samą domyślną konfigurację WLM, dopóki nie skonfigurujesz jej właściwości. Po dodaniu kolejnych kolejek ostatnia kolejka w konfiguracji jest kolejką domyślną. O ile zapytanie nie jest kierowane do innej kolejki na podstawie kryteriów w konfiguracji WLM, jest przetwarzane przez kolejkę domyślną. Nie można określić grup użytkowników ani grup zapytań dla domyślnej kolejki.

Podobnie jak w przypadku innych parametrów, nie można modyfikować konfiguracji WLM w domyślnej grupie parametrów. Klastry powiązane z domyślną grupą parametrów zawsze używają domyślnej konfiguracji WLM. Jeśli chcesz zmodyfikować konfigurację WLM, musisz utworzyć grupę parametrów, a następnie powiązać tę grupę parametrów z dowolnymi klastrami wymagającymi niestandardowej konfiguracji WLM.




## Auto Scaling Groups & Elastic Load Balancers

#### AS - SCchedule Scaling Policies
Znasz już dokładne godziny szczytu aplikacji. Zanim procesor lub pamięć osiągną wartość szczytową, aplikacja ma już problemy z wydajnością

    AUTO SCALING GROUPS - need to configure a SCHEDULE SCALING POLICY


#### ELB Health Checks failed
EC2 instance behind an ELB fails a health check

    Stop sending traffic to this instance
    
## Users, encryption, security

Both the master keys and the unencrypted data should never be sent to AWS

    Client side encryption + client side master key
    
#### Autoryzacja początku trasy (ROA)
migrate these applications to AWS without requiring your partners and customers to change their IP address whitelists.

    ROA + X509  

Autoryzacja początku trasy (ROA) to dokument, który można utworzyć za pomocą regionalnego rejestru internetowego (RIR), takiego jak amerykański rejestr numerów internetowych (ARIN) lub Réseaux IP Européens Network Coordination Centre (RIPE). Zawiera zakres adresów, numery ASN, które mogą reklamować zakres adresów oraz datę ważności. Dlatego opcja 3 jest prawidłową odpowiedzią.

ROA upoważnia Amazon do reklamowania zakresu adresów pod określonym numerem AS. Jednak nie upoważnia twojego konta AWS do przeniesienia zakresu adresów do AWS. Aby autoryzować swoje konto AWS w celu wprowadzenia zakresu adresów do AWS, musisz opublikować samopodpisany certyfikat X509 w uwagach RDAP dotyczących zakresu adresów. Certyfikat zawiera klucz publiczny, którego AWS używa do weryfikacji podanego przez siebie podpisu kontekstu autoryzacji. Należy zabezpieczyć swój klucz prywatny i używać go do podpisywania wiadomości kontekstu autoryzacji.

### Active Directory + on prem creds = SAML
Access resources on both environments using their on-premises credentials, which is stored in Active Directory.

    SAML

### Active Directory + ID federation & role based 
Company policy mandates the use of identity federation and role-based access control. Currently, the roles are already assigned using groups in the corporate Active Directory.

    AD Connector + IAM Roles
    
#### Active Directory + S3
1200 pracowników otrzymają dostęp do korzystania z Amazon S3 do przechowywania swoich dokumentów osobistych.

Które z poniższych należy wziąć pod uwagę, aby można było skonfigurować rozwiązanie, które zawiera funkcję pojedynczego logowania z firmowego katalogu AD lub LDAP, a także ogranicza dostęp dla każdego użytkownika do wyznaczonego folderu użytkownika w segmencie S3?

- Setup a Federation proxy or an Identity provider

- Setup an AWS Security Token Service to generate temporary tokens (Option 2)

- Configure an IAM role (Option 4) 



## CloudWatch, Cloud Trail

#### Custom metrics
You need to prepare a custom metric using CloudWatch Monitoring Scripts which is written in Perl. You can also install CloudWatch Agent to collect more system-level metrics from Amazon EC2 instances. Here's the list of custom metrics that you can set up:

    - Memory utilization
    - Disk swap utilization
    - Disk space utilization
    - Page file utilization
    - Log collection
    
### API calls to your Redshift instance
Which service will allow you to monitor all API calls to your Redshift instance and can also provide secured data for auditing and compliance purposes?

    Cloud Trail
    
Aby zapewnić wysoką dostępność systemów, musisz monitorować wykorzystanie pamięci i dysku we wszystkich instancjach.

Które z poniższych jest najbardziej odpowiednim rozwiązaniem do monitorowania?

    INSTALL CWATCH AGENT + which gathers memory and disk utilization
    AWS Systems Manager

***Enhanced Monitoring is a feature of RDS and not of CloudWatch.
    
## AWS Lambda, API Gateway

Jeśli jednak chcesz używać pomocników szyfrowania i KMS do szyfrowania zmiennych środowiskowych po utworzeniu funkcji Lambda, musisz utworzyć własny klucz KMS AWS i wybrać go zamiast klucza domyślnego.

Domyślne szyforwanie lambdy - The sensitive information would still be visible to other users who have access to the Lambda console.


#### Lambda + API Getaway
W tym scenariuszu, w jaki sposób można chronić systemy zaplecza platformy przed skokami ruchu?

    API Throttling

Usługa Amazon API Gateway zapewnia ograniczanie przepustowości na wielu poziomach, w tym na poziomie globalnym i serwisowym. Limity dławienia można ustawić dla standardowych stawek i serii. Na przykład właściciele API mogą ustawić limit prędkości 1000 żądań na sekundę dla określonej metody w swoich interfejsach API REST, a także skonfigurować bramę API Amazon, aby obsługiwała serię 2000 żądań na sekundę przez kilka sekund. Usługa Amazon API Gateway śledzi liczbę żądań na sekundę. Każde żądanie przekraczające limit otrzyma odpowiedź 429 HTTP. Pakiety SDK klienta generowane przez Amazon API Gateway ponawiają automatycznie połączenia po spełnieniu tej odpowiedzi. Dlatego opcja 2 jest prawidłową odpowiedzią.

Możesz dodać buforowanie do wywołań API, udostępniając pamięć podręczną bramy API Amazon i określając jej rozmiar w gigabajtach. Pamięć podręczna jest udostępniana na określonym etapie interfejsów API. Poprawia to wydajność i zmniejsza ruch wysyłany do zaplecza. Ustawienia pamięci podręcznej pozwalają kontrolować sposób budowania klucza pamięci podręcznej oraz czas życia (TTL) danych przechowywanych dla każdej metody. Usługa Amazon API Gateway udostępnia także interfejsy API do zarządzania, które pomagają unieważnić pamięć podręczną dla każdego etapu.

## VPCs

### CIDR Block

CIDR block of 172.0.0.0/27, with a netmask of /27, has an equivalent of 27 usable IP addresses. Take note that a netmask of /27 originally provides you with 32 IP addresses but in AWS, there are 5 IP addresses that are reserved which you cannot use

#### CIDR - IP

Wymagane jest, aby zezwolić tylko na indywidualny adres IP klienta, a nie na całą sieć. Dlatego należy zastosować poprawną notację CIDR. / 32 oznacza jeden adres IP, a / 0 odnosi się do całej sieci. Dlatego opcja 3 jest niepoprawna, ponieważ zezwala na całą sieć zamiast jednego adresu IP.


### Subnets ideal-solutions
In which subnet should you launch the new database server

    Private 


### ACLs

Zauważyłeś, że przychodzi wiele skanów portów z określonego bloku adresu IP, które próbują połączyć się z !!KILKOMA zasobami AWS w twoim VPC.

    Network ACLs

***NIE Security Group - na poziomie instancji, a nie na całym VPC.



### Redshift Enhanced VPC Routing
Amazon Redshift Enhanced VPC Routing, Amazon Redshift forces all COPY and UNLOAD traffic between your cluster and your data repositories through your Amazon VPC.





## Elastic File System


#### EFS as a POSIX-compliant
Your manager noticed that the system performance is quite slow and he has instructed you to improve the architecture of the system.

In this scenario, what will you do to implement a scalable, high throughput POSIX-compliant file system?

    EFS

## Elasticsearch

A traffic monitoring and reporting application uses Kinesis to accept real-time data. In order to process and store the data, they used Amazon Kinesis Data Firehose to load the streaming data to various AWS resources.  Which of the following services can you load streaming data into?

    ELASTICSEARCH
    
    
## SQS

#### Deleting messages by consumer-component
EC2 that consumes messages from an SQS queue and is integrated with SNS to send out an email to you once the process is complete. You received 5 orders but after a few hours, you saw more than 20 email notifications in your inbox.

    Web app not deleting messages

Zawsze pamiętaj, że komunikaty w kolejce SQS będą istniały nawet po przetworzeniu go przez instancję EC2, dopóki nie usuniesz tego komunikatu. 

Refer to the third step of the SQS Message Lifecycle:

1. Component 1 sends Message A to a queue, and the message is distributed across the Amazon SQS servers redundantly.

2. When Component 2 is ready to process a message, it consumes messages from the queue, and Message A is returned. While Message A is being processed, it remains in the queue and isn't returned to subsequent receive requests for the duration of the visibility timeout. 

3. Component 2 deletes Message A from the queue to prevent the message from being received and processed again once the visibility timeout expires.

## ECS

ECS. Jako architekt rozwiązań musisz upewnić się, że poświadczenia są bezpieczne i że nie można ich wyświetlić w postaci zwykłego tekstu w samym klastrze.

Amazon ECS umożliwia wstrzykiwanie wrażliwych danych do kontenerów poprzez przechowywanie wrażliwych danych w tajemnicach AWS Secrets Manager lub AWS Systems Manager Parameter Store, a następnie przywoływanie ich w definicji kontenera. Ta funkcja jest obsługiwana przez zadania korzystające zarówno z typów uruchamiania EC2, jak i Fargate.