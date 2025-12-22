# Proiect RNP - Predicție Consum Energie (Time Series)

## # Proiect RNP - Predictie Consum Energie (Time Series)

## Descriere
Acest proiect are ca scop predictia consumului de energie electrica utilizand serii de timp. Proiectul exploreaza si compara performanta a trei paradigme diferite:
1. Deep Learning (Secvential): LSTM, GRU, RNN, DeepAR.
2. Machine Learning Clasic (Tabelar): XGBoost, LightGBM (Gradient Boosting).
3. Model Statistic (Baseline): Prophet (Facebook).

Datele reprezinta consumul de energie (in watti) masurat la fiecare minut pentru o locuinta rezidentiala.

## Metodologie si Procesare

### 1. Pregatirea Datelor
- Curatare: Eliminarea valorilor lipsa si a outlierilor evidenti.
- Re-esantionare: Datele au fost aduse la o frecventa uniforma de 1 minut.
- Scalare: Normalizare MinMax (0-1) pentru retelele neuronale.
- Split: Impartirea datelor in Antrenare (80%) si Testare (20%).

### 2. Feature Engineering (Pentru modelele ML)
Pentru a aplica algoritmi de tip Gradient Boosting (care nu proceseaza secvente direct ca LSTM), am transformat seria de timp in date tabelare:
- Lag Features: Valorile consumului din trecut (t-1, t-5, t-15, t-60 minute).
- Rolling Statistics: Media mobila si deviatia standard pe fereastra de 1 ora.
- Date Temporale: Codificarea ciclica a orei (Sin/Cos) si indicatori pentru Weekend.

### 3. Modele Implementate
- LSTM (Long Short-Term Memory): Retea recurenta capabila sa invete dependente pe termen lung.
- GRU & RNN: Variante simplificate ale arhitecturilor recurente.
- DeepAR: Model probabilistic (bazat pe LSTM) care estimeaza si incertitudinea predictiei, nu doar valoarea fixa.
- XGBoost & LightGBM: Modele bazate pe arbori de decizie, antrenate pe features derivate manual.
- Prophet: Model statistic folosit ca referinta (baseline).

## Rezultate si Concluzii
Analiza comparativa a relevat urmatoarele:

1. Viteza vs. Performanta: Modelele de Machine Learning (LightGBM, XGBoost) au fost extrem de rapide la antrenare (secunde) si au oferit o acuratete surprinzatoare, depasind in unele cazuri retelele neuronale complexe pe datele tabulare.
2. Deep Learning: LSTM si DeepAR au demonstrat o capacitate buna de generalizare a trendului, dar cu un cost computational mult mai ridicat (timpi de antrenare de ordinul zecilor de minute).
3. Baseline: Prophet a oferit o baza de comparatie utila, dar a avut dificultati in a captura volatilitatea rapida a consumului la nivel de minut.

Graficele detaliate cu predictiile si tabelul complet cu metricile (MAE, RMSE) sunt disponibile in Notebook.

## Directii Viitoare
Pentru a imbunatati performanta si robustetea proiectului, se pot aborda urmatoarele directii:

1. Generalizare: Antrenarea si validarea modelelor pe mai multe case din dataset pentru a verifica capacitatea de generalizare a arhitecturilor.
2. Hyperparameter Tuning: Utilizarea unor biblioteci automate (precum Optuna) pentru a optimiza numarul de neuroni, learning rate-ul si parametrii arborilor decizionali.


## Cum sa rulezi proiectul

1. Cloneaza repository-ul:
   git clone <link-ul-tau-de-github>

2. Instaleaza bibliotecile necesare:
   pip install -r requirements.txt

3. Datele:
   Asigura-te ca fisierul CLEAN_House1.csv se afla in directorul radacina.

4. Ruleaza Notebook-ul:
   Deschide fisierul .ipynb in Jupyter Notebook, JupyterLab sau Google Colab si ruleaza celulele secvential.