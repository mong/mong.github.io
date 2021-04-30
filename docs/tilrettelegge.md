# Tilrettelegge data

### Fjerne linjeskift i lang beskrivelse

```r
long_description = gsub("\r?\n|\r", " ", Langbeskrivelse)
```
