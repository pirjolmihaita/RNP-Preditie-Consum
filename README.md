# Proiect RNP - Predictie Consum Energie (Time Series)

## Descriere
Acest proiect are ca scop predictia consumului de energie electrica utilizand serii de timp. Proiectul reprezinta o analiza comparativa intre trei paradigme diferite de modelare:
1. Deep Learning (Secvential): LSTM, GRU, SimpleRNN, DeepAR (Abordare probabilistica).
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
- Curatare: Eliminarea valorilor lipsa si re-esantionare la frecventa de 1 minut.
- Clustering: Utilizarea algoritmului K-Means pentru a identifica regimuri de consum (Cluster 0 vs Cluster 1) si utilizarea acestora ca feature suplimentar.
- Feature Engineering:
    - Extragerea informatiilor temporale: Ora, Ziua saptamanii.
    - Transformari ciclice: Sinus si Cosinus pentru ora (Hour_Sin, Hour_Cos).
    - Indicatori binari: IsWeekend.
- Scalare: Utilizarea RobustScaler pentru a reduce impactul outlierilor (varfurilor de consum).
- Secventiere: Crearea unor ferestre glisante (Window Size = 60 minute) pentru modelele de Deep Learning.

### 2. Feature Engineering pentru ML (XGBoost/LightGBM)
Deoarece modelele de tip Gradient Boosting nu proceseaza secvente temporale direct, datele au fost transformate in format tabelar folosind:
- Lag Features: Valorile consumului din trecut (t-1, t-5, t-15, t-60 minute).
- Rolling Statistics: Media mobila si deviatia standard calculate pe o fereastra de 60 de minute.

### 3. Modele Implementate
- **LSTM (Stacked)**: Retea recurenta cu straturi multiple si Dropout pentru regularizare.
- **GRU**: Arhitectura recurenta optimizata pentru eficienta computationala.
- **SimpleRNN**: Retea recurenta de baza.
- **DeepAR**: Model probabilistic bazat pe LSTM care estimeaza parametrii distributiei (Mu si Sigma) folosind Laplace Negative Log Likelihood.
- **XGBoost & LightGBM**: Algoritmi de Gradient Boosting antrenati pe setul de date tabelar extins.
- **Prophet**: Model aditiv utilizat ca referinta pentru sezonalitate.

## Rezultate si Concluzii
In urma experimentelor pe ambele case (House1 si House2), s-au observat urmatoarele:

1. **Performanta Superioara a Gradient Boosting**: Modelele LightGBM si XGBoost au obtinut cele mai mici erori (MAE intre 52W si 63W), avand o eroare relativa de aproximativ 11-13% din consumul mediu.
2. **Eficienta**: Modelele ML clasice s-au antrenat in cateva secunde (4-8 secunde), in timp ce retelele neuronale (LSTM, GRU, DeepAR) au necesitat peste o ora pentru antrenare, oferind rezultate usor inferioare.
3. **Deep Learning**: Dintre modelele neuronale, GRU a oferit cel mai bun echilibru, dar nu a reusit sa depaseasca modelele bazate pe arbori pe acest tip de date tabulare.
4. **Baseline**: Prophet nu a reusit sa captureze volatilitatea ridicata a datelor la nivel de minut, inregistrand cele mai mari erori.

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