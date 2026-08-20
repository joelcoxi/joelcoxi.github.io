---
title: "Tabela de calendário sem cursor nem loop"
ordem: 1
dialecto: T-SQL
categoria: Modelação
problema: "Quase todo o modelo dimensional precisa de uma tabela de datas contínua. Gerá-la com WHILE é lento e desnecessário."
---

```sql
DECLARE @inicio DATE = '2020-01-01'
,       @fim    DATE = '2030-12-31';

WITH numeros AS (
    SELECT  TOP (DATEDIFF(DAY, @inicio, @fim) + 1)
            ROW_NUMBER() OVER (ORDER BY (SELECT NULL)) - 1 AS n
    FROM    sys.all_objects a
    CROSS JOIN sys.all_objects b
)
SELECT  CAST(DATEADD(DAY, n, @inicio) AS DATE)          AS data
,       YEAR(DATEADD(DAY, n, @inicio))                  AS ano
,       MONTH(DATEADD(DAY, n, @inicio))                 AS mes
,       DATEPART(QUARTER, DATEADD(DAY, n, @inicio))     AS trimestre
,       DATEPART(WEEKDAY, DATEADD(DAY, n, @inicio))     AS dia_semana
,       CASE WHEN DATEPART(WEEKDAY, DATEADD(DAY, n, @inicio)) IN (1, 7)
             THEN 0 ELSE 1 END                          AS e_dia_util
FROM    numeros;
```

O `CROSS JOIN` sobre `sys.all_objects` dá linhas suficientes para décadas.
Para calcular dias úteis a sério, junta a esta tabela uma lista de feriados —
em Angola os feriados móveis obrigam a manutenção anual, por isso vale a pena
mantê-los numa tabela e não em `CASE`.
