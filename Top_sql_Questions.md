# Top_Sql_Interview_Questions

**Compiled By** : Prashant Tripathi <br />
**Linkedin**    : https://www.linkedin.com/in/prashanttripathi786/    <br/>
**Note**        : I have Just Compiled the Questions So that People can get the sql script and Practise on their Local machine. I never claim that these Questions have been created by me. 
                  

## Question - 1   [ Date Problem 1]

**Tables Needed** : `Datee` Table 

<details>
  <summary> Click to See the sql script needed to create Datee Table </summary>

``` sql
CREATE TABLE Datee (
    DateColumn DATE
);

INSERT INTO Datee (DateColumn)
VALUES 
    ('2025-01-01'),
    ('2025-01-02'),
    ('2025-01-03'),
    ('2025-01-04'),
    ('2025-01-05'),
    ('2025-01-06'),
    ('2025-01-07'),
    ('2025-01-08'),
    ('2025-01-09'),
    ('2025-01-10');

``` 
<p>
  </details>
a) Before Running the Script make sure You have selected a Database in which you will create `Datee` table. <br/>
b) After Running the above sql script in MYSQL, you will see a Datee table which will have 10 rows. <br/>


<p>

`Date` Table :
| DateColumn   |
|--------------|
| 2025-01-01   |
| 2025-01-02   |
| 2025-01-03   |
| 2025-01-04   |
| 2025-01-05   |
| 2025-01-06   |
| 2025-01-07   |
| 2025-01-08   |
| 2025-01-09   |
| 2025-01-10   |


### **Question** -->  Write a solution to select only year and month from the datecolumn. Also select the original date column.


*Expected Output* <br />

| DateColumn   | NewDate  |
|--------------|----------|
| 2025-01-01   | 2025-01 |
| 2025-01-02   | 2025-01 |
| 2025-01-03   | 2025-01 |
| 2025-01-04   | 2025-01 |
| 2025-01-05   | 2025-01 |
| 2025-01-06   | 2025-01 |
| 2025-01-07   | 2025-01 |
| 2025-01-08   | 2025-01 |
| 2025-01-09   | 2025-01 |
| 2025-01-10   | 2025-01 |


<details>
  
  <summary> Solution- Click to See the First Approach  (SQL Query ) </summary> <br/>
  
``` sql
select datecolumn,left(datecolumn,7) as Newdate
from Datee;

```
  </details>
</p>

<details>

<summary> Solution- Click to See the Second Approach  (SQL Query ) </summary> <br/>
   
``` sql
SELECT 
datecolumn,
concat(year(datecolumn),'-',format(datecolumn,'MM')) as newdate

FROM datetable;


```

 </details>
</p>

<br/>

we can also use month(datecolumn) but in this case month will come in single digit , but we want the month in double digit that is why we are using
format function.

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



## Question - 3 [Cinema Seat Availability ]

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


## Question 4 [Window Function Questions ]

Before starting we should KnoW few concepts that is Running Total vs Rolling Total.

The terms **running total** and **rolling total** are often used in analytics and reporting to describe cumulative or moving calculations.
Here's the difference between the two:

---

### **1. Running Total**
A **running total** is the cumulative sum of a series of values up to the current point. It keeps adding each new value to the sum of the previous values, starting from the beginning of the series.

#### Key Characteristics:
- Starts from the first value and continues adding up.
- Represents the total accumulation at each step.
- The focus is on the **entire history** up to the current point.

#### Example:
If you have sales data:

| Day   | Sales | Running Total |
|-------|-------|---------------|
| Day 1 | 100   | 100           |
| Day 2 | 200   | 300           |
| Day 3 | 150   | 450           |
| Day 4 | 250   | 700           |

The running total on Day 4 is the sum of all sales from Day 1 to Day 4.

---

### **2. Rolling Total**
A **rolling total** (also called a **moving total**) calculates the sum of values over a specific time window that moves with each new value. It focuses on the most recent data points, dropping older values as new ones are added.

