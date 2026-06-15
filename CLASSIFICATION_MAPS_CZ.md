# Popis klasifikačních map

Klasifikační mapy jsou orientační vrstvy pro rychlé předběžné vyhodnocení území a plánování terénního ověření. Nejde o finální odbornou klasifikaci vegetace nebo povrchu. Výsledky je vhodné chápat jako zvýraznění míst, která mohou být zajímavá pro následnou kontrolu v terénu.

## Mapa vegetační kondice

Mapa ukazuje základní stav vegetace podle spektrálního projevu Sentinel-2. Pomáhá rychle rozlišit kompaktní a aktivní vegetaci, vlhčí vegetační plochy, potenciálně stresovanou vegetaci, otevřenou vegetaci a plochy s nízkým vegetačním signálem.

### Třídy

| Třída | Význam | Orientační pravidlo |
| --- | --- | --- |
| `masked_or_invalid` | Plochy bez spolehlivého vyhodnocení, například mimo data nebo v neplatných pixelech. | Neplatná nebo chybějící hodnota v některém vstupním indexu. |
| `compact_active_vegetation` | Souvislejší, fotosynteticky aktivní vegetace. Pravděpodobně vitální kompaktní porosty. | `NDVI > 0.55` a `NDRE > NDRE_p60`. |
| `moist_active_vegetation` | Aktivní vegetace s vyšším vlhkostním signálem. Může ukazovat vlhčí nebo velmi vitální vegetaci. | `NDVI > 0.45` a `NDMI > NDMI_p70`. |
| `moisture_stressed_vegetation_candidate` | Vegetace s nižším vlhkostním signálem. Kandidát na sušší nebo vláhově limitovanou vegetaci. | `NDVI > 0.25` a `NDMI < NDMI_p25`. |
| `sparse_or_open_vegetation` | Řidší nebo otevřená vegetace, například travinné porosty, křoviny nebo mozaika vegetace a substrátu. | `0.25 <= NDVI < 0.55`. |
| `low_vegetation_or_bare_surface` | Nízký vegetační signál. Může jít o holý povrch, skálu, suť, půdu, cesty nebo velmi řídkou vegetaci. | `NDVI < 0.25`. |

### Orientační pravidla

- Kompaktní aktivní vegetace: vysoký vegetační a red-edge signál.
- Vlhčí aktivní vegetace: vegetace s vyšším vlhkostním signálem.
- Potenciálně stresovaná vegetace: vegetace s nízkým vlhkostním signálem.
- Řídká nebo otevřená vegetace: střední vegetační signál.
- Nízká vegetace nebo holý povrch: nízký vegetační signál.

Hodnoty `NDRE_p60`, `NDMI_p25` a `NDMI_p70` jsou percentily vypočtené z dané mozaiky. Například `NDMI_p25` znamená spodních 25 % hodnot NDMI v hodnoceném území.

## Mapa krajinné struktury

Mapa kombinuje vegetační signál se základními terénními parametry. Zvýrazňuje například kompaktní vegetační porosty, vegetaci na svazích, okraje porostů, potenciální suťové nebo skalnaté svahy, údolní polohy a suché jižně orientované svahy.

### Třídy

| Třída | Význam | Orientační pravidlo |
| --- | --- | --- |
| `unclassified_or_masked` | Nevyhodnocené nebo neklasifikované plochy. | Neplatná hodnota nebo žádné splněné pravidlo. |
| `compact_vegetation_patch` | Souvislejší kompaktní vegetační plocha. Může odpovídat hustším porostům, křovinám nebo lesním částem. | `NDVI > 0.55`, `NDRE > NDRE_p60` a jednoduché vyčištění souvislosti plochy. |
| `compact_vegetation_on_slope` | Kompaktní vegetace na svahu. Užitečné pro hledání souvislých vegetačních struktur v členitém terénu. | Kompaktní vegetace a `slope >= 15°`. |
| `compact_vegetation_edge_zone` | Okrajová zóna kompaktní vegetace. Může odpovídat lesním okrajům, křovinným lemům nebo přechodům mezi porostem a otevřenou plochou. | Přibližně 20m okraj kolem kompaktní vegetace. |
| `potential_scree_or_rocky_slope` | Strmější a členitější plocha s nízkým vegetačním signálem. Kandidát na suť, skálu, odkrytý substrát nebo velmi řídkou vegetaci. | `NDVI < 0.30`, `slope >= 15°`, `roughness > roughness_p60`. |
| `valley_bottom_or_accumulation_position` | Údolní nebo akumulační poloha podle tvaru terénu. Může označovat dna údolí, deprese nebo místa akumulace. | `TPI < TPI_p30` a `slope < 15°`. |
| `moist_valley_or_riparian_structure` | Vlhčí vegetovaná poloha v údolí nebo na mírnějším terénu. Kandidát na ripariální, údolní nebo akumulační vegetaci. | `TPI < TPI_p30`, `slope < 15°`, `NDVI > 0.40`, `NDMI > NDMI_p60`. |
| `dry_south_facing_slope` | Vegetovaný jižně až jihozápadně orientovaný svah s nižším vlhkostním signálem. Kandidát na suchý expozičně podmíněný vzorec. | `NDVI > 0.25`, `slope >= 15°`, expozice `135°-225°`, `NDMI < NDMI_p30`. |
| `open_slope_or_mosaic_structure` | Svažité plochy s otevřenou nebo mozaikovitou vegetací. Může jít o travinné porosty, křoviny, skalní výchozy nebo směs vegetace a substrátu. | `0.25 <= NDVI < 0.55` a `slope >= 15°`. |

### Orientační pravidla

- Kompaktní vegetace: vysoký vegetační signál a souvislejší prostorový výskyt.
- Okrajové zóny: přechody kolem kompaktních vegetačních ploch.
- Potenciální suť nebo skála: nízký vegetační signál na strmějším a členitějším terénu.
- Údolní polohy: nižší topografická poloha a mírnější sklon.
- Suché jižní svahy: vegetované svahy s jižní až jihozápadní expozicí a nižším vlhkostním signálem.

Hodnoty `NDRE_p60`, `NDMI_p30`, `NDMI_p60`, `TPI_p30` a `roughness_p60` jsou percentily vypočtené z dané mozaiky. Díky tomu se hranice tříd přizpůsobují konkrétnímu území.

