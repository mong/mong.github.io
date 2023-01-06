# Utvikling av kode

## Renovate

Vi bruker [Renovate](https://github.com/renovatebot/renovate) for å holde våre koder oppdatert. Dependabot kan sjekke versjoner i npm/yarn, Docker og Github Actions. Renovate aktiveres ved å legge til en `renovate.json`-fil.

Følgende `renovate.json`-fil brukes i `mongts`:
```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["config:base"],
  "baseBranches": ["dependency_updates"],
  "packageRules": [
    {
      "matchUpdateTypes": ["minor", "patch", "pin", "digest"],
      "automerge": true
    }
  ]
}
```
Denne filen forteller Renovate at det skal opprettes `PR` mot grenen `dependency_updates`. Dette er gjort for å slippe å få veldig mange commits direkte inn til `develop`/`main`. Fra tid til annen sendes det en `PR` fra `dependency_updates` til `develop`.

`dependency_updates`-grenen settes til å være beskyttet, men med *Allow force pushes* aktivert. Da vil den ikke bli slettet hver gang den er merget inn i `develop`-grenen.

Etter at en `PR` fra `dependency_updates` er kommet inn til `develop` kan man rydde opp `dependency_updates`-grenen ved å kjøre følgende kommandoer:

```bash
git checkout dependency_updates # stå på dependency_updates-grenen
git fetch origin develop:develop # oppdater develop lokalt
git reset --hard develop # dependency_updates settes lik develop
git push -f # dependency_updates settes lik master også på github
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
