# PROJETO-SQL-JOHANA

1. Tem alguns Clientes que são dependentes. Quero que vocês me digam de que clientes eles são dependentes.
○ Por exemplo “Filho A” é dependente de qual outro cliente?

SELECT conta.numero AS conta,cliente_pai ,nome AS cliente_dependente
FROM cliente
JOIN cliente_conta ON cliente_conta.id_cliente=cliente.id 
JOIN (SELECT cliente.nome AS cliente_pai,cliente_conta.id_conta as conta_pai
FROM cliente 
JOIN cliente_conta ON cliente_conta.id_cliente=cliente.id 
WHERE dependente=false) AS clientes_pai
ON conta_pai = cliente_conta.id_conta
JOIN conta ON conta.id=cliente_conta.id_conta
WHERE dependente=true
ORDER BY conta;


2. Quais foram as 5 contas que:
○ Mais fizeram transações
○ Menos fizeram transações

SELECT
cliente_conta.id_conta, conta.numero AS "numero de conta",
COUNT(transacao.id) AS "Quantidade_de_trasacao"
FROM transacao
JOIN cliente_conta
ON transacao.id_cliente_conta = cliente_conta.id
JOIN conta
ON cliente_conta.id = conta.id
GROUP BY conta.id
ORDER BY Quantidade_de_trasacao DESC
Limit 5;

SELECT
cliente_conta.id_conta, conta.numero AS "numero de conta",
COUNT(transacao.id) AS "Quantidade_de_trasacao"
FROM transacao
JOIN cliente_conta
ON transacao.id_cliente_conta = cliente_conta.id
JOIN conta
ON cliente_conta.id = conta.id
GROUP BY conta.id
ORDER BY Quantidade_de_trasacao ASC
LIMIT 5;

3. Tivemos uma perda de dados e não sabemos qual é o saldo de cada conta, mas temos todas as transações
efetuadas.
○ Queremos saber qual saldo total das contas registradas em banco!
■ Reparem que temos alguns tipos de transações que subtraem dinheiro e outros que
somam.

CREATE VIEW tb_creditos AS SELECT id_cliente_conta AS id_temp, SUM(transacao.valor) AS creditos
FROM transacao
WHERE id_tipo_transacao = 1
GROUP BY id_cliente_conta;

SELECT id_cliente_conta, tb_creditos.creditos - SUM(transacao.valor)  AS saldo_conta
FROM transacao
inner JOIN tb_creditos ON tb_creditos.id_temp = id_cliente_conta
WHERE id_tipo_transacao IN (2,3,4)
GROUP BY id_cliente_conta
ORDER BY saldo_conta ASC;


