# Eksamensoppgave  

## Fra Læreplan

"Egenutviklet applikasjon (eller prototype) med tilhørende dokumentasjon: teller 100% av karakteren i emnet. Applikasjonen skal være utviklet, og være vedlikeholdbar, i et DevOps- miljø i skyen. Kildekode, og annen dokumentasjon, skal gjøres tilgjengelig for allmenheten."

Siden et kontinuerlig kjørende DevOps-miljø kan være kostnadsbærende for studentene vil vi lette på kravet om miljøet skal kjøre kontinuerlig i skyen. Applikasjonen må istedet være mulig å etablere i skyen ved hjelp av infrastruktur som kode (Terraform) - og enkel automatisering.

På den måten kan de som måtte ønske å ta løsningen i bruk ( for eksempel eksaminator ) bruke egen infrastruktur (og da ta kostnaden for drift av miljøet)

## Krav

Applikasjonen og tilhørende DevOps infrastruktur skal gjøres tilgjenglig i offentlige GitHib repositories

Det skal lages minst to repositories,

* Ett til infrastruktur (concourse + terraform)
* Ett for en eksempel-applikasjon.
* N antall for andre applikasjoner eller komponenter

Studentene skal sende lenke til disse repositoriene til eksaminator. Dette er den eneste innleveringen som skal gjøres.

## Applikasjon

Applikasjonen må væreinnholdsrik nok demonstrere DevOps-prinisipper og bevise ferdigheter hos studenten.

* Applikasjonen skal bestå av både av et API og en database. Minimalt et REST API med CRUD kapabilitet.   
* Databasen kan være "in Memory" (h2 osv)- Dvs. Dere trenger ikke tenke på å sette opp database-infrastruktur.
* Applikasjonen skal bygge med Maven
* Applikasjonen skal ha enhetstester
* Dersom noen av testene feiler, skal bygget også feile

