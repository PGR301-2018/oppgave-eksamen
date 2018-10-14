# Eksamensoppgave 

Egenutviklet applikasjon (eller prototype) med tilhørende dokumentasjon: teller 100% av karakteren i emnet. Applikasjonen skal være utviklet, og være vedlikeholdbar, i et DevOps- miljø i skyen. Kildekode, og annen dokumentasjon, 
skal gjøres tilgjengelig for allmenheten.



## Krav

* Applikasjonen og tilhørende DevOps infrastruktur skal gjøres tilgjenglig i offentlige GitHib repositories
* Det skal lages to repositories, ett til infrastruktur (concourse + terraform) og ett for applikasjonen

## Krav til applikasjonen (20%)

Applikasjonen er ikke det viktigste elementet, men må være innholdsrik nok demonstrere DevOps ferdigheter

* Applikasjonen skal bestå av både kode og database. Lag gjerne et REST API med CRUD kapabilitet av noe slag  
* Applikasjonen skal bygge med Maven eller Gradle
* Applikasjonen skal ha enhetstester
* Dersom noen av testene feiler, skal maven- eller gradle bygget også feile 

Applikasjonen skal være skrevet på en slik måte at drift og vedlikehold er enkelt og i henhold til prinsipper i [the twelve factor app]](https://12factor.net/)

De viktigste prinsippene og overholde her ; 
 
* III Config. Ignen hemmeligheter eller konfigurasjon i applikasjonen (ingen config filer med passord/brukere/URLer osv) 

### Infrastruktur (30%)

* Det skal eksistere miljøer for CI, Stage og Prod
* Nødvendig infrastruktur skal i så stor grad som mulig opprettes med Terraform. 
* GitHub repositories kan opprettes manuelt, ikke bry dere med Terraform Github provider

De viktigste [12 factor prinsippene](https://12factor.net/) og overholde her ; 

* X Dev/Prod parity - Applikasjonen skal kjøre på *identisk kondfigurert* infrastruktur i alle miljløer (utviklking, stage, prod)

## Pipeline (30%)

Det skal eksistere en CI/CDpipeline for applikasjonen som tilfredstiller kravene 

* Pipeline skal implementeres med Concourse
* Pipeline skal deploye master branch til CI miljø på hver commit i applikasjons-repo
* Pipeline skal deploye til Stage  

* Ett bygg (artifakt) for alle miljøer.  
* Repeterbar deployment prosess
* Smoke tests 

### Dokumentasjon (20%)

Det må være mulig for foreleser å følge instruksjoner i README filer for å få opp applikasjon og pipeline. 

# Praktisk 


## Tekniske krav

* Pipeline skal implementeres med Concourse
* Det vil være enklest for dere å implementere løsningen basert på Heroku, siden øvingene vil gi en rask start. Men, dere  står fritt til å velge andre Cloud leverandører, så lenge øvrige krav opprettholdes
Som en del av prosessen vil jeg comitte kode

## Overlevering 

* Under evalueringen vil jeg 
