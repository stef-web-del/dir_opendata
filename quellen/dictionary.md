# Open-Data Quellenverzeichnis (Dictionary)

Stand: 2026-09-07. Lebendes Verzeichnis für Dashboard/Landkreisranking.
Scope: Deutschland, Ebene Landkreis | Gemeinde | Raster. Nur Einträge mit Download- oder API-URL.

Kernquellen (A) aus Testlauf `2026-09-07-test.md`. Updates (B) und Nischen (C) aus Nischenlauf `2026-09-07-nische.md` + Prio-1 `2026-09-07-prio1.md`. Portale (D) aus `startliste.md`. Länder-Datenpunkte (E) aus `2026-09-07-laender.md`.

## Schema (pro Eintrag)

```
### <kurz-id>
- Name:
- Betreiber:
- Thema: (Demografie | Einkommen | Soziales | Bildung | Gesundheit | Wirtschaft | Arbeitsmarkt | Wohnen | Energie | Klima | Umwelt | Mobilität | Sicherheit | Wahlen | Geobasis | Sonstiges)
- Ebene: Landkreis | Gemeinde | Raster | gemischt
- Format:
- URL:
- Lizenz:
- Stand: (Datum / Erscheinungsjahr wenn bekannt)
- Aktualität: Baseline | Update | Neu | Nische
- Inhalt: 2–4 Sätze — welche Variablen/Indikatoren/Spalten oder Layer im File/API stecken (Einheiten, Schlüssel wie AGS/Kreisname wenn bekannt). Keine erfundenen Spaltennamen.
- Warum spannend: 1–2 Sätze — konkreter Nutzen für Kreis-/Gemeinde-Vergleich oder Dashboard (Join-Key, Ranking-Dimension, Heatmap).
- Status: verifiziert | WAF | Account nötig | Katalog-only | unklar
- Notizen: Caveats (WAF, Account, Aggregation, Coverage-Lücken)
```

## A — Kern / Baseline (bekannt, unverzichtbar)

(Atlas pflegt; keine Löschung der Kernquellen.)

### kern-zensus-gitter
- Name: Zensus 2022 Gitterdaten (Bevölkerung, Eigentümerquote)
- Betreiber: Destatis
- Thema: Demografie
- Ebene: Raster
- Format: ZIP → CSV
- URL: https://www.destatis.de/static/DE/zensus/gitterdaten/Zensus2022_Bevoelkerungszahl.zip
- Lizenz: dl-de/by-2-0
- Stand: 15.05.2022
- Aktualität: Baseline
- Inhalt: Bevölkerungszahlen in Gitterzellen (100 m / 1 km / 10 km) zum Stichtag 15.05.2022. Familienprodukt `Eigentuemerquote_in_Gitterzellen.zip` liefert Eigentümerquote je Zelle. Aggregation auf Kreis/Gemeinde über Gitter-Zuordnung möglich.
- Warum spannend: Heatmap-Layer und feine Demografie-Normierung, wo nur Kreis-Mittelwerte zu grob sind; Join über Rasterzelle → VG250/AGS.
- Status: verifiziert
- Notizen: Familie inkl. Eigentümerquote `…/Eigentuemerquote_in_Gitterzellen.zip`. `/static/` kann WAF-403. Weitere Gitter (Heizung, Miete) → C.

### kern-zensus-regional
- Name: Zensus 2022 Regionaltabellen Bevölkerung / Demografie
- Betreiber: Destatis
- Thema: Demografie
- Ebene: gemischt
- Format: XLSX
- URL: https://www.destatis.de/static/DE/zensus/gitterdaten/Regionaltabelle_Bevoelkerung.xlsx
- Lizenz: dl-de/by-2-0
- Stand: 15.05.2022
- Aktualität: Baseline
- Inhalt: Einwohnerzahlen Gemeinde und Landkreis mit AGS zum Zensus-Stichtag. Pendant `Regionaltabelle_Demografie.xlsx` ergänzt Altersstruktur (Altersgruppen) für dieselben Gebietseinheiten.
- Warum spannend: Stichtags-Einwohner und Altersprofil als Basis-Score und Nenner für Pro-Kopf-Kennzahlen im Kreis-/Gemeinde-Ranking.
- Status: WAF
- Notizen: Pendant Demografie `…/Regionaltabelle_Demografie.xlsx`. Destatis `/static/` intermittierend 403.

### kern-regionalstatistik-api
- Name: Regionalstatistik REST + Bevölkerung/Fläche Kreise
- Betreiber: Statistische Ämter Bund/Länder
- Thema: Demografie
- Ebene: gemischt
- Format: API + ffcsv
- URL: https://www.regionalstatistik.de/genesisws/rest/2020/
- Lizenz: dl-de/by-2-0
- Stand: live; 12411/11111 200
- Aktualität: Baseline
- Inhalt: REST für Tabellenabruf (JSON/CSV/XLSX nach Token). Verifizierte Flat-CSVs: Bevölkerung Kreise `12411-01-01-4` (AGS in `KREISE`) und Gebietsfläche Kreise `11111-01-01-4`. Gemeinde-Codes `…-5` oft leerer Body.
- Warum spannend: Automatisierte Zeitreihen Bevölkerung und Fläche → Dichte und Pro-Kopf-Nenner für alle Kreis-Rankings; Join über AGS.
- Status: Account nötig
- Notizen: whoami 200; tablefile ohne Token 401. Deep-Links `…/download/12411-01-01-4/ffcsv` und `11111-01-01-4/ffcsv` verifiziert. Gemeinde-ffcsv (…-5) oft leer.

### kern-genesis-api
- Name: GENESIS-Online REST API
- Betreiber: Destatis
- Thema: Sonstiges
- Ebene: gemischt
- Format: API
- URL: https://genesis.destatis.de/genesisWS/rest/2020/
- Lizenz: dl-de/by-2-0
- Stand: live
- Aktualität: Baseline
- Inhalt: Destatis-GENESIS REST (whoami live): Tabellen, Zeitreihen und regionale Dimensionen inkl. AGS, sofern die jeweilige Statistik sie führt. Kein fertiger All-Kreise-Dump — Abruf tabellenweise nach Registrierung.
- Warum spannend: Programmatischer Nachzug amtlicher Indikatoren, die Regionalstatistik nicht als ffcsv liefert; Ergänzungspfad für Dashboard-Pipelines.
- Status: Account nötig
- Notizen: whoami HTTP 200.

### kern-inkar-2025
- Name: INKAR 2025 Gesamtdatei
- Betreiber: BBSR
- Thema: Sonstiges
- Ebene: gemischt
- Format: ZIP (~414 MB)
- URL: https://www.bbr-server.de/imagemap/inkar/download/inkar_2025.zip
- Lizenz: dl-de/by-2-0
- Stand: INKAR 2025
- Aktualität: Baseline
- Inhalt: Bulk mit ca. 600 Raumindikatoren der BBSR-Raumbeobachtung für u. a. Kreise (KRE) und Gemeinden (GEM). JSON-Nebenwege: Gebiete `…/GetGebieteZumRaumbezug/KRE` bzw. `/GEM`, Werte via POST `…/Table/GetDataTable`.
- Warum spannend: Fertige Multi-Indikator-Matrix für Atlas-Scoring ohne Einzelstatistik-Joins; Raumbezugsschlüssel für Kreis-/Gemeinde-Karten.
- Status: verifiziert
- Notizen: TLS ggf. `curl -k`. JSON: `https://www.inkar.de/Wizard/GetGebieteZumRaumbezug/KRE` · `/GEM` · POST `https://www.inkar.de/Table/GetDataTable`. ROR und Indikatorenkatalog → C.

### kern-vg250
- Name: BKG VG250 Verwaltungsgebiete
- Betreiber: BKG
- Thema: Geobasis
- Ebene: gemischt
- Format: Shapefile ZIP / Excel / WFS
- URL: https://daten.gdz.bkg.bund.de/produkte/vg/vg250_ebenen_1231/aktuell/vg250_12-31.utm32s.shape.ebenen.zip
- Lizenz: dl-de/by-2-0
- Stand: 31.12. / 01.01. Produktfamilie aktuell
- Aktualität: Baseline
- Inhalt: Amtliche Verwaltungsgrenzen Landkreis und Gemeinde (UTM32 Shape) mit AGS und Namen. Familie: Stand 01.01 Shape, Excel-Ebenen (Schlüssel/Namen ohne Geometrie), VG250-EW mit Einwohner und Fläche, WFS `wfs_vg250-ew`.
- Warum spannend: Geometrie- und AGS-Rückgrat für alle Joins, Choroplethen und Normierung (EW/Fläche) im Dashboard.
- Status: verifiziert
- Notizen: Familie: 01.01 Shape, Excel-Ebenen, VG250-EW 31.12., WFS `https://sgx.geodatenzentrum.de/wfs_vg250-ew`. GE250 (ROR) ist nicht VG250 → C.

### kern-dwd-duerre
- Name: DWD Dürreindex / Bodenfeuchte / Niederschlag
- Betreiber: DWD
- Thema: Klima
- Ebene: Raster
- Format: ESRI ASCII `.asc.gz` / TGZ
- URL: https://opendata.dwd.de/climate_environment/CDC/grids_germany/annual/drought_index/grids_germany_annual_drought_index_202517.asc.gz
- Lizenz: CC BY 4.0
- Stand: 2025 Jahresraster; Monat Jul 2025; Normal 1991–2020
- Aktualität: Baseline
- Inhalt: 1-km-Raster Deutschland: Jahres-Dürreindex (de Martonne), Monats-Dürreindex, Multi-annual 1991–2020, tägliche Bodenfeuchte (AMBAV, TGZ), monatlicher Niederschlag. `regional_averages_DE` nur Länder, nicht Kreis.
- Warum spannend: Klima-/Agrar-Heatmaps und Kreis-Mittel aus Raster für Vergleich Dürrebelastung und Niederschlag.
- Status: verifiziert
- Notizen: Familie: monthly drought, multi-annual 1991–2020, daily soil_moist 202608, monthly precipitation. regional_averages_DE nur Länder.

### kern-ew24
- Name: Europawahl 2024 Ergebnisse Kreisebene
- Betreiber: Bundeswahlleiterin
- Thema: Wahlen
- Ebene: Landkreis
- Format: CSV
- URL: https://www.bundeswahlleiterin.de/europawahlen/2024/ergebnisse/opendata/ew24/csv/kerg2.csv
- Lizenz: dl-de/by-2-0
- Stand: 2024
- Aktualität: Baseline
- Inhalt: Amtliche EW24-Ergebnisse kreisscharf (AGS 5-stellig) mit Stimmenanteilen der Parteien. BTW 2025 im Testkern nur Wahlkreis + Gemeinde↔WKR-Zuordnung — Stimmen nicht kreisscharf.
- Warum spannend: Politische Zusammensetzung als Ranking-/Kontext-Dimension direkt auf Kreisebene joinbar (AGS).
- Status: verifiziert
- Notizen: BTW 2025 im Testkern als Wahlkreis + Gemeinde↔WKR-Zuordnung; Stimmen nicht kreisscharf.

### kern-govdata-ckan
- Name: GovData CKAN API
- Betreiber: GovData / BMI
- Thema: Sonstiges
- Ebene: gemischt
- Format: JSON API
- URL: https://www.govdata.de/ckan/api/3/action/package_search
- Lizenz: Metadaten je Datensatz
- Stand: live
- Aktualität: Baseline
- Inhalt: Metadatenkatalog (Titel, Tags, Ressourcen-URLs, Lizenzen) — keine Indikatorwerte. `package_search` filtert nach Stichwort/Organisation.
- Warum spannend: Discovery-Schleife für neue Kreis-/Gemeinde-Downloads; Treffer werden in dieses Dictionary übernommen.
- Status: verifiziert
- Notizen: Katalog, keine Werte. Länderportale → D.

### kern-bka-pks
- Name: PKS 2024 Grundtabelle Kreise (Fälle + HZ)
- Betreiber: BKA
- Thema: Sicherheit
- Ebene: Landkreis
- Format: CSV
- URL: https://www.bka.de/SharedDocs/Downloads/DE/Publikationen/PolizeilicheKriminalstatistik/2024/Kreis/Faelle/KR-F-01-T01-Kreise-Faelle-HZ_csv.csv
- Lizenz: BKA-Nutzungsbedingungen
- Stand: 2024
- Aktualität: Baseline
- Inhalt: Polizeiliche Kriminalstatistik Kreise: Fallzahlen und Häufigkeitszahl (HZ je 100.000 EW) für ausgewählte Straftaten/Gruppen. Keine Gemeindeebene. T20 Tatverdächtige und kommunale Mirrors → C-Notizen.
- Warum spannend: Sicherheits-Score und Heatmap kreisscharf (Fälle + HZ) als eigene Ranking-Achse neben Sozial-/Wirtschaftsindikatoren.
- Status: WAF
- Notizen: GovData-Katalog `…/t01-grundtabelle-kreise-…`. Direktdownload mit Mandats-UA oft 303/400/403. T20 Tatverdächtige und kommunale Mirrors → C.

### kern-destatis-unfallatlas
- Name: Unfallatlas — Unfälle mit Personenschaden
- Betreiber: Statistische Ämter Bund/Länder (Hosting OpenGeodata.NRW)
- Thema: Sicherheit
- Ebene: Raster
- Format: ZIP→CSV / ZIP→Shape / WMS
- URL: https://www.opengeodata.nrw.de/produkte/transport_verkehr/unfallatlas/Unfallorte2025_EPSG25832_CSV.zip
- Lizenz: dl-de/by-2-0
- Stand: 2016–2025 (2025 Pub Juli 2026)
- Aktualität: Neu
- Inhalt: Punktgenaue Unfallorte Personenschaden EPSG:25832; Schwere, Beteiligung (Pkw/Rad/Krad/Fuß). Shape + WMS parallel.
- Warum spannend: Unfalldichte/Schwere je Kreis oder Straßennetz — Sicherheits-Score und Heatmaps.
- Status: verifiziert
- Notizen: Shape 2025 `…/Unfallorte2025_EPSG25832_Shape.zip`. WMS `https://www.wms.nrw.de/wms/unfallatlas`. Ranking-Dimension Unfälle Personenschaden. AGS über Geo-Join.

### kern-bmdv-mobilithek
- Name: Deutschland ÖPNV-Fahrplan DELFI / Mobilithek (GTFS frei + Full Account)
- Betreiber: DELFI e.V. / BMDV Mobilithek
- Thema: Mobilität
- Ebene: gemischt
- Format: GTFS ZIP (~260 MB frei); NeTEx/voll Account
- URL: https://download.gtfs.de/germany/nv_free/latest.zip
- Lizenz: CC-BY 4.0 (DELFI)
- Stand: live Free-Feed
- Aktualität: Neu
- Inhalt: agency/stops/routes/trips — routingfähiger Fahrplan; Free-Feed Coverage-Caveat (nicht 100 % aller regionalen Anbieter).
- Warum spannend: ÖPNV-Haltestellendichte / Takt-Proxy je Gemeinde/Kreis.
- Status: verifiziert
- Notizen: Full DELFI/Mobilithek **Account nötig** (`opendata-oepnv.de`, mobilithek.info). Free ≠ vollständiges NeTEx. Ranking-Dimension ÖPNV-Takt/Haltestellendichte.

## B — Aktuell / Updates 2024–2026

(Neuere Releases und Revisionen.)

### upd-vgrdl-einkommen
- Name: VGRdL Reihe 2 Band 3 — Verfügbares Einkommen Kreise
- Betreiber: Statistische Ämter Bund/Länder (VGRdL)
- Thema: Einkommen
- Ebene: Landkreis
- Format: XLSX
- URL: https://www.statistikportal.de/sites/default/files/2026-01/vgrdl_r2b3_bs2024.xlsx
- Lizenz: dl-de/by-2-0
- Stand: bis 2023; BS Feb 2025
- Aktualität: Update
- Inhalt: Primär- und verfügbares Einkommen der privaten Haushalte je Einwohner, kreisscharf (Landkreis / krfr. Stadt). Geschwister: R2B1 BIP `vgrdl_r2b1_bs2024.xlsx`, R2B2 Löhne `vgrdl_r2b2_bs2025.xlsx`. RS-82411 Deep-Link leer.
- Warum spannend: Kaufkraft-/Wohlstands-Ranking je Kreis; Join über Kreisschlüssel mit VG250 und anderen Sozio-Layern.
- Status: verifiziert
- Notizen: RS-82411 Deep-Link leer. Geschwister: R2B1 BIP `vgrdl_r2b1_bs2024.xlsx`, R2B2 Löhne `vgrdl_r2b2_bs2025.xlsx` (bis 2024).

### upd-vgrdl-loehne
- Name: VGRdL Reihe 2 Band 2 — Arbeitnehmerentgelt / Bruttolöhne Kreise
- Betreiber: VGRdL
- Thema: Einkommen
- Ebene: Landkreis
- Format: XLSX
- URL: https://www.statistikportal.de/sites/default/files/2026-07/vgrdl_r2b2_bs2025.xlsx
- Lizenz: dl-de/by-2-0
- Stand: bis 2024; BS Aug 2025
- Aktualität: Update
- Inhalt: Arbeitnehmerentgelt und Bruttolöhne/-gehälter kreisscharf, Berichtsjahre bis 2024 (aktuellster der drei VGRdL-Kreisbände). Ergänzt R2B3 (verfügbares Einkommen) und R2B1 (BIP).
- Warum spannend: Lohnniveau als Ranking-Dimension und Abgleich zu verfügbarem Einkommen / SvB-Beschäftigung.
- Status: verifiziert
- Notizen: Neueste der drei VGRdL-Kreisbände.

### upd-sgb2-aug2026
- Name: SGB II Kennzahlen §48a CSV ZIP
- Betreiber: BA / BMAS (sgb2.info)
- Thema: Soziales
- Ebene: gemischt
- Format: ZIP → CSV
- URL: https://www.sgb2.info/SharedDocs/Downloads/DE/Kennzahlen/Kennzahlen-System-A-August-2026-CSV.zip?__blob=publicationFile&v=2
- Lizenz: BA/BMAS (Quellenangabe)
- Stand: Apr 2026 / Pub Aug 2026
- Aktualität: Update
- Inhalt: Bulk-CSV der §48a-Kennzahlen (System A) auf Jobcenter-/Träger-Ebene; Dateien `dl_48a_daten_statistikba_*.csv`. Excel-ZIP parallel. Regionale Zuordnung auf Kreis oft über Trägergebiet nötig.
- Warum spannend: SGB-II-Leistungsfähigkeit maschinenlesbar für Sozial-Ranking; parallel zur BA-API-Zeitreihe `upd-ba-api-grusi`.
- Status: verifiziert
- Notizen: Excel-ZIP parallel. Dateien `dl_48a_daten_statistikba_*.csv`.

### upd-mastr-export
- Name: MaStR Gesamtdatenexport
- Betreiber: BNetzA MaStR
- Thema: Energie
- Ebene: gemischt
- Format: ZIP → XML (~3 GB)
- URL: https://download.marktstammdatenregister.de/Gesamtdatenexport_20260907_26.1.zip
- Lizenz: dl-de/by-2-0
- Stand: täglich ~05:00
- Aktualität: Update
- Inhalt: Stammdaten aller registrierten EE-/KWK-/Speicheranlagen (Standort, Leistung, Energieträger, Betriebsstatus u. a.). Roh-XML; Aggregation auf Gemeinde/Kreis über AGS selbst (z. B. open-mastr). Dateiname rotiert täglich.
- Warum spannend: EE-Leistung und Anlagenzahl je Kreis/Gemeinde für Energie-Ranking und Heatmaps (PV/Wind/Speicher).
- Status: verifiziert
- Notizen: Dateiname rotiert täglich. Portal `…/MaStR/Datendownload`. Parser: open-mastr. Stichtag `…/Stichtag/Gesamtdatenexport_20260701_26.1.zip`.

### upd-breitbandatlas-2025
- Name: Breitbandatlas Festnetz Ende 2025
- Betreiber: BNetzA / Gigabit-Grundbuch
- Thema: Sonstiges
- Ebene: gemischt
- Format: XLSX (~10 MB)
- URL: https://data.bundesnetzagentur.de/Bundesnetzagentur/GIGA/DE/Breitbandatlas/Downloads/bba_12_2025.xlsx
- Lizenz: BNetzA / Gigabit-Grundbuch
- Stand: Ende 2025
- Aktualität: Update
- Inhalt: Festnetz-/Gigabit-Versorgungsanteile Bund–Land–Kreis–Gemeinde (Ende 2025, `bba_12_2025`). Analyseplattform zugangsgeschützt; Tabellen-XLSX offen.
- Warum spannend: Digitalisierungs-Score und Versorgungsranking kreis- und gemeindescharf; Join über Gebietsschlüssel.
- Status: verifiziert
- Notizen: Analyseplattform zugangsgeschützt. Raster → `upd-breitband-gitter`.

