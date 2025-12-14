# Proiect RNP - Predictie Consum Energie

## Descriere
Acest proiect are ca scop predictia consumului de energie. Am folosit in principal Retele Neuronale Recurente, dar am testat si un model statistic pentru comparatie. Datele reprezinta consumul de energie (in watti) masurat la fiecare minut.

## Ce am facut pana acum
1. Pregatirea datelor:
   - Am citit datele din fisierul CSV.
   - Am curatat datele (sters valori lipsa).
   - Am reesantionat datele la o frecventa de 1 minut.
   - Am impartit datele in antrenare (80%) si testare (20%).
   - Am scalat datele (0-1) pentru ca retelele neuronale sa invete corect.

2. Modele implementate:
   - Retele Neuronale (Focusul proiectului):
     - LSTM (Long Short-Term Memory): retea buna pentru secvente lungi.
     - RNN (Recurrent Neural Network): retea clasica, folosind celule GRU.
     - DeepAR: implementare proprie folosind LSTM si o functie de loss probabilistica.
   - Model de comparatie (Baseline):
     - Prophet: model statistic de la Facebook, folosit pentru a vedea daca retelele neuronale obtin rezultate mai bune decat metodele clasice.

3. Evaluare:
   - Am antrenat fiecare model.
   - Am comparat erorile (RMSE si MAE).
   - Am generat grafice comparative intre predictiile modelelor si datele reale.

## Rezultate
Retelele neuronale (in special LSTM si RNN) au reusit sa invete tiparul consumului mult mai bine decat modelul statistic Prophet, avand o eroare mult mai mica. Detaliile si graficele sunt in notebook.

## Planuri de viitor
- Antrenare pe mai multe case din dataset pentru a verifica generalizarea.
- Optimizarea parametrilor retelelor (numar neuroni, epoci) pentru a scadea eroarea.
- Adaugarea de noi feature-uri (ora, ziua) pentru a ajuta reteaua sa inteleaga sezonalitatea.

## Cum sa rulezi proiectul
1. Cloneaza repository-ul.
2. Instaleaza bibliotecile din `requirements.txt`.
3. Asigura-te ca ai fisierul de date (CSV).
4. Ruleaza notebook-ul.
