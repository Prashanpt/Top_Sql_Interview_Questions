# Top_Sql_Interview_Questions

**Compiled By** : Prashant Tripathi <br />
**Linkedin**    : https://www.linkedin.com/in/prashanttripathi786/ 

## Question - 1  

**Tables Needed** : Weather Table 

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
a) Before Running the Script make sure You have selected a Database in which you will create Weather table. <br/>
b) After Running the above sql script in MYSQL, you will see a Weather table which will have 4 rows.

</p>
Weather Table <br/>

![Expected Output](https://github.com/Prashanpt/Top_Sql_Interview_Questions/blob/main/Weather_Table.png)

**Question** -->  Write a solution to find all dates' Id with higher temperatures compared to its previous dates (yesterday).
Example : Id = 4 will be one of the output because temperature  on id 4 is greater than temperature on Id = 3 , which is just one day before the date of Id =4. 

*Expected Output* <br />

![Expected Output](https://github.com/Prashanpt/Top_Sql_Interview_Questions/blob/main/Screenshot%202024-02-07%20233733.png?raw=true)



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