#### Key Characteristics:
- Uses a **fixed time period or window** (e.g., last 7 days, last 3 months).
- The window "rolls" forward as new data is considered.
- Helps analyze trends within a recent period rather than over the entire history.

#### Example:
If you have a 3-day rolling total:

| Day   | Sales | Rolling Total (Last 3 Days) |
|-------|-------|-----------------------------|
| Day 1 | 100   | 100                         |
| Day 2 | 200   | 300                         |
| Day 3 | 150   | 450                         |
| Day 4 | 250   | 600 (200 + 150 + 250)       |
| Day 5 | 300   | 700 (150 + 250 + 300)       |

For Day 4, the rolling total is the sum of sales for the last 3 days: Day 2, Day 3, and Day 4.

---

### **Key Differences**
| Aspect               | Running Total                   | Rolling Total                      |
|-----------------------|---------------------------------|------------------------------------|
| **Calculation Basis** | Cumulative from the start.      | Sum of values in a fixed window.  |
| **Focus**            | Entire history of the data.     | Recent values in a rolling window. |
| **Use Case**         | Long-term growth analysis.       | Trend or performance over a recent period. |

---

### Use Cases
- **Running Total**: 
  - Tracking total revenue, cumulative expenses, or the total number of items sold over time.
- **Rolling Total**: 
  - Analyzing short-term trends, such as the last 7 days of sales, or the moving average for stock prices.

## Lets start the window function question practise.

<details>
  <summary> Click to See the sql script  needed to create <code> Sales </code> Table </summary>
  
``` sql

CREATE TABLE Sales (
    SalesID INT PRIMARY KEY,
    SalesPerson NVARCHAR(50),
    SaleDate DATE,
    Amount DECIMAL(10, 2)
);


INSERT INTO Sales (SalesID, SalesPerson, SaleDate, Amount)
VALUES 
(1, 'Charlie', '2024-08-05', 2200.00),
(2, 'Diana', '2024-08-05', 1750.00),
(3, 'Charlie', '2024-08-07', 3100.00),
(4, 'Eve', '2024-08-08', 2800.00),
(5, 'Diana', '2024-08-09', 4000.00),
(6, 'Charlie', '2024-08-10', 1800.00),
(7, 'Eve', '2024-08-10', 2900.00);





```
</details>


### Question 5 Find the Sum of sales amount for each salesperson without affecting the Level of detail of the table.
It means output should contain the original table along with new column 'TotalSales'

*Expected Output*
| SalesID | SalesPerson | SaleDate   | Amount   | TotalSales |
|---------|-------------|------------|----------|------------|
| 1       | Charlie     | 2024-08-05 | 2200.00  | 7100.00    |
| 3       | Charlie     | 2024-08-07 | 3100.00  | 7100.00    |
| 6       | Charlie     | 2024-08-10 | 1800.00  | 7100.00    |
| 5       | Diana       | 2024-08-09 | 4000.00  | 5750.00    |
| 2       | Diana       | 2024-08-05 | 1750.00  | 5750.00    |
| 4       | Eve         | 2024-08-08 | 2800.00  | 5700.00    |
| 7       | Eve         | 2024-08-10 | 2900.00  | 5700.00    |



<details>
  <summary> Solution- Click to See the solution (SQL Query ) </summary> <br/>
  
``` sql


SELECT
* ,
sum(amount) over ( partition by salesperson) as TotalSales

FROM sales



```
  </details>

Note : Since we are not using order by that is why we are not getting cumulative/Rolling/Running Sales. It is summing all the amount values in each window.

### Question 6 Write a query to calculate the Running sales amount for each salesperson over time.
It means we need cumulative sum over the time.

Note - Use the Above script to create the sales table.

