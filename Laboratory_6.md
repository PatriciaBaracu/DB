
Exercise 1 

Sa se scrie o intructiune T-SQL, care ar popula coloana Adresa_Postala_Profesor din tabelul profesori cu valoarea "mun.Chisinau",unde adresa e necunoscuta.

```sql
UPDATE profesori 

SET Adresa_Postala_Profesor='mun.Chisinau' 

WHERE Adresa_Postala_Profesor IS NULL;

SELECT Nume_Profesor,Prenume_Profesor,Adresa_Postala_Profesor 

FROM profesori
```

<img width="408" alt="lab6_1" src="https://user-images.githubusercontent.com/43128526/47971541-3f383100-e09b-11e8-965e-497cff5ebe2b.png">

Exercise 2

Sa se modifice schema tabelului grupe, ca sa corespunda urmatoarelor cerinte:

a) Campul Cod_ Grupa sa accepte numai valorile unice și să nu accepte valori necunoscute. 

b) Să se țină cont că cheie primară, deja, este definită asupra coloanei Id_ Grupa. 


create unique index idx_cod_grupa

on grupe (cod_grupa)

exec sp_helpindex grupe;

<img width="448" alt="ex2" src="https://user-images.githubusercontent.com/43128526/48787325-38432c80-ecf1-11e8-98b8-15bc693b4d82.png">
<img width="365" alt="ex2_1" src="https://user-images.githubusercontent.com/43128526/48787334-3aa58680-ecf1-11e8-94e3-289291337981.png">

Exercise 3

La tabelul grupe, să se adauge 2 coloane noi Sef_grupa și Prof_Indrumator, ambele de tip INT. Să se populeze câmpurile nou-create cu cele mai potrivite candidaturi în baza criteriilor de mai jos:

a) Șeful grupei trebuie să aibă cea mai bună reușită (medie) din grupă la toate formele de evaluare și la toate disciplinele. Un student nu poate fi șef de grupa la mai multe grupe.

b) Profesorul îndrumător trebuie să predea un număr maximal posibil de discipline la grupa data. Daca nu există o singură candidatură, care corespunde primei cerințe, atunci este ales din grupul de candidați acel cu identificatorul (Id_Profesor) minimal. Un profesor nu poate fi indrumător la mai multe grupe.

c) Să se scrie instructiunile ALTER, SELECT, UPDATE necesare pentru crearea coloanelor în tabelul grupe, pentru selectarea candidaților și înserarea datelor.

```sql
alter table grupe add sef_grupa int;

alter table grupe add prof_indrumator int ;

DECLARE @nr_grup int = (SELECT COUNT(Id_Grupa) FROM grupe)

DECLARE @index int =1;


while(@index<=@nr_grup)

	begin

			update grupe
			
			SET sef_grupa = (SELECT top 1 stud.Id_Student
			FROM ( SELECT  Id_Student,avg(cast(Nota as float)) as Media_studentului
			FROM studenti_reusita 
			WHERE Id_Grupa=@index
			GROUP BY Id_Student) stud
			ORDER BY stud.Media_studentului DESC),
			
			prof_indrumator = (SELECT prof.Id_Profesor
			FROM( SELECT top 1 Id_Profesor ,count(DISTINCT Id_Disciplina) as numarul_disciplinei
			FROM studenti_reusita
			WHERE Id_Grupa=@index
			GROUP BY Id_Profesor 
			ORDER BY numarul_disciplinei DESC) prof)
			
			WHERE Id_Grupa=	@index
			SET @index = @index+1;
END

ALTER TABLE grupe ADD CONSTRAINT prof_stud UNIQUE (sef_grupa,prof_indrumator);
```

<img width="581" alt="ex3" src="https://user-images.githubusercontent.com/43128526/48787637-e3ec7c80-ecf1-11e8-8cd8-a5253d5f3002.png">
<img width="541" alt="ex3_2" src="https://user-images.githubusercontent.com/43128526/48787856-46de1380-ecf2-11e8-893f-bbd599d68861.png">
<img width="591" alt="ex3_1" src="https://user-images.githubusercontent.com/43128526/48787647-e949c700-ecf1-11e8-9ade-a3a19fe1cb30.png">

Exercise 4

Să se scrie o instrucțiune T-SQL, care ar mări toate notele de evaluare șefilor de grupe cu un punct. Nota maximală (10) nu poate fi mărită.

