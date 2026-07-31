# Data Dictionary — dataset_kpi_powerbi_v2.csv

Descrizione di tutte le colonne del dataset finale usato per la dashboard Power BI e per l'analisi.

## Dati ISTAT (incidenti stradali)

| Colonna | Descrizione |
|---|---|
| `DATAFLOW` | Codice identificativo del dataset ISTAT di origine (valore fisso) |
| `FREQ` | Frequenza della rilevazione — annuale ("A") |
| `REF_AREA` | Codice numerico del comune, usato come chiave di join con l'anagrafica SITUAS |
| `DATA_TYPE` | Tipo di dato — solo `ROADACC` (incidenti stradali totali) in questo dataset |
| `RESULT` | Tipo di misura riportata |
| `TIME_PERIOD` | Anno di riferimento (2021, 2022 o 2023) |
| `OBS_VALUE` | **Numero di incidenti** nel comune, nell'anno indicato |
| `OBS_STATUS` | Flag di qualità del dato ISTAT (es. dato provvisorio/definitivo) |
| `NOTE_DS`, `NOTE_REF_AREA`, `NOTE_DATA_TYPE`, `NOTE_RESULT`, `NOTE_TIME_PERIOD` | Note e metadati tecnici ISTAT, quasi sempre vuote |
| `BASE_PER` | Periodo base di riferimento per eventuali indici |
| `UNIT_MEAS` | Unità di misura del valore (es. "numero") |
| `UNIT_MULT` | Moltiplicatore dell'unità di misura |

## Anagrafica comuni (SITUAS)

| Colonna | Descrizione |
|---|---|
| `Codice Ripartizione geografica` | Macro-area geografica del comune (Nord-Ovest, Nord-Est, Centro, Sud, Isole) |
| `Codice Regione` | Codice della regione |
| `Codice Provincia (Storico)` | Codice storico della provincia |
| `Codice Provincia/Uts` | Codice attuale della provincia/unità territoriale |
| `Codice Comune (alfanumerico)` | Codice comune in formato alfanumerico (regione+comune) |
| `Codice Comune (numerico)` | Codice comune numerico nazionale — usato per il join con REF_AREA |
| `Comune` | Nome del comune |
| `Comune (dizione straniera)` | Nome del comune in lingua straniera, dove previsto (es. Alto Adige) — spesso vuota |
| `Sigla automobilistica` | Sigla della provincia (es. "TO", "MI") |
| `Capoluogo di Provincia/Uts` | Flag: il comune è capoluogo di provincia? |
| `Capoluogo di Regione` | Flag: il comune è capoluogo di regione? |
| `Popolazione legale` | Popolazione dell'ultimo censimento ufficiale |
| `Anno Censimento` | Anno del censimento di riferimento |
| `Superficie (Kmq)` | Estensione del comune in km² |
| `Anno (Superficie)` | Anno di riferimento del dato di superficie |
| `Popolazione residente` | Popolazione residente per l'anno specifico (usata nei calcoli) |
| `Anno (Popolazione residente)` | Anno di riferimento della popolazione residente |

## Metriche calcolate

| Colonna | Descrizione | Formula |
|---|---|---|
| `incidenti_pro_capite_1k` | Tasso di incidenti ogni 1.000 abitanti | `OBS_VALUE / Popolazione residente * 1000` |
| `densita_incidenti_kmq` | Incidenti per km² | `OBS_VALUE / Superficie (Kmq)` |
| `bassa_affidabilita_statistica` | TRUE se il comune ha meno di 500 abitanti: il tasso pro capite è statisticamente poco affidabile e va interpretato con cautela | `Popolazione residente < 500` |
| `Cluster_Rischio` | Fascia di rischio assegnata dal modello K-Means: Basso / Medio / Alto Rischio. Vuota per i comuni esclusi dal training (`bassa_affidabilita_statistica = TRUE`) | Output del clustering |
| `Forecast_Trend_Coefficiente` | Coefficiente angolare della regressione lineare sul trend nazionale degli incidenti (variazione media annua stimata a livello Italia). Stesso valore ripetuto su ogni riga, non è specifico del singolo comune | Output della regressione lineare |