### upd-breitband-gitter
- Name: Breitbandatlas Festnetz Gitterzellen
- Betreiber: BNetzA / Gigabit-Grundbuch
- Thema: Sonstiges
- Ebene: Raster
- Format: ZIP → GeoPackage (~307 MB)
- URL: https://data.bundesnetzagentur.de/Bundesnetzagentur/GIGA/DE/Breitbandatlas/Downloads/Versorgungsdaten_Gitterzellen_Stand_20251231_gpkg.zip
- Lizenz: BNetzA / Gigabit-Grundbuch
- Stand: 2025-12-31
- Aktualität: Update
- Inhalt: Festnetzversorgung je Gitterzelle als GeoPackage (Stand 2025-12-31). Mobilfunk-Pendant unter `…/MobilfunkMonitoring/…`. Aggregation auf Kreis/Gemeinde über Zellenmittel.
- Warum spannend: Feiner Digital-Heatmap-Layer, wo Gemeinde-Mittel Versorgungsinseln verdecken.
- Status: verifiziert
- Notizen: Mobilfunk-Pendant `…/MobilfunkMonitoring/2512/202601_MobilfunkMonitoring.zip`.

### upd-baufertig-2024
- Name: Baufertigstellungen Wohngebäude Kreise 2024
- Betreiber: Regionalstatistik (31121)
- Thema: Wohnen
- Ebene: Landkreis
- Format: ffcsv
- URL: https://www.regionalstatistik.de/genesis/online/download/31121-01-02-4/ffcsv
- Lizenz: dl-de/by-2-0
- Stand: 2024
- Aktualität: Update
- Inhalt: Baufertigstellungen Wohngebäude kreisscharf (Tabelle 31121-01-02-4, ffcsv). Verwandt: Nichtwohngebäude `31121-04-01-4`. Dynamik gegenüber Zensus-Gebäudebestand.
- Warum spannend: Bauaktivität als Ranking-Dimension Wohnen/Wachstum; Join AGS/`KREISE` zum Bestand.
- Status: verifiziert
- Notizen: Verwandt 31121-04-01-4 Nichtwohngebäude.

### upd-ba-api-alo
- Name: BA Statistik-API Arbeitslosigkeit Kreise
- Betreiber: Statistik der BA
- Thema: Arbeitsmarkt
- Ebene: Landkreis
- Format: API → CSV
- URL: https://statistik-dr.arbeitsagentur.de/bifrontend/bids-api/ct/v1/tableFetch/csv/EckwerteTabelleALOKr?Kreis=NAME
- Lizenz: Statistik der BA
- Stand: Aug 2026
- Aktualität: Update
- Inhalt: Eckwerte Arbeitslosigkeit und Unterbeschäftigung je Kreis (Parameter `Kreis=NAME`). Ein Region/Request; Kreisnamen aus `DR-LZR-persistListenfelderJSON.js`. Gemeinde nur mit Clientzertifikat.
- Warum spannend: Monatsaktuelle ALO-Quote für Arbeitsmarkt-Ranking und Zeitreihen-Charts je Kreis.
- Status: verifiziert
- Notizen: 1 Region/Request. Namen aus `…/DR-LZR-persistListenfelderJSON.js`. Gemeinde nur mit Clientzertifikat.

### upd-ba-api-bst
- Name: BA Statistik-API Beschäftigung Eckwerte
- Betreiber: Statistik der BA
- Thema: Arbeitsmarkt
- Ebene: Landkreis
- Format: API → CSV
- URL: https://statistik-dr.arbeitsagentur.de/bifrontend/bids-api/ct/v1/tableFetch/csv/EckwerteTabelleBST?Kreis%20AO=NAME
- Lizenz: Statistik der BA
- Stand: Feb/Aug 2026
- Aktualität: Update
- Inhalt: Sozialversicherungspflichtig Beschäftigte (SvB) Eckwerte kreisscharf (`Kreis AO=NAME`), ohne Clientzertifikat. Zeitreihe über `EckwerteZeitreiheBST`. Doku API-BST.html.
- Warum spannend: Beschäftigungsstand und -dynamik als Gegenstück zur ALO-Quote im Kreisvergleich.
- Status: verifiziert
- Notizen: Zeitreihe `EckwerteZeitreiheBST`. Doku API-BST.html.

### upd-ba-api-grusi
- Name: BA Statistik-API SGB II Zeitreihe Kreis
- Betreiber: Statistik der BA
- Thema: Soziales
- Ebene: Landkreis
- Format: API → CSV
- URL: https://statistik-dr.arbeitsagentur.de/bifrontend/bids-api/ct/v1/tableFetch/csv/EckwerteZeitreiheGrusi?Kreis=NAME
- Lizenz: Statistik der BA
- Stand: Aug 2026
- Aktualität: Update
- Inhalt: Grundsicherung SGB II Zeitreihe Eckwerte je Kreis (`Kreis=NAME`). Parallel zum §48a-Bulk `upd-sgb2-aug2026`.
- Warum spannend: Monats-/Zeitreihen-Sozialhilfe-Proxy kreisscharf für Dashboard-Trends und Ranking.
- Status: verifiziert
- Notizen: Parallel zu upd-sgb2-aug2026.

### upd-kwm-2024
- Name: Kreiswanderungsmatrix 2024
- Betreiber: Destatis / Stat. Ämter
- Thema: Mobilität
- Ebene: Landkreis
- Format: ZIP → CSV
- URL: https://www.statistikportal.de/sites/default/files/2025-06/KWM_2024.zip
- Lizenz: dl-de/by-2-0
- Stand: 2024
- Aktualität: Update
- Inhalt: Zu- und Fortzüge zwischen allen Kreisen (Herkunft×Ziel-Matrix) für 2024. Deckt Wanderung, nicht Tagespendel (kein BA-Pendler-API; RS-19321 leer).
- Warum spannend: Nettozuwanderung und Austauschbeziehungen als Mobilitäts-/Attraktivitäts-Indikator im Kreisranking.
- Status: verifiziert
- Notizen: Deckt nicht Tagespendel ab.

### upd-kraftwerksliste-2026
- Name: BNetzA Kraftwerksliste
- Betreiber: Bundesnetzagentur
- Thema: Energie
- Ebene: gemischt
- Format: CSV
- URL: https://www.bundesnetzagentur.de/DE/Fachthemen/ElektrizitaetundGas/Versorgungssicherheit/Erzeugungskapazitaeten/Kraftwerksliste/_DL/Kraftwerksliste_CSV.csv?__blob=publicationFile&v=9
- Lizenz: amtlich / MaStR-basiert
- Stand: 26.06.2026
- Aktualität: Update
- Inhalt: Kraftwerke ≥10 MW einzeln (Standort, Energieträger, Leistung); Kleinanlagen nach Land aggregiert. XLSX-Pendant und Zu-/Rückbau-Datei parallel. Kompakter als MaStR-XML.
- Warum spannend: Schneller Energieerzeugungs-Überblick je Standort/Land ohne 3-GB-Parse; Großanlagen-Marker auf Karte.
- Status: verifiziert
- Notizen: ≥10 MW einzeln; Kleinanlagen nach Land. XLSX-Pendant + ZuUndRueckbau.

### upd-vgrdl-bip
- Name: VGRdL Reihe 2 Band 1 — BIP / BWS Kreise
- Betreiber: Statistische Ämter Bund/Länder (VGRdL)
- Thema: Wirtschaft
- Ebene: Landkreis
- Format: XLSX (~6,3 MB)
- URL: https://www.statistikportal.de/sites/default/files/2026-01/vgrdl_r2b1_bs2024.xlsx
- Lizenz: dl-de/by-2-0
- Stand: bis 2023; BS Feb 2025; Pub Nov 2025
- Aktualität: Update
- Inhalt: BIP und BWS (WZ-Gliederung) sowie Erwerbstätige kreisscharf; Zeitreihe ab 1990er. Geschwister R2B3 Einkommen / R2B2 Löhne bereits vorhanden.
- Warum spannend: Wirtschaftskraft und Produktivität je Kreis (BIP/EW, BWS-Struktur) — Kern-Score Wirtschaft; Join Kreisschlüssel/AGS.
- Status: verifiziert
- Notizen: Ranking-Dimension Wirtschaftskraft. URL war in Dedup als Geschwister-Notiz — eigene ID. RS-82111 Deep-Link leer.

### upd-kba-fz1
- Name: KBA FZ 1 Bestand nach Zulassungsbezirken + BEV/Dichte Open-Layer
- Betreiber: Kraftfahrt-Bundesamt
- Thema: Mobilität
- Ebene: Landkreis
- Format: XLSX / ArcGIS FeatureServer
- URL: https://mobilithek.info/mdp-api/files/aux/719473918361911296/fz1_2024.xlsx
- Lizenz: KBA Statistik / Open-Layer Statistikportal
- Stand: FZ1 Stichtag 01.01.2026 gelistet; Mobilithek-Spiegel 2024; ArcGIS Sample ab 2023
- Aktualität: Update
- Inhalt: Bestand Kfz/Pkw/NFZ nach Zulassungsbezirk; BEV/PHEV-Anteile und Pkw-Dichte in FeatureServer-Layern (`Schluessel_Zulbz`, `Pkw_BEV_Anteil`, `Dichte_Pkw`).
- Warum spannend: Pkw-Dichte, E-Auto-Quote — Mobilitäts-/Klima-Score; Join Zulassungsbezirk ≈ AGS.
- Status: verifiziert
- Notizen: Canonical kba.de fz1_2025/2026 SharedDocs **WAF 503**. FS BEV: `…/FZ%20Pkw%20mit%20Elektroantrieb%20Zulassungsbezirk/FeatureServer/0`. FS Dichte: `…/Fahrzeugdichte_gesamt/FeatureServer/0`.

### upd-destatis-realsteuer
- Name: Realsteuervergleich / Hebesätze Grundsteuer A/B + Gewerbesteuer (71231)
- Betreiber: Destatis / Regionalstatistik
- Thema: Wirtschaft
- Ebene: Gemeinde
- Format: XLSX Bericht / ffcsv (Deep-Link leer) / REST
- URL: Discovery — https://www.destatis.de/DE/Themen/Staat/Steuern/Steuereinnahmen/Publikationen/_publikationen-innen-steuer-real.html
- Lizenz: dl-de/by-2-0
- Stand: Bericht 2024 (Pub 21.08.2025); Hebesätze RS ab 31.12.2023
- Aktualität: Update
- Inhalt: Istaufkommen, Grundbeträge, Hebesätze Grundsteuer A/B und Gewerbesteuer; gemeindescharf in RDB 71231-03-01.
- Warum spannend: Gewerbe-/Grundsteuerlast — Standort-/Attraktivitäts-Score; AGS in 71231-03-01-5.
- Status: Account nötig
- Notizen: Discovery-only. Destatis-Static **WAF 403**; RS `71231-03-01-5/ffcsv` Body 0 Byte. Steering-Code „71131“ falsch — korrekt **71231**. Ranking-Dimension Steuerlast.

### upd-destatis-schulden
- Name: Schulden der Gemeinden / Gemeindeverbände (71327)
- Betreiber: Destatis / Regionalstatistik
- Thema: Wirtschaft
- Ebene: gemischt
- Format: ffcsv / REST
- URL: Discovery — https://www.regionalstatistik.de/genesis/online?operation=statistic&code=71327
- Lizenz: dl-de/by-2-0
- Stand: RDB 71327
- Aktualität: Update
- Inhalt: Kommunale Schulden Gemeinde/Kreis (erwartet laut Statistikbeschreibung).
- Warum spannend: Verschuldung pro Kopf — Finanz-Score.
- Status: Account nötig
- Notizen: Deep-Links `71327-01-01-4/5` Body 0 Byte. Ranking-Dimension Verschuldung.

### upd-bnetza-lps
- Name: BNetzA Ladesäulenregister
- Betreiber: Bundesnetzagentur
- Thema: Mobilität
- Ebene: gemischt
- Format: CSV (~53 MB) + XLSX (~30 MB); FeatureServer Token
- URL: https://data.bundesnetzagentur.de/Bundesnetzagentur/DE/Fachthemen/ElektrizitaetundGas/E-Mobilitaet/Ladesaeulenregister_BNetzA_2026-09-01.csv
- Lizenz: BNetzA Open-Data-Liste
- Stand: Dateiname 2026-09-01; Karte/Statistik Juli 2026
- Aktualität: Update
- Inhalt: Öffentliche Ladepunkte (Normal/Schnell), Leistung, Betreiber, Adresse/Koordinaten.
- Warum spannend: Ladeinfrastruktur-Dichte je Kreis/Gemeinde (Ladepunkte/EW) — Mobilitäts-/Energie-Score.
- Status: verifiziert
- Notizen: XLSX parallel. FeatureServer Layer 7 **Token Required**. Aggregation über Geo → VG250. Ranking-Dimension Ladeinfrastruktur.

### upd-bnetza-mobilfunk
- Name: BNetzA Mobilfunkmonitoring Gitterdaten
- Betreiber: Bundesnetzagentur
- Thema: Sonstiges
- Ebene: Raster
- Format: ZIP → CSV
- URL: https://data.bundesnetzagentur.de/Bundesnetzagentur/GIGA/DE/MobilfunkMonitoring/2512/202601_MobilfunkMonitoring.zip
- Lizenz: dl-de/by-2-0
- Stand: 2512 / Paket 202601
- Aktualität: Update
- Inhalt: Mobilfunkversorgung je Gitterzelle (`202601_MobilfunkMonitoring.csv` im ZIP, ~107 MB).
- Warum spannend: Mobilfunk-Abdeckung als Digital-Score neben Festnetz-Breitband-Gitter; Aggregation Mittelwert je Kreis.
- Status: verifiziert
- Notizen: Eigene ID (bisher nur Notiz unter Breitband-Gitter). Ranking-Dimension Mobilfunk-Versorgung.

### upd-ba-pendler
- Name: BA Pendleratlas / BBSR Pendeldistanzen
- Betreiber: BA Statistik; BBSR
- Thema: Mobilität
- Ebene: gemischt
- Format: interaktiv / XLSX (WAF)
- URL: Discovery — https://statistik.arbeitsagentur.de/DE/Navigation/Statistiken/Interaktive-Statistiken/Pendleratlas/Pendleratlas-Nav.html
- Lizenz: Statistik der BA / BBSR
- Stand: BBSR auf BA-Beschäftigtenstatistik 30.06.2024
- Aktualität: Update
- Inhalt: Ein-/Auspendler, Pendeldistanzen Kreise (erwartet); kein Open-CSV Deep-Link in Session.
- Warum spannend: Verflechtung / Arbeitsplatzzentren; Pendeldistanz als Mobilitätskosten-Proxy — nicht KWM-Wanderung.
- Status: unklar
- Notizen: BBSR-Seiten **WAF 503**; BA tableFetch Pendler **500**; RS-19321 leer. Ranking-Dimension Verflechtung/Pendeldistanz.

### upd-destatis-anschriften
- Name: Anschriftenverzeichnis Gemeinde-/Stadtverwaltungen 2026
- Betreiber: Destatis / Statistische Ämter Bund/Länder
- Thema: Geobasis
- Ebene: gemischt
- Format: XLSX (~1,7 MB)
- URL: https://www.statistikportal.de/sites/default/files/2026-05/20260131_Anschriften_der_Gemeinde_und_Stadtverwaltungen.xlsx
- Lizenz: freie Nutzung mit Quellenangabe (Statistikportal)
- Stand: 31.01.2026
- Aktualität: Update
- Inhalt: Name, ARS, AGS, Zustell-PLZ, Straße/Hausnr., E-Mail, Fläche, Bevölkerung je Regionaleinheit.
- Warum spannend: Stammdaten-Join-Key ARS/AGS + aktuelle Fläche/EW für Normierung.
- Status: verifiziert
- Notizen: Destatis GV100AD2QAktuell.zip **403 WAF**. Ranking-Dimension Stammdaten.

## C — Nischen / Spezial

(Weniger bekannte, aber ranking-relevante Quellen.)

### nisch-zensus-arcgis
- Name: Zensus 2022 Gitter ArcGIS FeatureServer (Heizung, Energieträger, Leerstand, Miete)
- Betreiber: Destatis / Esri (Spiegel)
- Thema: Wohnen
- Ebene: Raster
- Format: ArcGIS FeatureServer JSON
- URL: https://services2.arcgis.com/jUpNdisbWqRpMo35/arcgis/rest/services/Zensus2022_grid_final/FeatureServer?f=pjson
- Lizenz: dl-de/by-2-0
- Stand: 15.05.2022
- Aktualität: Nische
- Inhalt: FeatureServer mit Gitterattributen zu Heizungsart, Energieträger der Heizung, Leerstandsquote und durchschnittlicher Nettokaltmiete (€/m²). Destatis-Zips katalogbelegt, oft 403 — ArcGIS-Query als Access-Pfad.
- Warum spannend: Wärme-/Wohnungsdruck-Heatmaps und Kreis-Aggregation, wenn Destatis-Static WAF blockiert.
- Status: verifiziert
- Notizen: Destatis-Zips `Zensus2022_Heizungsart.zip`, `Zensus2022_Energietraeger.zip`, `Leerstandsquote_in_Gitterzellen.zip`, `Zensus2022_Durchschn_Nettokaltmiete.zip` katalogbelegt, oft 403.

### nisch-eba-laerm
- Name: EBA Umgebungslärm Runde 4 Schiene
- Betreiber: Eisenbahn-Bundesamt
- Thema: Umwelt
- Ebene: Raster
- Format: GeoPackage ZIP / WFS
- URL: https://geoinformation.eisenbahn-bundesamt.de/daten/INSPIRE/EnvHealthDeterminantMeasure_NoiseContours_v5_23880947-58cc-4207-8d83-211e6bbe09e9_V2_Gesamt.gpkg.zip
- Lizenz: dl-de/by-2-0
- Stand: Runde 4 (2022), Akt. 01.06.2023
- Aktualität: Nische
- Inhalt: Schienenlärm-Isophonen bundesweit (LDEN/LNight) als GeoPackage (~843 MB) und WFS. EEA END-XLSX-Pfade tot.
- Warum spannend: Lärmbelastungsanteil je Kreis/Gemeinde aus Flächenanteil der Isophonen — Umwelt-Score neben Luft/Dürre.
- Status: verifiziert
- Notizen: WFS `https://geoinformation.eisenbahn-bundesamt.de/wfs/eba/services/wfs`. EEA END-XLSX-Pfade tot.

### nisch-hwrm
- Name: HWRMRL Hochwassergefahren-/risikokarten DE
- Betreiber: BfG
- Thema: Umwelt
- Ebene: Raster
- Format: ArcGIS MapServer
- URL: https://geoportal.bafg.de/arcgis/rest/services/HWRMRL?f=pjson
- Lizenz: BfG / Wasserwirtschaft
- Stand: HWRMRL-Zyklus; live 2026
- Aktualität: Nische
- Inhalt: Hochwassergefahren- und -risikokarten (HQ-Szenarien) als ArcGIS-MapServer-Familie, z. B. `HWRMRL_DE_MH`. opengeodata.nrw Wasser-HW-Pfad 404.
- Warum spannend: Exposure-Anteil überschwemmungsgefährdeter Fläche je Kreis für Risiko-/Standort-Ranking.
- Status: verifiziert
- Notizen: Beispiel `…/HWRMRL/HWRMRL_DE_MH/MapServer`. opengeodata.nrw `…/wasser/hw/` 404.

### nisch-uba-eneff-waerme
- Name: UBA EnEff-RL Nutzenergiebedarf
- Betreiber: UBA
- Thema: Energie
- Ebene: Raster
- Format: ArcGIS MapServer
- URL: https://datahub.uba.de/server/rest/services/KlEn/EnEff_RL_Nutzenergiebedarf/MapServer?f=pjson
- Lizenz: UBA / dl-de/by-2-0
- Stand: live
- Aktualität: Nische
- Inhalt: Flächenhafter Nutzenergiebedarf Wärme (EnEff-RL) als Raster-MapServer (EPSG:25832). Schwesterdienst Kälte `KlEn/EnEff_RL_Nutzenergiebedarf_Kaelte`. Kein proprietärer Wärmeatlas nötig.
- Warum spannend: Wärmebedarfs-Heatmap und Kreis-Mittel als Kontext zu MaStR-EE und Zensus-Heizung.
- Status: verifiziert
- Notizen: Schwester `KlEn/EnEff_RL_Nutzenergiebedarf_Kaelte`.

### nisch-clc2018
- Name: CORINE Land Cover 2018
- Betreiber: UBA / EEA
- Thema: Umwelt
- Ebene: Raster
- Format: ArcGIS MapServer
- URL: https://datahub.uba.de/server/rest/services/Lu/CLC_2018/MapServer?f=pjson
- Lizenz: Copernicus/EEA
- Stand: 2018
- Aktualität: Nische
- Inhalt: CORINE Land Cover 2018 Klassen (Siedlung, Acker, Wald, Wasser u. a.) über UBA- und EEA-MapServer. CLC2021+ separat bei Copernicus.
- Warum spannend: Siedlungs-/Freiflächenanteil je Kreis für Umwelt- und Flächennutzungs-Indikatoren.
- Status: verifiziert
- Notizen: EEA `https://image.discomap.eea.europa.eu/arcgis/rest/services/Corine/CLC2018_WM/MapServer?f=pjson`. CLC2021+ separat Copernicus.

### nisch-erosion
- Name: UBA Bodenerosion BoFlLa
- Betreiber: UBA
- Thema: Umwelt
- Ebene: Raster
- Format: ArcGIS MapServer
- URL: https://datahub.uba.de/server/rest/services/BoFlLa/Erosion/MapServer?f=pjson
- Lizenz: UBA
- Stand: live
- Aktualität: Nische
- Inhalt: Bodenerosions-Layer `SDE.BoFlLa_Erosion` flächendeckend DE (MapServer). Ergänzt DWD-Dürre/Bodenfeuchte um Bodenthema.
- Warum spannend: Agrar-/Umwelt-Risikoanteil je Kreis aus Rasteraggregation.
- Status: verifiziert
- Notizen: Layer `SDE.BoFlLa_Erosion`.

