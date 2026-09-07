# Open-Data Quellenverzeichnis (Dictionary)

Stand: 2026-09-07. Lebendes Verzeichnis für Dashboard/Landkreisranking.
Scope: Deutschland, Ebene Landkreis | Gemeinde | Raster. Nur Einträge mit Download- oder API-URL.

Kernquellen (A) aus Testlauf `2026-09-07-test.md`. Updates (B) und Nischen (C) aus Nischenlauf `2026-09-07-nische.md`. Portale (D) aus `startliste.md`.

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
- Relevanz:
- Status: verifiziert | WAF | Account nötig | Katalog-only | unklar
- Notizen:
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
- Relevanz: Hochauflösende Demografie für Heatmaps und Aggregation auf Kreis/Gemeinde.
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
- Relevanz: Einwohner und Altersstruktur Stichtag 15.05.2022 für kommunales Ranking.
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
- Relevanz: Automatisierte Kreis-/Gemeinde-Zeitreihen; Kern-Bevölkerung und Gebietsfläche.
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
- Relevanz: Programmatischer Abruf amtlicher Statistik inkl. regional über AGS.
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
- Relevanz: ~600 Raumindikatoren bundesweit — zentral für Atlas/Ranking.
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
- Relevanz: Amtliche Grenzen + AGS für Joins.
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
- Relevanz: Flächendeckende Dürre für Ranking/Heatmaps.
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
- Relevanz: Amtliche Wahlergebnisse kreisscharf (BTW nur Wahlkreis).
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
- Relevanz: Discovery weiterer Kreis-/Gemeinde-Datensätze.
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
- Relevanz: Kriminalität kreisscharf mit Häufigkeitszahl.
- Status: WAF
- Notizen: GovData-Katalog `…/t01-grundtabelle-kreise-…`. Direktdownload mit Mandats-UA oft 303/400/403. T20 Tatverdächtige und kommunale Mirrors → C.

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
- Relevanz: Primär- und verfügbares Einkommen je Einwohner kreisscharf.
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
- Relevanz: Aktuellste kreisscharfe Lohnreihe.
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
- Relevanz: Maschinenlesbare SGB-II-Kennzahlen Bulk.
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
- Relevanz: PV/Wind/Speicher/KWK-Stammdaten, aggregierbar auf Gemeinde/Kreis.
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
- Relevanz: Gigabit/Breitbandversorgung Kreis und Gemeinde.
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
- Relevanz: Hochauflösendes Digital-Layer.
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
- Relevanz: Bautätigkeit kreisscharf — Dynamik zum Zensus-Bestand.
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
- Relevanz: ALO + Unterbeschäftigung monatsaktuell kreisscharf.
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
- Relevanz: SvB monatsaktuell ohne Zertifikat.
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
- Relevanz: Kreisscharfe Grundsicherung-Zeitreihe.
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
- Relevanz: Wanderungssalden Kreis↔Kreis.
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
- Relevanz: Kompakte Kraftwerks-/EE-Übersicht ohne 3-GB-XML.
- Status: verifiziert
- Notizen: ≥10 MW einzeln; Kleinanlagen nach Land. XLSX-Pendant + ZuUndRueckbau.

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
- Relevanz: Wärme/Gebäude und Wohnungsdruck rasterscharf — Access-Pfad bei Destatis-WAF.
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
- Relevanz: Bundesweite Schienenlärm-Isophonen.
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
- Relevanz: Hochwasser-Exposure flächendeckend.
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
- Relevanz: Flächenhafter Wärmebedarf ohne proprietären Atlas.
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
- Relevanz: Landnutzung für Umwelt-/Siedlungsindikatoren.
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
- Relevanz: Boden jenseits DWD-Dürre.
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
- Relevanz: Luftqualität jenseits Dedup-OI_NO2.
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
- Relevanz: Zweiter Luft-Indikator neben Ozon.
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
- Relevanz: Schulen/Schüler/Hochschulen kreisscharf bundesweit.
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
- Relevanz: Kreis-Bildungsmatrix NRW — kompensiert bundesweite Flat-CSV-Lücke.
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
- Relevanz: Gemeinde-/Kreis-Einkommen ohne GENESIS-UI.
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
- Relevanz: Pflegebedürftige nach Pflegegrad kreisscharf.
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
- Relevanz: SGB XII und Armuts-Proxy gemeindescharf.
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
- Relevanz: Stationäre Versorgungsstruktur NRW.
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
- Relevanz: Gemeindescharfe Arbeitslosigkeit NRW.
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
- Relevanz: Tourismusintensität kreisscharf.
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
- Relevanz: Wirtschaftsdynamik kreisscharf.
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
- Relevanz: Unternehmensstruktur kreisscharf.
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
- Relevanz: Wirtschaftsstruktur jenseits Bevölkerung.
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
- Relevanz: ÖPNV BE+BB maschinenlesbar; DELFI nur nach Login.
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
- Relevanz: Nationale Leistungs-/Ausbau-Zeitreihen als Benchmark zu MaStR.
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
- Relevanz: Online-Leistungen nach ARS 12-stellig.
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
- Relevanz: ROR/BBSR-Raumgliederungen — nicht VG250.
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
- Relevanz: ROR-Schlüssel für Joins; Meta für Single-Indicator.
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
- Relevanz: Maschinenlesbare LTW BY.
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
- Relevanz: TH Landtag gemeindescharf.
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
- Relevanz: Sozialindikatoren stadtteilscharf.
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
- Relevanz: Schulbezirke und Bildungsstandorte landesweit HE.
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
- Relevanz: EE-Indikatoren gemeindescharf flächendeckend BY.
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
- Relevanz: Landesweites Verkehrsmessnetz BW.
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
- Relevanz: Tourismus gemeindescharf SN.
- Status: verifiziert
- Notizen: `format=ffcsv` Pflicht; Range-Requests können 0 B. Unfälle Kreise `46241-208K`.

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
- Relevanz: Bundeskatalog — Discovery Kreis/Gemeinde.
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
- Relevanz: Amtliche Statistik + Zensus-Gitter.
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
- Relevanz: Kreis-/Gemeinde-Tabellen (ffcsv + REST).
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
- Relevanz: Landesportal NRW + LDB + opengeodata.nrw.
- Status: Katalog
- Notizen: DVG2 im Testkern. LDB-Downloader für thematische CSVs.

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
- Relevanz: Discovery BY inkl. kommunaler ArcGIS-Hubs und Energie-Atlas.
- Status: Katalog
- Notizen: Energie-Atlas WMS, München/Ingolstadt/Cham-Hits in C.

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
- Relevanz: Discovery BW; MobiData als Mobility-Hub.
- Status: Katalog
- Notizen: CKAN-Base nicht Root `/api`. MobiData mobidata-bw.de.

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
- Relevanz: Stadtteil-Sozial- und Regionalstatistik HH.
- Status: Katalog
- Notizen: geodienste.hamburg.de WFS; api.hamburg.de OAF.

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
- Relevanz: Discovery HE; Bildungsatlas, kommunale Sozialatlanten.
- Status: Katalog
- Notizen: Darmstadt als starker Kommunal-Hub.

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
- Relevanz: Discovery NI — in diesem Lauf kaum frische thematische Direct-Downloads.
- Status: Katalog
- Notizen: GovData-Org `land-niedersachsen` dünn. Follow-up LGLN/LSN, Hannover, Braunschweig.

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
- Relevanz: Discovery SN; GenOnline ffcsv für Kreis/Gemeinde.
- Status: Katalog
- Notizen: statistik.sachsen.de GenOnline braucht `format=ffcsv` + Voll-GET.

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
- Relevanz: Discovery BE; datenregister oft 403/429 — GovData-Spiegel.
- Status: Katalog
- Notizen: ALKIS-Bezirke im Testkern. Thematisch: GSSA, Liegenschaftsenergie.

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
- Relevanz: Discovery HB — in diesem Lauf keine brauchbaren 2024–2026 Direct-File-Hits.
- Status: Katalog
- Notizen: Follow-up Stadtteil/Sozial/Energie.

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
- Relevanz: EU-Metakatalog über DE-Portale.
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
- Relevanz: Kommunale Hubs nur bei klarer Download-URL (Beispiel Cham EE-Anlagen).
- Status: Katalog
- Notizen: Startliste: nur bei klarem Direct-Download.