Applikasjonen skal være skrevet på en slik måte at drift og vedlikehold er enkelt og i henhold til prinsipper i [the twelve factor app]](https://12factor.net/)

De viktigste prinsippene og overholde er ;

* III Config. Ignen hemmeligheter eller konfigurasjon i applikasjonen (ingen config filer med passord/brukere/URLer osv)
* IV, Build, Release, run  - Bygg, Relase og kjøring av applikasjon er å betrakte som distinkte steg.
* XI Logs. Applikasjonen skal bruke et rammeverk for logging, og logge til Stdout (System.out i Java)
* X Dev/Prod parity - Applikasjonen skal kjøre på *identisk konfigurert* infrastruktur i alle miljløer (utviklking, stage, prod)

## Krav  Infrastruktur

* Det skal  tre miljøer

- CI (Contuinous integraiton) - Master branch i applikasjon-repository skal til enhver tid installeres i dette miljøet.   
- Stage - Dette er et miljø som typisk brukes for tester, for eksmpel ytelses- eller sikkerhetstester.
- Prod - Dette er miljøet som kundene- eller brukerene av løsningen opplever.

* Nødvendig infrastruktur skal så langt det lar seg gjøre opprettes med Terraform. Det skal ikke være nødvendig å for eksaminator å ha terraform installert på PC for å etablere infrastrukturen - terraformkoden skal kjøres av CI/CD verktøy (concourse) og ikke kjøres manuelt.

## Krav til DevOps Pipeline

Det skal eksistere en CI/CDpipeline for applikasjonen.

* Pipeline skal implementeres med Concourse
* Det skal være en concourse jobb som heter "infra" som oppretter nødvemndig infrastruktur ved hjelp av terraform-kode
* Pipeline skal kontinuerlig deploye master branch til "CI miljø"" på hver commit mot applikasjons-repository master.

## Evauluering

Karakter settes ved at studenten utvider pipeline gitt som eksempel med minst en ekstra modul. Studentene velger selv hvilke moduler de vil levere.

* 10 poeng (E)
* 20 poeng (D)
* 30 poeng (C)
* 40 poeng (B)
* 50 poeng (A)

* Hvis en oppgave er løst, men ikke 100% korrekt kan det gis delvis poeng. For eksempel; En student implementerer overvåkning og metrics. Koden er rikitg skrevet, men feil i konfigurasjon gjør at logging ikke gjøres riktig. Dette kan eksmpelvis gi 7/10 poeng på overvåkning.

## Overlevering - og hva eksaminator gjør  

Du har to alternativer for innlevering.

* Du koder "offentlig" og lager public Github repositories. Jeg vil lage forks av disse.
* Du koder "privat" og inviterer github bruker *glennbech* som collaborator med lesetilgang. Jeg lager så  forks av disse.

Det stilles følgende krav til leveransen

* Leveransen skal inneholde en credentials_example.yml som eksemplifiserer nødvendige hemmeligheter som er nødvendig (github_tokens, deploy keys, api keys til diverse tjenester osv).
* Eksaminator døper om filen credentials_example.yml til credentials.yml og legger inn sine hemmeligheter  
* Eksaminator utføre ekstra instruksjoner <infra repo>/README.md - (så lite som mulig).
* Eksaminator vil deretter kjøre ```fly -t (min target) set-pipeline <infra repo>/concourse/pipeline.yml -l <infra repo>/credentials.yml -p student_name``` i sitt eget Concourse-miljø.
*

## Oppgaver

# Docker (20 poeng)

I vår basis-pipeline gjør Heroku et bygg av koden på hver commit til master. Heroku bygger en "slug" som er en binær leveransepakke som inneholder din applikasjon, og annen nødvendig programvare. Dette ivaretar prinsippet om ett bygg, av en artifakt som flyttes mellom miljøer.

Heroku slugs er derimot ikke en standardisert måte å pakke applikasjoner på. Hvis man ønsker å bruke en annen sky-leverandør en Herku, må man endre måten man pakker og installerer applikasjoner på.

Docker, og containere støttes av de fleste public cloud leverandører og hvis man lager en container av applikasjonen man bygger har man stor fleksibilitet og utvalg av platformer.

Denne oppgaven består av følgende;

* Du skal lage en Concourse pipeline, som bygger et Docker Image, som lastes opp i en Container Registry. Heroku støtter dette, så man kan gjøre denne oppgaven uten å bruke en annen skyleverandør.
* En concourse jobb skal sørge for at hver commit til master branch i applikasjon skal resultere i en ny Docker image versjon
* En  concourse jobb skal sørge for at siste versjon av Docker image installeres i et CI miljø.
* En concourse jobb skal sørge for at docker images som vellykket har blitt installert i CI miljøet installeres i et produksjonsmiljø-

# Overvåkning, varsling og Metrics (20 poeng)

Denne oppgaven består av å implementere overvåkning, og varsling.

Krav til oppgaven;

- Applikasjonen skal logge egendefinerte metrics / custom metrics til en database som influxdb/carbon
- Med egendefinerte metrics menes datapunkter generert av egen skrevet kode i applikasjonen. Det kan for eksempel være
- Metrics skal vises visuelt i et dashboard for eksempel Grafana
- Infrastruktur skal i så stor grad som mulig opprettes fra terraform
- Dashboards, grafer osv skal også i så stor grad som mulig opprettes fra terraform
- Applikasjonen sine endepunkter skal overvåkes slik at problemer oppdages raskt. Ved avbrudd og feil skal det varlses til en eller flere kanaler (epost, lack, telefon etc)

Oppgaven kan enten løses ved å finne en PAAS løsning  

* Heroku m/ Heroku "hosted graphite" addon (gratis alternativ)
* Dersom ikke heroku, hosted graphite direkte (free trial)

Eller IAAS

* Studenten oppretter et miljø for Overvåkning, Metrics og varsling på AWS/GCP ved
hjelp av Terraform

OBS. Graden av automatisering. Altså hvor mye som opprettes fra Terraform påvirker
poengsetting i denne oppgaven.

# Applikasjonslogger (10 poeng)

Denne oppgaven består av å bruke en tjeneste for log-innsamling, og utvide applikasjonen på en slik
måte at logger sendes til denne tjenesten.

Krav til tjenesten

* Man skal kunne se- og søke logger i et  Dashboards

Eksempel på løsning;

* https://app.logz.io/ - ELK (ElasticSearch, Logstash, Kibana) as a Service.

# Ekstra Public Cloud provider & Serverless (10 poeng)

Denne oppgaven består av å ta i bruk en annen skyleverandør en Heroku for all infrastruktur. Studentene kan velge mellom;

* Google Cloud platform
* Amazon Web Services

All infrastruktur skal også her i så stor grad som mulig opprettes via Terraform provider for respektive skyleverandør.

Studentene  bør etterstrebe å holde seg innenfor gratis bruk av tjenester.

# Serverless (10 Poeng)

* Det skal leveres en Google Cloud Function, eller AWS Lambda function.
* Bygging, og deployment av Serverless-komponenten skal gjøres på samme måte som annen kode ved hjelp av concourse.
