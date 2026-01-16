# Proiect RNP - Predictie Consum Energie (Time Series)

## Descriere
Acest proiect are ca scop predictia consumului de energie electrica utilizand serii de timp. Proiectul reprezinta o analiza comparativa intre trei paradigme diferite de modelare:
1. Deep Learning (Secvential): LSTM, GRU, SimpleRNN.
2. Machine Learning Clasic (Tabelar): XGBoost, LightGBM (Gradient Boosting).
3. Model Statistic (Baseline): Prophet.

Datele reprezinta consumul de energie (in watti) masurat la fiecare minut pentru doua locuinte rezidentiale (House1 si House2).

## Structura Proiectului
Proiectul este impartit in 3 etape distincte (Notebook-uri):

1. **01_Data_Prep.ipynb**: Procesarea datelor brute, crearea de feature-uri si clustering (K-Means).
2. **02_Models_Training.ipynb**: Antrenarea efectiva a celor 7 modele si salvarea rezultatelor.
3. **03_Analysis_Comparison.ipynb**: Analiza performantei, calcularea metricilor si generarea clasamentului final.

## Metodologie si Procesare

### 1. Pregatirea Datelor
- Curatare: Eliminarea valorilor lipsa si re-esantionare la frecventa de 1 ora.
- Clustering: Utilizarea algoritmului K-Means pentru a identifica regimuri de consum (Cluster 0 vs Cluster 1) si utilizarea acestora ca feature suplimentar.
- Feature Engineering:
    - Extragerea informatiilor temporale: Ora, Ziua saptamanii, Luna.
    - Transformari ciclice: Sinus si Cosinus pentru ora (Hour_Sin, Hour_Cos).
    - Indicatori binari: IsWeekend.
- Scalare: Utilizarea RobustScaler pentru a reduce impactul outlierilor (varfurilor de consum).
- Secventiere: Crearea unor ferestre glisante (Window Size = 48 ore) pentru modelele de Deep Learning.

### 2. Feature Engineering pentru ML (XGBoost/LightGBM)
Deoarece modelele de tip Gradient Boosting nu proceseaza secvente temporale direct, datele au fost transformate in format tabelar folosind:
- Lag Features: Valorile consumului din trecut (t-1, t-24, t-68 ore).
- Rolling Statistics: Media mobila si deviatia standard calculate pe o fereastra de 24 de ore.

### 3. Modele Implementate
- **LSTM (Stacked)**: Retea recurenta cu straturi multiple si Dropout pentru regularizare.
- **GRU**: Arhitectura recurenta optimizata pentru eficienta computationala.
- **SimpleRNN**: Retea recurenta de baza.
- **XGBoost & LightGBM**: Algoritmi de Gradient Boosting antrenati pe setul de date tabelar extins.
- **Prophet**: Model aditiv utilizat ca referinta pentru sezonalitate.

## Rezultate si Concluzii

In urma experimentelor realizate pe cele doua gospodarii analizate (House1 si House2), pot fi formulate urmatoarele concluzii:

1. Performanta superioara a modelelor bazate pe Gradient Boosting
Modelele LightGBM si XGBoost au obtinut cele mai bune rezultate pe ambele case, inregistrand cele mai mici valori ale erorilor MAE si RMSE. Valorile MAE s-au situat in intervalul 149–176 W, demonstrand o capacitate ridicata de a surprinde relatiile neliniare dintre caracteristicile temporale si consumul agregat de energie.

2. Eficienta computationala ridicata pentru modelele ML clasice
Modelele LightGBM si XGBoost s-au antrenat foarte rapid (sub 1 secunda per casa), oferind in acelasi timp performante superioare fata de modelele de deep learning. Acest lucru le face extrem de potrivite pentru aplicatii practice unde timpul de antrenare si costurile computationale sunt critice.

3. Modelele de Deep Learning nu au depasit ML-ul clasic
Dintre modelele neuronale testate, LSTM si GRU au avut performante comparabile, insa erorile obtinute au fost mai mari decat cele ale modelelor bazate pe arbori de decizie. In plus, timpul de antrenare a fost semnificativ mai mare (peste 60 de secunde), fara un castig clar de acuratete. Acest rezultat sugereaza ca, pentru datele tabulare cu feature engineering explicit, modelele ML clasice sunt mai eficiente decat retelele recurente.

4. Performanta limitata pentru modelele de tip baseline
Modelul Prophet a inregistrat cele mai mari erori pentru ambele gospodarii. Acesta nu a reusit sa surprinda volatilitatea ridicata si schimbarile rapide de regim (weekday/weekend) caracteristice consumului energetic la nivel de locuinta, confirmand limitarile sale in contextul seriilor temporale foarte zgomotoase si neliniare.

## Cum sa rulezi proiectul

1. Cloneaza repository-ul:
   git clone https://github.com/pirjolmihaita/RNP-Preditie-Consum.git

2. Instaleaza bibliotecile necesare:
   pip install -r requirements.txt

3. Datele:
   Asigura-te ca fisierele `CLEAN_House1.csv` si `CLEAN_House2.csv` se afla in directorul radacina al proiectului.
   `CLEAN_House1.csv` -> https://drive.google.com/file/d/1HZdtZAHk93Gdk01I-Uq8FJfOHEMQ8U08/view?usp=drive_link
   `CLEAN_House2.csv1` -> https://drive.google.com/file/d/1LcBhxeuBioOCyqIfqOJrY7BMhI9e-KaX/view?usp=drive_link

4. Executie:
   Ruleaza notebook-urile in ordine numerica:
   - Pasul 1: `01_Data_Prep.ipynb` (Genereaza fisierele .pkl procesate)
   - Pasul 2: `02_Models_Training.ipynb` (Antreneaza modelele si salveaza predictiile)
   - Pasul 3: `03_Analysis_Comparison.ipynb` (Vizualizeaza rezultatele si clasamentul)
