Exercise 1

Creati o diagrama a bazei de date , folosind forma de vizulalizare standard,structura careia e descrisa la inceputul sarcinilor practice din capitolul 4.

<img width="932" alt="lab7_1" src="https://user-images.githubusercontent.com/43128526/48680747-ae874800-eba6-11e8-9780-7806c6aa3c5f.png">
  
 Exercise 2
 
 Sa se adauge constringeri referentiale (legate cu tabelele studenti si profesori) necesare coloanelor Sef_grupa și Prof_Indrumator (sarcina3, capitolul 6) din tabelul grupe.
 
 <img width="524" alt="lab7ex2_1" src="https://user-images.githubusercontent.com/43128526/49348452-36dd1100-f6ae-11e8-8884-c4e27bddc567.png">

<img width="497" alt="lab7ex2_2" src="https://user-images.githubusercontent.com/43128526/49348455-380e3e00-f6ae-11e8-91eb-67b52c69eab1.png">

Exercise 3 

La diagrama construita, sa se adauge și tabelul orarul definit în capitolul 6 al acestei lucrari:tabelul orarul conține identificatorul disciplinei (ld_Disciplina), identificatorul profesorului(Id_Profesor) și blocul de studii (Bloc). Cheia tabelului este constituita din trei campuri:identificatorul grupei (Id_ Grupa), ziua lectiei (Z1), ora de inceput a lectiei (Ora), sala unde are loc lectia (Auditoriu)

<img width="840" alt="lab7ex3" src="https://user-images.githubusercontent.com/43128526/49348478-64c25580-f6ae-11e8-8333-b5e38bc62871.png">

Exercise 4

Tabelul orarul trebuie să conțina și 2 chei secundare: (Zi, Ora, Id_ Grupa, Id_ Profesor) și (Zi, Ora, ld_Grupa, ld_Disciplina).

<img width="889" alt="lab7ex4_1" src="https://user-images.githubusercontent.com/43128526/49348533-a3581000-f6ae-11e8-8752-5767b0ce15ab.png">
<img width="899" alt="lab7ex4_2" src="https://user-images.githubusercontent.com/43128526/49348535-a6530080-f6ae-11e8-9c9e-0a32b58e6d37.png">
<img width="835" alt="lab7ex4_3" src="https://user-images.githubusercontent.com/43128526/49348537-a94df100-f6ae-11e8-9d8d-e62afe14163f.png">

Exercise 5

In diagrama, de asemenea, trebuie sa se defineasca constrangerile referentiale (FK-PK) ale atributelor ld_Disciplina, ld_Profesor, Id_ Grupa din tabelului orarul cu atributele tabelelor respective.

<img width="892" alt="lab7ex5_4" src="https://user-images.githubusercontent.com/43128526/49348585-e619e800-f6ae-11e8-8cd0-ca6152f94961.png">

Exercise 6

Creati, în baza de date universitatea, trei scheme noi: cadre_didactice, plan_studii și studenti. Transferati tabelul profesori din schema dbo in schema cadre didactice, tinind cont de dependentele definite asupra tabelului menționat. In acelasi mod ss se trateze tabelele orarul,discipline care aparțin schemei plan_studii și tabelele studenti, studenti_reusita, care apartin schemei studenti. Se scrie instructiunile SQL respective.

<img width="318" alt="lab7ex6_1" src="https://user-images.githubusercontent.com/43128526/49348632-2f6a3780-f6af-11e8-8c01-91e51b7af35f.png">
<img width="159" alt="lab7" src="https://user-images.githubusercontent.com/43128526/49348644-3db85380-f6af-11e8-985f-b5d9c02f9a2d.png">

Exercise 7

Modificati 2-3 interogari asupra bazei de date universitatea prezentate in capitolul 4 astfel ca numele tabelelor accesate sa fie descrise in mod explicit, tinînd cont de faptul ca tabelele au fost mutate in scheme noi.

<img width="825" alt="lab7ex7_1" src="https://user-images.githubusercontent.com/43128526/49348688-748e6980-f6af-11e8-8188-d4858e53e281.png">
<img width="821" alt="lab7ex7_2" src="https://user-images.githubusercontent.com/43128526/49348690-76582d00-f6af-11e8-9328-1945a9926026.png">

Exercise 8

Creați sinonimele respective pentru a simplifica interogările construite în exercițiul precedent și reformulați interogările, folosind sinonimele create.

<img width="693" alt="lab7ex8_1" src="https://user-images.githubusercontent.com/43128526/49348883-b4a21c00-f6b0-11e8-8e60-ad4a8262ab3a.png">
<img width="630" alt="lab7ex8_2" src="https://user-images.githubusercontent.com/43128526/49348884-b5d34900-f6b0-11e8-8709-323e96c4c1b6.png">


