Exercise 1 

1) Aflati toate datele despre grupele de studii de la facultate 
<img width="388" alt="1" src="https://user-images.githubusercontent.com/43128526/47270788-7aa20e00-d579-11e8-99bb-274f6b44424d.png">

Select *
from grupe;

 Exercise 2 
 
17)Aflati nume si prenumele profesorilor, care au predat cel putin o disciplina studentului cu id 100 
<img width="444" alt="2" src="https://user-images.githubusercontent.com/43128526/47270791-7bd33b00-d579-11e8-9620-a8a10507af36.png">

<img width="210" alt="3" src="https://user-images.githubusercontent.com/43128526/47270792-7d9cfe80-d579-11e8-9b7c-387220338388.png">

<img width="193" alt="4" src="https://user-images.githubusercontent.com/43128526/47270793-8261b280-d579-11e8-91c6-46ead0593d17.png">

Select Nume_Profesor, Prenume_Profesor
From profesori
inner join studenti_reusita on studenti_reusita.Id_Profesor=Profesori.Id_Profesor
Where id_student=100;

Exercise 3

32)Furnizati numele,prenumelesi media notelor pe grupe pentru studenti 

<img width="695" alt="lab4" src="https://user-images.githubusercontent.com/43128526/49340863-10878900-f64e-11e8-9057-a99612219148.png">

select studenti.studenti.Nume_Student,studenti.studenti.Prenume_Student,grupe.Cod_Grupa,avg(cast(Nota as float))as Media
from studenti.studenti_reusita 
inner join studenti.studenti  on  studenti.studenti_reusita.Id_Student = studenti.studenti.Id_Student
inner join grupe on  studenti.studenti_reusita.Id_Grupa = grupe.Id_Grupa
group by  studenti.studenti.Nume_Student, studenti.studenti.Prenume_Student,grupe.Cod_Grupa
order by grupe.Cod_Grupa;
