# Eksamensoppgave 

Fra Læreplan 

"Egenutviklet applikasjon (eller prototype) med tilhørende dokumentasjon: teller 100% av karakteren i emnet. Applikasjonen skal være utviklet, og være vedlikeholdbar, i et DevOps- miljø i skyen. Kildekode, og annen dokumentasjon, 
skal gjøres tilgjengelig for allmenheten."

Siden et kontinuerlig kjørende DevOps-miljø kan være kostnadsbærende for studentene vil vi lette på kravet om miljøet skal kjøre kontinuerlig i skyen. Applikasjonen må istedet være mulig å etablere i skyen ved hjelp av infrastruktur som kode (Terraform) - og enkel automatisering. På den måten kan de som måtte ønske å ta løsningen i bruk ( for eksempel eksaminator ) bruke egen infrastruktur (og da ta kostnaden for drift av miljøet)


## Krav

Applikasjonen og tilhørende DevOps infrastruktur skal gjøres tilgjenglig i offentlige GitHib repositories

Det skal lages to repositories, 
* Ett til infrastruktur (concourse + terraform) 
* Ett for applikasjonen. 

Studentene skal sende nanv på disse repositoriene til eksaminator. Dette er den eneste innleveringen som skal gjøres.

## Applikasjon (20%)

Applikasjonen er ikke det viktigste elementet, men må være innholdsrik nok demonstrere DevOps ferdigheter og teknologi

* Applikasjonen skal bestå av både kode og database. Minimalt et REST API med CRUD kapabilitet.   
* Applikasjonen skal bygge med Maven 
* Applikasjonen skal ha enhetstester
* Dersom noen av testene feiler, skal maven- eller gradle bygget også feile 

Applikasjonen skal være skrevet på en slik måte at drift og vedlikehold er enkelt og i henhold til prinsipper i [the twelve factor app]](https://12factor.net/)

De viktigste prinsippene og overholde her ; 
 
* III Config. Ignen hemmeligheter eller konfigurasjon i applikasjonen (ingen config filer med passord/brukere/URLer osv) 
* XI Logs. Applikasjonen skal bruke et rammeverk for logging, og logge til Stdout (System.out i Java)

_her vil det komme krav relatert til telemetri og overvåkning_

### Infrastruktur (40%)

* Det skal lages miljøer for CI, Stage og Prod
* Nødvendig infrastruktur skal opprettes med Terraform. Det skal ikke være nødvendig å for eksaminator å ha terraform installert på PC for å etablere infrastrukturen.  
* GitHub repositories kan opprettes manuelt, ikke bry dere med Terraform Github provider
* Terraformkoden skal kjøres av CI/CD verktøy (concourse) og ikke kjøres manuelt. 

De viktigste [12 factor prinsippene](https://12factor.net/) og overholde her ; 

* X Dev/Prod parity - Applikasjonen skal kjøre på *identisk konfigurert* infrastruktur i alle miljløer (utviklking, stage, prod)

## Pipeline (40%)

Det skal eksistere en CI/CDpipeline for applikasjonen som tilfredstiller kravene 

* Pipeline skal implementeres med Concourse
* Det skal være en jobb  som heter "infra" som oppretter nødvemndig infrastruktur ved hjelp av terraform-kode
* Pipeline skal deploye master branch til CI miljø på hver commit i applikasjons-repo

De viktigste faktorene å overholde her ; 

* Repeterbar deployment prosess

De viktigste [12 factor prinsippene](https://12factor.net/) og overholde her ; 

* IV, Build, Release, run  - Bygg, Relase og kjøring av applikasjon er å betrakte som distinkte steg. 


# Praktisk 

## Tekniske krav

## Overlevering 

Under evalueringen vil jeg gjøre følgende; (Subject to change)

Installasjon 

* Lage git fork av dine repositories 
* Kjøre mvn install i <app repo> for sjekke at tester kjører og app kompilerer
* Sette miljøvariable i henhold til README filer i <infra repo>/README.md 
* Lage deploy keys for mine fork repositories
* Lage credentials.yml med deploy keys, Heroku API keys og Github Personal access token for min bruker
* Utføre ekstra instruksjoner <infra repo>/README.md - så lite som mulig.
* Kjøre fly set-pipeline på en fil som skal hete <infra repo>/concourse/pipeline.yml i mitt eget Concoursemiljø. 
* Kjøre Infra-jobb i concourse 

Verifikasjon

* Endre kode og lage pull-request mot <app repo>, godkjenne / merge mot master
* Verifisere at applikasjonen bygges og deployes i ny versjon til CI miljøet
 
Promotere / demotere 

* Promotere bygg til Stage/Prod og verifisere at applikasjonen fortsatt fungerer









