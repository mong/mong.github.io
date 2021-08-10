# Utvikling av kode

## Dependabot

Vi bruker [Dependabot](https://docs.github.com/en/code-security/supply-chain-security/keeping-your-dependencies-updated-automatically/about-dependabot-version-updates) for å holde våre koder oppdatert. Dependabot kan sjekke versjoner i npm/yarn, Docker og Github Actions. Dependabot aktiveres ved å legge en `dependabot.yml`-fil i mappen `.github`.

Følgende eksempel på en `dependabot.yml`-fil ble brukt i `mong-api`:
```yml
version: 2
updates:

  # Maintain dependencies for npm
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "daily"
    target-branch: "dependabot_updates"
    open-pull-requests-limit: 20

  # Maintain dependencies for GitHub Actions
  - package-ecosystem: "github-actions"
    directory: "/"
    schedule:
      interval: "daily"
    target-branch: "dependabot_updates"

  # Maintain dependencies for Docker
  - package-ecosystem: "docker"
    directory: "/"
    schedule:
      interval: "daily"
    target-branch: "dependabot_updates"
```
Denne filen forteller Dependabot å sjekke om det er kommet nye versjoner av biblioteker både i `package.json` (npm), `Dockerfile` og alle Github Actions i `.github/workflows/` en gang om dagen. Hvis det er kommet nye versjoner vil Dependabot opprette en `PR` mot grenen `dependabot_updates`. Dette er gjort for å slippe å få veldig mange commits direkte inn til `master`/`main`. Fra tid til annen lages det en `PR` fra `dependabot_updates` til `master`.

`dependabot_updates`-grenen settes til å være beskyttet, men med *Allow force pushes* aktivert. Da vil den ikke bli slettet hver gang den er merget inn i master-grenen.

Etter at en `PR` fra `dependabot_updates` er kommet inn til `master` kan man rydde opp `dependabot_updates`-grenen ved å kjøre følgende kommandoer:

```bash
git checkout dependabot_updates # stå på dependabot_updates-grenen
git fetch origin master:master # oppdater master lokalt
git reset --hard master # dependabot_updates settes lik master
git push -f # dependabot_updates settes lik master også på github
```

## qmongjs og mong-api

Følgende må gjøres for å kunne kjøre opp `qmongjs` lokalt:

```bash
# Starte mysql for at mong-api skal få tak i data
sudo systemctl start mysqld # Start mysql server
# Kjøre mong-api:
cd mong-api
npm install # installer nødvendige pakker
npm start # start opp api på http://localhost:4000
# Kjøre qmongjs (i annen terminal):
cd ../qmongjs
npm install # installer nødvendige pakker
npm start # start opp app på http://localhost:3000
```


### Kjøre database lokalt

Denne må kjøre for at `mong-api` skal få tak i data.

```bash
sudo systemctl start mysqld # Start mysql server
sudo mysql_secure_installation # Førstegangs oppsett
```

```bash
sudo mysql -u root -p < Downloads/db.sql # legg inn databasedump (lastet ned fra mongr.no)
sudo mysql -u root -p # logg inn som root-bruker
```

```sql
CREATE USER imongr IDENTIFIED BY 'imongr'; -- lag imongr-bruker med passord imongr
GRANT ALL PRIVILEGES ON imongr TO 'imongr'; -- Gi bruker tilgang til imongr database
```

Hente inn ny database-dump:
```sql
drop database imongr;
create database imongr;
use imongr;
source <path>/<to>/<file>/imongr.sql;
```

### La imongr bruke samme database 

Åpne `imongr` i RStudio, installer pakken (`Shift-Ctrl-B`), definer globale variabler, og fyr opp appen:
```r
Sys.setenv(SHINYPROXY_USERNAME="imongr@mongr.no")
Sys.setenv(SHINYPROXY_USERGROUPS="MANAGER,PROVIDER")
Sys.setenv(IMONGR_DB_HOST="127.0.0.1")
Sys.setenv(IMONGR_DB_NAME="imongr")
Sys.setenv(IMONGR_DB_USER="imongr")
Sys.setenv(IMONGR_DB_PASS="imongr")
imongr::run_app()
```

For å få tilgang til Adminer-fanen må også Adminer kjøre og url settes i variablen `IMONGR_ADMINER_URL`.

## imongr

Utvikling av `imongr` kan være mest hensiktsmessig å gjøre i en docker-container. Enkelte av disse stegene (deler av steg 1, samt steg 3, 5 og 6) kan man droppe neste gang man fyrer opp sammer docker container. Hvis container er slettet må man gjøre disse stegene på nytt.

1. Last ned `imongr` og fyr opp docker
```bash
git clone git@github.com:mong/imongr
cd imongr
docker-compose up
```
2. Åpne http://localhost:8787/ og logg inn i RStudio (brukernavn `rstudio` og passord `password`).
3. Sett opp `imongr` inni docker. Enten `git clone git@github.com:mong/imongr` i terminalen og så lage et nytt prosjekt basert på denne mappen, eller lag nytt prosjekt basert på git-repository. Installer avhengigheter (`remotes::install_deps(dependencies = TRUE)`). Avhengigheter kan også installeres ved først å installer `imongr` fra github med `remotes` (`remotes::install_github("mong/imongr")`).
4. Definer `user` og `usergroups`
```r
Sys.setenv(SHINYPROXY_USERNAME="imongr@mongr.no")
Sys.setenv(SHINYPROXY_USERGROUPS="MANAGER,PROVIDER")
```
5. Last ned nyeste versjon av databasen fra https://mongr.no/imongr/app_direct/imongr/ (Adminer, Export, output gzip, Format SQL).
6. Fyr opp `imongr` (`imongr::run_app()`) og importer nettopp nedlastet dump av databasen (Adminer, Import, Execute).
