# Stima della temperatura corporea da immagini termiche del volto

Progetto di **Intelligenza Artificiale 2 – A.A. 2026/27**

**Autori:** Nome Cognome 1 (matricola 000000) · Nome Cognome 2 (matricola 000000)

## Descrizione

Le termocamere usate per lo screening della febbre misurano la temperatura della *pelle*, che dipende dall'ambiente. In questo progetto affrontiamo un problema di **regressione**: stimare la temperatura orale misurata con termometro clinico (`aveOralM`, °C) a partire da:
- 26 temperature lette su diverse regioni del volto da una termocamera a infrarossi;
- temperatura e umidità dell'ambiente e distanza dalla termocamera;
- dati demografici.

Il notebook contiene:
1. caricamento e controllo di qualità dei dati (valori mancanti, outlier, categorie incoerenti);
2. analisi esplorativa (statistiche, distribuzioni, correlazioni, PCA);
3. preprocessing e feature engineering in una `Pipeline` scikit-learn, senza data leakage;
4. confronto di 10 modelli di regressione con GridSearchCV a 5 fold: baseline, lineare, Ridge, Lasso, PCA+Ridge, SVR, KNN, Random Forest, Gradient Boosting;
5. valutazione sul test set, analisi degli errori (Bland–Altman, errore per fascia di temperatura), permutation importance e analisi del bias per sottogruppi;
6. estensione alla **classificazione febbre** (≥ 37.5 °C);
7. verifica di robustezza su una seconda termocamera.

**Risultato principale.** Il modello migliore è una SVR con kernel RBF. Sul test set ottiene **MAE 0.169 °C**, RMSE 0.222 °C e R² 0.787; l'86% dei soggetti ha un errore entro ±0.3 °C.

## Dataset

*Infrared Thermography Temperature Dataset*: [UCI ML Repository, id 925](https://archive.ics.uci.edu/dataset/925/infrared+thermography+temperature+dataset).
I dati originali vengono da [PhysioNet – Facial and oral temperature data from a large set of human subject volunteers v1.0.0](https://physionet.org/content/face-oral-temp-data/1.0.0/) e sono rilasciati con licenza CC0 1.0.

I file usati sono nella cartella `data/`:

| File | Contenuto |
|---|---|
| `FLIR_group1.csv`, `FLIR_group2.csv` | Termocamera IRT-1 (FLIR): dataset principale, 1020 soggetti |
| `ICI_group1.csv`, `ICI_group2.csv` | Termocamera IRT-2 (ICI): solo per la verifica di robustezza |
| `_Table1.pdf`, `_Table2.pdf` | Definizione delle variabili (dalla documentazione del dataset) |

Il gruppo 1 è stato acquisito con temperatura ambiente tra 20 e 24 °C, il gruppo 2 tra 24 e 29 °C.

Citazioni richieste dagli autori del dataset:
- Wang Q. et al., *Infrared Thermography for Measuring Elevated Body Temperature: Clinical Accuracy, Calibration, and Evaluation*, Sensors 22(1):215, 2022. https://doi.org/10.3390/s22010215
- Wang Q. et al., *Facial and oral temperature data from a large set of human subject volunteers* (v1.0.0), PhysioNet, 2023. https://doi.org/10.13026/3bhc-9065

## Struttura del repository

```
.
├── README.md
├── requirements.txt
├── progetto_IA2.ipynb   # notebook completo (già eseguito, con output)
├── data/                # dataset originale (CSV)
├── figures/             # grafici generati dal notebook
└── results/             # tabelle dei risultati (CSV) generate dal notebook
```

## Librerie utilizzate

- Python ≥ 3.10
- numpy, pandas: manipolazione dei dati
- matplotlib, seaborn: grafici
- scikit-learn: preprocessing, modelli, cross-validation, metriche

Le versioni esatte usate per i risultati sono stampate nella prima cella del notebook: numpy 2.5, pandas 3.0, scikit-learn 1.9, matplotlib 3.11, seaborn 0.13.

## Come riprodurre i risultati

```bash
git clone <url-del-repository>
cd <cartella-del-repository>
python -m venv .venv
source .venv/bin/activate        # su Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook progetto_IA2.ipynb
```

Poi esegui tutte le celle (*Run → Run All*). L'esecuzione completa richiede circa 2 minuti su un portatile. Il seed è fissato (`SEED = 42`), quindi i risultati sono riproducibili. Il notebook crea le cartelle `figures/` e `results/`.

**Google Colab:** carica il notebook e la cartella `data/` (oppure clona il repository in Colab) ed esegui tutte le celle. Le librerie sono già installate su Colab.
