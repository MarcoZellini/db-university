
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