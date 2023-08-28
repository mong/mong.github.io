# Diverse dokumentasjon om imongr

## En beskrivelse av hvordan `imongr` kjører på *AWS*

I *Route 53* er `imongr.skde.org` rutet til *Load balancer* `lb-imongr`. Denne fører videre trafikken til *Target group* `tg-imongr`, som har ett *target*. Dette *target* er en *EC2*-instans, der *ShinyProxy* kjøres som en docker-container. *ShinyProxy* er koblet til pålogging av bruker og snurrer opp `imongr`-containere etter behov. Alt dette kjører på en EC2-instans som er manuelt satt opp. Oppdateringer etc. gjøres gjennom `ssh`.

### Shinyproxy

Docker-images `hnskde/shinyproxy-imongr`, som er det som snurres opp som shinyproxy, er basert på følgende Docker-fil: https://github.com/mong/shinyproxy/blob/main/imongr/Dockerfile. Denne bygges enn så lenge manuelt og dyttes opp til dockerhub.


