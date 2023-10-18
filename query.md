
# Query List

## Query 1

- SELECT * FROM `students` WHERE YEAR(`date_of_birth`) = '1990';
- Risultati: 160


## Query 2

- SELECT * FROM `courses` WHERE `cfu` > 10; 
- Risultati: 479

## Query 3

- SELECT * FROM `students` WHERE (YEAR(NOW()) - YEAR(`date_of_birth`)) > 30;                 (*`Sbagliata perche' non tiene conto dei giorni ma solo degli anni`*).
- SELECT * FROM students WHERE DATE_FORMAT(FROM_DAYS(DATEDIFF(NOW(),`date_of_birth`)), '%Y') >= 30
- SELECT * FROM students WHERE TIMESTAMPDIFF(YEAR, `date_of_birth`, CURDATE()) >= 30;
- Risultati: 3501

## Query 4

- SELECT * FROM `courses` WHERE `period` = 'I semestre' AND `year` = 1;
- Risultati: 286

## Query 5
- SELECT * FROM `exams` WHERE `hour` >= "14:00:00" AND `date` = '2020-06-20';
- Risultati: 21

## Query 6
- SELECT * FROM `degrees` WHERE `name` LIKE '%magistrale%';
- SELECT * FROM `degrees` WHERE `level` = 'magistrale';
- Risultati: 38

## Query 7
- SELECT COUNT(id) FROM `departments`;
- Risultati: 12

## Query 8 
- SELECT * FROM `teachers` WHERE `phone` IS NOT NULL;
- Risultati: 50


# Lezione JOIN

## Join 1:

- Selezionare tutti i corsi del Corso di Laurea in Informatica (22)

```sql

SELECT `courses`.`id` AS `course_id`, `courses`.`name` AS `course_name`, `courses`.`period` AS `course_period`, `courses`.`year` AS `course_year`, `courses`.`cfu` AS `course_cfu`, `courses`.`website` AS `course_website`
FROM `courses` 
JOIN `degrees` 
ON `courses`.`degree_id` = `degrees`.`id`
WHERE `degrees`.`name` = 'Corso di Laurea in Informatica';

```

## Join 2:

- Selezionare le informazioni sul corso con id = 144, con tutti i relativi appelli d’esame

```sql

SELECT `courses`.`id` AS `course_id`, `courses`.`name` AS `course_name`, `courses`.`description` AS `course_description`, `courses`.`period` AS `course_period`, `courses`.`year` AS `course_year`, `courses`.`cfu` AS `course_cfu`, `courses`.`website` AS `course_website`, `exams`.`id` AS `exam_id`, `exams`.`course_id` AS `exam_course_id`, `exams`.`date` AS `exam_date`, `exams`.`hour` AS `exam_hour`, `exams`.`location` AS `exam_location`, `exams`.`address` AS `exam_address`   
FROM `courses`
JOIN `exams`
ON `courses`.`id` = `exams`.`course_id` 
WHERE `course_id` = 144;

```

## Join 3:
- Selezionare a quale dipartimento appartiene il Corso di Laurea in Diritto dell'Economia (Dipartimento di Scienze politiche, giuridiche e studi internazionali)

```sql

SELECT `departments`.*
FROM `departments`
JOIN `degrees`
ON `departments`.`id` = `degrees`.`department_id`
WHERE `degrees`.`name` = 'Corso di Laurea in Diritto dell\'Economia';

```

## Join 4:
- Selezionare tutti gli appelli d'esame del Corso di Laurea Magistrale in Fisica del primo anno

```sql

SELECT `courses`.`name`, `courses`.`period`, `courses`.`cfu`, `exams`.`date`, `exams`.`location`, `exams`.`address`
    FROM `courses`
    JOIN `degrees`
    ON `courses`.`degree_id` = `degrees`.`id`
    JOIN `exams`
    ON `exams`.`course_id` = `courses`.`id`
    WHERE `degrees`.`name` = 'Corso di Laurea Magistrale in Fisica' 
    AND `courses`.`year` = 1;

```

## Join 5:

- Selezionare tutti i docenti che insegnano nel Corso di Laurea in Lettere (21)

```sql

SELECT DISTINCT `teachers`.*
FROM `teachers`     
JOIN `course_teacher` 
ON `teachers`.`id` = `course_teacher`.`teacher_id`
JOIN `courses` 
ON `courses`.`id` = `course_teacher`.`course_id`
JOIN `degrees` 
ON `courses`.`degree_id` = `degrees`.`id`
WHERE `degrees`.`name` = 'Corso di Laurea in Lettere'
ORDER BY `teachers`.`surname` ASC;

```

## Join 6:

- Selezionare il libretto universitario di Mirco Messina (matricola n. 620320)

```sql

SELECT `students`.`name`, `students`.`surname`, `students`.`registration_number`, `courses`.`id`, `courses`.`name`, `exams`.`date`, `exam_student`.`vote`
FROM `exam_student`
JOIN `exams` 
ON `exam_student`.`exam_id` =  `exams`.`id`
JOIN `students` 
ON `exam_student`.`student_id` = `students`.`id`
JOIN `courses` 
ON `exams`.`course_id` = `courses`.`id`
WHERE `students`.`name` = 'Mirco' 
AND `students`.`surname` = 'Messina'
AND `exam_student`.`vote` >= 18;

```