### nisch-o3
- Name: UBA Ozon Jahresmittel OI_O3
- Betreiber: UBA
- Thema: Umwelt
- Ebene: Raster
- Format: ArcGIS MapServer
- URL: https://datahub.uba.de/server/rest/services/Lu/OI_O3_ANNUAL/MapServer?f=pjson
- Lizenz: UBA / dl-de
- Stand: Layer 2024
- Aktualität: Nische
- Inhalt: Ozon-Jahresmittel-Immissionsraster, u. a. Layer `OI_O3_2024_ANNUAL`. Dedup zu OI_NO2 aus dem Testkern.
- Warum spannend: Zweite Luftqualitäts-Achse für Umwelt-Ranking und Heatmaps neben NO₂/PM.
- Status: verifiziert
- Notizen: Layer `OI_O3_2024_ANNUAL`.

### nisch-pm25
- Name: UBA Feinstaub PM2.5 Jahresmittel
- Betreiber: UBA
- Thema: Umwelt
- Ebene: Raster
- Format: ArcGIS MapServer
- URL: https://datahub.uba.de/server/rest/services/Lu/OI_PM25/MapServer?f=pjson
- Lizenz: UBA / dl-de
- Stand: Layer 2024
- Aktualität: Nische
- Inhalt: PM2.5-Jahresmittel-Raster (u. a. 2024-Layer); verwandt `Lu/OI_PM10__ANNUAL`.
- Warum spannend: Feinstaub als Gesundheits-/Umwelt-Score je Kreis aus Rastermittel.
- Status: verifiziert
- Notizen: Auch `Lu/OI_PM10__ANNUAL`.

### nisch-bildungsmonitoring
- Name: Kommunale Bildungsdatenbank API
- Betreiber: Stat. Ämter Bund/Länder
- Thema: Bildung
- Ebene: Landkreis
- Format: API
- URL: https://www.bildungsmonitoring.de/bildungws/rest/2020/application.wadl
- Lizenz: dl-de/by-2-0
- Stand: live 2026
- Aktualität: Nische
- Inhalt: REST/WADL für kreisscharfe Bildungsstatistik (Schulen, Schüler, Hochschulen u. a. Themen der Kommunalen Bildungsdatenbank). tablefile seit 2025 nur mit Registrierung; kein öffentlicher All-Kreise-Flat-CSV.
- Warum spannend: Bundesweite Bildungsschicht für Kreisranking ohne Landes-Silo-Joins.
- Status: Account nötig
- Notizen: whoami 200. tablefile Registrierung seit 2025.

### nisch-nrw-schulen
- Name: NRW Schul-Open-Data Schulen/Schüler/Lehrkräfte
- Betreiber: Schulministerium NRW
- Thema: Bildung
- Ebene: Landkreis
- Format: CSV
- URL: https://www.schulministerium.nrw/system/files/media/document/file/opendata2025-26.csv
- Lizenz: OD-NRW / dl-de/by-2-0
- Stand: bis 2025
- Aktualität: Nische
- Inhalt: CSV mit Header u. a. `JAHR;SCHULFORM;KREIS;SCHULEN;SCHUELER_INNEN;…` (Lehrkräfte-Spalten im File). Nur NRW; kompensiert bundesweite Flat-CSV-Lücke.
- Warum spannend: Sofort joinbare Kreis-Bildungsmatrix NRW für Schülerdichte und Schulform-Mix.
- Status: verifiziert
- Notizen: Nur NRW. Header JAHR;SCHULFORM;KREIS;SCHULEN;SCHUELER_INNEN;…

### nisch-ldb-nrw-einkommen
- Name: LDB NRW 82411 verfügbares Einkommen
- Betreiber: IT.NRW
- Thema: Einkommen
- Ebene: gemischt
- Format: CSV
- URL: https://www.landesdatenbank.nrw.de/ldbnrwws/downloader/00/tables/82411-01i_00.csv
- Lizenz: dl-de/by-2-0
- Stand: 2023
- Aktualität: Nische
- Inhalt: Verfügbares Einkommen Tabelle 82411-01i für Gemeinde und Kreis in NRW als Direkt-CSV (Downloader-Pattern `…/tables/<code>_00.csv`). Workarounds für leere RS-/GENESIS-Deep-Links.
- Warum spannend: Gemeindefeines Einkommen-Ranking in NRW ohne GENESIS-UI; Muster für andere Länder-LDBs.
- Status: verifiziert
- Notizen: Downloader-Pattern `…/tables/<code>_00.csv` stabiler als `/online?operation=download`.

### nisch-ldb-nrw-pflege
- Name: LDB NRW 22411 Pflege ambulant
- Betreiber: IT.NRW
- Thema: Gesundheit
- Ebene: Landkreis
- Format: CSV
- URL: https://www.landesdatenbank.nrw.de/ldbnrwws/downloader/00/tables/22411-02i_00.csv
- Lizenz: dl-de/by-2-0
- Stand: 15.12.2023
- Aktualität: Nische
- Inhalt: Pflegebedürftige ambulant nach Pflegegrad, kreisscharf NRW (22411-02i). Destatis Kreisvergleich-Publikation eingestellt — LDB als Ersatzpfad.
- Warum spannend: Pflege-Nachfrage je Kreis für Gesundheits-/Sozial-Score in NRW.
- Status: verifiziert
- Notizen: Destatis Kreisvergleich-Publikation eingestellt.

### nisch-ldb-nrw-sgbxii
- Name: LDB NRW 22151 Grundsicherung Alter + 22811 Mindestsicherung
- Betreiber: IT.NRW
- Thema: Soziales
- Ebene: Gemeinde
- Format: CSV
- URL: https://www.landesdatenbank.nrw.de/ldbnrwws/downloader/00/tables/22151-01i_00.csv
- Lizenz: dl-de/by-2-0
- Stand: 2024–2026 / Quote bis 2024
- Aktualität: Nische
- Inhalt: Grundsicherung im Alter/Erwerbsminderung (22151-01i) gemeindescharf Quartale; Mindestsicherungsquote 22811-01i; Sozialhilfe-Ausgaben Kreise 22111-01i.
- Warum spannend: SGB-XII- und Armuts-Proxy unterhalb der Kreisebene für NRW-Gemeinde-Dashboard.
- Status: verifiziert
- Notizen: 22811-01i Quote; 22111-01i Sozialhilfe-Ausgaben Kreise.

### nisch-ldb-nrw-kh
- Name: LDB NRW 23111 Krankenhäuser/Betten
- Betreiber: IT.NRW
- Thema: Gesundheit
- Ebene: Landkreis
- Format: CSV
- URL: https://www.landesdatenbank.nrw.de/ldbnrwws/downloader/00/tables/23111-01i_00.csv
- Lizenz: dl-de/by-2-0
- Stand: aktueller LDB
- Aktualität: Nische
- Inhalt: Krankenhäuser und Betten kreisscharf NRW (23111-01i). Bund RS 23111-01-05-4 Deep-Link leer.
- Warum spannend: Stationäre Versorgungsdichte (Betten je EW) als Gesundheits-Ranking-Dimension NRW.
- Status: verifiziert
- Notizen: Bund RS 23111-01-05-4 Deep-Link leer.

### nisch-ldb-nrw-alo-gem
- Name: LDB NRW 13211 Arbeitslose Gemeinden
- Betreiber: IT.NRW
- Thema: Arbeitsmarkt
- Ebene: Gemeinde
- Format: CSV
- URL: https://www.landesdatenbank.nrw.de/ldbnrwws/downloader/00/tables/13211-05i_00.csv
- Lizenz: dl-de/by-2-0
- Stand: Katalog 2026-09-06
- Aktualität: Nische
- Inhalt: Arbeitslose nach Geschlecht gemeindescharf NRW (13211-05i). Zusatz: SvB nach WZ Kreise 13111-46i Stichtag 31.12.2025.
- Warum spannend: Gemeinde-ALO unterhalb BA-API (die Gemeinde nur mit Zertifikat liefert) für NRW-Drilldown.
- Status: verifiziert
- Notizen: 13111-46i SvB nach WZ Kreise Stichtag 31.12.2025.

### nisch-tourismus-45412
- Name: Tourismus Übernachtungen Kreise
- Betreiber: Regionalstatistik (45412-01-03-4)
- Thema: Wirtschaft
- Ebene: Landkreis
- Format: ffcsv
- URL: https://www.regionalstatistik.de/genesis/online/download/45412-01-03-4/ffcsv
- Lizenz: dl-de/by-2-0
- Stand: ab 2018; Sample 2024
- Aktualität: Nische
- Inhalt: Übernachtungen/Tourismusstatistik kreisscharf (45412-01-03-4). SN-Gemeinde-Pendant `45412-001M` via statistik.sachsen.de.
- Warum spannend: Tourismusintensität (Übernachtungen je EW) als Wirtschafts-Ranking-Achse.
- Status: verifiziert
- Notizen: SN Gemeinde `45412-001M` via statistik.sachsen.de GenOnline ffcsv.

### nisch-gewerbe-52311
- Name: Gewerbeanmeldungen/-abmeldungen Kreise
- Betreiber: Regionalstatistik (52311-01-04-4)
- Thema: Wirtschaft
- Ebene: Landkreis
- Format: ffcsv
- URL: https://www.regionalstatistik.de/genesis/online/download/52311-01-04-4/ffcsv
- Lizenz: dl-de/by-2-0
- Stand: bis 2025
- Aktualität: Nische
- Inhalt: Gewerbean- und -abmeldungen Jahressummen kreisscharf (`KREISE`). Hessen-CKAN-Spiegel `genesisws/downloader/…/52311-01-04-4_06.csv`.
- Warum spannend: Gründungs-/Abgangsnetto als Dynamik-Indikator im Wirtschaftsranking.
- Status: verifiziert
- Notizen: genesisws-Spiegel `…/genesisws/downloader/06/tables/52311-01-04-4_06.csv` (Hessen-CKAN).

### nisch-urs-52111
- Name: URS Niederlassungen nach Größenklassen Kreise
- Betreiber: Regionalstatistik (52111-01-02-4)
- Thema: Wirtschaft
- Ebene: Landkreis
- Format: ffcsv
- URL: https://www.regionalstatistik.de/genesis/online/download/52111-01-02-4/ffcsv
- Lizenz: dl-de/by-2-0
- Stand: ab 2019; Sample 2024
- Aktualität: Nische
- Inhalt: Unternehmensregister-Statistik: Niederlassungen nach Größenklassen kreisscharf. Weitere Codes: `52111-04-01-4` (WZ), `52111-05-*` Beschäftigte.
- Warum spannend: Unternehmensstruktur (KMU vs. Groß) als Standort- und Wirtschaftsprofil je Kreis.
- Status: verifiziert
- Notizen: Weitere `52111-04-01-4` (WZ), `52111-05-*` Beschäftigte.

### nisch-erwerb-13312
- Name: Erwerbstätige nach Wirtschaftsbereichen Kreise
- Betreiber: Regionalstatistik / VGRdL (13312-01-05-4)
- Thema: Arbeitsmarkt
- Ebene: Landkreis
- Format: ffcsv
- URL: https://www.regionalstatistik.de/genesis/online/download/13312-01-05-4/ffcsv
- Lizenz: dl-de/by-2-0
- Stand: Sample 2024
- Aktualität: Nische
- Inhalt: Erwerbstätige nach Wirtschaftsbereichen kreisscharf (Erwerbstätigenrechnung der Länder, 13312-01-05-4).
- Warum spannend: Sektorstruktur (Industrie/Dienstleistung/…) als Wirtschaftsprofil neben reiner Bevölkerungszahl.
- Status: verifiziert
- Notizen: Erwerbstätigenrechnung der Länder.

### nisch-vbb-gtfs
- Name: VBB GTFS Sollfahrplan
- Betreiber: VBB
- Thema: Mobilität
- Ebene: gemischt
- Format: GTFS ZIP
- URL: https://www.vbb.de/fileadmin/user_upload/VBB/Dokumente/API-Datensaetze/gtfs-mastscharf/GTFS.zip
- Lizenz: VBB Open Data
- Stand: laufend
- Aktualität: Nische
- Inhalt: GTFS Sollfahrplan Berlin-Brandenburg (~83 MB): Haltestellen, Linien, Fahrten. DELFI bundesweit nur nach Login — VBB als regionaler Direktlink.
- Warum spannend: ÖPNV-Angebotsdichte (Haltestellen/Fahrten je Gemeinde) für BE+BB-Mobilitäts-Score.
- Status: verifiziert
- Notizen: ~83 MB.

### nisch-energy-charts
- Name: Energy-Charts REST API
- Betreiber: Fraunhofer ISE
- Thema: Energie
- Ebene: gemischt
- Format: JSON API
- URL: https://api.energy-charts.info/installed_power?country=de&time_step=yearly
- Lizenz: Attribution (ENTSO-E/BNetzA u. a.)
- Stand: live; Projektion bis 2030
- Aktualität: Nische
- Inhalt: Nationale installierte Leistung und Ausbau-Zeitreihen (`installed_power`, auch `public_power`, `total_power`). Kein Kreisraster. HEAD 405 — GET nutzen.
- Warum spannend: Bundes-Benchmark und Zielpfad gegen regionale MaStR-Aggregate.
- Status: verifiziert
- Notizen: HEAD 405 — GET. Auch `public_power`, `total_power`. Kein Kreisraster.

### nisch-pvog
- Name: Open PVOG Export Digitale Verwaltung
- Betreiber: BMDS / FITKO (OZG Data Hub)
- Thema: Sonstiges
- Ebene: gemischt
- Format: API → CSV
- URL: https://api.ozg-umsetzung.de/api/public/v1/dash/open-pvog/{ars}/export
- Lizenz: öffentlicher Data-Hub
- Stand: täglich
- Aktualität: Nische
- Inhalt: Online-Leistungen / PVOG-Export je ARS (12-stellig); ARS-Liste `…/open-ars`. Accept `application/octet-stream` (text/csv → 406). Kein fertiger Bund-Score-CSV.
- Warum spannend: Digitalisierungsgrad der Verwaltung je Gemeinde/Kreis (ARS) als eigene Dashboard-Achse.
- Status: verifiziert
- Notizen: Accept `application/octet-stream` (text/csv → 406). ARS-Liste `…/open-ars`. UI dashboard-daten.digitale-verwaltung.de. Kein fertiger Bund-Score-CSV.

### nisch-ge250
- Name: BKG GE250 Gebietseinheiten 1:250000
- Betreiber: BKG
- Thema: Geobasis
- Ebene: gemischt
- Format: Shapefile ZIP
- URL: https://daten.gdz.bkg.bund.de/produkte/sonstige/ge250/aktuell/ge250.utm32s.shape.zip
- Lizenz: dl-de/by-2-0
- Stand: aktuell/
- Aktualität: Nische
- Inhalt: Gebietseinheiten 1:250000 inkl. Raumordnungsregionen / BBSR-Raumgliederungen — nicht VG250. Layer-Inventar im ZIP prüfen.
- Warum spannend: Join-Geometrie für ROR-Indikatoren (INKAR) oberhalb der Kreisebene.
- Status: verifiziert
- Notizen: Layer-Inventar im ZIP prüfen.

### nisch-inkar-ror
- Name: INKAR ROR-Gebiete + Indikatorenkatalog
- Betreiber: BBSR
- Thema: Geobasis
- Ebene: gemischt
- Format: JSON / XLSX
- URL: https://www.inkar.de/Wizard/GetGebieteZumRaumbezug/ROR
- Lizenz: dl-de/by-2-0
- Stand: Gebietsstand 31.12.2023 / INKAR 2025
- Aktualität: Nische
- Inhalt: ROR-Gebietsschlüssel als JSON; Indikatorenübersicht XLSX `Uebersicht der Indikatoren.xlsx`. Keine direkten Einzel-Indikator-File-URLs außer Bulk `kern-inkar-2025`.
- Warum spannend: Meta für Single-Indicator-Auswahl und ROR-Joins neben KRE/GEM.
- Status: verifiziert
- Notizen: Katalog `https://www.inkar.de/documents/Uebersicht%20der%20Indikatoren.xlsx`. TLS oft `-k`. Keine direkten Indikator-File-URLs außer Bulk.

### nisch-ltw-by-2023
- Name: Landtagswahl Bayern 2023 Stimmkreise
- Betreiber: Landeswahlleitung BY
- Thema: Wahlen
- Ebene: gemischt
- Format: CSV
- URL: https://landtagswahl2023.bayern.de/08_10_2023_Landtagswahl_2023_Stimmkreise_Bayern.csv
- Lizenz: Landeswahlleitung BY
- Stand: 08.10.2023
- Aktualität: Nische
- Inhalt: LTW Bayern 2023 Ergebnisse Stimmkreise (CSV, datumspräfigierter Dateiname). Wahlkreise-CSV und XML parallel.
- Warum spannend: Landespolitische Stimmenanteile für BY-Layer; Aggregation auf Kreis über Stimmkreis-Zuordnung.
- Status: verifiziert
- Notizen: Dateinamen datumspräfigiert. Wahlkreise-CSV + XML parallel.

### nisch-ltw-th-2024
- Name: Landtagswahl Thüringen 2024 Gemeinden
- Betreiber: TLS / Landeswahlleiter TH
- Thema: Wahlen
- Ebene: Gemeinde
- Format: XLSX
- URL: https://wahlen.thueringen.de/downloads/LWINFOG2024.xlsx
- Lizenz: TLS
- Stand: 2024-09-01
- Aktualität: Nische
- Inhalt: Endgültige LTW TH 2024 gemeindescharf (`LWINFOG2024.xlsx`); Wahlkreise in `LWINFO2024.xlsx`. HE/HH/SN landesweit ohne geprüften CSV in diesem Lauf.
- Warum spannend: Gemeinde-Wahlergebnisse TH direkt für Karte und Ranking ohne Wahlkreis-Aggregation.
- Status: verifiziert
- Notizen: `LWINFO2024.xlsx` Wahlkreise. HE/HH/SN landesweit ohne geprüften CSV in diesem Lauf.

### nisch-hh-sozialmon-2025
- Name: Hamburg Sozialmonitoring 2025 Ergebnistabellen
- Betreiber: FHH / Transparenzportal
- Thema: Soziales
- Ebene: Gemeinde
- Format: XLSX
- URL: https://daten.transparenz.hamburg.de/Dataport.HmbTG.ZS.Webservice.GetRessource100/GetRessource100.svc/ee0ecec9-c569-43ca-bc90-fc3f560ff1a9/Upload__Sozialmonitoring-Bericht_2025_Ergebnistabellen.XLSX
- Lizenz: dl-de-by-2.0
- Stand: 2025
- Aktualität: Nische
- Inhalt: Sozialmonitoring-Ergebnistabellen 2025 stadtteilscharf. Ergänzend: Regionalstatistik-WFS Stadtteile und Kita-Betreuungsquote OAF/WFS.
- Warum spannend: Kleinräumige Sozialindikatoren HH für Stadtteil-Dashboard und Benchmark zu Kreis-Bund-Layern.
- Status: verifiziert
- Notizen: Regionalstatistik WFS `https://geodienste.hamburg.de/wfs_regionalstatistische_daten_stadtteile`. Kita OAF `https://api.hamburg.de/datasets/v1/betreuungsquote_kindertagesbetreuung`.

### nisch-he-bildungsatlas
- Name: Hessischer Bildungsatlas WFS
- Betreiber: HMKB
- Thema: Bildung
- Ebene: gemischt
- Format: OGC WFS/WMS
- URL: https://gis.hmkblusd.de/ows/hsbk?VERSION=1.1.0&REQUEST=GetCapabilities&SERVICE=WFS
- Lizenz: dl-zero-de/2.0
- Stand: Katalog 2026-09-07
- Aktualität: Nische
- Inhalt: Schulbezirke und Bildungsstandorte Hessen als WFS/WMS (`hsbk`, `bildungsstandorte`). Kommunal: Darmstadt Sozialatlas 2025 XLSX.
- Warum spannend: Bildungslandschaft HE als Kartenlayer; kommunale Sozialdaten als Drilldown-Muster.
- Status: verifiziert
- Notizen: Bildungsstandorte `…/ows/bildungsstandorte`. Darmstadt Sozialatlas 2025 `https://opendata.darmstadt.de/sites/default/files/Daten_Sozialatlas_2025_0.xlsx`.

### nisch-by-energieatlas
- Name: Energie-Atlas Bayern EE-Anteil Stromverbrauch Gemeinde
- Betreiber: LfU Bayern
- Thema: Energie
- Ebene: Gemeinde
- Format: OGC WMS
- URL: https://www.lfu.bayern.de/gdi/wms/energieatlas/statistik_ee?REQUEST=GetCapabilities&SERVICE=WMS
- Lizenz: LfU / Energie-Atlas
- Stand: Layer 2025-08 / 2026-08
- Aktualität: Nische
- Inhalt: WMS-Layer Anteil EE am Stromverbrauch u. a. EE-Statistik gemeinde- und kreisscharf BY. Kein landesweiter Bulk-CSV; kommunale ArcGIS-Hub-CSVs (z. B. Cham) als Muster.
- Warum spannend: EE-Selbstversorgungsgrad je Gemeinde in BY für Energie-Ranking und Karte.
- Status: verifiziert
- Notizen: Kein landesweiter Bulk-CSV. Kommunal Cham ArcGIS Hub CSV als Muster.

