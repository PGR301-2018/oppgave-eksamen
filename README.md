# Eksamensoppgave 

Egenutviklet applikasjon (eller prototype) med tilhørende dokumentasjon: teller 100% av karakteren i emnet. Applikasjonen skal være utviklet, og være vedlikeholdbar, i et DevOps- miljø i skyen. Kildekode, og annen dokumentasjon, 
skal gjøres tilgjengelig for allmenheten.



## Krav

* Applikasjonen og tilhørende DevOps infrastruktur skal gjøres tilgjenglig i offentlige GitHib repositories
* 

## Krav til applikasjonen (10%)

Applikasjonen er ikke det viktigste elementet, men for å demonstrere DevOps ferdigheter må den bestå av både applikasjon 
og database. 

* Applikasjonen skal bygge med Maven eller Gradle
* Applikasjonen skal ha enhetstester
* Applikasjonen skal ha integrasjonstester
* Dersom noen av testene feiler, skal maven- eller gradle bygget også feile 

Applikasjonen skal være skrevet på en slik måte at drift og vedlikehold er enkelt og i henhold til prinsipper i [the twelve factor app]](https://12factor.net/)
De viktigste prinsippene og overholde her ; 
 
- V. Build, release, run.
- III Config. Ignen hemmeligheter eller konfigurasjon i applikasjonen (ingen config filer med passord/brukere/URLer osv)
- X Dev/Prod parity - Applikasjonen skal kjøre i like miljøer uavhengig av 

## Pipeline (20%)

Det skal eksistere en CI/CDpipeline for applikasjonen som tilfredstiller kravene 

* Pipeline skal implementeres med Concourse
* Pipeline skal ha stegene *CI*, *Stage* & *Prod*
* Pipeline skal deploye til CI på hver commit som gjøres mot Master Branch i applikasjons-repo
* Pipeline skal deploye til Stage  


* Ett bygg (artifakt) for alle miljøer.  
* Repeterbar deployment prosess
* Smoke tests 


## Evalueringskriterier

### Infrastruktur (20%)

Nødvendig infrastruktur skal i så stor grad som mulig opprettes med Terraform. 


### Dokumentasjon (20%)

Det må være mulig for foreleser å følge instruksjoner i README filer for å få opp applikasjon og pipeline. 

# Praktisk 


## Tekniske krav

* Pipeline skal implementeres med Concourse
* Det vil være enklest for dere å implementere løsningen basert på Heroku, siden øvingene vil gi en rask start. Men, dere  står fritt til å velge andre Cloud leverandører, så lenge øvrige krav opprettholdes
Som en del av prosessen vil jeg comitte kode

## Overlevering 

* Under evalueringen vil jeg 
