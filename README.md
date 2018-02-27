# Social Database

Students at your hometown high school have decided to organize their social
network using databases. So far, they have collected information about sixteen
students in four grades, 9-12.
Here's the schema:
* **Highschooler ( ID, name, grade )**: there is a high school student with
  unique ID and a given first name in a certain grade.
* **Friend ( ID1, ID2 )**: the student with ID1 is friends with the student with
  ID2. Friendship is mutual, so if `(123, 456)` is in the Friend table, so is
  `(456, 123)`.
* **Likes ( ID1, ID2 )**: the student with ID1 likes the student with ID2. Liking
  someone is not necessarily mutual, so if `(123, 456)` is in the Likes table,
  there is no guarantee that `(456, 123)` is also present.

Answer the following questions by writing SQL queries that computes the desired
result. Your queries shouldn't depend on the content of the database. You can
test your solutions by tracing them on a small sample database (see the following
setup instructions). You need to submit your queries in the correct syntax.

The following graph shows the various connections between the students in our
database. 9th graders are blue, 10th graders are green, 11th graders are yellow,
and 12th graders are purple. Undirected black edges indicate friendships, and
directed red edges indicate that one student likes another student.

![social graph](images/social.png)

## Write queries
1. Find the names of all students who are friends with someone named Gabriel.
1. Find all students who do not appear in the Likes table (as a student who
  likes or is liked) and return their names and grades.
1. (*) Find the name and grade of all students who are liked by more than one
  other student.
1. (*) For every student who likes someone 2 or more
  grades younger than themselves, return that student's name and grade, and the
  name and grade of the student they like.
1. (**) Find names and grades of students who only have friends in the same
  grade. Return the result sorted by grade, then by name within each grade.
  (what about students with no friends?)
1. (***) For each student A who likes a student B where the two are not friends,
  find if they have a friend C in common (who can introduce them!). For all
  such trios, return the name and grade of A, B, and C.
1. (**) Find the difference between the number of students in the school and
  the number of different first names.
1. (**) What is the average number of friends per student? (Your result should
  be just one number.)
1. (***) Find the number of students who are either friends with Cassandra or
  are friends of friends of Cassandra. Do not count Cassandra, even though
  technically she is a friend of a friend.
1. (***) Find the name and grade of the student(s) with the greatest number of
  friends.

## Setup MySQL on Cloud 9
Install MySQL database engine on command line if you haven't done so:
```
mysql-ctl install
```
The output will be:
```
MySQL 5.5 database added.  Please make note of these credentials:

Root User: username
Database Name: c9
```
No password is set on MySQL, which is rarely an issue in a development
environment. You can setup user authentication if you want.

We will use the MySQL client on the command line to interact with MySQL database
engine:
```
mysql-ctl cli
```

## Import testing database
Type the commands after the MySQL command prompt (`mysql>`):

Create an empty database:
```
mysql> create database social;
Query OK, 1 row affected (0.00 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| c9                 |
| mysql              |
| performance_schema |
| phpmyadmin         |
| social             |
+--------------------+
6 rows in set (0.00 sec)
```
Populate the database with data from `social.sql`:
```
mysql> use social
Database changed
mysql> source social.sql
```
Note: the last command assumes that 'social.sql' is in the directory where you
run `mysql-ctl cli`. You can always exit by typing `exit` and restart the MySQL
client in another directory.

## View database tables
```
mysql> select * from Highschooler;
+------+-----------+-------+
| ID   | name      | grade |
+------+-----------+-------+
| 1510 | Jordan    |     9 |
| 1689 | Gabriel   |     9 |
| 1381 | Tiffany   |     9 |
| 1709 | Cassandra |     9 |
| 1101 | Haley     |    10 |
| 1782 | Andrew    |    10 |
| 1468 | Kris      |    10 |
| 1641 | Brittany  |    10 |
| 1247 | Alexis    |    11 |
| 1316 | Austin    |    11 |
| 1911 | Gabriel   |    11 |
| 1501 | Jessica   |    11 |
| 1304 | Jordan    |    12 |
| 1025 | John      |    12 |
| 1934 | Kyle      |    12 |
| 1661 | Logan     |    12 |
+------+-----------+-------+
16 rows in set (0.00 sec)

mysql> select * from Likes;
+------+------+
| ID1  | ID2  |
+------+------+
| 1689 | 1709 |
| 1709 | 1689 |
| 1782 | 1709 |
| 1911 | 1247 |
| 1247 | 1468 |
| 1641 | 1468 |
| 1316 | 1304 |
| 1501 | 1934 |
| 1934 | 1501 |
| 1025 | 1101 |
+------+------+
10 rows in set (0.00 sec)

mysql> select * from Friend;
+------+------+
| ID1  | ID2  |
+------+------+
| 1510 | 1381 |
| 1510 | 1689 |
| 1689 | 1709 |
| 1381 | 1247 |
| 1709 | 1247 |
| 1689 | 1782 |
| 1782 | 1468 |
| 1782 | 1316 |
| 1782 | 1304 |
| 1468 | 1101 |
| 1468 | 1641 |
| 1101 | 1641 |
| 1247 | 1911 |
| 1247 | 1501 |
| 1911 | 1501 |
| 1501 | 1934 |
| 1316 | 1934 |
| 1934 | 1304 |
| 1304 | 1661 |
| 1661 | 1025 |
| 1381 | 1510 |
| 1689 | 1510 |
| 1709 | 1689 |
| 1247 | 1381 |
| 1247 | 1709 |
| 1782 | 1689 |
| 1468 | 1782 |
| 1316 | 1782 |
| 1304 | 1782 |
| 1101 | 1468 |
| 1641 | 1468 |
| 1641 | 1101 |
| 1911 | 1247 |
| 1501 | 1247 |
| 1501 | 1911 |
| 1934 | 1501 |
| 1934 | 1316 |
| 1304 | 1934 |
| 1661 | 1304 |
| 1025 | 1661 |
+------+------+
40 rows in set (0.00 sec)
```

answers:

1. select Distinct name 
from Highschooler,(select ID AS ID3 from Highschooler where name = 'Gabriel') Card, Friend
where (ID = ID1 or ID = ID2) && (ID1 = ID3 or ID2 = ID3) && (ID != ID3);

2. select name,grade 
from Highschooler
where ID not in (select ID1 from Likes) and ID not in (select ID2 from Likes); 

3. select name, grade 
from Highschooler 
where (select count(*) from Likes where ID = ID2)>1;

4. select H1.name, H1.grade, H2.name, H2.grade
from Highschooler H1, Highschooler H2, Likes 
where H1.grade-H2.grade = 2 && H1.ID = ID1 && H2.ID = ID2;

8. select AVG(number.T) 
from (select count(*) AS T, from Highschooler, Friend where ID1 = ID group by ID1) number;