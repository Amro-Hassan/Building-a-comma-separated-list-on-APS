# Building-a-comma-separated-list-on-APS
--While in SQL Server  two great methods for grouped concatenation: STRING_AGG(), introduced in SQL Server 2017 (and now available in ----Azure SQL Database), and FOR XML PATH, if you are on an older version. both options are not available on APS PDW as alternative code I --have created the code below

-- Base Table contain the data
CREATE TABLE #BaseLine 

	(
	ID INT,
	NAME VARCHAR(50),
	seq int)
 INSERT INTO #BASELINE   VALUES  (1, 'awdwd', 1) 
 INSERT INTO #BASELINE   VALUES  (1,'dwdv', 2) 
 INSERT INTO #BASELINE   VALUES  (2,'xzzxz', 1) 
 INSERT INTO #BASELINE   VALUES  (2, 'wqwqw', 4) 
 INSERT INTO #BASELINE   VALUES  (2, 'uyuy', 5) 
 INSERT INTO #BASELINE   VALUES  (2, 'Esq.', 6) 
 INSERT INTO #BASELINE   VALUES  (3, 'T978', 1)
  INSERT INTO #BASELINE   VALUES  (3,'mum676', 2) 
 INSERT INTO #BASELINE   VALUES  (3,'2342342d', 3) 
  INSERT INTO #BASELINE   VALUES  (4, '76797', 1) 
  INSERT INTO #BASELINE   VALUES  (4,'3cd 32', 2)
   INSERT INTO #BASELINE   VALUES  (4,'1a1qqqqqq', 3) 


   SELECT id, CAST(name as varchar(8000))name, ROW_NUMBER() OVER(Partition By id order by seq) rw,
 COUNT(*) OVER (Partition By id) Recursiv  
 INTO #basetable
 FROM  #BaseLine  
 
SELECT * INTO  #rCTE  FROM ( SELECT Recursiv, id, name, rw 
 from #basetable 
 where rw=1 
 UNION ALL 
 SELECT b.Recursiv, r.ID, r.name +', '+ b.name name, r.rw+1 
 FROM #basetable b inner join 
#basetable r 
 on b.id = r.id and b.rw = r.rw+1 ) z
 SELECT NAME FROM #rCTE WHERE Recursiv = rw 
Results:

NAME
awdwd, dwdv
uyuy, Esq.
3cd 32, 1a1qqqqqq
mum676, 2342342d
