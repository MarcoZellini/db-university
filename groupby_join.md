
# Group By and Join Queries

## Group by:

### Contare quanti iscritti ci sono stati ogni anno

```sql
SELECT 
YEAR(`enrolment_date`), 
COUNT(*) AS `total_students`
FROM `students`
GROUP BY YEAR(`enrolment_date`);
```

- Risultati: 4

### Contare gli insegnanti che hanno l'ufficio nello stesso edificio

```sql
SELECT 
`office_address`, 
COUNT(*) AS `total_teachers`
FROM `teachers`
GROUP BY `office_address`;
```

- Risultati: 29

### Calcolare la media dei voti di ogni appello d'esame

```sql
SELECT 
`exam_id`, 
AVG(`vote`) AS `average_vote`
FROM `exam_student`
GROUP BY `exam_id`;
```

- Risultati: 4078

### Contare quanti corsi di laurea ci sono per ogni dipartimento

```sql
SELECT 
`department_id`, 
COUNT(*) AS `total_courses`
FROM `degrees`
GROUP BY `department_id`;
```

- Risultati: 12



## Joins:

### Selezionare tutti gli studenti iscritti al Corso di Laurea in Economia

```sql
SELECT 
`students`.`id` AS `student_id`, `students`.`name` AS `student_name`, `students`.`surname` AS `student_surname`, `students`.`fiscal_code` AS `student_fiscal_code`, `students`.`email` AS `student_email`, `degrees`.`name` AS `degree_name`
FROM `students`
JOIN `degrees`
ON `degrees`.`id` = `students`.`degree_id`
WHERE `degrees`.`name` = 'Corso di Laurea in Economia';
```
- Risultati: 68

### Selezionare tutti i Corsi di Laurea Magistrale del Dipartimento di Neuroscienze

```sql
SELECT 
`degrees`.`id` AS `degree_id`, `degrees`.`name` AS `degree_name`, `departments`.`name` AS `department_name`
FROM `degrees`
JOIN `departments`
ON `departments`.`id` = `degrees`.`department_id`
WHERE `degrees`.`level` = 'magistrale'
AND `departments`.`name` = 'Dipartimento di Neuroscienze';
```
- Risultati: 1

### Selezionare tutti i corsi in cui insegna Fulvio Amato (id=44)

```sql
SELECT 
`courses`.`id` AS `course_id`, `courses`.`name` AS `course_name`, `teachers`.`name` AS `teacher_name`
FROM `courses`
JOIN `course_teacher`
ON `courses`.`id` = `course_teacher`.`course_id`  
JOIN `teachers`
ON `teachers`.`id` = `course_teacher`.`teacher_id`
WHERE `teachers`.`id` = 44;
```

- Risultati: 1

### Selezionare tutti gli studenti con i dati relativi al corso di laurea a cui sono iscritti e il relativo dipartimento, in ordine alfabetico per cognome e nome

```sql
SELECT 
`students`.`id` AS `student_id`,
`students`.`name` AS `student_name`, 
`students`.`surname` AS `student_surname`, 
`students`.`fiscal_code` AS `student_fiscal_code`,
`degrees`.`id` AS `degree_id`,
`degrees`.`name` AS `degree_name`,
`degrees`.`email` AS `degree_email`,
`departments`.`id` AS `department_id`,
`departments`.`name` AS `department_name` 
FROM `students`
JOIN `degrees` 
ON `students`.`degree_id` = `degrees`.`id`
JOIN `departments`
ON `degrees`.`department_id` = `departments`.`id`
ORDER BY `students`.`surname`, `students`.`name` ASC;
```

- Risultati: 5000

### Selezionare tutti i corsi di laurea con i relativi corsi e insegnanti

```sql
SELECT 
`degrees`.`id` AS `degree_id`,
`degrees`.`name` AS `degree_name`,
`courses`.`id` AS `course_id`,
`courses`.`name` AS `course_name`,
`teachers`.`id` AS `teacher_id`,
`teachers`.`name` AS `teacher_name`,
`teachers`.`surname` AS `teacher_surname`
FROM `degrees`
JOIN `courses`
ON `degrees`.`id` = `courses`.`degree_id`
JOIN `course_teacher`
ON `courses`.`id` = `course_teacher`.`course_id`
JOIN `teachers`
ON `teachers`.`id` = `course_teacher`.`teacher_id`;
```

- Risultati: 1317

### Selezionare tutti i docenti che insegnano nel Dipartimento di Matematica (54)

```sql
SELECT DISTINCT 
`teachers`.`id` AS `teacher_id`,
`teachers`.`name` AS `teacher_name`,
`teachers`.`surname` AS `teacher_surname`,
`departments`.`id` AS `department_id`,
`departments`.`name` AS `department_name`
FROM `teachers`
JOIN `course_teacher`
ON `teachers`.`id` = `course_teacher`.`teacher_id`
JOIN `courses`
ON `courses`.`id` = `course_teacher`.`course_id`
JOIN `degrees`
ON `degrees`.`id` = `courses`.`degree_id`
JOIN `departments`
ON `departments`.`id` = `degrees`.`department_id`
WHERE `departments`.`name` = 'Dipartimento di Matematica';
```

- Risultati: 54

### BONUS: Selezionare per ogni studente il numero di tentativi sostenuti per ogni esame, stampando anche il voto massimo. Successivamente, filtrare i tentativi con voto minimo 18.

```sql
SELECT 
`students`.`id` AS `student_id`,
`students`.`name` AS `student_name`,
`students`.`surname` AS `student_surname`,
`courses`.`id` AS `course_id`,
`courses`.`name` AS `course_name`,
COUNT(CASE WHEN `exam_student`.`exam_id` >= 18 THEN `exam_student`.`exam_id` END) AS `try_count`,
MAX(`exam_student`.`vote`) AS `max_vote`
FROM `students`
JOIN `exam_student`
ON `students`.`id` = `exam_student`.`student_id`
JOIN `exams`
ON `exams`.`id` = `exam_student`.`exam_id`
JOIN `courses`
ON `courses`.`id` = `exams`.`course_id`
GROUP BY `students`.`id`, 
`courses`.`id`;

```

- Risultati: 32346