## Index nach Thema

- **Demografie:** kern-zensus-gitter, kern-zensus-regional, kern-regionalstatistik-api
- **Einkommen:** upd-vgrdl-einkommen, upd-vgrdl-loehne, nisch-ldb-nrw-einkommen
- **Soziales:** upd-sgb2-aug2026, upd-ba-api-grusi, nisch-ldb-nrw-sgbxii, nisch-hh-sozialmon-2025
- **Bildung:** nisch-bildungsmonitoring, nisch-nrw-schulen, nisch-he-bildungsatlas
- **Gesundheit:** nisch-ldb-nrw-pflege, nisch-ldb-nrw-kh
- **Wirtschaft:** nisch-tourismus-45412, nisch-gewerbe-52311, nisch-urs-52111, nisch-sn-tourismus-gem
- **Arbeitsmarkt:** upd-ba-api-alo, upd-ba-api-bst, nisch-erwerb-13312, nisch-ldb-nrw-alo-gem
- **Wohnen:** upd-baufertig-2024, nisch-zensus-arcgis
- **Energie:** upd-mastr-export, upd-kraftwerksliste-2026, nisch-uba-eneff-waerme, nisch-energy-charts, nisch-by-energieatlas
- **Klima:** kern-dwd-duerre
- **Umwelt:** nisch-eba-laerm, nisch-hwrm, nisch-clc2018, nisch-erosion, nisch-o3, nisch-pm25
- **Mobilität:** upd-kwm-2024, nisch-vbb-gtfs, nisch-bw-svz-2024
- **Sicherheit:** kern-bka-pks
- **Wahlen:** kern-ew24, nisch-ltw-by-2023, nisch-ltw-th-2024
- **Geobasis:** kern-vg250, nisch-ge250, nisch-inkar-ror
- **Sonstiges (Digital/Katalog):** upd-breitbandatlas-2025, upd-breitband-gitter, nisch-pvog, kern-govdata-ckan, kern-inkar-2025, kern-genesis-api, D-Portale
