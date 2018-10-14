# Eksamensoppgave 

Egenutviklet applikasjon (eller prototype) med tilhørende dokumentasjon: teller 100% av karakteren i emnet. Applikasjonen skal være utviklet, og være vedlikeholdbar, i et DevOps- miljø i skyen. Kildekode, og annen dokumentasjon, 
skal gjøres tilgjengelig for allmenheten.

## Krav

* Applikasjonen og tilhørende DevOps infrastruktur skal gjøres tilgjenglig i offentlige GitHib repositories
* Det skal lages to repositories, ett til infrastruktur (concourse + terraform) og ett for applikasjonen
* Det må være mulig å identifsere studentnavnet ut ifra repositorynavn

## Applikasjon (20%)

Applikasjonen er ikke det viktigste elementet, men må være innholdsrik nok demonstrere DevOps ferdigheter

* Applikasjonen skal bestå av både kode og database. Minimalt et REST API med CRUD kapabilitet.   
* Applikasjonen skal bygge med Maven 
* Applikasjonen skal ha enhetstester
* Dersom noen av testene feiler, skal maven- eller gradle bygget også feile 

Applikasjonen skal være skrevet på en slik måte at drift og vedlikehold er enkelt og i henhold til prinsipper i [the twelve factor app]](https://12factor.net/)

De viktigste prinsippene og overholde her ; 
 
* III Config. Ignen hemmeligheter eller konfigurasjon i applikasjonen (ingen config filer med passord/brukere/URLer osv) 

### Infrastruktur (40%)

* Det skal lages miljøer for CI, Stage og Prod
* Nødvendig infrastruktur skal i så stor grad som mulig opprettes med Terraform. 
* GitHub repositories kan opprettes manuelt, ikke bry dere med Terraform Github provider
* Terraformkoden skal kjøres av CI/CD verktøy (concourse) og ikke kjøres manuelt. 

De viktigste [12 factor prinsippene](https://12factor.net/) og overholde her ; 

* X Dev/Prod parity - Applikasjonen skal kjøre på *identisk kondfigurert* infrastruktur i alle miljløer (utviklking, stage, prod)

## Pipeline (40%)

Det skal eksistere en CI/CDpipeline for applikasjonen som tilfredstiller kravene 

* Det skal være en jobb  som heter "infra" som oppretter nødvemndig infrastruktur ved hjelp av terraform-kode
* Pipeline skal implementeres med Concourse
* Pipeline skal deploye master branch til CI miljø på hver commit i applikasjons-repo
  
De viktigste faktorene å overholde her ; 

* Repeterbar deployment prosess
* Smoke tests 

De viktigste [12 factor prinsippene](https://12factor.net/) og overholde her ; 

* IV, Build, Release, run  - Bygg, Relase og kjøring av applikasjon er å betrakte som distinkte steg. 


# Praktisk 

## Tekniske krav


## Overlevering 

Under evalueringen vil jeg gjøre følgende; (Subject to change)

Installasjon 

* Lage git fork av dine repositories 
* Kjøre mvn install i <app repo>
* Sette miljøvariable i henhold til README filer i <infra repo>/README.md
* Kjøre terraform apply i <infra repo>/terraform/ mappen 
* Lage deploy keys for mine fork repositories
* Lage credentials.yml med deploy keys, Heroku API keys og Github Personal access token
* Kjøre fly set-pipeline på <infra repo>/concourse/pipeline.yml i mitt eget Concoursemiljø. 
* Utføre ekstra instruksjoner <infra repo>/README.md

Verifikasjon

* Endre kode og lage pull-request mot <app repo>, godkjenne / merge 

Promotere / demotere 

* Promotere bygg til Stage/Prod og verifisere at applikasjonen fortsatt fungerer








