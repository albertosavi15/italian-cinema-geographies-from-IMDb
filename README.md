# Regionalità, localizzazione e prospettive data-driven sulle geografie del cinema italiano

**[Italiano](#italiano) · [English](#english)**

---

<a id="italiano"></a>

# Italiano

Questo repository raccoglie il codice Python e la documentazione delle analisi quantitative sviluppate per la tesi di dottorato *Regionalità, localizzazione e prospettive data-driven sulle geografie del cinema italiano*. I dataset di partenza non sono pubblicati nel repository, ma possono essere richiesti all'autore.

L'obiettivo è rendere consultabili le procedure computazionali applicate a un corpus di film italiani costruito a partire da IMDb, con particolare attenzione alla distribuzione territoriale delle *filming locations*, alle differenze regionali e alla loro evoluzione nel tempo. Il repository documenta la **fase di analisi di dati già raccolti, puliti e arricchiti**: non ricostruisce automaticamente l'intero processo preliminare di estrazione e preparazione del corpus.

## 1. Corpus e fonti

Il corpus di lavoro comprende **11.418 film**. I dati derivano da operazioni di selezione, pulizia e arricchimento di informazioni provenienti da IMDb, con particolare riferimento al campo *filming locations*. Il corpus costituisce uno **snapshot di ricerca**, non una copia continuamente aggiornata del database di origine; il processo di raccolta descritto nella tesi si arresta a novembre 2025.

Le informazioni sulle location sono state normalizzate e, dove possibile, riconciliate con **Wikidata** attraverso **OpenRefine**, per associare ai luoghi identificativi e attributi geografici. Per integrare le informazioni territoriali sono stati utilizzati anche dati geografici aperti, compreso **OpenStreetMap**. La selezione iniziale, le procedure di pulizia, la riconciliazione e i relativi limiti sono discussi nella sezione metodologica della tesi. Il progetto OpenRefine e la cronologia delle trasformazioni preliminari non sono inclusi nel repository.

Le *filming locations* registrate su IMDb costituiscono una fonte opportunistica: presenza, completezza e precisione delle informazioni variano da film a film. I risultati descrivono quindi le geografie **documentate nel corpus**, non la totalità delle location effettivamente impiegate dal cinema italiano.

## 2. Contenuto del repository e accesso ai dati

Il repository pubblico contiene il codice delle analisi e la relativa documentazione. Lo script utilizza due dataset di partenza, **non inclusi nel repository**:

| File | Contenuto | Disponibilità |
| --- | --- | --- |
| `clean_version_italian_cinema_geo_imdb.py` | Esportazione Python del notebook Google Colab contenente le analisi. | Pubblico nel repository |
| `1_reference_backup_film_IMDb.csv` | Una riga per film: identificativo IMDb, titoli, anno e altri metadati cinematografici. | Su richiesta |
| `geografie_cinema_ita_imdb.csv` | Dataset geografico in formato lungo: un film può comparire in più righe in relazione alle location registrate. Contiene, dove disponibili, luogo, coordinate e classificazioni territoriali. | Su richiesta |

Il primo CSV contiene **11.418 righe**; il secondo **18.495 righe**, riferite agli stessi **11.418 identificativi IMDb**. La differenza deriva dalla struttura uno-a-molti delle associazioni film–location e dalla presenza di film senza informazioni geografiche utilizzabili.

**Richiesta dei dati.** Chi desidera accedere ai dataset per finalità di studio, verifica o riproducibilità della ricerca può scrivere ad **Alberto Savi**: [albertosavi95@gmail.com](mailto:albertosavi95@gmail.com). La condivisione avverrà nel rispetto delle condizioni applicabili alle fonti originali.

Lo script produce tabelle CSV derivate, grafici e `film_locations_map.html`, una mappa interattiva delle location dotate di coordinate. Gli output vengono generati durante l'esecuzione e non sono necessari come input.

## 3. Analisi incluse

Il codice comprende le seguenti operazioni:

1. **Mappatura delle location:** visualizzazione interattiva con Folium, marcatori raggruppati e collegamenti alle schede IMDb.
2. **Distribuzione geografica:** conteggi per continente, paese e regione italiana, anche su base quinquennale.
3. **Incidenza regionale e variazione temporale:** frequenze relative regionali e coefficienti di variazione, con aggregazioni annuali e quinquennali.
4. **Mobilità regionale:** numero di regioni associate a ciascun film, coefficiente di mobilità, distribuzione per classi e andamento temporale; sono comprese elaborazioni che escludono il Lazio o i film associati esclusivamente al Lazio.
5. **Autonomia regionale:** quota di film monoregionali sul totale dei film associati a ciascuna regione, anche per il periodo 2000–2025 e per quinquennio.
6. **Tipologia territoriale:** classificazione delle regioni in quattro quadranti sulla base dell'incidenza nazionale e dell'indice di autonomia regionale, sia per il periodo 2000–2025 sia per il dataset completo.

La versione pubblicata comprende **le operazioni selezionate per la tesi e alcune elaborazioni ulteriori**, ma non l'intero percorso esplorativo sviluppato durante la ricerca.

## 4. Unità di analisi e indicatori

**Associazioni film–location.** Nei conteggi geografici, ciascuna coppia unica `IMDb_ID`–territorio vale una sola occorrenza al livello considerato. Per esempio, un film associato a cinque location nella stessa regione contribuisce una sola volta al conteggio di quella regione; se è associato a due regioni, contribuisce una volta a ciascuna. La somma dei conteggi regionali può quindi superare il numero di film distinti.

**Frequenza relativa regionale.** Il numero di associazioni film–regione di una regione è diviso per la somma delle associazioni film–regione di tutte le regioni nell'intervallo considerato. L'indicatore rappresenta la quota regionale delle associazioni, **non** la percentuale dei film unici del corpus che interessano quella regione.

**Coefficiente di mobilità regionale.** Per ogni film, il numero di regioni distinte (`Region_Variety`) viene normalizzato secondo la formula:

```text
Mobility_Coefficient = (Region_Variety - 1) / (max_region_variety - 1)
```

Un film associato a una sola regione ottiene 0; il valore 1 corrisponde alla massima varietà osservata nel campione di riferimento. Lo script ricalcola il massimo in alcune analisi su sottoinsiemi: valori normalizzati ottenuti con massimi differenti non sono direttamente confrontabili senza considerare tale differenza.

**Indice di autonomia regionale.** È il rapporto tra il numero di film associati esclusivamente a una regione e il numero complessivo di film associati a quella regione, anche in compresenza con altre regioni. L'indice misura l'esclusività dell'associazione territoriale **all'interno dei dati disponibili**; non esprime indipendenza economica o produttiva.

**Quadranti regionali.** Le mediane dell'incidenza nazionale e dell'indice di autonomia dividono il piano in quattro categorie interpretative: `centrale`, `periferica autonoma`, `di snodo` e `subordinata`. Le categorie dipendono dagli indicatori e dal periodo scelto; non costituiscono giudizi generali sulle regioni.

## 5. Esecuzione

Il codice è stato sviluppato ed eseguito principalmente in **Google Colab**. Per riprodurre le analisi è necessario ottenere prima i due CSV tramite la procedura di richiesta indicata nella sezione 2.

1. Aprire il file `.py` in un ambiente Python compatibile oppure utilizzare la corrispondente versione notebook in Colab, se disponibile.
2. Collocare entrambi i CSV nella directory di lavoro di Colab (`/content/`).
3. Installare, se necessario, le librerie richieste.
4. Eseguire il codice dall'inizio alla fine, senza saltare le sezioni che producono variabili riutilizzate successivamente.

Dipendenze principali: `pandas`, `numpy`, `matplotlib`, `seaborn` e `folium`. Per un'installazione locale:

```bash
python -m pip install pandas numpy matplotlib seaborn folium
```

**Nota sui percorsi:** lo script è un'esportazione da Colab e utilizza sia percorsi assoluti `/content/...` sia nomi di file relativi. Per eseguirlo fuori da Colab occorre adattare i percorsi di lettura e salvataggio. Alcune istruzioni, come `display()` e la visualizzazione diretta dell'oggetto mappa, sono pensate per un ambiente notebook.

Gli output CSV sono salvati prevalentemente in `/content/`; la mappa interattiva viene salvata come `film_locations_map.html` nella directory di lavoro. Grafici e tabelle vengono anche mostrati durante l'esecuzione.

## 6. Periodizzazione

Le analisi distinguono il lungo periodo storico dal sottoinsieme contemporaneo **2000–2025**. Gran parte delle serie quinquennali parte dal **1945**; alcune sezioni dedicate a mobilità e autonomia utilizzano un filtro che parte dal **1944**, aggregando comunque gli anni in blocchi di cinque mediante divisione intera (`anno // 5 * 5`). Per confrontare tabelle o riprodurre una figura, verificare l'intervallo indicato nella specifica sezione del codice.

## 7. Limiti, riproducibilità e visualizzazioni della tesi

Il repository permette di esaminare il codice e, **una volta ottenuti i due CSV**, di rieseguire le analisi quantitative. Non comprende gli script originari di raccolta automatizzata da IMDb né la cronologia completa di OpenRefine: la riproducibilità riguarda quindi la fase analitica e non l'intera costruzione del dataset a partire dalle fonti primarie.

I risultati dipendono dalla copertura e dalla qualità delle informazioni presenti in IMDb al momento della raccolta, dalle scelte di selezione del corpus e dalla disponibilità di corrispondenze geografiche. I film senza location utilizzabili restano nel dataset di riferimento, ma non contribuiscono ai conteggi che richiedono un territorio identificato. Le coordinate sono necessarie per la mappa interattiva, ma non per tutte le analisi territoriali.

Come specificato nel testo della tesi, **le visualizzazioni presenti nell'elaborato sono state realizzate a partire dai dati prodotti dalle operazioni documentate in questo repository, anche mediante strumenti esterni**, tra cui **Datawrapper** e **Flourish**. Il repository documenta i calcoli e la produzione dei dati intermedi, ma non necessariamente la configurazione grafica esatta di ogni figura pubblicata.

## 8. Riferimento alla ricerca

Questo repository accompagna la tesi di dottorato *Regionalità, localizzazione e prospettive data-driven sulle geografie del cinema italiano*. Per la discussione metodologica, l'interpretazione dei risultati e la contestualizzazione teorica, fare riferimento al testo della tesi.

**Autore:** Alberto Savi  
**Istituzione:** Unimi e Unimore  
**Anno:** 2026  
**Tesi / DOI:** [collegamento da aggiungere quando disponibile]

## 9. Fonti e attribuzioni

I metadati cinematografici e le informazioni sulle *filming locations* provengono da **IMDb**. Le procedure di arricchimento geografico si sono avvalse di **Wikidata** e **OpenStreetMap**. La presenza dei rispettivi dati e identificativi non implica che tali organizzazioni abbiano partecipato alla ricerca o ne approvino le conclusioni.

Le condizioni di utilizzo e redistribuzione dei dati seguono quelle applicabili alle rispettive fonti. Un'eventuale licenza del codice deve essere specificata separatamente e non si estende automaticamente ai dati di terze parti.

---

<a id="english"></a>

# English

This repository contains the Python code and documentation for the quantitative analyses developed as part of the doctoral thesis *Regionalità, localizzazione e prospettive data-driven sulle geografie del cinema italiano* (*Regionality, Localization, and Data-Driven Perspectives on the Geographies of Italian Cinema*). The source datasets are **not publicly included** in this repository but may be requested from the author.

Its purpose is to make available the computational procedures applied to a corpus of Italian films compiled from IMDb, focusing on the territorial distribution of filming locations, regional differences, and their evolution over time. The repository documents the **analysis of data that have already been collected, cleaned, and enriched**; it does not automatically reproduce the entire preliminary process of corpus extraction and preparation.

## 1. Corpus and sources

The research corpus comprises **11,418 films**. It results from the selection, cleaning, and enrichment of IMDb information, with particular attention to the *filming locations* field. It is a **research snapshot**, not a continuously updated copy of the original database; the data collection process described in the thesis ends in November 2025.

Location information was standardized and, where possible, reconciled with **Wikidata** using **OpenRefine**, in order to associate places with identifiers and geographical attributes. Open geographical data, including **OpenStreetMap**, were also used to enrich territorial information. The initial selection, cleaning and reconciliation procedures, and their limitations are discussed in the methodology section of the thesis. The OpenRefine project and the history of preliminary transformations are not included in this repository.

IMDb filming locations are an opportunistic source: their availability, completeness, and accuracy vary between films. Consequently, the findings describe the geographies **documented in the corpus**, not every location actually used in Italian filmmaking.

## 2. Repository contents and data access

The public repository contains the analysis code and its documentation. The script requires two source datasets, **neither of which is included in the public repository**:

| File | Description | Availability |
| --- | --- | --- |
| `clean_version_italian_cinema_geo_imdb.py` | Python export of the Google Colab notebook containing the analyses. | Publicly available in the repository |
| `1_reference_backup_film_IMDb.csv` | One row per film, including IMDb identifier, titles, year, and other film metadata. | Available on request |
| `geografie_cinema_ita_imdb.csv` | Long-format geographical dataset: a film may appear in multiple rows corresponding to its recorded locations. Includes place, coordinates, and territorial classifications where available. | Available on request |

The first CSV contains **11,418 rows**; the second contains **18,495 rows** relating to the same **11,418 IMDb identifiers**. This difference reflects the one-to-many structure of film–location associations and the inclusion of films without usable geographical information.

**Requesting the data.** Researchers wishing to access the datasets for study, verification, or reproducibility purposes may contact **Alberto Savi** at [albertosavi95@gmail.com](mailto:albertosavi95@gmail.com). Data sharing is subject to the applicable terms of the original sources.

The script generates derived CSV tables, charts, and `film_locations_map.html`, an interactive map of locations with available coordinates. These outputs are created during execution and are not required as inputs.

## 3. Included analyses

The code includes the following operations:

1. **Location mapping:** interactive Folium map with clustered markers and links to IMDb film pages.
2. **Geographical distribution:** counts by continent, country, and Italian region, including five-year aggregations.
3. **Regional incidence and temporal variation:** regional relative frequencies and coefficients of variation, aggregated annually and over five-year periods.
4. **Regional mobility:** number of regions associated with each film, mobility coefficient, class distributions, and temporal trends; additional analyses exclude Lazio or films associated exclusively with Lazio.
5. **Regional autonomy:** the proportion of single-region films among all films associated with a given region, including analyses for 2000–2025 and five-year periods.
6. **Territorial typology:** classification of regions into four quadrants based on their national incidence and regional autonomy index, both for 2000–2025 and for the full dataset.

The published version contains **the operations selected for the thesis as well as some additional analyses**, but not the entire exploratory workflow developed during the research.

## 4. Units of analysis and indicators

**Film–location associations.** For geographical counts, each unique `IMDb_ID`–territory pair contributes one occurrence at the geographical level being considered. For example, a film with five locations in the same region is counted once for that region; if it is associated with two regions, it contributes once to each. The sum of regional counts may therefore exceed the number of distinct films.

**Regional relative frequency.** A region's number of film–region associations is divided by the sum of film–region associations across all regions in the relevant period. This indicator represents the regional share of associations, **not** the percentage of unique films in the corpus associated with that region.

**Regional mobility coefficient.** For each film, the number of distinct regions (`Region_Variety`) is normalized using:

```text
Mobility_Coefficient = (Region_Variety - 1) / (max_region_variety - 1)
```

A film associated with a single region receives 0; a value of 1 corresponds to the maximum regional variety observed in the reference sample. The script recalculates this maximum for certain subsets; normalized values obtained using different maxima are not directly comparable without accounting for this difference.

**Regional autonomy index.** This is the ratio between films associated exclusively with a given region and all films associated with that region, including those also associated with other regions. It measures the exclusivity of territorial associations **within the available data**; it does not measure economic or production independence.

**Regional quadrants.** The medians of national incidence and the regional autonomy index divide the regions into four interpretative categories: `centrale` (central), `periferica autonoma` (autonomous peripheral), `di snodo` (connecting), and `subordinata` (subordinate). These categories depend on the indicators and period selected and are not general evaluations of individual regions.

## 5. Running the analyses

The code was developed and run primarily in **Google Colab**. To reproduce the analyses, first request the two CSV files as described in Section 2.

1. Open the `.py` file in a compatible Python environment, or use the corresponding Colab notebook version if available.
2. Place both CSV files in the Colab working directory (`/content/`).
3. Install the required libraries if necessary.
4. Run the code from beginning to end, without skipping sections that define variables reused later.

Main dependencies: `pandas`, `numpy`, `matplotlib`, `seaborn`, and `folium`. For local installation:

```bash
python -m pip install pandas numpy matplotlib seaborn folium
```

**Path note:** the script is exported from Colab and uses both absolute `/content/...` paths and relative filenames. Reading and output paths must be adapted when running it outside Colab. Some instructions, including `display()` and direct map display, are intended for a notebook environment.

Output CSVs are mainly saved under `/content/`; the interactive map is saved as `film_locations_map.html` in the working directory. Charts and tables are also displayed during execution.

## 6. Periodization

The analyses distinguish the longer historical period from the contemporary **2000–2025** subset. Most five-year series begin in **1945**; some mobility and autonomy sections use a filter beginning in **1944**, while still grouping years into five-year intervals through integer division (`anno // 5 * 5`). When comparing tables or reproducing a figure, check the date range specified in the relevant section of the code.

## 7. Limitations, reproducibility, and thesis visualizations

The repository makes the code available for inspection and allows the quantitative analyses to be rerun **once the two CSV files have been obtained**. It does not include the original IMDb collection scripts or the complete OpenRefine history. Reproducibility therefore applies to the analytical stage, not to the entire construction of the dataset from primary sources.

The results depend on the coverage and quality of IMDb information at the time of collection, corpus selection decisions, and the availability of geographical matches. Films without usable locations remain in the reference dataset but do not contribute to counts requiring an identified territory. Coordinates are required for the interactive map but not for every territorial analysis.

As explained in the thesis, **the visualizations included in the dissertation were created from data produced by the operations documented in this repository, also using external visualization tools**, including **Datawrapper** and **Flourish**. The repository documents the calculations and generation of intermediate data, but does not necessarily reproduce the exact graphical configuration of every published figure.

## 8. Research reference

This repository accompanies the doctoral thesis *Regionalità, localizzazione e prospettive data-driven sulle geografie del cinema italiano*. For methodological discussion, interpretation of the findings, and theoretical context, please refer to the thesis.

**Author:** Alberto Savi  
**Institutions:** University of Milan (Unimi) and University of Modena and Reggio Emilia (Unimore)  
**Year:** 2026  
**Thesis / DOI:** [link to be added when available]

## 9. Sources and attribution

Film metadata and *filming locations* information originate from **IMDb**. Geographical enrichment used **Wikidata** and **OpenStreetMap**. The inclusion of their data and identifiers does not imply that these organizations participated in the research or endorse its findings.

Use and redistribution of data are subject to the applicable terms of their respective sources. Any license applied to the code must be specified separately and does not automatically extend to third-party data.
