1. Create a list of students showing first and last names only.

mysql> SELECT first_name, last_name FROM student;
+------------+-----------+
| first_name | last_name |
+------------+-----------+
| Eric       | Ephram    |
| Greg       | Gould     |
| Adam       | Ant       |
| Howard     | Hess      |
| Charles    | Caldwell  |
| James      | Joyce     |
| Doug       | Dumas     |
| Kevin      | Kraft     |
| Frank      | Fountain  |
| Brian      | Biggs     |
+------------+-----------+
10 rows in set (0.00 sec)

2. Create a list of students with all the columns but only if the student has less than 8 years experience

mysql> SELECT * FROM student WHERE years_of_experience < 8;
+------------+------------+-----------+---------------------+------------+
| student_id | first_name | last_name | years_of_experience | start_date |
+------------+------------+-----------+---------------------+------------+
|          1 | Eric       | Ephram    |                   2 | 2016-03-31 |
|          2 | Greg       | Gould     |                   6 | 2016-09-30 |
|          3 | Adam       | Ant       |                   5 | 2016-06-02 |
|          4 | Howard     | Hess      |                   1 | 2016-02-28 |
|          5 | Charles    | Caldwell  |                   7 | 2016-05-07 |
|          8 | Kevin      | Kraft     |                   3 | 2016-04-15 |
|         10 | Brian      | Biggs     |                   4 | 2015-12-25 |
+------------+------------+-----------+---------------------+------------+
7 rows in set (0.00 sec)

3. Create a list of students showing the lastname, startdate, and id columns only and sorted by last_name in ascending sequence

mysql> SELECT
    -> last_name, start_date, student_id
    -> FROM student
    -> ORDER BY last_name ASC;
+-----------+------------+------------+
| last_name | start_date | student_id |
+-----------+------------+------------+
| Ant       | 2016-06-02 |          3 |
| Biggs     | 2015-12-25 |         10 |
| Caldwell  | 2016-05-07 |          5 |
| Dumas     | 2016-07-04 |          7 |
| Ephram    | 2016-03-31 |          1 |
| Fountain  | 2016-01-31 |          9 |
| Gould     | 2016-09-30 |          2 |
| Hess      | 2016-02-28 |          4 |
| Joyce     | 2016-08-27 |          6 |
| Kraft     | 2016-04-15 |          8 |
+-----------+------------+------------+
10 rows in set (0.01 sec)

4. Create a list of students showing all columns but only if the student first name is Adam, James, or Frank and sort them in last_name descending sequence.

mysql> SELECT
    -> *
    -> FROM student
    -> WHERE first_name IN ("Adam","James","Frank")
    -> ORDER BY last_name DESC;
+------------+------------+-----------+---------------------+------------+
| student_id | first_name | last_name | years_of_experience | start_date |
+------------+------------+-----------+---------------------+------------+
|          6 | James      | Joyce     |                   9 | 2016-08-27 |
|          9 | Frank      | Fountain  |                   8 | 2016-01-31 |
|          3 | Adam       | Ant       |                   5 | 2016-06-02 |
+------------+------------+-----------+---------------------+------------+
3 rows in set (0.01 sec)

5. Create a list of students showing firstname, lastname and startdate where the startdate is between Jan 1, 2016 and June 30, 2016 and order the list by descending start_date sequence.

mysql> SELECT  start_date, first_name, last_name FROM student WHERE start_date BETWEEN "2016-01-01" AND "2016-06-06" ORDER BY start_date DESC;
+------------+------------+-----------+
| start_date | first_name | last_name |
+------------+------------+-----------+
| 2016-06-02 | Adam       | Ant       |
| 2016-05-07 | Charles    | Caldwell  |
| 2016-04-15 | Kevin      | Kraft     |
| 2016-03-31 | Eric       | Ephram    |
| 2016-02-28 | Howard     | Hess      |
| 2016-01-31 | Frank      | Fountain  |
+------------+------------+-----------+
6 rows in set (0.00 sec)

Medium:

//Create grade table
CREATE TABLE grade (
  `grade_id` int NOT NULL AUTO_INCREMENT,
  `grade_description` VARCHAR(100) NOT NULL,
  PRIMARY KEY (`grade_id`)
);

