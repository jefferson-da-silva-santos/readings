# 🧑‍🤝‍🧑 O Que São Joins?

Antes de começar, é bom entender o que são esses *joins*. Imagine que você tem duas tabelas e quer unir informações delas de alguma forma. O *join* é a forma de fazer isso! Ele vai juntar os dados das tabelas com base em alguma condição, normalmente uma chave primária e uma chave estrangeira.

---

## 🔗 INNER JOIN - A União Perfeita!

### O que é?

O *INNER JOIN* é o tipo mais comum e básico de *join*. Ele vai unir as duas tabelas e trazer apenas as linhas em que a condição de união seja verdadeira. Ou seja, só aparecem os registros que têm correspondência nas duas tabelas.

### Exemplo:

Imagina que temos duas tabelas: `tb_produtos` e `tb_vendas`.

- **tb_produtos**: Tem o ID do produto e o nome do produto.
- **tb_vendas**: Tem o ID da venda, ID do produto e quantidade vendida.

Agora, queremos encontrar os produtos vendidos e o total vendido.

```sql
SELECT p.id_produto, p.nome_produto, SUM(v.quantidade) AS total_vendido
FROM tb_produtos p
INNER JOIN tb_vendas v ON p.id_produto = v.id_produto
GROUP BY p.id_produto;
```

### Explicação:

- **"INNER JOIN tb_vendas v"**: A gente está dizendo para o SQL unir as duas tabelas, onde o campo `id_produto` de `tb_produtos` seja igual ao campo `id_produto` de `tb_vendas`.
- **SUM(v.quantidade) AS total_vendido**: Estamos somando a quantidade de vendas por produto.
- **GROUP BY p.id_produto**: Vamos agrupar os dados por produto para ter o total vendido de cada um.

Resultado: Só aparecerão os produtos que foram vendidos, porque o `INNER JOIN` só traz registros onde existe uma correspondência entre as duas tabelas.

---

## 🚪 LEFT JOIN - Tudo da Esquerda, Mesmo Sem Correspondência!

### O que é?

O *LEFT JOIN* traz todos os registros da tabela à esquerda (a primeira na consulta) e só as correspondências da tabela à direita. Ou seja, se não houver correspondência na tabela da direita, ainda assim ele trará os dados da esquerda, mas com valores `NULL` nas colunas da tabela à direita.

### Exemplo:

Usando as mesmas tabelas (`tb_produtos` e `tb_vendas`), agora queremos ver todos os produtos, mesmo os que não foram vendidos:

```sql
SELECT p.id_produto, p.nome_produto, SUM(v.quantidade) AS total_vendido
FROM tb_produtos p
LEFT JOIN tb_vendas v ON p.id_produto = v.id_produto
GROUP BY p.id_produto;
```

### Explicação:

- **"LEFT JOIN tb_vendas v"**: Isso significa que queremos todos os produtos, mesmo aqueles que não foram vendidos.
- Mesmo que não haja vendas para um produto, ele será listado com `NULL` na coluna de quantidade vendida.

Resultado: Produtos sem vendas terão a quantidade de vendas como `NULL`.

---

## ➡️ RIGHT JOIN - Tudo da Direita, Mesmo Sem Correspondência!

### O que é?

O *RIGHT JOIN* é praticamente o oposto do *LEFT JOIN*. Ele traz todos os registros da tabela à direita (a segunda na consulta) e as correspondências da tabela à esquerda. Se não houver correspondência, ele retorna `NULL` para os dados da tabela à esquerda.

### Exemplo:

Imagina agora que queremos ver todas as vendas, mesmo aquelas que não correspondem a nenhum produto (digamos que há registros de vendas com `id_produto` inexistente).

```sql
SELECT v.id_venda, v.id_produto, v.quantidade, p.nome_produto
FROM tb_vendas v
RIGHT JOIN tb_produtos p ON v.id_produto = p.id_produto;
```

### Explicação:

- **"RIGHT JOIN tb_produtos p"**: Queremos ver todas as vendas, mesmo que não haja um produto correspondente.
- Mesmo que a venda não tenha produto correspondente, a consulta vai exibir os dados da venda com o campo `nome_produto` como `NULL`.

Resultado: Vendas sem um produto correspondente terão o nome do produto como `NULL`.

---

## 🔄 FULL OUTER JOIN - O Melhor dos Dois Mundos!

### O que é?

O *FULL OUTER JOIN* é uma mistura do *LEFT JOIN* com o *RIGHT JOIN*. Ele traz todos os registros de ambas as tabelas, com `NULL` nos lugares onde não há correspondência.

### Exemplo:

Agora queremos ver todos os produtos e todas as vendas, mesmo que não haja correspondência entre as duas tabelas.

```sql
SELECT p.id_produto, p.nome_produto, v.id_venda, v.quantidade
FROM tb_produtos p
FULL OUTER JOIN tb_vendas v ON p.id_produto = v.id_produto;
```

### Explicação:

- **"FULL OUTER JOIN"**: O SQL vai trazer todos os produtos e todas as vendas.
- Se houver produtos sem vendas, eles terão a coluna `id_venda` e `quantidade` como `NULL`. E se houver vendas sem produtos correspondentes, a coluna `nome_produto` será `NULL`.

Resultado: Todos os produtos e todas as vendas serão exibidos, com `NULL` onde não houver correspondência.

---

## 🔁 CROSS JOIN - A União Total!

### O que é?

O *CROSS JOIN* não precisa de uma condição de união! Ele vai simplesmente juntar cada linha da primeira tabela com cada linha da segunda tabela, formando um produto cartesiano. Isso significa que, se a primeira tabela tem 3 linhas e a segunda tem 4, o resultado terá 12 linhas!

### Exemplo:

Vamos pegar todas as combinações possíveis entre dois conjuntos de dados.

```sql
SELECT p.nome_produto, c.nome_categoria
FROM tb_produtos p
CROSS JOIN tb_categorias c;
```

### Explicação:

- **"CROSS JOIN"**: Ele vai combinar todos os produtos com todas as categorias, sem qualquer condição de união.

Resultado: O número de registros será o produto do número de registros das duas tabelas. No exemplo, se houver 3 produtos e 4 categorias, o resultado será 12 combinações!

---

## 🧑‍🏫 Dicas e Considerações Importantes

- **Evite o CROSS JOIN em tabelas grandes**: Se você não precisa realmente de todas as combinações possíveis, o *CROSS JOIN* pode gerar um volume de dados enorme.
- **Prefira INNER JOIN quando possível**: Ele é rápido e eficiente, já que retorna apenas os dados que realmente têm correspondência.
- **LEFT JOIN é útil para identificar registros "órfãos"**: Se você precisa encontrar dados sem correspondência, o *LEFT JOIN* é ideal.

---

Espero que tenha ficado claro! Agora você já tem um guia para todos os tipos de *join* mais comuns no SQL e sabe como usá-los na prática. Qualquer dúvida, me avisa! 😄