*Expected Output*
| SalesID | SalesPerson | SaleDate   | Amount   | TotalSales |
|---------|-------------|------------|----------|------------|
| 1       | Charlie     | 2024-08-05 | 2200.00  | 2200.00    |
| 3       | Charlie     | 2024-08-07 | 3100.00  | 5300.00    |
| 6       | Charlie     | 2024-08-10 | 1800.00  | 7100.00    |
| 2       | Diana       | 2024-08-05 | 1750.00  | 1750.00    |
| 5       | Diana       | 2024-08-09 | 4000.00  | 5750.00    |
| 4       | Eve         | 2024-08-08 | 2800.00  | 2800.00    |
| 7       | Eve         | 2024-08-10 | 2900.00  | 5700.00    |



<details>
  <summary> Solution- Click to See the solution (SQL Query ) </summary> <br/>
  
``` sql

SELECT
*,
sum(amount) over ( partition by salesperson order by saledate) as TotalSales

FROM sales


```
  </details>

### Question 7 Write a query to calculate the Rolling sales amount over time without including the Sales person over time.
Find rolling sum based only on datecolumn. We need to see sales performance over time.
Note: This Question is tricky because saledate column has repating date due to which we will not get correct answer.
Knowledge of FRAME CLAUSE is important to solve this question.

*Expected output*
| SalesID | SaleDate   | TotalSales |
|---------|------------|------------|
| 1       | 2024-08-05 | 2200.00    |
| 2       | 2024-08-05 | 3950.00    |
| 3       | 2024-08-07 | 7050.00    |
| 4       | 2024-08-08 | 9850.00    |
| 5       | 2024-08-09 | 13850.00   |
| 6       | 2024-08-10 | 15650.00   |
| 7       | 2024-08-10 | 18550.00   |

In this case, we will consider 4 different scenarios , because here we can see that date values is repeating.


<details>
  <summary> Solution- Click to See the solution (SQL Query ) </summary> <br/>
  
``` sql


SELECT
salesid,
saledate,
sum(amount) over (order by saledate ) as TotalSales ,
sum(amount) over (order by saledate Range between unbounded preceding and current row)  TotalSales2,
sum(amount) over (order by saledate Rows between unbounded preceding and current row)  TotalSales3,
sum(amount) over (order by salesid,saledate Rows between unbounded preceding and current row) totalsales4


FROM sales


```
  </details>

  *Our Code Ouput*

  | SalesID | SaleDate   | TotalSales | TotalSales2 | TotalSales3 | TotalSales4 |
|---------|------------|------------|-------------|-------------|-------------|
| 1       | 2024-08-05 | 3950.00    | 3950.00     | 2200.00     | 2200.00     |
| 2       | 2024-08-05 | 3950.00    | 3950.00     | 3950.00     | 3950.00     |
| 3       | 2024-08-07 | 7050.00    | 7050.00     | 7050.00     | 7050.00     |
| 4       | 2024-08-08 | 9850.00    | 9850.00     | 9850.00     | 9850.00     |
| 5       | 2024-08-09 | 13850.00   | 13850.00    | 13850.00    | 13850.00    |
| 6       | 2024-08-10 | 18550.00   | 18550.00    | 15650.00    | 15650.00    |
| 7       | 2024-08-10 | 18550.00   | 18550.00    | 18550.00    | 18550.00    |


The last two column gives us the correct answer.The reason we are getting different answer lies in the FRAME CLAUSE concept which comes 
after order by clause.
The first and second column gives us same result because both are same.
In first frame clause is hidden.
In second the frame clause is written which is nothing but the defualt frame clause which was hidden
In third we have replaced the range with rows to get the right answer. why? Because Range works on the actual values inside the column but rows work on the index number of the row.
so when date values is repeating, range mode groups the same values which comes in the saleddate column.
There is one more solution to that, which is the last column, we can use unique combinations in the order by clause.
since we are using both saledate and sales id  which is  a unique values in this case it is not grouping the values.




