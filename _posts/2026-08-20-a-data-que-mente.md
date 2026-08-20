---
title: "A data que mente: quando o número reportado muda sozinho"
categoria: Qualidade de dados
resumo: "Um relatório fechado no mês passado deixou de bater certo esta semana. Ninguém mexeu em nada. O problema estava na data que escolhemos para filtrar."
---

Acontece em quase todas as organizações que carregam dados por batches: um número
fechado e reportado num mês passa a aparecer diferente semanas depois. Ninguém
alterou o relatório. Ninguém corrigiu a fonte. E, no entanto, o valor mudou.

Quando isto aparece, a primeira reacção costuma ser desconfiar do relatório. Quase
sempre o relatório está bem. O que está errado é a data pela qual se filtra.

## Duas datas, dois significados

Numa tabela carregada por processos de integração há tipicamente duas datas — e
elas não respondem à mesma pergunta:

- **A data da operação** diz quando o facto aconteceu no negócio: quando a apólice
  foi emitida, quando o sinistro foi participado.
- **A data de carga** diz quando o registo entrou no destino.

A segunda é tentadora porque é fiável, está sempre preenchida e é fácil de indexar.
O problema é o que lhe acontece quando o registo é actualizado ou recarregado: a
data de carga é reescrita, e o registo desloca-se de período. Sai de Junho, entra
em Agosto. O total de Junho encolhe — sem que nada de real tenha mudado.

## Como se confirma

Antes de discutir com alguém, vale a pena provar. Uma contagem cruzada entre as
duas datas mostra o desvio de imediato:

```sql
SELECT  YEAR(data_operacao)   AS ano_operacao
,       MONTH(data_operacao)  AS mes_operacao
,       YEAR(data_carga)      AS ano_carga
,       MONTH(data_carga)     AS mes_carga
,       COUNT(*)              AS registos
FROM    factos
GROUP BY YEAR(data_operacao), MONTH(data_operacao)
,        YEAR(data_carga), MONTH(data_carga)
HAVING  COUNT(*) > 0
ORDER BY 1, 2, 3, 4;
```

Se toda a diagonal estiver limpa, as datas coincidem e não há problema. Se
houver massa fora da diagonal, tens registos de um período a ser carregados
noutro — e é exactamente esse volume que faz os números "mudarem sozinhos".

## O que fazer com isto

A regra prática é simples: **reporte de negócio filtra por data de operação;
monitorização de carga filtra por data de carga.** Trocá-las é o erro.

Duas notas que fazem diferença na prática:

1. Se a data de carga for reescrita nas actualizações, perdeste a informação de
   quando o registo entrou pela primeira vez. Vale a pena guardar as duas: uma
   data de primeira inserção e outra de última actualização.
2. Sempre que um número reportado for revisto, o relatório deve dizê-lo. Um número
   que muda em silêncio custa mais credibilidade do que um número revisto com
   nota de rodapé.

Nada disto é sofisticado. Mas é o género de detalhe que separa um relatório em
que as pessoas confiam de um relatório que elas verificam à parte antes de usar.