### nisch-bw-svz-2024
- Name: MobiData BW SVZ-Zählstellen 2024
- Betreiber: MobiData BW
- Thema: Mobilität
- Ebene: gemischt
- Format: CSV
- URL: https://mobidata-bw.de/vm/Karte_Strassenverkehrszaehlung_BW/SVZ-Zaehlstellen_2026-06-26_augmented_SVZ2024.csv
- Lizenz: MobiData BW / daten.bw
- Stand: DTV2024; Datei 2026-06-26
- Aktualität: Nische
- Inhalt: Straßenverkehrszählstellen landesweit BW mit DTV2024; Endergebnisse-ZIP und Pkm/Zugkm-XLSX im Portal.
- Warum spannend: Verkehrslast-Punkte und Kreis-Aggregate für Mobilitäts-/Erreichbarkeitskontext BW.
- Status: verifiziert
- Notizen: Endergebnisse ZIP `…/Ergebnisse_2024_Excel.zip`. Pkm/Zugkm XLSX im Portal.

### nisch-sn-tourismus-gem
- Name: Statistik Sachsen Tourismus Gemeinden
- Betreiber: Statistik Sachsen
- Thema: Wirtschaft
- Ebene: Gemeinde
- Format: ffcsv
- URL: https://www.statistik.sachsen.de/genonline/online?sequenz=tabelleDownload&selectionname=45412-001M&regionalschluessel=.c&format=ffcsv
- Lizenz: amtlich
- Stand: Katalog 2026
- Aktualität: Nische
- Inhalt: Tourismus Gemeinden SN (45412-001M): Einrichtungen, Betten, Übernachtungen. `format=ffcsv` Pflicht; Range-Requests können 0 B. Unfälle Kreise `46241-208K`.
- Warum spannend: Gemeindescharfe Tourismusintensität SN unterhalb der Bundes-Kreistabelle.
- Status: verifiziert
- Notizen: `format=ffcsv` Pflicht; Range-Requests können 0 B. Unfälle Kreise `46241-208K`.

### nisch-zi-versorgungsatlas
- Name: Zi Versorgungsatlas (ärztliche Versorgung)
- Betreiber: Zi Berlin
- Thema: Gesundheit
- Ebene: gemischt
- Format: CSV/XLSX je Publikation
- URL: Discovery — https://www.versorgungsatlas.de/
- Lizenz: Zi / je Bericht
- Stand: Portal live
- Aktualität: Nische
- Inhalt: Analysen zu Versorgungskontakten, Arztzahlen je 100k EW; Bedarfsplanung primär KV-PDF — kein bundesweiter Bulk-API-Dump.
- Warum spannend: med. Unter-/Überversorgung — Gesundheits-Score.
- Status: Katalog-only
- Notizen: zi.de Open Data / Versorgungsatlas-Berichte. Ranking-Dimension med. Versorgung. Kein stabiler All-Kreise-Direct.

### nisch-dwd-hitzetage
- Name: DWD CDC Jahresgitter Hitzetage
- Betreiber: DWD
- Thema: Klima
- Ebene: Raster
- Format: ASC.GZ
- URL: https://opendata.dwd.de/climate_environment/CDC/grids_germany/annual/hot_days/grids_germany_annual_hot_days_2025_17.asc.gz
- Lizenz: CC BY 4.0
- Stand: 2025
- Aktualität: Nische
- Inhalt: Jahresgitter Anzahl Hitzetage Deutschland.
- Warum spannend: Klima-Exposure (Hitze) je Kreis aus Rastermittel.
- Status: verifiziert
- Notizen: Prio-1 Bonus. Ranking-Dimension Hitze-Exposure.

### nisch-dwd-strahlung
- Name: DWD CDC Jahresgitter Globalstrahlung
- Betreiber: DWD
- Thema: Klima
- Ebene: Raster
- Format: ZIP
- URL: https://opendata.dwd.de/climate_environment/CDC/grids_germany/annual/radiation_global/grids_germany_annual_radiation_global_2025.zip
- Lizenz: CC BY 4.0
- Stand: 2025
- Aktualität: Nische
- Inhalt: Jahresgitter Globalstrahlung.
- Warum spannend: Solarpotenzial-Proxy / Hitze-Kontext je Kreis.
- Status: verifiziert
- Notizen: Prio-1 Bonus. Ranking-Dimension Solar-/Strahlung.

## D — Portale / Discovery

(Metakataloge und Länderportale als Einstieg, plus konkrete Datensätze darunter.)

### portal-govdata
- Name: GovData
- Betreiber: GovData / BMI
- Thema: Sonstiges
- Ebene: gemischt
- Format: CKAN API / HTML
- URL: https://www.govdata.de/ckan/api/3/action/package_search
- Lizenz: Metadaten je Datensatz
- Stand: live
- Aktualität: Baseline
- Inhalt: Bundesweiter Metakatalog: Suche/Filter nach Organisation, Tags, Format; liefert Ressourcen-URLs und Lizenzfelder, keine Messwerte.
- Warum spannend: Einstieg für Dictionary-Nachzüge (Kreis/Gemeinde-Downloads) und Dedup gegen bestehende IDs.
- Status: Katalog
- Notizen: Startliste Bund. UI govdata.de/suche.

### portal-destatis
- Name: Destatis Genesis / Open Data / Zensus
- Betreiber: Destatis
- Thema: Demografie
- Ebene: gemischt
- Format: Portal
- URL: https://www.destatis.de/
- Lizenz: dl-de/by-2-0
- Stand: live
- Aktualität: Baseline
- Inhalt: Einstieg zu GENESIS, Open-Data-Bereich und Zensus-Gitter/Regionaltabellen. Static-Downloads oft WAF; REST in A.
- Warum spannend: Quelle für neue amtliche Tabellen und Zensus-Nebenprodukte, die nach A/C wandern.
- Status: Katalog
- Notizen: Static-Zensus oft WAF. GENESIS REST → A.

### portal-regionalstatistik
- Name: Regionaldatenbank Deutschland
- Betreiber: Stat. Ämter Bund/Länder
- Thema: Sonstiges
- Ebene: gemischt
- Format: Portal / API
- URL: https://www.regionalstatistik.de/genesis/online/
- Lizenz: dl-de/by-2-0
- Stand: live
- Aktualität: Baseline
- Inhalt: Katalog und Download von Kreis-/Gemeinde-Tabellen (ffcsv `…/download/<code>/ffcsv`) plus REST. Viele thematische Deep-Links leer — dann VGRdL/LDB.
- Warum spannend: Primärer Fundort für neue kreisscharfe Statistik-Codes, die als B/C-Einträge landen.
- Status: Katalog
- Notizen: Deep-Links für Nische oft leer; REST-Registrierung.

### portal-open-nrw
- Name: Open.NRW
- Betreiber: Land NRW
- Thema: Sonstiges
- Ebene: gemischt
- Format: Portal / CKAN
- URL: https://open.nrw/
- Lizenz: dl-de/by-2-0 / zero
- Stand: live
- Aktualität: Baseline
- Inhalt: Landesportal NRW inkl. Verweise auf LDB-Downloader und opengeodata.nrw (z. B. DVG2). Thematische CSVs oft über LDB-Pattern.
- Warum spannend: Pipeline für NRW-Nischen (Einkommen, Pflege, ALO-Gemeinde) in Abschnitt C.
- Status: Katalog
- Notizen: Discovery. DVG2 im Testkern. LDB-Downloader. Land-Kinder: `land-nw-*` (Quartiersatlas Düsseldorf, Grundversorger Gas, LDB 73111, progres.nrw, BuT Neuss u. a.) in E.

### portal-bayern
- Name: Bayern Open Data / open.bydata / Geoportal
- Betreiber: Freistaat Bayern
- Thema: Sonstiges
- Ebene: gemischt
- Format: Portal / Hub
- URL: https://open.bydata.de/
- Lizenz: je Datensatz
- Stand: live
- Aktualität: Baseline
- Inhalt: Discovery BY inkl. kommunaler ArcGIS-Hubs und Energie-Atlas-Dienste. Kein einheitliches Bulk-Schema.
- Warum spannend: Fundort für BY-Energie/LTW/kommunale Downloads, die nach C wandern.
- Status: Katalog
- Notizen: Discovery. Energie-Atlas WMS in C. Land-Kinder: `land-by-alkis-*`, `land-by-muenchen-*` (Indikatorenatlas) in E.

### portal-bw
- Name: Open Data Baden-Württemberg / daten.bw
- Betreiber: Land BW
- Thema: Sonstiges
- Ebene: gemischt
- Format: CKAN
- URL: https://www.daten-bw.de/ckan/api/3/action/package_search
- Lizenz: je Datensatz
- Stand: live
- Aktualität: Baseline
- Inhalt: CKAN-Suche BW (Base nicht Root `/api`). Mobility-Hub MobiData für Verkehr/ÖPNV.
- Warum spannend: Discovery für BW-Mobilitäts- und Fachdaten (z. B. SVZ) ins Dictionary.
- Status: Katalog
- Notizen: Discovery. CKAN-Base nicht Root `/api`. MobiData. Land-Kinder: `land-bw-*` (Stuttgart/Freiburg/Konstanz/Heidelberg, LUBW) in E.

### portal-hh
- Name: Transparenzportal / Open Data Hamburg
- Betreiber: FHH
- Thema: Sonstiges
- Ebene: Gemeinde
- Format: Portal / WFS
- URL: https://daten.transparenz.hamburg.de/
- Lizenz: dl-de/by-2-0
- Stand: live
- Aktualität: Baseline
- Inhalt: Stadtteil- und Fachdaten HH; Geodienste WFS und api.hamburg.de OAF. Sozialmonitoring und Kita als konkrete Hits in C.
- Warum spannend: Kleinräumige HH-Quellen für Dictionary-Einträge unter Soziales/Bildung.
- Status: Katalog
- Notizen: Discovery. geodienste/OAF. Sozialmonitoring/Kita in C. Land-Kinder: `land-hh-waermenetz-kwp`, `land-hh-wahlen-stadtteile`, `land-hh-laermkarten`, Bike/P+R in E.

### portal-he
- Name: Open Data Hessen
- Betreiber: Land Hessen
- Thema: Sonstiges
- Ebene: gemischt
- Format: Portal
- URL: https://opendata.hessen.de/
- Lizenz: je Datensatz (oft dl-zero-de/2.0)
- Stand: live
- Aktualität: Baseline
- Inhalt: Landes-Discovery HE; Bildungsatlas-Geodienste und kommunale Sozialatlanten (Darmstadt).
- Warum spannend: Nachzug HE-Bildung/Sozial für C; oft dl-zero-de/2.0.
- Status: Katalog
- Notizen: Discovery. Bildungsatlas in C. Land-Kinder: `land-he-darmstadt-*`, `land-he-ltw-gemeinden-14336`, Offenbach Zensus in E.

### portal-ni
- Name: Open Data Niedersachsen
- Betreiber: Land Niedersachsen
- Thema: Sonstiges
- Ebene: gemischt
- Format: Portal
- URL: https://www.opengeodata.niedersachsen.de/
- Lizenz: je Datensatz
- Stand: unklar
- Aktualität: Baseline
- Inhalt: Geodaten-/Open-Data-Einstieg NI. In diesem Lauf kaum frische thematische Direct-Downloads; GovData-Org `land-niedersachsen` dünn.
- Warum spannend: Watchlist für künftige NI-Kreis/Gemeinde-Hits (LGLN/LSN, Hannover, Braunschweig).
- Status: Katalog
- Notizen: Discovery. Land-Kinder: `land-ni-lgln-vwg-wfs`, `land-ni-pm10-2025`, Umweltkarten-ZIPs (IED, Umweltzonen, Fluglärm…) in E. Sozial/Wahl Direct weiter dünn.

### portal-sn
- Name: Open Data Sachsen
- Betreiber: Freistaat Sachsen
- Thema: Sonstiges
- Ebene: gemischt
- Format: EntryStore / GENESIS
- URL: https://opendata.sachsen.de/
- Lizenz: je Datensatz
- Stand: live
- Aktualität: Baseline
- Inhalt: Katalog plus statistik.sachsen.de GenOnline (ffcsv mit `format=ffcsv` + Voll-GET). Tourismus-Gemeinde und Unfälle als Hits.
- Warum spannend: SN-Gemeinde-Tabellen für Dictionary C unter Wirtschaft/Mobilität.
- Status: Katalog
- Notizen: Discovery. GenOnline `format=ffcsv`. Tourismus/Unfälle in C. Land-Kinder: `land-sn-vwg-shape`, `land-sn-baufertig-heiz`, `land-sn-svb-kreise`, ASV, Baustellen in E.

### portal-berlin
- Name: daten.berlin.de
- Betreiber: Land Berlin
- Thema: Sonstiges
- Ebene: Gemeinde
- Format: CKAN / Portal
- URL: https://daten.berlin.de/
- Lizenz: dl-de/by-2-0 / zero
- Stand: live
- Aktualität: Baseline
- Inhalt: Berliner Open-Data-Katalog; datenregister oft 403/429 — GovData-Spiegel nutzen. ALKIS-Bezirke im Testkern; thematisch GSSA, Liegenschaftsenergie.
- Warum spannend: Discovery BE-Bezirks-/Fachlayer für Dictionary und Geobasis-Joins.
- Status: Katalog
- Notizen: Discovery. ALKIS-Bezirke Testkern. Land-Kinder: `land-be-lor-*`, Umweltatlas Versorggrün/Dichte, infravelo, Verkehrsdetektion in E. AfSBBB `/opendata/*` derzeit SPA-Stub.

### portal-bremen
- Name: Transparenzportal Bremen
- Betreiber: Freie Hansestadt Bremen
- Thema: Sonstiges
- Ebene: gemischt
- Format: Portal
- URL: https://www.transparenz.bremen.de/
- Lizenz: je Datensatz
- Stand: live
- Aktualität: Baseline
- Inhalt: Transparenz-/Open-Data-Einstieg HB. In diesem Lauf keine brauchbaren 2024–2026 Direct-File-Hits.
- Warum spannend: Watchlist für Stadtteil/Sozial/Energie-Downloads, sobald Direct-URLs stabil sind.
- Status: Katalog
- Notizen: Discovery. Land-Kinder: `land-hb-kita`, `land-hb-haushalt-*`, `land-hb-zebra-zuwendungen` in E — thematisch weiter dünn; Geo Direct instabil.

### portal-sh
- Name: Open Data Schleswig-Holstein / Statistikamt Nord
- Betreiber: Land SH / Statistikamt Nord
- Thema: Sonstiges
- Ebene: gemischt
- Format: Portal / XLSX Direct
- URL: https://opendata.schleswig-holstein.de/
- Lizenz: je Datensatz
- Stand: live
- Aktualität: Baseline
- Inhalt: OD-SH CKAN + statistik-nord.de fileadmin (Kreismonitor, SH.regional, Statistikhefte).
- Warum spannend: Stärkste thematische Land-CSV/XLSX-Quelle im Länder-Crawl.
- Status: Katalog-only
- Notizen: Discovery. Land-Kinder: `land-sh-kreismonitor`, `land-sh-est-gemeinden`, frische 2026-Hefte (Tourismus/Gewerbe/Bau) in E.

### portal-rp
- Name: Open Data / Geobasis / Meine Heimat RLP
- Betreiber: Land RP / LVermGeo / Stat. Landesamt
- Thema: Sonstiges
- Ebene: gemischt
- Format: Portal / API / WFS
- URL: https://meine-heimat-statistik.de/api/docs/
- Lizenz: je Datensatz
- Stand: live
- Aktualität: Baseline
- Inhalt: Meine-Heimat-REST für Kreis/VG/Gemeinde-Indikatoren; LVermGeo WFS Verwaltungsgrenzen; wahlen.rlp.de LTW.
- Warum spannend: API + LTW 2026 + Grenzen — Top-Land im Crawl.
- Status: Katalog-only
- Notizen: Discovery. daten.rlp.de oft Verkündungs-PDFs. Land-Kinder: `land-rp-meine-heimat-api`, `land-rp-ltw2026-*`, `land-rp-*-shapezip` in E.

### portal-sl
- Name: Geoportal Saarland
- Betreiber: Land Saarland
- Thema: Sonstiges
- Ebene: gemischt
- Format: ArcGIS WFS
- URL: https://geoportal.saarland.de/
- Lizenz: je Datensatz (oft dl-de)
- Stand: live
- Aktualität: Baseline
- Inhalt: ArcGIS-WFS Gesundheit/Bildung/Verkehr/Energie/Naturschutz; wahlergebnis.saarland.de KERG.
- Warum spannend: Dichte Standort-/Netz-Layer landesweit.
- Status: Katalog-only
- Notizen: Discovery. Statistikamt oft 403. Land-Kinder: `land-sl-*` (Apotheken, Schulen, ÖPNV, Ladesäulen, EVU, NSG, LTW KERG) in E.

### portal-bb
- Name: DatenAdler / Geobasis BB (LGB)
- Betreiber: Land Brandenburg / LGB
- Thema: Sonstiges
- Ebene: gemischt
- Format: Portal / WFS
- URL: https://isk.geobasis-bb.de/ows/gazetteer_wfs?SERVICE=WFS&REQUEST=GetCapabilities
- Lizenz: dl-de/by-2-0
- Stand: live
- Aktualität: Baseline
- Inhalt: Gazetteer Gemeinden/Kreise/Kacheln; BORIS BRW; Schutzgebiete; bvdaten.csv.
- Warum spannend: Geobasis/Bodenmarkt; thematische Statistik dünn.
- Status: Katalog-only
- Notizen: Discovery. DatenAdler-Hub-API nicht offen. Land-Kinder: `land-bb-boris-brw`, `land-bb-gazetteer-*`, `land-bb-schutzgebiete`, `land-bb-bvdaten` in E.

### portal-mv
- Name: Geodaten-MV / OpenData.HRO
- Betreiber: LAiV MV / Hansestadt Rostock
- Thema: Sonstiges
- Ebene: gemischt
- Format: Portal / ZIP / CSV
- URL: https://www.geodaten-mv.de/
- Lizenz: CC-BY 4.0 GeoBasis-DE/MV / OpenData.HRO
- Stand: live
- Aktualität: Baseline
- Inhalt: DVG ZIP/WFS; Rostock OpenData Ämter/Teile/PLZ/Uferlinie; ORKa WMTS; DGM.
- Warum spannend: Landes-Geobasis jenseits dedupter Gemeinden/Kreise-CSV.
- Status: Katalog-only
- Notizen: Discovery. Gemeinden/Kreise-CSV Rostock = Dedup. Land-Kinder: `land-mv-dvg-*`, `land-mv-gemeindeverbaende`, `land-mv-uferlinie`, `land-mv-dgm` in E.

### portal-st
- Name: Geodatenportal Sachsen-Anhalt
- Betreiber: LVermGeo ST
- Thema: Sonstiges
- Ebene: gemischt
- Format: Portal / ZIP / CSV / WFS
- URL: https://www.geodatenportal.sachsen-anhalt.de/
- Lizenz: dl-de/by-2-0
- Stand: live
- Aktualität: Baseline
- Inhalt: DVG_ALKIS, Gemarkung-CSV, Hausumringe, BRW 2026, S2-Raster, INSPIRE AU.
- Warum spannend: Starke Geobasis/Bodenmarkt-Quelle Ost.
- Status: Katalog-only
- Notizen: Discovery. gfds datei/anzeigen oft Session-Cookie. Land-Kinder: `land-st-dvg-alkis`, `land-st-brw-2026`, `land-st-hausumringe`, `land-st-s2-raster` in E.

### portal-th
- Name: Geoportal Thüringen / geoproxy giz
- Betreiber: GDI-TH / TLBG
- Thema: Sonstiges
- Ebene: gemischt
- Format: Portal / ZIP / WFS
- URL: https://geoportal.thueringen.de/
- Lizenz: dl-de/by-2-0
- Stand: live
- Aktualität: Baseline
- Inhalt: Verwaltungsgrenzen ZIP/WFS; HU/HK; Nutzungsarten; giz Open GeoData (KH, Trink-/Abwasser, Bildung, Tourismus, Pflege).
- Warum spannend: Direct-ZIP-Suite statt Portal-Stubs.
- Status: Katalog-only
- Notizen: Discovery. LTW in C (`nisch-ltw-th-2024`). Land-Kinder: `land-th-*` in E.

### portal-edp
- Name: European Data Portal (DE-Filter)
- Betreiber: Publications Office / data.europa.eu
- Thema: Sonstiges
- Ebene: gemischt
- Format: Katalog
- URL: https://data.europa.eu/data/datasets?locale=de&country=de
- Lizenz: je Datensatz
- Stand: live
- Aktualität: Baseline
- Inhalt: EU-Metakatalog mit DE-Filter; oft Spiegel von GovData/Länderportalen, selten neue Direct-URLs.
- Warum spannend: Zweitkanal-Discovery und Abgleich, welche DE-Datensätze EU-seitig indexiert sind.
- Status: Katalog
- Notizen: Startliste. Oft Spiegel von GovData/Länder.

