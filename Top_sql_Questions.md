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

  </details>

After Running the above sql script in MYSQL, you will see a Weather table which will have 4 rows.

**Question** -->  Write a solution to find all dates' Id with higher temperatures compared to its previous dates (yesterday).
Example : Id = 4 will be one of the output because temperature  on id 4 is greater than temperature on Id = 3 , which is just one day before the date of Id =4. 

*Expected Output* :
![](paste-the-copied-image-url-here)
