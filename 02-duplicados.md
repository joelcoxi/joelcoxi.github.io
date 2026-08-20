---
title: "Apagar duplicados mantendo o registo mais recente"
ordem: 2
dialecto: T-SQL
categoria: Qualidade de dados
problema: "A mesma chave de negócio aparece várias vezes depois de uma recarga. Só interessa a última versão de cada uma."
---

```sql
WITH ordenados AS (
    SELECT  *
    ,       ROW_NUMBER() OVER (
                PARTITION BY chave_negocio
                ORDER BY     data_actualizacao DESC, id DESC
            ) AS ordem
    FROM    destino
)
DELETE  FROM ordenados
WHERE   ordem > 1;
```

Antes de apagar seja o que for, troca o `DELETE` por `SELECT * FROM ordenados
WHERE ordem > 1` e olha para o que sai. O desempate por `id DESC` existe para
o caso — frequente — de a data de actualização estar repetida ao segundo.

Se os duplicados forem recorrentes, o problema não está aqui: está na chave
do processo de carga.
