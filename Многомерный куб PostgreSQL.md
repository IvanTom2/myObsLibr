Это многомерный куб на SQL. 

___
```
SELECT name,
cube_ur_coord(‘(0,7,0,0,0,0,0,0,0,7,0,0,0,0,10,0,0,0)’, position) as score
FROM genres g
WHERE cube_ur_coord(‘(0,7,0,0,0,0,0,0,0,7,0,0,0,0,10,0,0,0)’, position) > 0;
```

___
Relations: [[PostgreSQL]]  
Tags: #code 
References: [[Семь баз данных]] 
Query: подбор похожих позиций в SQL