```sql
declare @counter int = 0

select distinct top (10) st.Nume_Student, st.Prenume_Student,sr.Nota

from studenti_reusita as sr

	inner join studenti as st
  
		on sr.Id_Student = st.Id_Student
    
	inner join discipline as d
  
	on d.Id_Disciplina = sr.Id_Disciplina
  
  where d.Disciplina = 'Baze de date' and sr.Tip_Evaluare = 'Testul 1'
	
  and NOT sr.Nota=6 and NOT sr.Nota=8
```

<img width="342" alt="ex4" src="https://user-images.githubusercontent.com/43128526/48788109-cb309680-ecf2-11e8-9680-b238495c0f67.png">

Exercise 5
Sa se creeze un tabel profesori_new, care include urmatoarele coloane: Id_Profesor,Nume _ Profesor, Prenume _ Profesor, Localitate, Adresa _ 1, Adresa _ 2.

a) Coloana Id_Profesor trebuie sa fie definita drept cheie primara și, în baza ei, sa fie construit un index CLUSTERED.

b) Cîmpul Localitate trebuie sa posede proprietatea DEFAULT= 'mun. Chisinau'.

c) Să se insereze toate datele din tabelul profesori în tabelul profesori_new. Să se scrie, cu acest scop, un număr potrivit de instrucțiuni T-SQL.

În coloana Localitate să fie inserata doar informatia despre denumirea localității din coloana-sursă Adresa_Postala_Profesor. În coloana Adresa_l, doar denumirea străzii. În coloana Adresa_2, să se păstreze numărul casei și (posibil) a apartamentului.

```sql
create table profesori_new

(Id_Profesor int NOT NULL,

 Nume_Profesor char(255),
 
 Prenume_Profesor char(255),
 
 Localitate char (255) default ('mun. Chisinau'),
 
 Adresa_1 char (255),
 
 Adresa_2 char (255),
 
 constraint [PK_profesori_new] primary key clustered
 
(	Id_Profesor )) ON [PRIMARY]

insert into profesori_new (Id_Profesor,Nume_Profesor, Prenume_Profesor, Localitate,Adresa_1, Adresa_2)

(select Id_Profesor, Nume_Profesor, Prenume_Profesor, Adresa_Postala_Profesor, Adresa_Postala_Profesor, Adresa_Postala_Profesor
from profesori)

update profesori_new

set Localitate = case when charindex(', s.',Localitate) >0

then case when charindex (', str.',Localitate) > 0

then substring (Localitate,1, charindex (', str.',Localitate)-1)

when charindex (', bd.',Localitate) > 0

then substring (Localitate,1, charindex (', bd.',Localitate)-1)

				      end
              
 when  charindex(', or.',Localitate) >0
 
then case when charindex (', str.',Localitate) > 0

then substring (Localitate,1, charindex('str.',Localitate)-3)

when charindex (', bd.',Localitate) > 0

then substring (Localitate,1, charindex ('bd.',Localitate)-3)

					  end
            
when charindex('nau',Localitate) >0

then substring(Localitate, 1, charindex('nau',Localitate)+2)

				end
        
update profesori_new

set Adresa_1 = case when charindex('str.', Adresa_1)>0

then substring(Adresa_1,charindex('str',Adresa_1), patindex('%, [0-9]%',Adresa_1)- charindex('str.',Adresa_1))

when charindex('bd.',Adresa_1)>0

then substring (Adresa_1,charindex('bd',Adresa_1), patindex('%, [0-9]%',Adresa_1) -  charindex('bd.',Adresa_1))

			   end

update profesori_new

set Adresa_2 = case when patindex('%, [0-9]%',Adresa_2)>0

then substring(Adresa_2, patindex('%, [0-9]%',Adresa_2)+1,len(Adresa_2) - patindex('%, [0-9]%',Adresa_2)+1)

				end
				
select * from profesori_new

```

<img width="717" alt="ex6_5" src="https://user-images.githubusercontent.com/43128526/48806673-c6d0a180-ed23-11e8-82f4-83dac8b0267e.png">
<img width="716" alt="ex6_5_2" src="https://user-images.githubusercontent.com/43128526/48806678-ccc68280-ed23-11e8-9b70-6bf33136fc6c.png">
<img width="525" alt="ex6_5_1" src="https://user-images.githubusercontent.com/43128526/48806674-c9cb9200-ed23-11e8-820f-536fb5665e67.png">