### portal-arcgis-hub
- Name: ArcGIS Hub kommunal
- Betreiber: Esri / Kommunen
- Thema: Sonstiges
- Ebene: gemischt
- Format: Hub API / CSV/GeoJSON
- URL: https://hub.arcgis.com/
- Lizenz: je Datensatz
- Stand: live
- Aktualität: Baseline
- Inhalt: Kommunale Hubs mit CSV/GeoJSON nur aufnehmen, wenn klare Direct-Download-URL vorliegt (Muster Cham EE-Anlagen).
- Warum spannend: Kommunale Feindaten und Muster-CSVs für Länder ohne landesweiten Bulk.
- Status: Katalog
- Notizen: Startliste: nur bei klarem Direct-Download.

## E — Länder-Datenpunkte

(Konkrete Direct-Hits aus `2026-09-07-laender.md`. Portale bleiben in D.)

### land-nw-duesseldorf-quartiersatlas-2024
- Name: Quartiersatlas Sozialer Handlungsbedarf & Fluktuation
- Betreiber: Land NRW / Kommunen
- Thema: Soziales
- Ebene: Gemeinde
- Format: CSV
- URL: https://opendata.duesseldorf.de/sites/default/files/Daten_QA_2024.csv
- Lizenz: dl-de/by-2-0
- Stand: 2024
- Aktualität: Neu
- Inhalt: Kennzahlen Sozialer Handlungsbedarf und Fluktuation je Sozialraum/Quartier.
- Warum spannend: Kleinräumiges Sozialmonitoring — Atlas-Join auf Sozialräume.
- Status: verifiziert
- Notizen: Land NW. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-nw-duesseldorf-sozialraeume-geojson
- Name: Sozialräume Quartiersatlas WGS84
- Betreiber: Land NRW / Kommunen
- Thema: Soziales
- Ebene: gemischt
- Format: GEOJSON
- URL: https://opendata.duesseldorf.de/sites/default/files/Sozialr%C3%A4ume_QA_Daten_WGS84_EPSG_4326.geojson
- Lizenz: dl-de/by-2-0
- Stand: 2024
- Aktualität: Neu
- Inhalt: Sozialraum-Polygone mit QA-Attributen (~2.2 MB).
- Warum spannend: Geometrie-Basis für Quartiersatlas ohne WMS.
- Status: verifiziert
- Notizen: Land NW. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-nw-bielefeld-arbeitsmarkt-bezirke
- Name: Arbeitsmarkt Stadtbezirke 31.12.2025
- Betreiber: Land NRW / Kommunen
- Thema: Arbeitsmarkt
- Ebene: Gemeinde
- Format: CSV
- URL: https://open-data.bielefeld.de/sites/default/files/arbeitsmarkt_stadtbezirke_31.12.2025.csv
- Lizenz: dl-de/by-2-0
- Stand: 2025-12-31
- Aktualität: Neu
- Inhalt: Arbeitsmarktindikatoren je Stadtbezirk.
- Warum spannend: Frischer bezirksscharfer Arbeitsmarkt.
- Status: verifiziert
- Notizen: Land NW. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-nw-dortmund-mietspiegel
- Name: Mietspiegel Fortschreibung API-CSV
- Betreiber: Land NRW / Kommunen
- Thema: Wohnen
- Ebene: gemischt
- Format: CSV
- URL: https://open-data.dortmund.de/api/v2/catalog/datasets/fb64-mietspiegel-2025-2026/exports/csv?use_labels=true
- Lizenz: dl-de/by-2-0
- Stand: 2025–2026
- Aktualität: Neu
- Inhalt: Mietspiegel als maschinenlesbarer API-Export (~736 KB).
- Warum spannend: Wohnen/Miete direkt downloadbar.
- Status: verifiziert
- Notizen: Land NW. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-nw-koeln-erhaltungssatzungen
- Name: Milieuschutz-Gebiete Shapefile
- Betreiber: Land NRW / Kommunen
- Thema: Soziales
- Ebene: Gemeinde
- Format: ZIP→SHP
- URL: https://www.offenedaten-koeln.de/sites/default/files/distribution/Soziale_Erhaltungssatzung_2.zip
- Lizenz: dl-de/by-2-0
- Stand: aktuell
- Aktualität: Neu
- Inhalt: Gebiete sozialer Erhaltungssatzungen.
- Warum spannend: Verdrängungsdruck räumlich abgrenzbar.
- Status: verifiziert
- Notizen: Land NW. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-nw-gelsenkirchen-alo-stadtteil
- Name: Arbeitslose Berichtsmonat × Stadtteil
- Betreiber: Land NRW / Kommunen
- Thema: Soziales
- Ebene: Gemeinde
- Format: CSV
- URL: https://gelsenkirchen.opendata.ruhr/dataset/e51a3ce9-94da-4f6b-ba25-7f090982ee73/resource/d0565560-9b51-420e-a07d-d87d14241bb9/download/stadt-gelsenkirchen_soziales_arbeitslose_stadtteil.csv
- Lizenz: dl-de/by-2-0
- Stand: Zeitreihe
- Aktualität: Neu
- Inhalt: Arbeitslose nach Stadtteil (~91 KB).
- Warum spannend: Hochfrequente kleinräumige ALO Ruhr.
- Status: verifiziert
- Notizen: Land NW. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-nw-ldb-est-73111
- Name: Einkommensteuerstatistik regional
- Betreiber: Land NRW / Kommunen
- Thema: Einkommen
- Ebene: Gemeinde
- Format: CSV
- URL: https://www.landesdatenbank.nrw.de/ldbnrwws/downloader/00/tables/73111-316i_00.csv
- Lizenz: dl-de/by-2-0
- Stand: bis 2018
- Aktualität: Neu
- Inhalt: Unbeschränkt Steuerpflichtige / ESt regional.
- Warum spannend: NEW LDB-Tabelle Einkommen.
- Status: verifiziert
- Notizen: Land NW. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-nw-mwike-energie-mobilitaet
- Name: Landesverwaltung Energie & Mobilität 2024/25
- Betreiber: Land NRW / Kommunen
- Thema: Energie
- Ebene: gemischt
- Format: CSV
- URL: https://www.wirtschaft.nrw/system/files/media/document/file/umfrage_energie_und_mobilitaetsverhalten_landesverwaltung_2024_2025.csv
- Lizenz: dl-de/by-2-0
- Stand: 2024/2025
- Aktualität: Neu
- Inhalt: Umfragedaten Energie/Mobilität (~3.1 MB).
- Warum spannend: Frischer Landesbezug MWIKE.
- Status: verifiziert
- Notizen: Land NW. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-nw-grundversorger-gas
- Name: Gas-Grundversorger je Gemeinde
- Betreiber: Land NRW / Kommunen
- Thema: Energie
- Ebene: Gemeinde
- Format: CSV
- URL: https://www.wirtschaft.nrw/system/files/media/document/file/grundversorger_gas_nrw_2025-2027.csv
- Lizenz: dl-de/by-2-0
- Stand: 2025–2027
- Aktualität: Neu
- Inhalt: Gas-Grundversorger mit AGS und PLZ.
- Warum spannend: Wärme/Energie gemeindescharf.
- Status: verifiziert
- Notizen: Land NW. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-nw-progres-innovation
- Name: Förderprojekte Innovation
- Betreiber: Land NRW / Kommunen
- Thema: Wirtschaft
- Ebene: gemischt
- Format: CSV
- URL: https://www.wirtschaft.nrw/system/files/media/document/file/progres.nrw_innovation_2025.csv
- Lizenz: dl-de/by-2-0
- Stand: 2025
- Aktualität: Neu
- Inhalt: Förderprojekte Titel/Ort/Programmteil.
- Warum spannend: Wirtschaft/Innovation landesspezifisch.
- Status: verifiziert
- Notizen: Land NW. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-by-muenchen-sgb2-alter
- Name: Indikatorenatlas Regel-Leistungsberechtigte Alter
- Betreiber: Freistaat Bayern / München
- Thema: Soziales
- Ebene: Gemeinde
- Format: CSV
- URL: https://opendata.muenchen.de/dataset/db1b4889-9b7f-498d-b96f-3883b87a5a83/resource/ea8d933b-8ae8-434c-95c2-9f9722b267e4/download/indikat_2605arbeitsmarkt_regel-leistungsberechtigte_nach_alter_18_05_26.csv
- Lizenz: dl-de/by-2-0
- Stand: 2025 / 05/2026
- Aktualität: Neu
- Inhalt: Regel-LB nach Altersgruppen × Bezirk (~112 KB).
- Warum spannend: SGB II kleinräumig.
- Status: verifiziert
- Notizen: Land BY. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-by-muenchen-svb
- Name: Indikatorenatlas SV-Beschäftigte
- Betreiber: Freistaat Bayern / München
- Thema: Arbeitsmarkt
- Ebene: Gemeinde
- Format: CSV
- URL: https://opendata.muenchen.de/dataset/d2656d0a-dba8-4ea7-bdec-705f47a7c49c/resource/8ae97812-e077-4bc4-8a21-a1deb2b61adb/download/indikat_2605arbeitsmarkt_sozialversicherungspflichtig_beschaeftigte_18_05_26.csv
- Lizenz: dl-de/by-2-0
- Stand: 2025 / 05/2026
- Aktualität: Neu
- Inhalt: SV-Beschäftigte bezirksscharf (~759 KB).
- Warum spannend: Arbeitsmarkt-Kernindikator München.
- Status: verifiziert
- Notizen: Land BY. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-by-muenchen-alo
- Name: Indikatorenatlas Arbeitslose
- Betreiber: Freistaat Bayern / München
- Thema: Soziales
- Ebene: Gemeinde
- Format: CSV
- URL: https://opendata.muenchen.de/dataset/4bed747e-a771-4f0d-a3a7-8aa9f78c95f3/resource/0f0ea161-c152-41a0-87da-35c737b66923/download/indikat_2605arbeitsmarkt_arbeitslose_18_05_26.csv
- Lizenz: dl-de/by-2-0
- Stand: 2025 / 05/2026
- Aktualität: Neu
- Inhalt: Arbeitslose nach Stadtbezirk (~623 KB).
- Warum spannend: Arbeitsmarktspannung kleinräumig.
- Status: verifiziert
- Notizen: Land BY. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-by-muenchen-kita
- Name: Indikatorenatlas Betreuungsangebot
- Betreiber: Freistaat Bayern / München
- Thema: Soziales
- Ebene: Gemeinde
- Format: CSV
- URL: https://opendata.muenchen.de/dataset/4c144e5f-6968-44e9-896c-04f7d488111f/resource/007510f2-a282-4edb-b5a3-7608e52f74a9/download/indikat_2605kinderbetreuung_betreuungsangebot_18_05_26.csv
- Lizenz: dl-de/by-2-0
- Stand: 2025 / 05/2026
- Aktualität: Neu
- Inhalt: Kita-Betreuungsangebot je Bezirk.
- Warum spannend: Bildung/Soziales Standortfaktor.
- Status: verifiziert
- Notizen: Land BY. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-by-muenchen-baufertig
- Name: Indikatorenatlas Baufertigstellungen
- Betreiber: Freistaat Bayern / München
- Thema: Wohnen
- Ebene: Gemeinde
- Format: CSV
- URL: https://opendata.muenchen.de/dataset/30c110e6-e16b-484f-ad94-2a8bea11db32/resource/47ea6e55-974e-483d-bf44-3f0593bd7ef7/download/indikat_2605bauen_baufertigstellungen_18_05_26.csv
- Lizenz: dl-de/by-2-0
- Stand: 2025 / 05/2026
- Aktualität: Neu
- Inhalt: Baufertigstellungen bezirksscharf (~361 KB).
- Warum spannend: Neubauaktivität kleinräumig.
- Status: verifiziert
- Notizen: Land BY. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-by-muenchen-soz-leistungen
- Name: Soziale Leistungen Zeitreihe
- Betreiber: Freistaat Bayern / München
- Thema: Soziales
- Ebene: gemischt
- Format: CSV
- URL: https://opendata.muenchen.de/dataset/145a8fcf-5fdf-4fb2-98c3-436b6adbd847/resource/bee7fdb6-7333-4c21-a3a7-d2fb96f61dfe/download/soziale_leistungen.csv
- Lizenz: dl-de/by-2-0
- Stand: Monatszeitreihe
- Aktualität: Neu
- Inhalt: Soziale Leistungen als Monatszahlen (~306 KB).
- Warum spannend: Hochfrequente Sozialstatistik.
- Status: verifiziert
- Notizen: Land BY. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-by-alkis-verwaltung
- Name: ALKIS® Verwaltungsgrenzen
- Betreiber: Freistaat Bayern / München
- Thema: Geobasis
- Ebene: Gemeinde
- Format: ZIP→SHP
- URL: https://geodaten.bayern.de/odd/m/4/verwaltung/alkis-verwaltung.zip
- Lizenz: dl-de/by-2-0 (BVV)
- Stand: ~2026-08
- Aktualität: Neu
- Inhalt: Offizielle ALKIS-Verwaltungsgrenzen (~63.6 MB).
- Warum spannend: Landes-Geobasis für Joins.
- Status: verifiziert
- Notizen: Land BY. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-by-alkis-katasterbezirk
- Name: ALKIS® Katasterbezirke
- Betreiber: Freistaat Bayern / München
- Thema: Geobasis
- Ebene: Gemeinde
- Format: ZIP→SHP
- URL: https://geodaten.bayern.de/odd/m/4/verwaltung/alkis-katasterbezirk.zip
- Lizenz: dl-de/by-2-0 (BVV)
- Stand: ~2026-08
- Aktualität: Neu
- Inhalt: Katasterbezirksgrenzen (~50.1 MB).
- Warum spannend: Feinere Geo-Joins.
- Status: verifiziert
- Notizen: Land BY. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-bw-freiburg-armutsgefaehrdung
- Name: Armutsgefährdungsquote Stadtbezirke
- Betreiber: Land BW / Kommunen
- Thema: Soziales
- Ebene: Gemeinde
- Format: CSV
- URL: https://fritz.freiburg.de/duva2dcat/dataset/de-bw-freiburg-grunddaten_zur_indikatorenbildung_auf_ebene_der_stadtbezirke_-_soziales_armutsgefaehrdungsquote/content.csv
- Lizenz: dl-de/by-2-0
- Stand: aktuell
- Aktualität: Neu
- Inhalt: Armutsgefährdungsquote Stadtbezirk.
- Warum spannend: Einkommen/Soziales kleinräumig.
- Status: verifiziert
- Notizen: Land BW. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-bw-freiburg-indikatoren-bezirke
- Name: Indikatorenset Stadtbezirke
- Betreiber: Land BW / Kommunen
- Thema: Soziales
- Ebene: Gemeinde
- Format: CSV
- URL: https://fritz.freiburg.de/duva2dcat/dataset/de-bw-freiburg-indikatoren_auf_ebene_der_stadtbezirke/content.csv
- Lizenz: dl-de/by-2-0
- Stand: aktuell
- Aktualität: Neu
- Inhalt: Breites Indikatorenset (~734 KB).
- Warum spannend: Multi-Themen-Paket Sozial/Demografie.
- Status: verifiziert
- Notizen: Land BW. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-bw-stuttgart-schulen
- Name: Öffentliche allgemeinbildende Schulen
- Betreiber: Land BW / Kommunen
- Thema: Bildung
- Ebene: Gemeinde
- Format: CSV
- URL: https://opendata.stuttgart.de/dataset/31d16ae9-606d-4585-83ed-fafd932d39db/resource/76e2bf93-6878-4721-b611-33132f85c652/download/oeffentliche_allgemeinbildende_schulen_klassen_und_schueler.csv
- Lizenz: dl-de/by-2-0
- Stand: aktuell
- Aktualität: Neu
- Inhalt: Schulen, Klassen, Schüler.
- Warum spannend: Bildung Schulstruktur.
- Status: verifiziert
- Notizen: Land BW. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-bw-stuttgart-sozialwohnungen
- Name: Bestand Sozialwohnungen
- Betreiber: Land BW / Kommunen
- Thema: Soziales
- Ebene: gemischt
- Format: CSV
- URL: https://opendata.stuttgart.de/dataset/a4be48a8-6aa7-45ca-adc5-9dd59b2d75af/resource/abf3fde1-1417-4a56-8632-8cfee270c8e3/download/sozialwohnungen.csv
- Lizenz: dl-de/by-2-0
- Stand: aktuell
- Aktualität: Neu
- Inhalt: Bestand Sozialwohnungen.
- Warum spannend: Geförderter Wohnraum.
- Status: verifiziert
- Notizen: Land BW. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-bw-stuttgart-energie-wasser
- Name: Energie- und Wasserverbrauch
- Betreiber: Land BW / Kommunen
- Thema: Energie
- Ebene: gemischt
- Format: CSV
- URL: https://opendata.stuttgart.de/dataset/1ec2bdaf-e4f0-4f94-8d9d-120f2da6e851/resource/03259f36-b1cc-4743-9ba1-c6ad9d445bbc/download/energie_und_wasserverbrauch.csv
- Lizenz: dl-de/by-2-0
- Stand: aktuell
- Aktualität: Neu
- Inhalt: Energie-/Wasserverbrauch Zeitreihe.
- Warum spannend: Energie/Umwelt kommunal.
- Status: verifiziert
- Notizen: Land BW. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-bw-stuttgart-wohnungsbestand
- Name: Wohnungsbestand Stadtbezirke seit 1995
- Betreiber: Land BW / Kommunen
- Thema: Einkommen
- Ebene: Gemeinde
- Format: CSV
- URL: https://opendata.stuttgart.de/dataset/d8884140-2158-4634-a384-3408091afec7/resource/969e0759-a0e6-45e6-8608-bf5a69ad4c30/download/tidy_bestand_wohnungen_stadtbezirke_seit_1995.csv
- Lizenz: dl-de/by-2-0
- Stand: seit 1995
- Aktualität: Neu
- Inhalt: Wohnungsbestand tidy Zeitreihe (~203 KB).
- Warum spannend: Wohnen bezirksscharf Historie.
- Status: verifiziert
- Notizen: Land BW. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-bw-konstanz-kita
- Name: Kinderbetreuung Plätze 2018–2024
- Betreiber: Land BW / Kommunen
- Thema: Soziales
- Ebene: Gemeinde
- Format: CSV
- URL: https://offenedaten-konstanz.de/sites/default/files/Kinderbetreuung_Plaetze_Stadtteile_2018-2024.csv
- Lizenz: Open Data Konstanz
- Stand: 2018–2024
- Aktualität: Neu
- Inhalt: Kita-Plätze nach Stadtteilen.
- Warum spannend: Bildung/Soziales kleinräumig.
- Status: verifiziert
- Notizen: Land BW. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-bw-heidelberg-ltw26
- Name: LTW26 Ergebnisse nach Gebieten
- Betreiber: Land BW / Kommunen
- Thema: Wahlen
- Ebene: Gemeinde
- Format: CSV
- URL: https://wahlergebnisse.komm.one/22/produktion/8221000/0/20260308/landtagswahl_kwl_1_wk/LTW26_Heidelberg_Ergebnisse_nach_Gebieten.csv
- Lizenz: amtlich / komm.one
- Stand: 2026-03-08
- Aktualität: Neu
- Inhalt: LTW26 Heidelberg gebietsscharf.
- Warum spannend: Sehr frische LTW26.
- Status: verifiziert
- Notizen: Land BW. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-bw-lubw-biosphaere
- Name: Biosphärengebiet-Zonen
- Betreiber: Land BW / Kommunen
- Thema: Umwelt
- Ebene: gemischt
- Format: ZIP→Geo
- URL: https://rips-datenlink.lubw.de/UDO_download/Biosphaerengebiet.zip
- Lizenz: dl-de/by-2-0 (LUBW)
- Stand: aktuell
- Aktualität: Neu
- Inhalt: Biosphärengebiet-Zonen (~2.6 MB).
- Warum spannend: Umwelt/Schutzgebiete landesspezifisch.
- Status: verifiziert
- Notizen: Land BW. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-he-darmstadt-sgb2-q1
- Name: SGB-II-Leistungen Quartal
- Betreiber: Land Hessen / Kommunen
- Thema: Soziales
- Ebene: gemischt
- Format: CSV
- URL: https://opendata.darmstadt.de/sites/default/files/SGB2_2026_Q1.csv
- Lizenz: dl-de/by-2-0
- Stand: 2026-Q1
- Aktualität: Neu
- Inhalt: SGB-II-Leistungen Quartalsstand.
- Warum spannend: Aktuelles Sozial/SGB II.
- Status: verifiziert
- Notizen: Land HE. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-he-darmstadt-alo-q2
- Name: Arbeitslosigkeit Quartal
- Betreiber: Land Hessen / Kommunen
- Thema: Soziales
- Ebene: gemischt
- Format: CSV
- URL: https://opendata.darmstadt.de/sites/default/files/ALO_2026_Q2.csv
- Lizenz: dl-de/by-2-0
- Stand: 2026-Q2
- Aktualität: Neu
- Inhalt: Arbeitslosigkeit Quartalsstand.
- Warum spannend: Frischer Quartalswert 2026.
- Status: verifiziert
- Notizen: Land HE. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-he-darmstadt-merkmalskatalog
- Name: Globaler Merkmalskatalog 29.07.2026
- Betreiber: Land Hessen / Kommunen
- Thema: Soziales
- Ebene: Gemeinde
- Format: CSV
- URL: https://opendata.darmstadt.de/sites/default/files/Merkmalskatalog_DA_29_07_2026.csv
- Lizenz: dl-de/by-2-0
- Stand: 2026-07-29
- Aktualität: Neu
- Inhalt: Schlüssel für DA-Indikatoren (~65 KB).
- Warum spannend: Metadaten/Atlas-Schema.
- Status: verifiziert
- Notizen: Land HE. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-he-ltw-gemeinden-14336
- Name: LTW Hessen gemeindescharf 14336-01-03-5
- Betreiber: Land Hessen / Kommunen
- Thema: Wahlen
- Ebene: Gemeinde
- Format: CSV
- URL: https://www.regionalstatistik.de/genesisws/downloader/06/tables/14336-01-03-5_06.csv
- Lizenz: dl-de/by-2-0
- Stand: LTW Genesis
- Aktualität: Neu
- Inhalt: Wahlberechtigte, Beteiligung, Stimmen × Gemeinde (~122 KB).
- Warum spannend: Landesweite HE-LTW mit AGS.
- Status: verifiziert
- Notizen: Land HE. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-he-offenbach-zensus
- Name: Zensus-2022-Grundinfo Bevölkerung
- Betreiber: Land Hessen / Kommunen
- Thema: Sonstiges
- Ebene: gemischt
- Format: XLSX
- URL: https://www.offenbach.de/medien/bindata/of/Statistik_und_wahlen_/dir-18/dir-38/20240625_Zensus-2022-Ergebnis-f.Offenbach-am-Main-Grundinfo-Bevoelkerung.xlsx
- Lizenz: dl-de/by-2-0
- Stand: Zensus 2022 / 2024-06
- Aktualität: Neu
- Inhalt: Zensus-Grundinfo Bevölkerung.
- Warum spannend: Kommunale Zensus-Auswertung HE.
- Status: verifiziert
- Notizen: Land HE. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-he-darmstadt-wohnungsbestand
- Name: Wohnungsbestand nach Zimmerzahl
- Betreiber: Land Hessen / Kommunen
- Thema: Einkommen
- Ebene: gemischt
- Format: CSV
- URL: https://opendata.darmstadt.de/sites/default/files/Wohnungsbestand_2025.csv
- Lizenz: dl-de/by-2-0
- Stand: 2025
- Aktualität: Neu
- Inhalt: Wohnungsbestand inkl. Salden mit AGS.
- Warum spannend: Wohnen NEW.
- Status: verifiziert
- Notizen: Land HE. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-ni-lgln-vwg-wfs
- Name: OpenGeoData.NI Verwaltungsgrenzen
- Betreiber: Land Niedersachsen / LGLN
- Thema: Geobasis
- Ebene: Gemeinde
- Format: OGC WFS
- URL: https://opendata.lgln.niedersachsen.de/doorman/noauth/verwaltungsgrenzen_wfs?SERVICE=WFS&REQUEST=GetCapabilities
- Lizenz: OpenGeoData LGLN
- Stand: live 2026-09-07
- Aktualität: Neu
- Inhalt: Polygone EPSG:25832; Layer ni_landkreise / ni_gemeinden.
- Warum spannend: Basis-Join NI Kreis/Gemeinde.
- Status: verifiziert
- Notizen: Land NI. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-ni-pm10-2025
- Name: Mittlere PM10-Belastung 5-Jahres-Mittel
- Betreiber: Land Niedersachsen / LGLN
- Thema: Gesundheit
- Ebene: Raster
- Format: ZIP
- URL: https://numis.niedersachsen.de/documents-ige-ng/ouk_ni/0FC3ED34-E945-4E01-8D7E-7FBB9243AE9E/Mittlere_PM10_Belastung_2025.zip
- Lizenz: Umweltkarten NI
- Stand: 2025
- Aktualität: Neu
- Inhalt: PM10-Belastungskarte flächendeckend.
- Warum spannend: Umwelt-/Gesundheitsindikator Raster.
- Status: verifiziert
- Notizen: Land NI. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-ni-umweltzonen
- Name: Umweltzonen in Niedersachsen
- Betreiber: Land Niedersachsen / LGLN
- Thema: Gesundheit
- Ebene: gemischt
- Format: ZIP
- URL: https://numis.niedersachsen.de/documents-ige-ng/ouk_ni/B883D3BA-4BA6-4C4B-A046-76A75731F646/Umweltzonen.zip
- Lizenz: Amtlich
- Stand: 2026-09-03
- Aktualität: Neu
- Inhalt: Geometrie der Umweltzonen.
- Warum spannend: Verkehr/Luftreinhaltung.
- Status: verifiziert
- Notizen: Land NI. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-ni-messstationen-luen
- Name: Standorte Luftmessnetz LÜN
- Betreiber: Land Niedersachsen / LGLN
- Thema: Gesundheit
- Ebene: gemischt
- Format: ZIP
- URL: https://numis.niedersachsen.de/documents-ige-ng/ouk_ni/DC9CAF92-8868-4E42-A718-C0EA9A99A5F0/Standorte_der_Messstationen_des_L%C3%9CN_UTM.zip
- Lizenz: Amtlich
- Stand: 2026-09-03
- Aktualität: Neu
- Inhalt: Luftmessnetz-Standorte.
- Warum spannend: Ankerpunkte Luftqualität.
- Status: verifiziert
- Notizen: Land NI. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-ni-ied-anlagen
- Name: Industrieemissions-Richtlinie Anlagen
- Betreiber: Land Niedersachsen / LGLN
- Thema: Umwelt
- Ebene: gemischt
- Format: ZIP
- URL: https://www.umweltkarten-niedersachsen.de/download_OE/GAV/IED_Anlagen.zip
- Lizenz: Amtlich
- Stand: 2026-09-03
- Aktualität: Neu
- Inhalt: Industrieanlagen IED.
- Warum spannend: Emissionslast ortsscharf.
- Status: verifiziert
- Notizen: Land NI. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-ni-fluglaerm
- Name: Fluglärmschutzbereiche
- Betreiber: Land Niedersachsen / LGLN
- Thema: Wohnen
- Ebene: gemischt
- Format: ZIP
- URL: https://numis.niedersachsen.de/documents-ige-ng/ouk_ni/E3BB2B79-FC47-46CE-98DD-F681FFD47EF5/Fluglaermschutzbereiche.zip
- Lizenz: Amtlich
- Stand: 2026-09-03
- Aktualität: Neu
- Inhalt: Lärmschutzbereiche Flughäfen.
- Warum spannend: Wohnen/Umwelt Hotspots.
- Status: verifiziert
- Notizen: Land NI. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-hh-wahlen-stadtteile
- Name: Wahlergebnisse zu Stadtteilen
- Betreiber: FHH
- Thema: Wahlen
- Ebene: Gemeinde
- Format: WFS + CSV-ZIP
- URL: https://geodienste.hamburg.de/download?url=https://geodienste.hamburg.de/HH_WFS_Statistik_Stadtteile_Wahlergebnisse&f=csv
- Lizenz: dl-de/by-2.0
- Stand: 2026-09-02
- Aktualität: Neu
- Inhalt: Wahlergebnisse kleinräumig; CSV-Bulk ~7.1 MB.
- Warum spannend: Politik/Sozialraum.
- Status: verifiziert
- Notizen: Land HH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-hh-luftmessnetz
- Name: Luftmessnetz WFS/OAF
- Betreiber: FHH
- Thema: Gesundheit
- Ebene: gemischt
- Format: WFS + OAF
- URL: https://geodienste.hamburg.de/HH_WFS_Luftmessnetz?SERVICE=WFS&REQUEST=GetCapabilities
- Lizenz: dl-de/by-2.0
- Stand: 2026-04-22
- Aktualität: Neu
- Inhalt: Messwerte/Stationen Luftqualität.
- Warum spannend: Umwelt/Gesundheit live-fähig.
- Status: verifiziert
- Notizen: Land HH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-hh-laermkarten
- Name: Strategische Lärmkarten §47c
- Betreiber: FHH
- Thema: Wohnen
- Ebene: Raster
- Format: GeoTIFF-ZIP
- URL: https://www.daten-hamburg.de/opendata/strategische_laermkarten/strategische_laermkarten.zip
- Lizenz: dl-de/zero-2.0
- Stand: 2026-07-21
- Aktualität: Neu
- Inhalt: Strategische Lärmkartierung.
- Warum spannend: Wohnen/Umwelt Belastung flächig.
- Status: verifiziert
- Notizen: Land HH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-hh-waermenetz-kwp
- Name: Kommunale Wärmeplanung Wärmenetz
- Betreiber: FHH
- Thema: Energie
- Ebene: Gemeinde
- Format: WFS + CSV
- URL: https://geodienste.hamburg.de/download?url=https://geodienste.hamburg.de/wfs_gebiete_mit_waermenetz_kwp&f=csv
- Lizenz: dl-de/by-2.0
- Stand: 2026-07-27
- Aktualität: Neu
- Inhalt: Wärmenetz-Gebiete KWP; CSV ~3.4 MB.
- Warum spannend: Energie/Wärme hochaktuell.
- Status: verifiziert
- Notizen: Land HH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-hh-bike-ride
- Name: B+R Anlagenstandorte
- Betreiber: FHH
- Thema: Mobilität
- Ebene: gemischt
- Format: WFS + CSV
- URL: https://geodienste.hamburg.de/download?url=https://geodienste.hamburg.de/HH_WFS_Bike_und_Ride&f=csv
- Lizenz: dl-de/by-2.0
- Stand: 2026-07-16
- Aktualität: Neu
- Inhalt: B+R-Anlagenstandorte.
- Warum spannend: Mobilität ÖPNV-Verknüpfung.
- Status: verifiziert
- Notizen: Land HH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-hh-park-ride
- Name: P+R Anlagen
- Betreiber: FHH
- Thema: Mobilität
- Ebene: gemischt
- Format: WFS + CSV
- URL: https://geodienste.hamburg.de/download?url=https://geodienste.hamburg.de/HH_WFS_P_und_R&f=csv
- Lizenz: dl-de/by-2.0
- Stand: 2026-07-27
- Aktualität: Neu
- Inhalt: P+R-Anlagen.
- Warum spannend: Mobilität MIV–ÖPNV.
- Status: verifiziert
- Notizen: Land HH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-hh-regionalstatistik-bulk
- Name: Regionalstatistische Daten Stadtteile
- Betreiber: FHH
- Thema: Sonstiges
- Ebene: Gemeinde
- Format: CSV-ZIP + OAF
- URL: https://geodienste.hamburg.de/download?url=https://geodienste.hamburg.de/wfs_regionalstatistische_daten_stadtteile&f=csv
- Lizenz: dl-de/by-2.0
- Stand: 2026-09-02
- Aktualität: Neu
- Inhalt: Regionalstatistische Indikatoren; Bulk ~12.4 MB (WFS-URL dedup — Delivery neu).
- Warum spannend: Demografie/Sozio Bulk.
- Status: verifiziert
- Notizen: Land HH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-hb-kita
- Name: KiTa-Einrichtungen XLSX
- Betreiber: Freie Hansestadt Bremen
- Thema: Soziales
- Ebene: gemischt
- Format: XLSX
- URL: https://www.transparenz.bremen.de/sixcms/media.php/bremen02.a.13.de/download/KiTa_Daten.xlsx
- Lizenz: Transparenzportal Bremen
- Stand: Portal modified 2015
- Aktualität: Neu
- Inhalt: Kita-Standorte/-daten.
- Warum spannend: Bildung/Sozialraum — seltene HB Direct-File.
- Status: verifiziert
- Notizen: Land HB. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-hb-haushalt-ist-2025
- Name: IST-Haushaltsdaten 2025
- Betreiber: Freie Hansestadt Bremen
- Thema: Wirtschaft
- Ebene: Gemeinde
- Format: CSV
- URL: https://www.finanzen.bremen.de/sixcms/media.php/bremen02.a.13.de/download/IST-Daten_2025_Stadtgemeinde_Bremen.csv
- Lizenz: Transparenz/Finanzen
- Stand: 2025 / Pub 2026-06
- Aktualität: Neu
- Inhalt: IST-Haushaltsdaten.
- Warum spannend: Governance/Finanzen frisch.
- Status: verifiziert
- Notizen: Land HB. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-hb-zebra-zuwendungen
- Name: Quartalsbericht Zuwendungen
- Betreiber: Freie Hansestadt Bremen
- Thema: Wirtschaft
- Ebene: gemischt
- Format: CSV
- URL: https://www.finanzen.bremen.de/sixcms/media.php/bremen02.a.13.de/download/20260701-Quartalsbericht_ZEBRA_2-2026.csv
- Lizenz: Portal
- Stand: 2026-07-01
- Aktualität: Neu
- Inhalt: Zuwendungsdaten.
- Warum spannend: Frischste HB-Transparenz-Serie.
- Status: verifiziert
- Notizen: Land HB. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-be-lor-shapefiles
- Name: Lebensweltlich orientierte Räume 2021
- Betreiber: Land Berlin
- Thema: Soziales
- Ebene: Gemeinde
- Format: 7z→SHP
- URL: https://www.berlin.de/sen/stadt/_assets/stadtdaten/stadtwissen/lebensweltlich-orientierte-raeume/lor_2021-01-01_k3_shapefiles_nur_id.7z
- Lizenz: CC-BY-3.0 DE
- Stand: 01.01.2021
- Aktualität: Neu
- Inhalt: Drei Hierarchie-Ebenen LOR (542 PLR).
- Warum spannend: Sozialraum-Basis ≠ ALKIS.
- Status: verifiziert
- Notizen: Land BE. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-be-lor-wfs
- Name: LOR Downloaddienst WFS
- Betreiber: Land Berlin
- Thema: Soziales
- Ebene: gemischt
- Format: OGC WFS
- URL: https://gdi.berlin.de/services/wfs/lor_2021?SERVICE=WFS&REQUEST=GetCapabilities
- Lizenz: CC-BY-3.0 DE
- Stand: Geometrie 2021
- Aktualität: Neu
- Inhalt: API-Zugriff Sozialraum-Geometrie.
- Warum spannend: Sozialraum API.
- Status: verifiziert
- Notizen: Land BE. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-be-ua-versorggruen
- Name: Versorgungsgrün WFS
- Betreiber: Land Berlin
- Thema: Wohnen
- Ebene: gemischt
- Format: WFS
- URL: https://gdi.berlin.de/services/wfs/ua_versorggruen_2020?SERVICE=WFS&REQUEST=GetCapabilities
- Lizenz: Berlin Open Data
- Stand: 2020
- Aktualität: Neu
- Inhalt: Grünversorgung.
- Warum spannend: Umwelt/Wohnqualität.
- Status: verifiziert
- Notizen: Land BE. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-be-ua-dichte
- Name: GFZ/Dichte WFS
- Betreiber: Land Berlin
- Thema: Wohnen
- Ebene: gemischt
- Format: WFS
- URL: https://gdi.berlin.de/services/wfs/ua_staedtebauliche_dichte2019?SERVICE=WFS&REQUEST=GetCapabilities
- Lizenz: Berlin Open Data
- Stand: 2019
- Aktualität: Neu
- Inhalt: GFZ/Dichte.
- Warum spannend: Wohnen/Siedlungsstruktur.
- Status: verifiziert
- Notizen: Land BE. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-be-lichtenberg-energie-2024
- Name: Energieverbrauch bezirklicher Gebäude
- Betreiber: Land Berlin
- Thema: Energie
- Ebene: Gemeinde
- Format: XLSX
- URL: https://www.berlin.de/ba-lichtenberg/service/daten/energieverbrauchsuebersicht-bezirklicher-gebaeude-2024.xlsx
- Lizenz: Berlin Open Data
- Stand: 2024
- Aktualität: Neu
- Inhalt: Energieverbrauch Liegenschaften.
- Warum spannend: Energie — frischer Bezirksschnitt.
- Status: verifiziert
- Notizen: Land BE. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-be-infravelo
- Name: infravelo Projekte API
- Betreiber: Land Berlin
- Thema: Mobilität
- Ebene: gemischt
- Format: JSON API
- URL: https://www.infravelo.de/api/v1/projects/
- Lizenz: Berlin OD / infravelo
- Stand: live
- Aktualität: Neu
- Inhalt: Radprojekte/-maßnahmen (~70 KB JSON).
- Warum spannend: Mobilität aktuell.
- Status: verifiziert
- Notizen: Land BE. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-be-verkehrsdetektion
- Name: TEU Detektor-Standorte
- Betreiber: Land Berlin
- Thema: Mobilität
- Ebene: gemischt
- Format: JSON
- URL: https://api.viz.berlin.de/daten/verkehrsdetektion/teu_standorte.json
- Lizenz: Berlin / VIZ
- Stand: live
- Aktualität: Neu
- Inhalt: Detektor-Standorte.
- Warum spannend: Verkehr Sensornetz.
- Status: verifiziert
- Notizen: Land BE. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sn-vwg-shape
- Name: vwg_sn.zip Kreis/Gemeinde
- Betreiber: Freistaat Sachsen
- Thema: Geobasis
- Ebene: Gemeinde
- Format: ZIP→SHP
- URL: https://geocloud.landesvermessung.sachsen.de/public.php/dav/files/r36FazxKWiB7Qq3/vwg_sn.zip
- Lizenz: GeoSN dl-de/by-2-0
- Stand: 2025-01-01
- Aktualität: Neu
- Inhalt: Geometrien LK/Gem ETRS89/UTM33 (~338 KB).
- Warum spannend: Grenzen Basis-Join.
- Status: verifiziert
- Notizen: Land SN. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sn-inspire-au-wfs
- Name: Verwaltungseinheiten Download-WFS
- Betreiber: Freistaat Sachsen
- Thema: Geobasis
- Ebene: gemischt
- Format: OGC WFS
- URL: https://geodienste.sachsen.de/aaa/public_inspire/alkis/au/dls/wfs?SERVICE=WFS&REQUEST=GetCapabilities
- Lizenz: GeoSN / INSPIRE
- Stand: live
- Aktualität: Neu
- Inhalt: Verwaltungsgrenzen aus ALKIS.
- Warum spannend: API-Zugang Grenzen.
- Status: verifiziert
- Notizen: Land SN. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sn-baustellen-geojson
- Name: Sperrungen/Umleitungen SPERRINFOSYS
- Betreiber: Freistaat Sachsen
- Thema: Wohnen
- Ebene: gemischt
- Format: ZIP→GeoJSON
- URL: http://www.list.smwa.sachsen.de/gdi/download/baustelleninfo/Baustelleninfo_Sachsen_geojson.zip
- Lizenz: SMWA / Open Data
- Stand: live
- Aktualität: Neu
- Inhalt: Aktuelle Sperrungen/Baustellen (~2 MB).
- Warum spannend: Mobilität Netzstörungslage.
- Status: verifiziert
- Notizen: Land SN. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sn-kh-diagnosen
- Name: 23131-019K Diagnosen Wohnort
- Betreiber: Freistaat Sachsen
- Thema: Gesundheit
- Ebene: Landkreis
- Format: ffcsv
- URL: https://www.statistik.sachsen.de/genonline/online?sequenz=tabelleDownload&selectionname=23131-019K&regionalschluessel=.c&format=ffcsv
- Lizenz: Amtlich
- Stand: Zeitreihe
- Aktualität: Neu
- Inhalt: Patienten Diagnosen × Kreis (~2.4 MB).
- Warum spannend: Gesundheit kreisscharf.
- Status: verifiziert
- Notizen: Land SN. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sn-baufertig-heiz
- Name: 31121-311Z Fertigstellungen Heizenergie
- Betreiber: Freistaat Sachsen
- Thema: Wohnen
- Ebene: Gemeinde
- Format: ffcsv
- URL: https://www.statistik.sachsen.de/genonline/online?sequenz=tabelleDownload&selectionname=31121-311Z&regionalschluessel=.c&format=ffcsv
- Lizenz: Amtlich
- Stand: Sample 2025
- Aktualität: Neu
- Inhalt: Fertigstellungen × Heizenergie × Gemeinde (~1.1 MB).
- Warum spannend: Wohnen + Energie gemeindescharf.
- Status: verifiziert
- Notizen: Land SN. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sn-baugen-heiz
- Name: 31111-311Z Genehmigungen Heizenergie
- Betreiber: Freistaat Sachsen
- Thema: Wohnen
- Ebene: Gemeinde
- Format: ffcsv
- URL: https://www.statistik.sachsen.de/genonline/online?sequenz=tabelleDownload&selectionname=31111-311Z&regionalschluessel=.c&format=ffcsv
- Lizenz: Amtlich
- Stand: Sample 2025
- Aktualität: Neu
- Inhalt: Genehmigungen × Heizenergie (~923 KB).
- Warum spannend: Wohnen/Energie Frühindikator.
- Status: verifiziert
- Notizen: Land SN. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sn-gewerbe-an
- Name: 52311-105K Anmeldungen WZ08
- Betreiber: Freistaat Sachsen
- Thema: Wirtschaft
- Ebene: Landkreis
- Format: ffcsv
- URL: https://www.statistik.sachsen.de/genonline/online?sequenz=tabelleDownload&selectionname=52311-105K&regionalschluessel=.c&format=ffcsv
- Lizenz: Amtlich
- Stand: Sample 2026
- Aktualität: Neu
- Inhalt: Gewerbeanmeldungen × WZ × Kreis.
- Warum spannend: Wirtschaft dynamisch.
- Status: verifiziert
- Notizen: Land SN. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sn-svb-kreise
- Name: 13111-150K SvB Arbeitsort
- Betreiber: Freistaat Sachsen
- Thema: Arbeitsmarkt
- Ebene: Landkreis
- Format: ffcsv
- URL: https://www.statistik.sachsen.de/genonline/online?sequenz=tabelleDownload&selectionname=13111-150K&regionalschluessel=.c&format=ffcsv
- Lizenz: Amtlich
- Stand: 30.06.2025
- Aktualität: Neu
- Inhalt: Beschäftigte Geschlecht/WZ × Kreis.
- Warum spannend: Arbeitsmarkt kreisscharf.
- Status: verifiziert
- Notizen: Land SN. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sn-ust-kreise
- Name: 73311-024K USt
- Betreiber: Freistaat Sachsen
- Thema: Einkommen
- Ebene: Landkreis
- Format: ffcsv
- URL: https://www.statistik.sachsen.de/genonline/online?sequenz=tabelleDownload&selectionname=73311-024K&regionalschluessel=.c&format=ffcsv
- Lizenz: Amtlich
- Stand: Sample 2024
- Aktualität: Neu
- Inhalt: USt-Voranmeldungen × WZ × Kreis.
- Warum spannend: Wirtschaft Steuerbasis.
- Status: verifiziert
- Notizen: Land SN. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sn-asv-csv
- Name: ASV CSV Export
- Betreiber: Freistaat Sachsen
- Thema: Bildung
- Ebene: gemischt
- Format: CSV
- URL: https://www.lds.sachsen.de/lfabf/csv
- Lizenz: Freistaat Sachsen / LDS
- Stand: live
- Aktualität: Neu
- Inhalt: Ausbildungsstätten Bildungsgang/Adresse (~1.2 MB).
- Warum spannend: Bildung Standortliste.
- Status: verifiziert
- Notizen: Land SN. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-st-dvg-alkis
- Name: DVG_ALKIS.zip
- Betreiber: LVermGeo ST
- Thema: Geobasis
- Ebene: Gemeinde
- Format: ZIP→SHP
- URL: https://www.geodatenportal.sachsen-anhalt.de/gfds_webshare/download/LVermGeo/Geodatenportal/externedaten/DVG_ALKIS.zip
- Lizenz: dl-de/by-2-0
- Stand: tagaktuell
- Aktualität: Neu
- Inhalt: Digitale Verwaltungsgrenzen (~8.4 MB).
- Warum spannend: Grenzen Kern-Geometrie ST.
- Status: verifiziert
- Notizen: Land ST. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-st-kg-gemarkung
- Name: kreis_gemeinde_gemarkung.csv
- Betreiber: LVermGeo ST
- Thema: Geobasis
- Ebene: Gemeinde
- Format: CSV
- URL: https://www.geodatenportal.sachsen-anhalt.de/gfds_webshare/download/LVermGeo/Geodatenportal/externedaten/kreis_gemeinde_gemarkung.csv
- Lizenz: dl-de/by-2-0
- Stand: tagaktuell
- Aktualität: Neu
- Inhalt: Schlüssel knr;kreis;gmdnr;gemeinde;gmknr;gemarkung.
- Warum spannend: Lookup-Tabelle Joins.
- Status: verifiziert
- Notizen: Land ST. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-st-tn-uebersicht
- Name: Topographische Namen Übersicht
- Betreiber: LVermGeo ST
- Thema: Soziales
- Ebene: Gemeinde
- Format: CSV
- URL: https://www.geodatenportal.sachsen-anhalt.de/gfds_webshare/download/LVermGeo/Geodatenportal/externedaten/TN_%C3%9Cbersicht-ST%2020251231.csv
- Lizenz: dl-de/by-2-0
- Stand: 2025-12-31
- Aktualität: Neu
- Inhalt: Namens-/Schlüsselkatalog (~1.7 MB).
- Warum spannend: Frischer Schlüsselkatalog.
- Status: verifiziert
- Notizen: Land ST. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-st-hausumringe
- Name: Hausumringe landesweit
- Betreiber: LVermGeo ST
- Thema: Wohnen
- Ebene: gemischt
- Format: ZIP
- URL: https://www.geodatenportal.sachsen-anhalt.de/gfds_webshare/download/LVermGeo/Geodatenportal/content/Hausumringe.zip
- Lizenz: dl-de/by-2-0
- Stand: Open-Data
- Aktualität: Neu
- Inhalt: Gebäudeumringe (~142 MB).
- Warum spannend: Wohnen Siedlungsstruktur.
- Status: verifiziert
- Notizen: Land ST. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-st-brw-2026
- Name: BRW-Zonen ZIP
- Betreiber: LVermGeo ST
- Thema: Wirtschaft
- Ebene: gemischt
- Format: ZIP
- URL: https://geodatenportal.sachsen-anhalt.de/gfds/datei/anzeigen/id/646905,501/brw_20260101_v1.zip
- Lizenz: dl-de/by-2-0
- Stand: 2026-01-01
- Aktualität: Neu
- Inhalt: Bodenrichtwertzonen (~51 MB; Session-Cookie).
- Warum spannend: Wohnen/Wirtschaft Bodenmarkt.
- Status: verifiziert
- Notizen: Land ST. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-st-inspire-au-wfs
- Name: Verwaltungseinheiten WFS
- Betreiber: LVermGeo ST
- Thema: Geobasis
- Ebene: gemischt
- Format: OGC WFS
- URL: https://geodatenportal.sachsen-anhalt.de/ows_INSPIRE_LVermGeo_ATKIS_AU_WFS?service=wfs&version=2.0.0&request=getcapabilities
- Lizenz: dl-de/by-2-0 / INSPIRE
- Stand: live
- Aktualität: Neu
- Inhalt: Verwaltungseinheiten Feature-Download.
- Warum spannend: API-Alternative DVG.
- Status: verifiziert
- Notizen: Land ST. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-st-s2-raster
- Name: S2R20 LSA 2026-05-02
- Betreiber: LVermGeo ST
- Thema: Klima
- Ebene: Raster
- Format: ZIP
- URL: https://www.geodatenportal.sachsen-anhalt.de/gfds_webshare/download/LVermGeo/Geodatenportal/Online-Bereitstellung-LVermGeo/Fernerkundung/S2R20_LSA_20260502.zip
- Lizenz: dl-de/by-2-0
- Stand: 2026-05-02
- Aktualität: Neu
- Inhalt: Landesweites S2-Raster (~1.5 GB).
- Warum spannend: Umwelt/Raster frisch.
- Status: verifiziert
- Notizen: Land ST. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-th-vg-zip
- Name: ALKIS-Abgabe Verwaltungsgrenzen
- Betreiber: GDI-TH / TLBG
- Thema: Geobasis
- Ebene: Gemeinde
- Format: ZIP→SHP
- URL: https://geoportal.geoportal-th.de/ALKIS/Verwaltungsgrenzen_Thueringen.zip
- Lizenz: dl-de/by-2-0
- Stand: jährlich / Meta 2026-03
- Aktualität: Neu
- Inhalt: Gebietsübersichten (~33.8 MB).
- Warum spannend: Grenzen Direct-ZIP.
- Status: verifiziert
- Notizen: Land TH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-th-grenzueb-wfs
- Name: Gebietsübersichten WFS
- Betreiber: GDI-TH / TLBG
- Thema: Geobasis
- Ebene: Gemeinde
- Format: OGC WFS
- URL: https://www.geoproxy.geoportal-th.de/geoproxy/services/GRENZUEB_wfs?SERVICE=WFS&REQUEST=GetCapabilities
- Lizenz: dl-de/by-2-0
- Stand: live
- Aktualität: Neu
- Inhalt: Feature-Download Verwaltungsgrenzen.
- Warum spannend: Grenzen API.
- Status: verifiziert
- Notizen: Land TH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-th-hu-zip
- Name: Hausumringe
- Betreiber: GDI-TH / TLBG
- Thema: Wohnen
- Ebene: gemischt
- Format: ZIP
- URL: https://geoportal.geoportal-th.de/hausko_umr/HU-TH.zip
- Lizenz: dl-de/by-2-0
- Stand: Open-Data
- Aktualität: Neu
- Inhalt: Gebäudeumringe (~174 MB).
- Warum spannend: Wohnen.
- Status: verifiziert
- Notizen: Land TH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-th-hk-zip
- Name: Hauskoordinaten
- Betreiber: GDI-TH / TLBG
- Thema: Wohnen
- Ebene: gemischt
- Format: ZIP
- URL: https://geoportal.geoportal-th.de/hausko_umr/HK-TH.zip
- Lizenz: dl-de/by-2-0
- Stand: Open-Data
- Aktualität: Neu
- Inhalt: Hauskoordinaten (~10.9 MB).
- Warum spannend: Wohnen/Digital Geocoding.
- Status: verifiziert
- Notizen: Land TH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-th-nas
- Name: Nutzungsartenstatistik TH
- Betreiber: GDI-TH / TLBG
- Thema: Wirtschaft
- Ebene: Gemeinde
- Format: ZIP
- URL: https://geoportal.thueringen.de/media/Download/Kataloge/Nutzungsartenstatistik-TH.zip
- Lizenz: dl-de/by-2-0
- Stand: Katalog
- Aktualität: Neu
- Inhalt: Flächennutzungsarten-Statistik.
- Warum spannend: Umwelt/Wohnen/Wirtschaft Landnutzung.
- Status: verifiziert
- Notizen: Land TH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-th-krankenhaus
- Name: Krankenhausstandorte giz
- Betreiber: GDI-TH / TLBG
- Thema: Gesundheit
- Ebene: gemischt
- Format: ZIP→SHP
- URL: https://www.geoproxy.geoportal-th.de/download-service/opendata/giz/krankenhaus.zip
- Lizenz: Open GeoData TH
- Stand: giz Feed
- Aktualität: Neu
- Inhalt: Krankenhausstandorte.
- Warum spannend: Gesundheit.
- Status: verifiziert
- Notizen: Land TH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-th-trinkwasser
- Name: Trinkwasser-Gebiete
- Betreiber: GDI-TH / TLBG
- Thema: Gesundheit
- Ebene: gemischt
- Format: ZIP→SHP
- URL: https://www.geoproxy.geoportal-th.de/download-service/opendata/giz/trinkwasser.zip
- Lizenz: Open GeoData TH
- Stand: giz Feed
- Aktualität: Neu
- Inhalt: Trinkwasser-Versorgungsgebiete (~2.5 MB).
- Warum spannend: Umwelt/Gesundheit/Wohnen.
- Status: verifiziert
- Notizen: Land TH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-th-abwasser
- Name: Abwasser-Gebiete
- Betreiber: GDI-TH / TLBG
- Thema: Umwelt
- Ebene: gemischt
- Format: ZIP→SHP
- URL: https://www.geoproxy.geoportal-th.de/download-service/opendata/giz/abwasser.zip
- Lizenz: Open GeoData TH
- Stand: giz Feed
- Aktualität: Neu
- Inhalt: Abwasser-Entsorgungsgebiete (~2.6 MB).
- Warum spannend: Umwelt/Infrastruktur.
- Status: verifiziert
- Notizen: Land TH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sh-kreismonitor
- Name: 33 Indikatoren Kreis
- Betreiber: Statistikamt Nord / Land SH
- Thema: Sonstiges
- Ebene: Landkreis
- Format: XLSX
- URL: https://www.statistik-nord.de/fileadmin/Dokumente/KreismonitorSH_Indikatoren2005_2024.xlsx
- Lizenz: Statistikamt Nord
- Stand: bis 2024
- Aktualität: Neu
- Inhalt: Bevölkerung, Sozial, Wohnen, Bildung, Wirtschaft, Tourismus, Fläche.
- Warum spannend: Multi-Thema kreisscharf — Top-Hit SH.
- Status: verifiziert
- Notizen: Land SH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sh-regional-bev
- Name: Bevölkerung Regionalband
- Betreiber: Statistikamt Nord / Land SH
- Thema: Sonstiges
- Ebene: Gemeinde
- Format: XLSX
- URL: https://www.statistik-nord.de/fileadmin/Dokumente/NORD.regional/Schleswig-Holstein.regional/Band_1_-_Bevoelkerung/SH_regional_Band_1_2023.xlsx
- Lizenz: Statistikamt Nord
- Stand: 2023
- Aktualität: Neu
- Inhalt: Bevölkerungsdaten gemeinde-/kreisscharf (~587 KB).
- Warum spannend: Demografie-Basis.
- Status: verifiziert
- Notizen: Land SH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sh-regional-wohnen
- Name: Bau und Wohnen Regionalband
- Betreiber: Statistikamt Nord / Land SH
- Thema: Wohnen
- Ebene: Gemeinde
- Format: XLSX
- URL: https://www.statistik-nord.de/fileadmin/Dokumente/NORD.regional/Schleswig-Holstein.regional/Band_2_-_Bau%2C_Wohnen/SH_regional_Band_2_2022.xlsx
- Lizenz: Statistikamt Nord
- Stand: 2022
- Aktualität: Neu
- Inhalt: Bau- und Wohnungskennzahlen.
- Warum spannend: Wohnen.
- Status: verifiziert
- Notizen: Land SH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sh-est-gemeinden
- Name: ESt-Statistik Gemeinden
- Betreiber: Statistikamt Nord / Land SH
- Thema: Einkommen
- Ebene: Gemeinde
- Format: XLSX
- URL: https://www.statistik-nord.de/fileadmin/Dokumente/Lohn-_und_Einkommensteuerstatistik_in_Schleswig-Holstein_2022_nach_Gemeinden.xlsx
- Lizenz: Statistikamt Nord
- Stand: 2022
- Aktualität: Neu
- Inhalt: Steuerstatistik gemeindescharf.
- Warum spannend: Einkommen — seltener Direct-File.
- Status: verifiziert
- Notizen: Land SH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sh-bauland-2025
- Name: M I 6 - j 25 SH
- Betreiber: Statistikamt Nord / Land SH
- Thema: Wirtschaft
- Ebene: Landkreis
- Format: XLSX
- URL: https://www.statistik-nord.de/fileadmin/Dokumente/M_I_6_j25_SH.xlsx
- Lizenz: Statistikamt Nord
- Stand: 2025
- Aktualität: Neu
- Inhalt: Bauland-Kaufwerte.
- Warum spannend: Wohnen/Wirtschaft.
- Status: verifiziert
- Notizen: Land SH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sh-beherbergung-2026-06
- Name: G IV 1 Reiseverkehr
- Betreiber: Statistikamt Nord / Land SH
- Thema: Wirtschaft
- Ebene: Landkreis
- Format: XLSX
- URL: https://www.statistik-nord.de/fileadmin/Dokumente/G_IV_1-m_26-06_SH.xlsx
- Lizenz: Statistikamt Nord
- Stand: 2026-06
- Aktualität: Neu
- Inhalt: Ankünfte/Übernachtungen.
- Warum spannend: Tourismus sehr frisch.
- Status: verifiziert
- Notizen: Land SH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sh-unfaelle-2025
- Name: H I 1 - j 25 SH
- Betreiber: Statistikamt Nord / Land SH
- Thema: Mobilität
- Ebene: Landkreis
- Format: XLSX
- URL: https://www.statistik-nord.de/fileadmin/Dokumente/H_I_1_j_25_SH.xlsx
- Lizenz: Statistikamt Nord
- Stand: 2025
- Aktualität: Neu
- Inhalt: Unfallstatistik (~830 KB).
- Warum spannend: Mobilität.
- Status: verifiziert
- Notizen: Land SH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sh-gewerbe-2026q2
- Name: D I 2 Gewerbe
- Betreiber: Statistikamt Nord / Land SH
- Thema: Wirtschaft
- Ebene: Landkreis
- Format: XLSX
- URL: https://www.statistik-nord.de/fileadmin/Dokumente/D_I_2-vj_2_26_SH.xlsx
- Lizenz: Statistikamt Nord
- Stand: 2026 Q2
- Aktualität: Neu
- Inhalt: Gewerbean-/abmeldungen.
- Warum spannend: Wirtschaft.
- Status: verifiziert
- Notizen: Land SH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sh-baugen-2026-06
- Name: F II 1 Baugenehmigungen
- Betreiber: Statistikamt Nord / Land SH
- Thema: Wohnen
- Ebene: Landkreis
- Format: XLSX
- URL: https://www.statistik-nord.de/fileadmin/Dokumente/F_II_1_m_06_26_SH.xlsx
- Lizenz: Statistikamt Nord
- Stand: 2026-06
- Aktualität: Neu
- Inhalt: Baugenehmigungen.
- Warum spannend: Wohnen.
- Status: verifiziert
- Notizen: Land SH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sh-zufish-area
- Name: Area AGS/RS
- Betreiber: Statistikamt Nord / Land SH
- Thema: Geobasis
- Ebene: Gemeinde
- Format: CSV
- URL: https://opendata.schleswig-holstein.de/data/zufish/area_2020-10-29.csv
- Lizenz: Open Data SH
- Stand: 2020-10-29
- Aktualität: Neu
- Inhalt: Area-IDs mit ags, rs, Parent (~726 KB).
- Warum spannend: Grenzen/Digital Join-Keys.
- Status: verifiziert
- Notizen: Land SH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sh-atkis-vwg-wfs
- Name: OpenGBD VWG WFS
- Betreiber: Statistikamt Nord / Land SH
- Thema: Geobasis
- Ebene: gemischt
- Format: OGC WFS
- URL: https://dienste.gdi-sh.de/WFS_SH_ATKIS_BDLM_VWG_OpenGBD?service=wfs&version=2.0.0&request=getCapabilities
- Lizenz: Open Geobasisdaten SH
- Stand: live
- Aktualität: Neu
- Inhalt: VWG-Layer aus Basis-DLM.
- Warum spannend: Grenzen.
- Status: verifiziert
- Notizen: Land SH. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-rp-gemeinden-shapezip
- Name: Verwaltungsgrenzen Gemeinden
- Betreiber: Land RP / LVermGeo
- Thema: Geobasis
- Ebene: Gemeinde
- Format: ZIP→SHP
- URL: http://geo5.service24.rlp.de/wfs/verwaltungsgrenzen_rp.fcgi?request=GetFeature&TYPENAME=gemeinde_rlp&VERSION=1.1.0&SERVICE=WFS&OUTPUTFORMAT=SHAPEZIP
- Lizenz: dl-de/by-2-0 · LVermGeoRP
- Stand: live
- Aktualität: Neu
- Inhalt: Amtliche Gemeindepolygone (~13 MB).
- Warum spannend: Grenzen Basis-Join.
- Status: verifiziert
- Notizen: Land RP. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-rp-landkreise-shapezip
- Name: Verwaltungsgrenzen Landkreise
- Betreiber: Land RP / LVermGeo
- Thema: Geobasis
- Ebene: Landkreis
- Format: ZIP→SHP
- URL: http://geo5.service24.rlp.de/wfs/verwaltungsgrenzen_rp.fcgi?request=GetFeature&TYPENAME=landkreise_rlp&VERSION=1.1.0&SERVICE=WFS&OUTPUTFORMAT=SHAPEZIP
- Lizenz: dl-de/by-2-0
- Stand: live
- Aktualität: Neu
- Inhalt: Kreis-/KS-Grenzen (~2.3 MB).
- Warum spannend: Grenzen Kreisprofile.
- Status: verifiziert
- Notizen: Land RP. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-rp-verbandsgemeinden
- Name: VG-Polygone
- Betreiber: Land RP / LVermGeo
- Thema: Sonstiges
- Ebene: Gemeinde
- Format: ZIP→SHP
- URL: http://geo5.service24.rlp.de/wfs/verwaltungsgrenzen_rp.fcgi?request=GetFeature&TYPENAME=verbandsgemeinde_rlp&VERSION=1.1.0&SERVICE=WFS&OUTPUTFORMAT=SHAPEZIP
- Lizenz: dl-de/by-2-0
- Stand: live
- Aktualität: Neu
- Inhalt: VG-Polygone (~3.8 MB).
- Warum spannend: RP-spezifische Zwischenstufe.
- Status: verifiziert
- Notizen: Land RP. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-rp-gemarkungsverzeichnis
- Name: gkverz_rlp.xlsx
- Betreiber: Land RP / LVermGeo
- Thema: Geobasis
- Ebene: gemischt
- Format: XLSX
- URL: https://lvermgeo.rlp.de/fileadmin/lvermgeo/pdf/open-data/gkverz_rlp.xlsx
- Lizenz: dl-de/by-2-0
- Stand: 2026
- Aktualität: Neu
- Inhalt: Verzeichnis der Gemarkungen.
- Warum spannend: Kataster-Schlüssel.
- Status: verifiziert
- Notizen: Land RP. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-rp-inspire-au
- Name: dlkm_au Administrative Units
- Betreiber: Land RP / LVermGeo
- Thema: Geobasis
- Ebene: gemischt
- Format: ZIP
- URL: https://geobasis-rlp.de/data/inspire-annexi/current/xml/dlkm_au.zip
- Lizenz: dl-de/by-2-0
- Stand: current
- Aktualität: Neu
- Inhalt: INSPIRE AU (~33.8 MB).
- Warum spannend: EU-HVD-konform.
- Status: verifiziert
- Notizen: Land RP. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-rp-meine-heimat-api
- Name: REST Export Statistik
- Betreiber: Land RP / LVermGeo
- Thema: Sonstiges
- Ebene: Gemeinde
- Format: CSV/FLATCSV API
- URL: https://meine-heimat-statistik.de/api/v1/export
- Lizenz: Stat. Landesamt RLP
- Stand: 2026-09-07
- Aktualität: Neu
- Inhalt: Bevölkerung, Einkommen, Wohnfläche, Soziales, Bildung, Gesundheit, Tourismus, LTW — ARS-gesteuert.
- Warum spannend: Eine API für viele Atlas-Indikatoren.
- Status: verifiziert
- Notizen: Land RP. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-rp-ltw2026-stimmbezirk
- Name: Endergebnis Stimmbezirksebene
- Betreiber: Land RP / LVermGeo
- Thema: Wahlen
- Ebene: Gemeinde
- Format: CSV
- URL: https://www.wahlen.rlp.de/fileadmin/wahlen.rlp.de/dokumente-wahlen/ltw/Ergebnisdateien/2026/LW_2026_Endergebnis_Stimmbezirksebene.csv
- Lizenz: Landeswahlleiter RLP
- Stand: 2026-03-22
- Aktualität: Neu
- Inhalt: Vollständige Stimmen (~3.0 MB).
- Warum spannend: Frischeste LTW hochauflösend.
- Status: verifiziert
- Notizen: Land RP. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-rp-ltw2026-lk
- Name: Endgültiges Ergebnis LK/KS
- Betreiber: Land RP / LVermGeo
- Thema: Wahlen
- Ebene: Landkreis
- Format: XLSX
- URL: https://www.wahlen.rlp.de/fileadmin/wahlen.rlp.de/dokumente-wahlen/ltw/Ergebnisdateien/2026/Endgueltiges_Ergebnis_LW_2026_LK_KS.xlsx
- Lizenz: Landeswahlleiter RLP
- Stand: 2026-03-22
- Aktualität: Neu
- Inhalt: Kreis-/KS-Ergebnisse.
- Warum spannend: LTW kreisscharf Ranking.
- Status: verifiziert
- Notizen: Land RP. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sl-apotheken
- Name: Apothekenstandorte
- Betreiber: Geoportal Saarland
- Thema: Gesundheit
- Ebene: gemischt
- Format: WFS/GML
- URL: https://geoportal.saarland.de/arcgis/services/Internet/Gesundheit/MapServer/WFSServer?SERVICE=WFS&VERSION=1.1.0&REQUEST=GetFeature&TYPENAME=Gesundheit:Apotheken
- Lizenz: Geoportal SL
- Stand: live
- Aktualität: Neu
- Inhalt: Apothekenstandorte landesweit.
- Warum spannend: Gesundheit.
- Status: verifiziert
- Notizen: Land SL. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sl-krankenhaeuser
- Name: Krankenhausstandorte
- Betreiber: Geoportal Saarland
- Thema: Gesundheit
- Ebene: gemischt
- Format: WFS/GML
- URL: https://geoportal.saarland.de/arcgis/services/Internet/Gesundheit/MapServer/WFSServer?SERVICE=WFS&VERSION=1.1.0&REQUEST=GetFeature&TYPENAME=Gesundheit:Krankenhaeuser
- Lizenz: Geoportal SL
- Stand: live
- Aktualität: Neu
- Inhalt: Krankenhausstandorte.
- Warum spannend: Gesundheit.
- Status: verifiziert
- Notizen: Land SL. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sl-schulen
- Name: Schulstandorte
- Betreiber: Geoportal Saarland
- Thema: Bildung
- Ebene: gemischt
- Format: WFS/GML
- URL: https://geoportal.saarland.de/arcgis/services/Internet/Staatliche_Dienste/MapServer/WFSServer?SERVICE=WFS&VERSION=1.1.0&REQUEST=GetFeature&TYPENAME=Staatliche_Dienste:Schulen_SL
- Lizenz: Geoportal SL
- Stand: live
- Aktualität: Neu
- Inhalt: Schulstandorte.
- Warum spannend: Bildung.
- Status: verifiziert
- Notizen: Land SL. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sl-kiga
- Name: Kita-Standorte
- Betreiber: Geoportal Saarland
- Thema: Soziales
- Ebene: gemischt
- Format: WFS/GML
- URL: https://geoportal.saarland.de/arcgis/services/Internet/Staatliche_Dienste/MapServer/WFSServer?SERVICE=WFS&VERSION=1.1.0&REQUEST=GetFeature&TYPENAME=Staatliche_Dienste:KIGA_Saarl
- Lizenz: Geoportal SL
- Stand: live
- Aktualität: Neu
- Inhalt: Kita-Standorte.
- Warum spannend: Soziales/Bildung.
- Status: verifiziert
- Notizen: Land SL. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sl-oepnv
- Name: Haltestellen ÖPNV
- Betreiber: Geoportal Saarland
- Thema: Einkommen
- Ebene: gemischt
- Format: WFS/GML
- URL: https://geoportal.saarland.de/arcgis/services/Internet/Verkehr_WFS/MapServer/WFSServer?SERVICE=WFS&VERSION=1.1.0&REQUEST=GetFeature&TYPENAME=Verkehr_WFS:Haltestellen_OEPNV
- Lizenz: Geoportal SL
- Stand: live
- Aktualität: Neu
- Inhalt: ÖPNV-Haltepunkte.
- Warum spannend: Mobilität.
- Status: verifiziert
- Notizen: Land SL. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sl-ladestationen
- Name: Ladestationen
- Betreiber: Geoportal Saarland
- Thema: Einkommen
- Ebene: gemischt
- Format: WFS/GML
- URL: https://geoportal.saarland.de/arcgis/services/Internet/Verkehr_WFS/MapServer/WFSServer?SERVICE=WFS&VERSION=1.1.0&REQUEST=GetFeature&TYPENAME=Verkehr_WFS:Ladestationen
- Lizenz: Geoportal SL
- Stand: live
- Aktualität: Neu
- Inhalt: Ladeinfrastruktur.
- Warum spannend: Energie/Mobilität.
- Status: verifiziert
- Notizen: Land SL. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sl-evu-wasser
- Name: Netzbetreiber Strom/Gas + Wasser
- Betreiber: Geoportal Saarland
- Thema: Energie
- Ebene: gemischt
- Format: WFS/GML
- URL: https://geoportal.saarland.de/arcgis/services/Internet/Versorgungsgebiete_EVU/MapServer/WFSServer?SERVICE=WFS&VERSION=1.1.0&REQUEST=GetFeature&TYPENAME=Versorgungsgebiete_EVU:Wasserversorgungsgebiete
- Lizenz: Geoportal SL
- Stand: live
- Aktualität: Neu
- Inhalt: Versorgungs-/Netzbetreiberpolygone.
- Warum spannend: Energie/Umwelt Wasser.
- Status: verifiziert
- Notizen: Land SL. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sl-nsg
- Name: NSG (auch FFH/SPA/LSG)
- Betreiber: Geoportal Saarland
- Thema: Umwelt
- Ebene: gemischt
- Format: WFS/GML
- URL: https://geoportal.saarland.de/arcgis/services/Internet/Naturschutz/MapServer/WFSServer?SERVICE=WFS&VERSION=1.1.0&REQUEST=GetFeature&TYPENAME=Naturschutz:Naturschutzgebiet
- Lizenz: Geoportal SL
- Stand: live
- Aktualität: Neu
- Inhalt: NSG-Polygone.
- Warum spannend: Umwelt.
- Status: verifiziert
- Notizen: Land SL. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-sl-ltw2022-kerg
- Name: Amtliches Endergebnis KERG
- Betreiber: Geoportal Saarland
- Thema: Wahlen
- Ebene: gemischt
- Format: CSV
- URL: https://wahlergebnis.saarland.de/LTW/KERG_SAARLAND.csv
- Lizenz: Landeswahlleitung SL
- Stand: 2022
- Aktualität: Neu
- Inhalt: Stimmen nach Parteien.
- Warum spannend: LTW.
- Status: verifiziert
- Notizen: Land SL. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-bb-bvdaten
- Name: bvdaten.csv
- Betreiber: LGB Brandenburg
- Thema: Geobasis
- Ebene: gemischt
- Format: CSV
- URL: https://service.brandenburg.de/service/de/adressen/kommunalverzeichnis/bvdaten.csv
- Lizenz: Land BB
- Stand: live
- Aktualität: Neu
- Inhalt: Behördenkeys, Adressen, UTM (~131 KB).
- Warum spannend: Grenzen/Digital Stammdaten.
- Status: verifiziert
- Notizen: Land BB. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-bb-gazetteer-gemeinden
- Name: BB-BE Gazetteer Gemeinden
- Betreiber: LGB Brandenburg
- Thema: Geobasis
- Ebene: Gemeinde
- Format: WFS/GML
- URL: https://isk.geobasis-bb.de/ows/gazetteer_wfs?SERVICE=WFS&VERSION=2.0.0&REQUEST=GetFeature&TYPENAMES=app:Gemeinden
- Lizenz: dl-de/by-2-0 · LGB
- Stand: 2026
- Aktualität: Neu
- Inhalt: Amtliche Gemeinden + Geometrie.
- Warum spannend: Grenzen.
- Status: verifiziert
- Notizen: Land BB. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-bb-gazetteer-kreise
- Name: BB-BE Gazetteer Kreise
- Betreiber: LGB Brandenburg
- Thema: Geobasis
- Ebene: Landkreis
- Format: WFS/GML
- URL: https://isk.geobasis-bb.de/ows/gazetteer_wfs?SERVICE=WFS&VERSION=2.0.0&REQUEST=GetFeature&TYPENAMES=app:Kreise
- Lizenz: LGB / dl-de/by-2-0
- Stand: 2026
- Aktualität: Neu
- Inhalt: Landkreise / krfr. Städte.
- Warum spannend: Grenzen.
- Status: verifiziert
- Notizen: Land BB. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-bb-kachel-1km
- Name: Gazetteer Kachelung 1 km
- Betreiber: LGB Brandenburg
- Thema: Geobasis
- Ebene: Raster
- Format: WFS/GML
- URL: https://isk.geobasis-bb.de/ows/gazetteer_wfs?SERVICE=WFS&VERSION=2.0.0&REQUEST=GetFeature&TYPENAMES=app:kachel_01x01km
- Lizenz: LGB
- Stand: live
- Aktualität: Neu
- Inhalt: Regelmäßiges Gitter.
- Warum spannend: Raster Zensus-Anschluss.
- Status: verifiziert
- Notizen: Land BB. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-bb-boris-brw
- Name: BORIS BRW
- Betreiber: LGB Brandenburg
- Thema: Wirtschaft
- Ebene: gemischt
- Format: WFS 2.0
- URL: https://isk.geobasis-bb.de/ows/boris_wfs?SERVICE=WFS&REQUEST=GetCapabilities
- Lizenz: LGB / Gutachterausschüsse
- Stand: ab 2010 laufend
- Aktualität: Neu
- Inhalt: Bodenrichtwerte numberMatched≈113293.
- Warum spannend: Wohnen/Wirtschaft Bodenmarkt.
- Status: verifiziert
- Notizen: Land BB. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-bb-schutzgebiete
- Name: NSG/LSG/FFH/SPA
- Betreiber: LGB Brandenburg
- Thema: Umwelt
- Ebene: gemischt
- Format: WFS/GML
- URL: https://inspire.brandenburg.de/services/schutzg_wfs?SERVICE=WFS&VERSION=2.0.0&REQUEST=GetFeature&TYPENAMES=app:nsg&COUNT=2
- Lizenz: LfU BB / INSPIRE
- Stand: live
- Aktualität: Neu
- Inhalt: Naturschutz + Natura-2000.
- Warum spannend: Umwelt.
- Status: verifiziert
- Notizen: Land BB. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-mv-dvg-zip
- Name: DVG_MV.zip
- Betreiber: LAiV MV / OpenData.HRO
- Thema: Geobasis
- Ebene: gemischt
- Format: ZIP
- URL: https://www.geodaten-mv.de/dienste/dvg_download?index=0&dataset=d79bc9d7-c175-444a-86ce-df7195725e67&file=DVG_MV.zip
- Lizenz: CC-BY 4.0 · GeoBasis-DE/MV
- Stand: live
- Aktualität: Neu
- Inhalt: Amtliche DVG (~8.1 MB).
- Warum spannend: Grenzen Landesdownload.
- Status: verifiziert
- Notizen: Land MV. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-mv-dvg-wfs
- Name: WFS Digitale Verwaltungsgrenzen
- Betreiber: LAiV MV / OpenData.HRO
- Thema: Geobasis
- Ebene: gemischt
- Format: OGC WFS
- URL: https://www.geodaten-mv.de/dienste/dvg_laiv_wfs?SERVICE=WFS&REQUEST=GetCapabilities
- Lizenz: CC-BY 4.0
- Stand: live
- Aktualität: Neu
- Inhalt: Feature-Download DVG.
- Warum spannend: Grenzen API.
- Status: verifiziert
- Notizen: Land MV. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-mv-gemeindeverbaende
- Name: Ämter + amtsfreie Kommunen
- Betreiber: LAiV MV / OpenData.HRO
- Thema: Geobasis
- Ebene: Gemeinde
- Format: CSV + GeoJSON
- URL: https://geo.sv.rostock.de/download/opendata/gemeindeverbaende_mecklenburg-vorpommern/gemeindeverbaende_mecklenburg-vorpommern.csv
- Lizenz: OpenData.HRO
- Stand: 2026-07
- Aktualität: Neu
- Inhalt: Ämter mit Kreisschlüssel.
- Warum spannend: Grenzen neue Ebene.
- Status: verifiziert
- Notizen: Land MV. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-mv-gemeindeteile
- Name: Gemeindeteile CSV
- Betreiber: LAiV MV / OpenData.HRO
- Thema: Geobasis
- Ebene: Gemeinde
- Format: CSV
- URL: https://geo.sv.rostock.de/download/opendata/gemeindeteile_mecklenburg-vorpommern/gemeindeteile_mecklenburg-vorpommern.csv
- Lizenz: OpenData.HRO
- Stand: 2026
- Aktualität: Neu
- Inhalt: Feine Ortsteilebene (~571 KB).
- Warum spannend: Grenzen kleinräumig.
- Status: verifiziert
- Notizen: Land MV. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-mv-uferlinie
- Name: Uferlinie CSV
- Betreiber: LAiV MV / OpenData.HRO
- Thema: Umwelt
- Ebene: gemischt
- Format: CSV
- URL: https://geo.sv.rostock.de/download/opendata/uferlinie_mecklenburg-vorpommern/uferlinie_mecklenburg-vorpommern.csv
- Lizenz: OpenData.HRO
- Stand: 2026
- Aktualität: Neu
- Inhalt: Uferliniengeometrie (~21.7 MB).
- Warum spannend: Umwelt/Tourismus Küste.
- Status: verifiziert
- Notizen: Land MV. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-mv-orka-wmts
- Name: Offene Regionalkarte WMTS
- Betreiber: LAiV MV / OpenData.HRO
- Thema: Geobasis
- Ebene: Raster
- Format: WMTS
- URL: https://www.orka-mv.de/geodienste/orkamv/wmts/1.0.0/WMTSCapabilities.xml
- Lizenz: ORKa.MV / Open Data
- Stand: live
- Aktualität: Neu
- Inhalt: Offene topografische Kacheln.
- Warum spannend: Digital/Raster Basiskarte.
- Status: verifiziert
- Notizen: Land MV. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-mv-dgm
- Name: Digitales Geländemodell
- Betreiber: LAiV MV / OpenData.HRO
- Thema: Umwelt
- Ebene: Raster
- Format: WCS / ATOM
- URL: https://www.geodaten-mv.de/dienste/dgm_wcs?SERVICE=WCS&REQUEST=GetCapabilities
- Lizenz: CC-BY 4.0
- Stand: live
- Aktualität: Neu
- Inhalt: DGM Downloaddienst.
- Warum spannend: Umwelt/Raster.
- Status: verifiziert
- Notizen: Land MV. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

