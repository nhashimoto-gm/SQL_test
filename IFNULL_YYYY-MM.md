[SQL Fiddle][1]

**MySQL 5.6 Schema Setup**:

    CREATE TABLE source (cat int, price int, date timestamp);
    INSERT INTO source (cat, price, date)
    VALUES
      (1, 1000,'2022-01-04 15:00:22'),
    	(2, 1300,'2022-02-05 15:00:22'),
    	(2, 1200,'2022-03-10 10:14:52'),
    	(1, 400,'2022-04-11 09:02:01'),
    	(1, 1200,'2022-05-11 11:04:22'),
    	(2, 5000,'2022-06-20 15:50:22'),
    	(1, 1200,'2022-07-20 19:42:19'),
    	(2, 5000,'2022-08-21 14:00:22');
    
**Query 1**:

    SELECT
        all_months.p_month,
        IFNULL(SUM(source.price), 0) AS t_price
    FROM
        (
            SELECT DISTINCT
                date_format(date, '%Y-%m') AS p_month
            FROM
                source
        ) all_months
        
    LEFT JOIN source ON all_months.p_month = date_format(source.date, '%Y-%m') AND source.cat = 1
    
    GROUP BY
        all_months.p_month

**[Results][2]**:

    | p_month | t_price |
    |---------|---------|
    | 2022-01 |    1000 |
    | 2022-02 |       0 |
    | 2022-03 |       0 |
    | 2022-04 |     400 |
    | 2022-05 |    1200 |
    | 2022-06 |       0 |
    | 2022-07 |    1200 |
    | 2022-08 |       0 |

  [1]: http://sqlfiddle.com/#!9/7485ac/1
  [2]: http://sqlfiddle.com/#!9/7485ac/1/0