Exercise 6

Să se insereze datele in tabelul orarul pentru Grupa= 'CIBJ 71' (Id_ Grupa= 1) pentru ziua de luni. Toate lectiile vor avea loc în blocul de studii 'B'. Mai jos, sunt prezentate detaliile de inserare:
(ld_Disciplina = 107, Id_Profesor= 101, Ora ='08:00', Auditoriu = 202);
(Id_Disciplina = 108, Id_Profesor= 101, Ora ='11:30', Auditoriu = 501);
(ld_Disciplina = 119, Id_Profesor= 117, Ora ='13:00', Auditoriu = 501);

CREATE TABLE  orarul ( Id_Disciplina int,

Id_Profesor int,

Id_Grupa smallint default(1),

Zi char(2),

Ora Time,

Auditoriu int,

Bloc char(1) default(' B') 

PRIMARY KEY (Id_Grupa, Zi, Ora, Auditoriu))

INSERT orarul (Id_Disciplina , Id_Profesor, Zi, Ora, Auditoriu)

       VALUES ( 107, 101, 'Lu','08:00', 202 )
       
INSERT orarul (Id_Disciplina , Id_Profesor, Zi, Ora, Auditoriu)

       VALUES ( 108, 101, 'Lu','11:30', 501 )
       
INSERT orarul (Id_Disciplina , Id_Profesor, Zi, Ora, Auditoriu)

       VALUES ( 109, 117, 'Lu','13:00', 501 )
       
       SELECT * FROM orarul
       
  
      
     
    
<img width="448" alt="ex6_6" src="https://user-images.githubusercontent.com/43128526/48807648-e0bfb380-ed26-11e8-851a-3a97fcb9732f.png">

Exercise 7

Sa se scrie expresiile T-SQL necesare pentru a popula tabelul orarul pentru grupa INF171 , ziua de luni. Datele necesare pentru inserare
trebuie sa fie colectate cu ajutorul instructiunii/instructiunilor SELECT si introduse in tabelul-destinatie, stiind ca: 

lectie #1 (Ora ='08:00', Disciplina = 'Structuri de date si algoritmi', Profesor ='Bivol Ion')

lectie #2 (Ora ='11 :30', Disciplina = 'Programe aplicative', Profesor ='Mircea Sorin')

lectie #3 (Ora ='13:00', Disciplina ='Baze de date', Profesor = 'Micu Elena')


insert into orarul (Id_Disciplina, Id_Profesor, Id_Grupa, Zi, Ora)

values (

(select Id_Disciplina from discipline where Disciplina = 'Structuri de date si algoritmi'),

(select Id_Profesor from profesori where Nume_Profesor = 'Bivol' and Prenume_Profesor = 'Ion'),

(select Id_Grupa from grupe where Cod_Grupa = 'INF171'), 'Luni', '08:00'),
(
(select Id_Disciplina from discipline where Disciplina = 'Programe aplicative'),

(select Id_Profesor from profesori where Nume_Profesor = 'Mircea' and Prenume_Profesor = 'Sorin'),

(select Id_Grupa from grupe where Cod_Grupa = 'INF171'), 'Luni', '11:30'),
(
(select Id_Disciplina from discipline where Disciplina = 'Baze de date'),

(select Id_Profesor from profesori where Nume_Profesor = 'Micu' and Prenume_Profesor = 'Elena'),

(select Id_Grupa from grupe where Cod_Grupa = 'INF171'), 'Luni', '13:00')

		select * from orarul
<img width="623" alt="ex7" src="https://user-images.githubusercontent.com/43128526/48810516-7ca2ec80-ed32-11e8-9548-bfe9a3becbef.png">

Exercise 8

Sa se scrie interogarile de creare a indecsilor asupra tabelelor din baza de date universitatea pentru a asigura o performanta sporita la executarea interogarilor SELECT din Lucrarea practica 4.

<img width="883" alt="exercise6_1" src="https://user-images.githubusercontent.com/43128526/49349952-b3272280-f6b5-11e8-90d3-b496f73ab703.png">
<img width="951" alt="exr6_2" src="https://user-images.githubusercontent.com/43128526/49349957-b6221300-f6b5-11e8-80ab-380599ec77a4.png">
<img width="939" alt="exrc6_3" src="https://user-images.githubusercontent.com/43128526/49349959-b7ebd680-f6b5-11e8-8520-145c548b17f1.png">

