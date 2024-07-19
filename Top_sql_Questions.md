# Top_Sql_Interview_Questions

**Compiled By** : Prashant Tripathi <br />
**Linkedin**    : https://www.linkedin.com/in/prashanttripathi786/    <br/>
**Note**        : I have Just Compiled the Questions So that People can get the sql script and Practise on their Local machine. I never claim that these Questions have been created by me. 
                  

## Question - 1   [ Temperature Comparison ]

**Tables Needed** : `Weather` Table 

<details>
  <summary> Click to See the sql script  needed to create Weather Table </summary>

``` sql
CREATE TABLE weather (
    id INT PRIMARY KEY,
    recordDate DATE,
    temperature INT
);

-- Insert data into the table
INSERT INTO weather (id, recordDate, temperature) VALUES
(1, '2015-01-01', 10),
(2, '2015-01-02', 25),
(3, '2015-01-03', 20),
(4, '2015-01-04', 30);

``` 
<p>
  </details>
a) Before Running the Script make sure You have selected a Database in which you will create `Weather` table. <br/>
b) After Running the above sql script in MYSQL, you will see a Weather table which will have 4 rows. <br/>


<p>

`Weather` Table :

| ID | recorddate | temperature |
|----|------------|-------|
| 1  | 2015-01-01 | 10    |
| 2  | 2015-01-02 | 25    |
| 3  | 2015-01-03 | 20    |
| 4  | 2015-01-04 | 30    |

**Question** -->  Write a solution to find all dates' Id with higher temperatures compared to its previous dates (yesterday).
Example : Id = 4 will be one of the output because temperature  on id 4 is greater than temperature on Id = 3 , which is just one day before the date of Id =4. 

*Expected Output* <br />


| id      |
|---------|
|    2    |
|    4    |



<details>
  
  <summary> Solution- Click to See the First Approach  (SQL Query ) </summary> <br/>
  
``` sql
select 
b.id as id 
from weather a , weather b
where datediff( b.recorddate,a.recorddate)=1 and b.temperature > a.temperature 
order by a.recorddate
```
  </details>
</p>

<details>

<summary> Solution- Click to See the Second Approach  (SQL Query ) </summary> <br/>
   
``` sql
with cte as 
(   select a.id as aid,
           b.id as bid,
           datediff(b.recorddate,a.recorddate) as ddiff
    from weather a , weather b   -- This is Cross Join 
    where b.temperature > a.temperature 
    order by a.recorddate )

select bid as id
from 
cte where ddiff=1 ;

```

 </details>
</p>

<br/>
I am using order by because we dont know how the order of the rows will be after doing the cross Join.<br/>
So order by helps me writing my logic correctly
<br/>


## Question - 2  [ Employee Logged Hours ]

**Tables Needed** : `Clocked_hours`

<details>
  <summary> Click to See the sql script  needed to create Clocked_hours Table </summary>

```sql
create table clocked_hours(

empd_id int,

swipe time,

flag char

);

insert into clocked_hours values

(11114,'08:30','I'),

(11114,'10:30','O'),

(11114,'11:30','I'),

(11114,'15:30','O'),

(11115,'09:30','I'),

(11115,'17:30','O');
``` 
<p>
  </details>
  
a) Before Running the Script make sure You have selected a Database in which you will create `Clocked_hours` table. <br/>
b) After Running the above sql script in MYSQL, you will see a `Clocked_hours`  table which will have 6 rows.
<br/>


 `Clocked_hours` Table :
 
|empd_id| swipe      | flag |
|-------|------------|------|
| 11114 | 08:30:00   | I    |
| 11114 | 10:30:00   | O    |
| 11114 | 11:30:00   | I    |
| 11114 | 15:30:00   | O    |
| 11115 | 09:30:00   | I    |
| 11115 | 17:30:00   | O    |

Question--> What is the Total logged hours by each employee?

*Expected Output* 
| EMD_ID | Total_logged_hours |
|--------|---------------------|
| 11114  | 6                   |
| 11115  | 8                   |




<details>
  
  <summary> Solution- Click to See the First Approach  (SQL Query ) </summary> <br/>
  
