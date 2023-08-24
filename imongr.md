# Diverse dokumentasjon om imongr

## En beskrivelse av hvordan `imongr` kjører på *AWS*

`imongr` kjøres som docker-containere, som snurres opp av `shinyproxy`, som også kjøres som en docker-container. Alt dette kjører på en egen EC2 instans som er manuelt satt opp.

### Shinyproxy

Docker-images `hnskde/shinyproxy-imongr`, som er det som snurres opp som shinyproxy, er basert på følgende Docker-fil: https://github.com/mong/shinyproxy/blob/main/imongr/Dockerfile. Denne bygges sannsynligvis manuelt og dyttes opp til dockerhub.


