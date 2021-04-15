# Utvikling av kode

## Javascript/Typescript

### database

```bash
sudo systemctl start mysqld # Start mysql server
sudo mysql_secure_installation # First time setup
```

## R

[Hentet fra README til imongr-prosjektet]

### First time setup

1. Fire up docker-compose (`docker-compose up`)
2. Enter *RStudio* on http://localhost:8787/
3. Set up `imongr` inside docker. Either do `git clone git@github.com:mong/imongr` and create new project based on folder, or create new project based on git repository. Dependencies can be installed by first install `remotes` (`install.packages("remotes")`) and then install `imongr` from `github` (`remotes::install_github("mong/imongr")`).
4. Define user, groups etc.
```r
Sys.setenv(SHINYPROXY_USERNAME="imongr@mongr.no")
Sys.setenv(SHINYPROXY_USERGROUPS="MANAGER,PROVIDER")
```
5. Download the current version of the database from https://mongr.no/imongr/app_direct/imongr/ (Adminer, Export, output gzip, Format SQL).
6. Fire up `imongr` (`imongr::run_app()`) and import current version of the database (Adminer, Import, Execute).
