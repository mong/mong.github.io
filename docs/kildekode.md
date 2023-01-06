# Forvaltning av kildekode 

Kildekode til alle egenutviklede komponenter ligger åpent på [github](https://github.com/mong) med lisensen [GNU General Public License 3](https://www.gnu.org/licenses/gpl-3.0.en.html). 

I alle depoene (*repositories*), bortsett fra `mongts`, er det én hovedgren: `main`. Denne grenen skal alltid være klar for å deploye til produksjon. Når en ny versjon lages i github, brukes [Semantic Versioning](https://semver.org/) for å navngi versjonen. Når en ny versjon lages, vil denne automatisk deployes til produksjon.

I depoet [mongts](https://github.com/mong/mongts) er det to hovedgrener: `main` og `develop`. Alle *commits* til `develop` deployes til [QA-miljøet](https://test.skde.no/), mens *commits* til `main` deployes til [produksjon](https://www.skde.no/). Grenen `develop` merges med ujevne mellomrom inn i `main`. Tekstlige endringer gjort gjennom vår *CMS* sjekkes rett inn til `main`.

Grenen `main` (og `develop`) er i alle depoer beskyttet for direkte commits. Alle endringer må gjennom en *pull request* for å komme inn i `main`. For at kode skal komme inn i `main` må ingen av testene feile og koden må gjennom en «code review» (fagfellevurdering) av ett av medlemmene på [mong](https://github.com/mong).

[Medlemmer av *mong* på github](https://github.com/orgs/mong/people) må ha aktivert to-faktor-autentisering (2FA) for å kunne bli og være medlem, og dette er et krav som er satt i instillingene på github. Kun medlemmer av gruppen kan dytte opp endringer direkte til en gren på https://github.com/mong. Unntaket er `main`-grenen, der kun administratorene kan dytte opp endringer direkte.  Det er kun to av medlemmene som har administratorrettigheter (Are og Arnfinn). Andre, som ikke er medlemmer av `mong`, må lage en *fork* av koden og foreslå endringer til `main` gjennom denne *forken*.

## Repositories aktivt i bruk

- [mongts](https://github.com/mong/mongts): Selve nettsiden (front-end) skrevet i javascript/typescript og React. I tillegg API for kommunikasjon mellom nettside og database, i Node.js.
- [tmongr](https://github.com/mong/tmongr): Koden bak undersiden `/pasientstrommer`. Kodet i R og Shiny. Leveres i et docker image som brukes av *EC2*-instanser snurret opp av *Elastic Beanstalk* på *AWS*.
- [tmongr-base-r](https://github.com/mong/tmongr-base-r): Docker image som brukes av `tmongr` docker image. Inneholder nødvendige R-pakker, som brukes av `tmongr`. Dette gjør det kjappere å bygge `tmongr` sitt docker image.
- [imongr](https://github.com/mong/imongr): Koden bak nettside for å legge inn data til databasen. Kodet i R og Shiny. Leveres i et docker image som brukes av EC2-instanser på AWS.
- [lb-rp](https://github.com/mong/lb-rp): Lastbalanserer og reverse proxy, basert på NGINX og levert som et docker image, som står foran shinyproxy.
- [shinyproxy](https://github.com/mong/shinyproxy). Levert som docker image. Kjører shinyproxy (https://www.shinyproxy.io/) for å serve shiny-appen imongr.
- [db](https://github.com/mong/db): 
- [imongr-base-r](https://github.com/mong/imongr-base-r): Docker image som brukes av imongr docker image. Inneholder nødvendige R-pakker, som brukes av imongr. Dette gjør det kjappere å bygge imongr sitt docker image.

## Aktive repositories som skal ut av produksjon

- [tmongr](https://github.com/mong/tmongr) og [tmongr-base-r](https://github.com/mong/tmongr-base-r): Shiny-applikasjonen bak tabellverket. Kan på sikt bakes inn i [mongts](https://github.com/mong/mongts).

## Eksterne komponenter

### mongts og dens støtteprogrammer

Koden [mongts](https://github.com/mong/mongts) er avhengig av følgende [eksterne bibliotek](https://github.com/mong/mongts/network/dependencies): 

- [TypeScript](https://github.com/microsoft/TypeScript). Store deler av koden er skrevet i Typescript, for å redusere sannsynligheten for bugs. Utviklet og utvikles av Microsoft og ekstremt utbredt. 
- [React](https://github.com/facebook/react). qmongjs er en React-app. Utviklet og utvikles av Facebook og ekstremt utbredt. 
- [Sentry](https://github.com/getsentry/sentry-javascript) for automatisk rapportering av feil som oppstår under bruk av nettside.  
- [D3](https://github.com/d3/d3) for visualisering/figurer.
- [react-query](https://github.com/tannerlinsley/react-query) 
- ...

### imongr og dens støtteprogrammer

Koden [imongr](https://github.com/mong/imongr) er avhengig av [følgende eksterne R-pakker](https://raw.githubusercontent.com/mong/imongr/main/DESCRIPTION):
- [digest](https://github.com/eddelbuettel/digest)
- [dplyr](https://github.com/tidyverse/dplyr/)
- [DT](https://github.com/rstudio/DT)
- [lifecycle](https://github.com/r-lib/lifecycle)
- [magrittr](https://github.com/tidyverse/magrittr/)
- [pool](https://github.com/rstudio/pool)
- [readr](https://github.com/tidyverse/readr/)
- [rlang](https://github.com/r-lib/rlang)
- [RMariaDB](https://github.com/r-dbi/RMariaDB)
- [shiny](https://github.com/rstudio/shiny)
- [shinyalert](https://github.com/daattali/shinyalert)
- [shinycssloaders](https://github.com/daattali/shinycssloaders/)
- [shinyjs](https://github.com/daattali/shinyjs)
- [tibble](https://github.com/tidyverse/tibble/)
- [yaml](https://github.com/viking/r-yaml/)
