Exercise 1 

1) Aflati toate datele despre grupele de studii de la facultate 
<img width="388" alt="1" src="https://user-images.githubusercontent.com/43128526/47270788-7aa20e00-d579-11e8-99bb-274f6b44424d.png">

Select *
from grupe;

 Exercise 2 
 
17)Aflati nume si prenumele profesorilor, care au predat cel putin o disciplina studentului cu id 100 
<img width="444" alt="2" src="https://user-images.githubusercontent.com/43128526/47270791-7bd33b00-d579-11e8-9620-a8a10507af36.png">

Select Nume_Profesor, Prenume_Profesor
From profesori
inner join studenti_reusita on studenti_reusita.Id_Profesor=Profesori.Id_Profesor
Where id_student=100;

<img width="210" alt="3" src="https://user-images.githubusercontent.com/43128526/47270792-7d9cfe80-d579-11e8-9b7c-387220338388.png">

<img width="193" alt="4" src="https://user-images.githubusercontent.com/43128526/47270793-8261b280-d579-11e8-91c6-46ead0593d17.png">

Exercise 3

32)Furnizati numele,prenumelesi media notelor pe grupe pentru studenti 

<img width="291" alt="6" src="https://user-images.githubusercontent.com/43128526/47270978-13d22400-d57c-11e8-967c-4d6809e82077.png">

<img width="297" alt="7" src="https://user-images.githubusercontent.com/43128526/47270979-16cd1480-d57c-11e8-8e2a-28487cbc617a.png">

<img width="284" alt="8" src="https://user-images.githubusercontent.com/43128526/47270981-1af93200-d57c-11e8-8d46-ce18bf5aa382.png">

<img width="308" alt="9" src="https://user-images.githubusercontent.com/43128526/47270982-1df42280-d57c-11e8-830f-64f72dfda8d5.png">

Select Id_Grupa Nume_Student ,Prenume_Student,AVG(Nota)
From studenti
inner join studenti_reusita on studenti_reusita.Id_Student=studenti.Id_Student
group by  Id_Grupa,Nume_Student ,Prenume_Student;
