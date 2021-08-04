# Tilrettelegge data

### Fjerne linjeskift i lang beskrivelse

```r
long_description = gsub("\r?\n|\r", " ", Langbeskrivelse)
```

### Legge på organisasjonnummer hvis man har kortnavn

```r
orgnr <- read.csv2("orgnr/orgnr.csv") %>%
  dplyr::filter(!stringr::str_detect(full_name, "HABIL")) %>%
  dplyr::filter(!stringr::str_detect(full_name, "HABL")) %>%
  dplyr::distinct(short_name, .keep_all = TRUE) %>%
  dplyr::transmute(short_name = dplyr::case_when(short_name == "Kristiansand" ~ "Kristiansand S",
                                            short_name == "Kristiansund" ~ "Kristiansund N",
                                            short_name == "Diakonhjemmet sykehus AS" ~ "Diakonhjemmet sykehus",
                                            short_name == "Lovisenberg diakonale sykehus AS" ~ "Lovisenberg diakonale sykehus",
                                            TRUE ~ short_name),
                orgnr,
                full_name)

data <- read.csv2("intensiv/2021/nipar_isolasjon.csv") %>%
  dplyr::rename(short_name = HealthUnitShortName) %>%
  dplyr::left_join(orgnr, by = "short_name")
```
