# ISTAT Traffic Analysis & Risk Prevention — Capstone Project

Il progetto analizza la pericolosità stradale nei comuni italiani nel triennio 2021-2023, con l'obiettivo di individuare quali comuni presentano il rischio più alto e potrebbero essere prioritari per investimenti in sicurezza stradale.

## Struttura del Progetto
*   `notebooks/`: notebook Python con tutta la pipeline — fetch dei dati, pulizia, EDA, feature engineering, clustering e regressione.
*   `data/`: CSV di input/output (ISTAT/SITUAS), il dataset arricchito usato per la dashboard e il grafico dei cluster salvato in `.png`.
*   `Presentazione_Capstone_Traffic.pptx`: dashboard Power BI.
*   `Dashboard_Incidenti.pbix`: pitch deck di 5 slide.

## Tecnologie Utilizzate
*   **Python** (pandas, scikit-learn, matplotlib, seaborn, requests): fetch dati, pulizia, feature engineering, clustering K-Means e regressione lineare.
*   **Power BI**: dashboard interattiva con i risultati.
*   **PowerPoint**: pitch deck.

## Fonti dati
*   Incidenti stradali: API ISTAT (dataflow `41_983`), scaricata automaticamente con richiesta CSV via header HTTP (`Accept: application/vnd.sdmx.data+csv;version=1.0.0`).
*   Popolazione e superficie dei comuni: SITUAS, file scaricati manualmente anno per anno (2021-2023) perché il dato è specifico per anno.

## Pipeline
1. **Data ingestion**: la pipeline prova prima a scaricare i dati aggiornati dall'API ISTAT; se la richiesta fallisce (server down, rate limit) usa l'ultima copia salvata in locale, avvisando chiaramente quale delle due fonti sta usando. Se non c'è nessuna copia locale e il download fallisce, la pipeline si ferma con un errore invece di proseguire con dati vuoti.
2. **Join** tra incidenti e anagrafica comuni, fatto anno per anno (non un unico merge, perché popolazione e superficie cambiano di anno in anno).
3. **EDA**: controllo di distribuzione, valori mancanti e range delle variabili principali prima di calcolare qualsiasi KPI.
4. **Feature engineering**: tasso di incidenti pro capite (ogni 1.000 abitanti) e densità di incidenti per km², per confrontare comuni di dimensioni molto diverse.
5. **Trattamento outlier**: i comuni con popolazione sotto i 500 abitanti vengono esclusi dal ranking e dal clustering, perché con numeri così piccoli anche un solo incidente genera un tasso pro capite fuori scala e poco significativo. Restano comunque nel dataset con un flag (`bassa_affidabilita_statistica`), così l'informazione non si perde, solo non entra nel calcolo del rischio.
6. **Clustering (K-Means)**: segmentazione dei comuni in 3 fasce di rischio (Basso, Medio, Alto) sulle feature standardizzate, calcolata sull'anno più recente (2023) e propagata agli altri anni per lo stesso comune. Risultato: 152 comuni ad Alto Rischio.
7. **Forecasting**: regressione lineare sulla serie storica nazionale degli incidenti per stimare il trend 2026 (~190.700 incidenti). Con solo 3 anni di dati è una stima di massima, non una previsione precisa.

## Note
Il codice è pensato per girare anche senza connessione all'API ISTAT attiva: se il server non risponde, la pipeline usa i dati già scaricati in precedenza e lo segnala a schermo, invece di bloccarsi o produrre dati finti.
