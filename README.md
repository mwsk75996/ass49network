# Assignment 49 – OSPF på Juniper SRX

Dette repository indeholder de komplette Junos-konfigurationer til de tre fysiske SRX-routere i assignment 49:

- `r2-config.txt` – konfiguration til R2
- `r4-config.txt` – konfiguration til R4
- `r5-config.txt` – konfiguration til R5

## Hvad går konfigurationen ud på?

R2, R4 og R5 er forbundet i en trekant via tre `/28`-netværk: Lan5, Lan6 og Lan10. Routerne bruger OSPF i area `0.0.0.0` til automatisk at udveksle ruter mellem netværkene.

OSPF-costs er sat, så hver router har én foretrukken vej til det fjerne LAN og en alternativ vej gennem den tredje router. Hvis et link går ned, beregner OSPF automatisk en ny rute.

På hver SRX ligger de to labinterfaces i security-zonen `lab`. Ping og OSPF er tilladt som host-inbound-trafik, og en intra-zone-policy tillader trafik mellem labnetværkene. Konfigurationerne bruger ingen statiske ruter.

## Primære OSPF-veje

- R5 når Lan5 gennem R4.
- R2 når Lan6 gennem R4.
- R4 når Lan10 gennem R2.

## Indlæsning

Den relevante fil kan indlæses fra Junos configuration mode med:

```text
load override terminal
```

Indsæt hele filen, afslut med `Ctrl+D`, og kontrollér derefter ændringen med `show | compare` og `commit check`. Brug gerne `commit confirmed 5`, så routeren automatisk ruller tilbage, hvis forbindelsen eller routingen fejler.

> **Bemærk:** `load override` erstatter hele routerens konfiguration. Filerne indeholder de krypterede root-passwordhashes fra labrouterne og bør kun bruges på de tilsigtede enheder.
