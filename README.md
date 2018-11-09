# Eksamensoppgave   (NB. NB. Ikke å anse som utlevert før beskjed blir gitt, og tidligst mandag 11. November)

## Fra Læreplan

"Egenutviklet applikasjon (eller prototype) med tilhørende dokumentasjon: teller 100% av karakteren i emnet. Applikasjonen skal være utviklet, og være vedlikeholdbar, i et DevOps- miljø i skyen. Kildekode, og annen dokumentasjon, skal gjøres tilgjengelig for allmenheten."

Siden et kontinuerlig kjørende DevOps-miljø kan være kostnadsbærende for studentene vil vi lette på kravet om miljøet skal kjøre kontinuerlig i skyen. Applikasjonen må istedet være mulig å etablere i skyen ved hjelp av infrastruktur som kode (Terraform) - og enkel automatisering.

På den måten kan de som måtte ønske å ta løsningen i bruk ( for eksempel eksaminator ) bruke egen infrastruktur (og da ta kostnaden for drift av miljøet)

## Krav

Applikasjonen og tilhørende DevOps infrastruktur skal gjøres tilgjenglig i offentlige GitHib repositories.

Det skal lages minst to repositories,

* Ett til infrastruktur (concourse + terraform)
* Ett for en eksempel-applikasjon.
* N antall for andre applikasjoner eller komponenter

Studentene skal sende lenke til disse repositoriene til eksaminator. Dette er den eneste innleveringen som skal gjøres.

## Applikasjon

Applikasjonen må være innholdsrik nok demonstrere DevOps-prinisipper og bevise ferdigheter hos studenten.

* Applikasjonen skal bestå av både av et API og en database. Minimalt et REST API med CRUD kapabilitet.   
* Databasen kan være "in Memory" (h2 osv)- Dvs. Dere trenger ikke tenke på å sette opp database-infrastruktur.
* Applikasjonen skal bygge med Maven
* Applikasjonen skal ha enhetstester
* Dersom noen av testene feiler, skal bygget også feile