``` sql
select empd_id ,sum( hour(timediff(t2,swipe) )) as total_logged_hours
from 
(
select * ,LEAD(swipe) over (order by  empd_id , swipe) as t2
from clocked_hours
) as a
where flag='I'
group by empd_id

```
  </details>
</p>
<details>
  
  <summary> Solution- Click to See the Second Approach  (SQL Query ) </summary> <br/>
  
``` sql
WITH CTE1 AS (select * from clocked_hours
where flag='I'
Union
select * from clocked_hours
where flag='O'
)
,

 cte2 as (
SELECT * , row_number() over (Partition by flag order by empd_id , swipe ) as rnk
from cte1 )
,
cte3 as (
select  empd_id , min(swipe) ,max(swipe) ,hour(timediff(max(swipe),min(swipe) )) as hrs
from cte2
group by empd_id,rnk )

select empd_id ,sum(hrs) as total_logged_hours
from cte3
group by empd_id
```
  </details>
</p>



## Question - 2  [Cinema Seat Availability ]

**Tables Needed** : `Cinema`


<details>
  <summary> Click to See the sql script  needed to create <code> Cinema</code> Table </summary>
  
``` sql
CREATE TABLE cinema (
    seat_id INT PRIMARY KEY,
    free int
);
delete from cinema;
INSERT INTO cinema (seat_id, free) VALUES (1, 1);
INSERT INTO cinema (seat_id, free) VALUES (2, 0);
INSERT INTO cinema (seat_id, free) VALUES (3, 1);
INSERT INTO cinema (seat_id, free) VALUES (4, 1);
INSERT INTO cinema (seat_id, free) VALUES (5, 1);
INSERT INTO cinema (seat_id, free) VALUES (6, 0);
INSERT INTO cinema (seat_id, free) VALUES (7, 1);
INSERT INTO cinema (seat_id, free) VALUES (8, 1);
INSERT INTO cinema (seat_id, free) VALUES (9, 0);
INSERT INTO cinema (seat_id, free) VALUES (10, 1);
INSERT INTO cinema (seat_id, free) VALUES (11, 0);
INSERT INTO cinema (seat_id, free) VALUES (12, 1);
INSERT INTO cinema (seat_id, free) VALUES (13, 0);
INSERT INTO cinema (seat_id, free) VALUES (14, 1);
INSERT INTO cinema (seat_id, free) VALUES (15, 1);
INSERT INTO cinema (seat_id, free) VALUES (16, 1);


```
</details>

a) After Running the above sql script in MYSQL, you will see a `Cinema`  table which will have 16 rows.

 `Cinema` Table :

| seat_id | free |
|---------|------|
| 1       | 1    |
| 2       | 0    |
| 3       | 1    |
| 4       | 1    |
| 5       | 1    |
| 6       | 0    |
| 7       | 1    |
| 8       | 1    |
| 9       | 0    |
| 10      | 1    |
| 11      | 0    |
| 12      | 1    |
| 13      | 0    |
| 14      | 1    |
| 15      | 1    |
| 16      | 1    |

Question--> Find the Seat id  of Consecutive seats  which are Free. Free is boolean ( 1=Free , 0= Occupied) </br>
Note - Atleast two Consecutive seats should be there.


*Expected Output* 
| seat_id |
|---------|
| 3       |
| 4       |
| 5       |
| 7       |
| 8       |
| 14      |
| 15      |
| 16      |


<details>
  
  <summary> Solution- Click to See the First Approach  (SQL Query ) </summary> <br/>
  
``` sql

with cte1 as 

(
select * ,
lead(free) over( order by seat_id) as next_Seat,
lag(free) over( order by seat_id) as previous_seat

from cinema)
select seat_id from cte1

where (free=1 and next_Seat=1) or  (free=1 and previous_seat=1)




```
  </details>

<details>
  <summary> Solution- Click to See the Second Approach  (SQL Query ) </summary> <br/>
  
``` sql


with cte1 as
(
select 
* ,
lead(free) over (order by seat_id) next_seat,
lead(seat_id) over (order by seat_id) next_seat_id
from cinema)

select seat_id as seat_id from cte1 
where free=1 and next_seat=1
union
select next_Seat_id as seat_id from cte1 
where free=1 and next_seat=1



```
  </details>


