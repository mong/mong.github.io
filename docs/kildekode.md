# Forvaltning av kildekode 

Kildekode til alle egenutviklede komponenter ligger åpent på [github](https://github.com/mong) med lisensen [GNU General Public License 3](https://www.gnu.org/licenses/gpl-3.0.en.html). 

I alle repositories er det én hovedgren: master. Denne grenen skal alltid være klar for å deployes til produksjon. Hver commit til master deployes til QA-miljø, som er identisk med produksjonsmiljø. Når en ny versjon lages i github, brukes [Semantic Versioning](https://semver.org/) for å navngi versjonen. Når en ny versjon lages, vil denne automatisk deployes til produksjon. 

Grenen ´master´ er i alle *repositories* beskyttet for direkte commits. Alle endringer må gjennom en *pull request* for å komme inn i ´master´. For at kode skal komme inn i ´master´ må ingen av testene feile og koden må gjennom en *code review* av ett av medlemmene på https://github.com/mong. 

Det er et krav til medlemmer av mong på github at de har aktivert 2FA (krav gjennom innstillinger). Det er kun to av medlemmene som har administratorrettigheter (Are og Arnfinn). Kun medlemmer av gruppen kan dytte opp endringer direkte til en branch på https://github.com/mong. Andre må lage en fork av koden og foreslå endringer til master gjennom denne forken. 

## Følgende repositories er aktivt i bruk: 

- [qmongjs](https://github.com/mong/qmongjs): Selve nettsiden (front-end) skrevet i javascript/typescript og bruker React.  

- [mong-api](https://github.com/mong/mong-api): API for kommunikasjon mellom nettside og database. 

- [imongr](https://github.com/mong/imongr): Koden bak nettside for å legge inn data til databasen. Kodet i R og Shiny. Leveres i et docker image som brukes av EC2-instanser på AWS. 

- [lb-rp](https://github.com/mong/lb-rp): Lastbalanserer og reverse proxy, basert på NGINX og levert som et docker image, som står foran shinyproxy. 

- [shinyproxy](https://github.com/mong/shinyproxy). Levert som docker image. Kjører shinyproxy (https://www.shinyproxy.io/) for å serve shiny-appen imongr. 

- [db](https://github.com/mong/db) 

- [imongr-base-r](https://github.com/mong/imongr-base-r): Docker image som brukes av imongr docker image. Inneholder nødvendige R-pakker, som brukes av imongr. Dette gjør det kjappere å bygge imongr sitt docker image. 

### Repositories som skal ut av produksjon: 

- [qmongr](https://github.com/mong/qmongr) og [qmongr-base-r](https://github.com/mong/qmongr-base-r): Shiny-applikasjonen bak https://skde-resultater.no/. Denne skal erstattes av qmongjs. 

- [tmongr](https://github.com/mong/tmongr) og [tmongr-base-r](https://github.com/mong/tmongr-base-r): Shiny-applikasjonen bak tabellverket. Skal på sikt bakes inn i qmongjs.
