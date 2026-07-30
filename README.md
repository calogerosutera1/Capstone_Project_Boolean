# ISTAT Traffic Analysis & Risk Prevention — Capstone Project

Il progetto si concentra sull'analisi della pericolosità e del rischio stradale nei comuni italiani nel triennio 2021-2023.

## Struttura del Progetto
*   `notebooks/`: Contiene lo script Python principale per l'ingestion, la pulizia dei dati e la pipeline di modellazione statistica.
*   `data/`: Contiene i file CSV di input/output (SITUAS/ISTAT), il dataset arricchito per la dashboard e il grafico dei cluster salvato in `.png`.
*   `Presentazione_Capstone_Traffic.pptx`: Il Pitch Deck di 5 slide con la sintesi aziendale.
*   `Dashboard_Incidenti_Boolean.pbix`: Il file di report interattivo sviluppato su Power BI Desktop.

## Tecnologie Utilizzate
*   **Python (Pandas, Scikit-Learn, Matplotlib, Seaborn)**: Data cleaning, feature engineering, clustering K-Means e regressione lineare.
*   **Power BI**: Storytelling visivo, filtri interattivi e analytics dashboard.
*   **PowerPoint**: Pitch aziendale orientato al business.

## Logica Analitica e di Business
1.  **Resilienza Dati**: Pipeline protetta da meccanismo di caching locale contro i downtime e i rate limit dell'API ISTAT.
2.  **Rimozione Bias**: Calcolo delle metriche *Tasso Pro Capite* e *Densità Spaziale per Kmq* per una normalizzazione geografica democratica dei comuni.
3.  **Machine Learning (K-Means)**: Segmentazione automatica del territorio in 3 fasce oggettive di rischio, isolando **148 comuni target ad Alto Rischio** su cui concentrare gli investimenti aziendali.
4.  **Forecasting**: Regressione Lineare per stimare il trend nazionale futuro dei sinistri stradali.
