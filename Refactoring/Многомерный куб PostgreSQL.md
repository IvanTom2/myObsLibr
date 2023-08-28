___

___
[[Code]]:
```
SELECT name,
cube_ur_coord(‘(0,7,0,0,0,0,0,0,0,7,0,0,0,0,10,0,0,0)’, position) as score
FROM genres g
WHERE cube_ur_coord(‘(0,7,0,0,0,0,0,0,0,7,0,0,0,0,10,0,0,0)’, position) > 0;
```
___
Language: [[SQL]]
Key-words:  [[PostgreSQL]] [[]]
Question query?: подбор похожих позиций SQL