mysql> explain grade;
+-------------------+--------------+------+-----+---------+----------------+
| Field             | Type         | Null | Key | Default | Extra          |
+-------------------+--------------+------+-----+---------+----------------+
| grade_id          | int(11)      | NO   | PRI | NULL    | auto_increment |
| grade_description | varchar(100) | NO   |     | NULL    |                |
+-------------------+--------------+------+-----+---------+----------------+
2 rows in set (0.00 sec)

mysql> select * FROM grade;
+----------+-----------------------------+
| grade_id | grade_description           |
+----------+-----------------------------+
|        1 | Incomplete                  |
|        2 | Complete and unsatisfactory |
|        3 | Complete and satisfactory   |
|        5 | Not graded                  |
|        7 | Exceeds expectations        |
+----------+-----------------------------+
5 rows in set (0.00 sec)

//Add foreign key
mysql> ALTER TABLE assignment ADD CONSTRAINT fk_grade_id FOREIGN KEY (grade_id) REFERENCES grade(grade_id);
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql> explain assignment;
+----------------+------------------+------+-----+---------+----------------+
| Field          | Type             | Null | Key | Default | Extra          |
+----------------+------------------+------+-----+---------+----------------+
| assignment_id  | int(11) unsigned | NO   | PRI | NULL    | auto_increment |
| student_id     | int(11)          | NO   |     | NULL    |                |
| assignment_nbr | int(11)          | NO   |     | NULL    |                |
| class_id       | int(11)          | YES  |     | NULL    |                |
| grade_id       | int(11)          | YES  | MUL | NULL    |                |
+----------------+------------------+------+-----+---------+----------------+
5 rows in set (0.00 sec)

//Add 5 records to assignment table
INSERT INTO assignment (assignment_id,student_id,assignment_nbr,class_id,grade_id) VALUES (1,4,6,8,1);

INSERT INTO assignment (assignment_id,student_id,assignment_nbr,class_id,grade_id) VALUES (2,3,5,3,2);

INSERT INTO assignment (assignment_id,student_id,assignment_nbr,class_id,grade_id) VALUES (3,7,2,1,3);

INSERT INTO assignment (assignment_id,student_id,assignment_nbr,class_id,grade_id) VALUES (5,9,3,2,5);

INSERT INTO assignment (assignment_id,student_id,assignment_nbr,class_id,grade_id) VALUES (6,3,1,4,7);

mysql> select * from assignment;                                                   +---------------+------------+----------------+----------+----------+
| assignment_id | student_id | assignment_nbr | class_id | grade_id |
+---------------+------------+----------------+----------+----------+
|             1 |          4 |              6 |        8 |        1 |
|             2 |          3 |              5 |        3 |        2 |
|             3 |          7 |              2 |        1 |        3 |
|             5 |          9 |              3 |        2 |        5 |
|             6 |          3 |              1 |        4 |        7 |
+---------------+------------+----------------+----------+----------+
5 rows in set (0.00 sec)

//Hard mode

//Create index on student_id of assignment table
mysql> CREATE INDEX idx_studentid ON assignment (student_id);
Query OK, 0 rows affected (0.02 sec)
Records: 0  Duplicates: 0  Warnings: 0

mysql>

//Add foriegn key to student_id
//Add foreign key
mysql> ALTER TABLE assignment ADD CONSTRAINT fk_student_id FOREIGN KEY (student_id) REFERENCES student(student_id);
Query OK, 5 rows affected (0.03 sec)
Records: 5  Duplicates: 0  Warnings: 0

//Change student_id to int(11) unsigned NOT NULL
mysql> ALTER TABLE assignment
    -> MODIFY `student_id` int(11) unsigned NOT NULL;
Query OK, 5 rows affected (0.02 sec)
Records: 5  Duplicates: 0  Warnings: 0

//Test relationship constraint
mysql> INSERT INTO assignment (assignment_id,student_id,assignment_nbr,class_id,grade_id) VALUES (23,100,1,4,7);
ERROR 1452 (23000): Cannot add or update a child row: a foreign key constraint fails (`tiy`.`assignment`, CONSTRAINT `fk_student_id` FOREIGN KEY (`student_id`) REFERENCES `student` (`student_id`))
mysql>