### land-mv-vernaessung
- Name: OpenAgrar XLSX
- Betreiber: LAiV MV / OpenData.HRO
- Thema: Wirtschaft
- Ebene: gemischt
- Format: XLSX
- URL: https://www.openagrar.de/servlets/MCRFileNodeServlet/openagrar_derivate_00067635/2025_09_05_Datensatz_Vernaessungskosten.xlsx
- Lizenz: OpenAgrar
- Stand: 2025-09-05
- Aktualität: Neu
- Inhalt: Kostenkennzahlen Vernässung (MV & BB).
- Warum spannend: Umwelt/Wirtschaft Moorschutz.
- Status: verifiziert
- Notizen: Land MV. Aus Länder-Crawl 2026-09-07. AGS/ARS je nach Datei — siehe Inhalt/Ebene.

## Index nach Thema

- **Demografie:** kern-zensus-gitter, kern-zensus-regional, kern-regionalstatistik-api, land-sh-regional-bev, land-he-offenbach-zensus, land-be-lor-shapefiles
- **Einkommen:** upd-vgrdl-einkommen, upd-vgrdl-loehne, nisch-ldb-nrw-einkommen, land-sh-est-gemeinden, land-nw-ldb-est-73111, land-rp-meine-heimat-api
- **Soziales:** upd-sgb2-aug2026, upd-ba-api-grusi, nisch-ldb-nrw-sgbxii, nisch-hh-sozialmon-2025, land-nw-duesseldorf-quartiersatlas-2024, land-by-muenchen-sgb2-alter, land-he-darmstadt-sgb2-q1, land-bw-freiburg-armutsgefaehrdung
- **Bildung:** nisch-bildungsmonitoring, nisch-nrw-schulen, nisch-he-bildungsatlas, land-sl-schulen, land-sl-kiga, land-sn-asv-csv, land-by-muenchen-kita, land-hb-kita
- **Gesundheit:** nisch-ldb-nrw-pflege, nisch-ldb-nrw-kh, nisch-zi-versorgungsatlas, land-sn-kh-diagnosen, land-sl-apotheken, land-sl-krankenhaeuser, land-th-krankenhaus, land-ni-pm10-2025
- **Wirtschaft:** upd-vgrdl-bip, upd-destatis-realsteuer, upd-destatis-schulden, nisch-tourismus-45412, nisch-gewerbe-52311, nisch-urs-52111, land-sn-gewerbe-an, land-sn-ust-kreise, land-sh-kreismonitor, land-sh-gewerbe-2026q2
- **Arbeitsmarkt:** upd-ba-api-alo, upd-ba-api-bst, nisch-erwerb-13312, nisch-ldb-nrw-alo-gem, land-sn-svb-kreise, land-by-muenchen-svb, land-by-muenchen-alo
- **Wohnen:** upd-baufertig-2024, nisch-zensus-arcgis, land-sn-baufertig-heiz, land-st-brw-2026, land-st-hausumringe, land-bb-boris-brw, land-th-hu-zip, land-bw-stuttgart-wohnungsbestand, land-nw-dortmund-mietspiegel
- **Energie:** upd-mastr-export, upd-kraftwerksliste-2026, upd-bnetza-lps, nisch-uba-eneff-waerme, nisch-energy-charts, nisch-by-energieatlas, land-hh-waermenetz-kwp, land-nw-grundversorger-gas, land-sl-ladestationen, land-sl-evu-wasser
- **Klima:** kern-dwd-duerre, nisch-dwd-hitzetage, nisch-dwd-strahlung, land-st-s2-raster
- **Umwelt:** nisch-eba-laerm, nisch-hwrm, nisch-clc2018, nisch-erosion, nisch-o3, nisch-pm25, land-hh-laermkarten, land-ni-umweltzonen, land-bb-schutzgebiete, land-th-trinkwasser, land-mv-uferlinie
- **Mobilität:** upd-kwm-2024, upd-kba-fz1, upd-ba-pendler, kern-bmdv-mobilithek, nisch-vbb-gtfs, nisch-bw-svz-2024, land-sn-baustellen-geojson, land-hh-bike-ride, land-be-infravelo, land-sl-oepnv, land-sh-unfaelle-2025
- **Sicherheit:** kern-bka-pks, kern-destatis-unfallatlas
- **Wahlen:** kern-ew24, nisch-ltw-by-2023, nisch-ltw-th-2024, land-rp-ltw2026-stimmbezirk, land-hh-wahlen-stadtteile, land-he-ltw-gemeinden-14336, land-sl-ltw2022-kerg, land-bw-heidelberg-ltw26
- **Geobasis:** kern-vg250, upd-destatis-anschriften, nisch-ge250, nisch-inkar-ror, land-by-alkis-verwaltung, land-ni-lgln-vwg-wfs, land-sn-vwg-shape, land-st-dvg-alkis, land-th-vg-zip, land-rp-gemeinden-shapezip, land-mv-dvg-zip, land-be-lor-shapefiles, land-bb-gazetteer-gemeinden, land-sh-atkis-vwg-wfs
- **Sonstiges (Digital/Katalog):** upd-breitbandatlas-2025, upd-breitband-gitter, upd-bnetza-mobilfunk, nisch-pvog, kern-govdata-ckan, kern-inkar-2025, kern-genesis-api, D-Portale, E land-*
