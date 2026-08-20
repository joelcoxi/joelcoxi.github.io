---
title: "Reconciliar dois sistemas e ver exactamente onde divergem"
ordem: 3
dialecto: T-SQL
categoria: Reconciliação
problema: "Dois sistemas deviam ter os mesmos registos e não têm. Saber que 'faltam 143' não chega — é preciso saber quais e de que lado."
---

```sql
SELECT  COALESCE(a.chave, b.chave)  AS chave
,       CASE
            WHEN b.chave IS NULL THEN 'só no sistema A'
            WHEN a.chave IS NULL THEN 'só no sistema B'
            ELSE                      'valor diferente'
        END                         AS situacao
,       a.valor                     AS valor_a
,       b.valor                     AS valor_b
,       ABS(ISNULL(a.valor, 0) - ISNULL(b.valor, 0)) AS diferenca
FROM        sistema_a AS a
FULL JOIN   sistema_b AS b  ON  b.chave = a.chave
WHERE       a.chave IS NULL
   OR       b.chave IS NULL
   OR       a.valor <> b.valor
ORDER BY    diferenca DESC;
```

O `FULL JOIN` é o que permite ver os dois lados de uma vez; com `LEFT JOIN`
só encontras metade do problema e ficas com a falsa sensação de o ter resolvido.

Ordenar por diferença descendente é deliberado: em reconciliações reais, meia
dúzia de registos costuma explicar quase todo o desvio. Ataca esses primeiro
antes de investigar a cauda.