Applikasjonen skal være skrevet på en slik måte at drift og vedlikehold er enkelt og i henhold til prinsipper i [the twelve factor app]](https://12factor.net/)

De viktigste prinsippene og overholde er ;

* III Config. Ignen hemmeligheter eller konfigurasjon i applikasjonen (ingen config filer med passord/brukere/URLer osv) - se på application.yml i eksempel-applikasjonene vi har laget i timene.

Pass spesielt godt på å ikke sjekke inn API nøkler, Github Personal access token, Deploy keys osv.

* XI Logs. Applikasjonen skal bruke et rammeverk for logging, og logge til Stdout (System.out i Java)

Bruk Logback, Log4j eller tilsvarende, ikke System.out.println();

* X Dev/Prod parity - Applikasjonen bør kunne kjøre i identisk infrastruktur i alle miljløer (utviklking, stage, prod, og lokal utviklingsmaskin)

## Krav til Infrastruktur

* Det skal opprettes tre identiske miljøer

- CI (Contuinous integraiton) - Master branch i applikasjon-repository skal til enhver tid installeres i dette miljøet.   
- Stage - Dette er et miljø som typisk brukes for tester, for eksmpel ytelses- eller sikkerhetstester.
- Prod - Dette er miljøet som kundene- eller brukerene av løsningen opplever.

* Nødvendig infrastruktur skal så langt det lar seg gjøre opprettes med Terraform. Det skal ikke være nødvendig å for eksaminator å ha terraform installert på PC for å etablere infrastrukturen - terraformkoden skal kjøres av CI/CD verktøy (concourse).

For Infrastruktur som ikke kan opprettes med Terraform, kan dere lage instruksjoner i README.md i <infra-repository>    

## Krav til DevOps Pipeline

Det skal lages en CI/CDpipeline for applikasjonen.

* Pipeline skal implementeres med Concourse
* Det skal være en concourse jobb som heter "infra" som oppretter nødvemndig infrastruktur ved hjelp av terraform-kode.
* Pipeline skal kontinuerlig deploye master branch til "CI miljø"" på hver commit mot master branch i applikasjons-repository.

* Strategi for Deployment fra CI-miljø videre til Stage og produksjon er valgfritt. Dere kan velge continous deployment, hvor hver commit automatisk flyter igjennom alle miljøene. Eller, dere kan implementere en strategi der man manuelt promoterer et bygg, slik vi har gjort med Heroku pipelines.


## Evauluering

Karakter settes ved at studenten utvider pipeline gitt i utlevering med minst en ekstra modul. Studentene velger selv hvilke moduler de vil levere, og implementasjon av en modul gir poeng.

* 10 poeng (E)
* 20 poeng (D)
* 30 poeng (C)
* 40 poeng (B)
* 50 poeng (A)

* Hvis en oppgave er løst, men ikke 100% korrekt kan det fortsatt gis poeng. Bruk README filen til å forklare hva som var hansikt å mål, dersom man ikke får til en oppgave.

## Overlevering - og hva eksaminator gjør  

Du har to alternativer for innlevering.

* Du koder "offentlig" og lager public Github repositories. Jeg vil lage forks av disse.
* Du koder "privat" og inviterer github bruker *glennbech* som collaborator med lesetilgang. Jeg lager så  forks av disse.

Det stilles følgende krav til leveransen

* Leveransen skal inneholde en credentials_example.yml som eksemplifiserer nødvendige hemmeligheter som er nødvendig (github_tokens, deploy keys, api keys til diverse tjenester osv).
* Eksaminator døper om filen credentials_example.yml til credentials.yml og legger inn sine egne hemmeligheter  
* Eksaminator utføre ekstra instruksjoner <infra repo>/README.md - (så lite som mulig).
* Eksaminator vil deretter kjøre ```fly -t (min target) set-pipeline <infra repo>/concourse/pipeline.yml -l <infra repo>/credentials.yml -p student_name``` i sitt eget Concourse-miljø.
* Eksaminator kjører "infra" jobben i Concourse
* Eksaminator committer kode på master branch for å teste pipeline

## Oppgaver

# Docker (40 poeng)

I vår basis-pipeline gjør Heroku et bygg av koden på hver commit til master. Heroku bygger en "slug" som er en binær leveransepakke som inneholder din applikasjon, og annen nødvendig programvare. Dette ivaretar prinsippet om ett bygg, av en artifakt som "flyter" mellom miljøer.

Heroku slugs er derimot ikke en standardisert måte å pakke applikasjoner på. Hvis man ønsker å bruke en annen sky-leverandør en Herku, må man endre måten man pakker og installerer applikasjoner på.

Docker, og containere støttes av de fleste public cloud leverandører og hvis man benytter containers, får man stor fleksibilitet og utvalg av platformer.

Denne oppgaven består av følgende;

* Du skal lage en Concourse pipeline, som bygger et Docker Image, som lastes opp i et Container Registry. Heroku støtter dette, så man kan gjøre denne oppgaven med Heroku.
* En concourse jobb skal sørge for at hver commit til master branch i applikasjon skal resultere i en ny Docker image versjon
* En  concourse jobb skal sørge for at siste versjon av Docker image installeres i et CI miljø.
* En concourse jobb skal sørge for at docker images som vellykket har blitt installert i CI miljøet installeres i et stage miljøer
* En concourse jobb skal kjøre minst en test mot appliksjonen i stage miljøet.
* En concourse jobb skal docker images som vellykket har passert Stage tester, blir installert i produksjonsmiljøet

# Overvåkning, varsling og Metrics (20 poeng)

Denne oppgaven består av å implementere overvåkning, og varsling.

Krav til oppgaven;

- Applikasjonen skal logge egendefinerte metrics. Med det menes at man på valgte steder i applikasjonskoden skal logge datapunkter.
- Metrics skal implementeres ved hjelp av Heroku Addon https://elements.heroku.com/addons/hostedgraphite
- Applikasjonen sine endepunkter skal også overvåkes slik at problemer oppdages raskt. Vi har gjort øvinger der vi har brukt StatusCake til dette

Oppgaven kan enten løses ved å finne en SAAS løsning, eller ved å opprette infrastruktur selv.

* Heroku m/ Heroku "hosted graphite" addon er et gratis alternativ.

# Applikasjonslogger (10 poeng)

Denne oppgaven består av å bruke en tjeneste for log-innsamling, og utvide applikasjonen på en slik
måte at logger sendes til denne tjenesten.

Eksempel på løsning;

* https://app.logz.io/ - ELK (ElasticSearch, Logstash, Kibana) as a Service.

# Ekstra Public Cloud provider (10 poeng)

Denne oppgaven består av å ta i bruk en annen skyleverandør en Heroku for all infrastruktur. Studentene kan velge mellom;

* Google Cloud platform
* Amazon Web Services

All infrastruktur skal også her i så stor grad som mulig opprettes via Terraform provider for respektive skyleverandør. Studentene  bør etterstrebe å holde seg innenfor gratis bruk av tjenester. Obs.


# Serverless (10 Poeng)

* Det skal leveres en Google Cloud Function, eller AWS Lambda function.
* Bygging, og deployment av Serverless-komponenten skal gjøres på samme måte som annen kode ved hjelp av concourse.
