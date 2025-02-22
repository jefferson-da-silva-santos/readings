# LIMIT e OFFSET
Imagina que você tem uma lista gigante de nomes, tipo um caderno com 1.000 contatos, e você só quer ver os primeiros 10 nomes. No MySQL, a gente usa o **LIMIT** pra isso.  

### 📌 Como funciona o **LIMIT**  
Ele basicamente diz: "Ei, banco de dados, me mostra só um número X de resultados".  

Exemplo:  
```sql
SELECT * FROM usuarios LIMIT 10;
```
Isso pega **apenas os primeiros 10 usuários** da tabela `usuarios`.  

---  

### 📌 E o **OFFSET**, o que é?  
Pensa que agora você quer ver **do 11º nome em diante**, ou seja, pular os primeiros 10. O **OFFSET** serve pra isso!  

Exemplo:  
```sql
SELECT * FROM usuarios LIMIT 10 OFFSET 10;
```
Aqui ele pega **10 resultados, mas começando do 11º**.  

Outra forma de escrever (mais comum em MySQL) é:  
```sql
SELECT * FROM usuarios LIMIT 10, 10;
```
O primeiro `10` é o OFFSET (pula os 10 primeiros), e o segundo `10` é o número de resultados que você quer ver.  

---  

### 📝 Resumo  
🔹 **LIMIT** = Quantos resultados eu quero ver.  
🔹 **OFFSET** = Quantos resultados eu quero pular antes de começar.  

Isso é super útil pra fazer paginação, tipo quando você vê uma lista de produtos numa loja online e vai clicando em "Próxima página". Cada página mostra um pedaço dos resultados, usando **LIMIT e OFFSET** no banco.  

Ficou mais claro assim? 🚀😃

<hr/>

O comando:  
```sql
SELECT * FROM usuarios LIMIT 10 OFFSET 10;
```
Significa:  
👉 **"Me traga 10 registros, mas começando do 11º"** (porque o banco de dados começa a contagem no 0).  

💡 **Explicação prática**:  
Se sua tabela tem esses usuários:  
```
ID | Nome  
---------------
1  | Ana  
2  | Bruno  
3  | Carla  
4  | Diego  
5  | Eduardo  
6  | Fernanda  
7  | Gabriel  
8  | Helena  
9  | Igor  
10 | Juliana  
11 | Karen  
12 | Lucas  
13 | Marina  
14 | Nathalia  
15 | Otávio  
```
O comando **LIMIT 10 OFFSET 10** vai pegar **10 registros a partir do 11º**, ou seja:  
```
ID | Nome  
---------------
11 | Karen  
12 | Lucas  
13 | Marina  
14 | Nathalia  
15 | Otávio  
```
E assim por diante! 🚀  

Se fosse `LIMIT 5 OFFSET 5`, ele pegaria 5 registros **a partir do 6º**.  

Agora tá mais claro? 😃


Beleza! Vou te explicar como fazer **paginação** usando `LIMIT` e `OFFSET`, com exemplos práticos e cálculos simples.  

### 📝 O que é paginação?  
Paginação é quando você divide os resultados em várias páginas, tipo numa loja online que mostra 10 produtos por vez. A ideia é carregar só um pedaço dos dados de cada vez, pra não pesar no banco de dados.  

---

## 🔢 **Cálculo do OFFSET**  

A fórmula pra calcular o `OFFSET` é:  
```sql
OFFSET = (número_da_página - 1) * itens_por_página
```
Ou seja, o `OFFSET` serve pra **pular** os itens das páginas anteriores.  

### 📌 **Exemplo 1: Pegando a 1ª página**  
Suponha que queremos mostrar **5 itens por página**.  

#### Página 1 (primeiros 5 itens):  
Cálculo:  
```
OFFSET = (1 - 1) * 5 = 0  (não pula nada)
```
SQL:  
```sql
SELECT * FROM produtos LIMIT 5 OFFSET 0;
```
Resultados (5 primeiros produtos):  
```
ID | Nome  
---------------
1  | Camiseta  
2  | Calça  
3  | Tênis  
4  | Boné  
5  | Relógio  
```

---

### 📌 **Exemplo 2: Pegando a 2ª página**  
Agora queremos pegar os **próximos 5 produtos** (página 2).  

Cálculo:  
```
OFFSET = (2 - 1) * 5 = 5  (pula os primeiros 5)
```
SQL:  
```sql
SELECT * FROM produtos LIMIT 5 OFFSET 5;
```
Resultados (5 próximos produtos):  
```
ID | Nome  
---------------
6  | Mochila  
7  | Óculos  
8  | Carteira  
9  | Pulseira  
10 | Chaveiro  
```

---

### 📌 **Exemplo 3: Pegando a 3ª página**  
Agora queremos a **página 3**, ou seja, **os próximos 5 produtos**.  

Cálculo:  
```
OFFSET = (3 - 1) * 5 = 10  (pula os primeiros 10)
```
SQL:  
```sql
SELECT * FROM produtos LIMIT 5 OFFSET 10;
```
Resultados (5 próximos produtos):  
```
ID | Nome  
---------------
11 | Meia  
12 | Cinto  
13 | Luva  
14 | Cachecol  
15 | Gorro  
```

---

## 🔄 **Generalizando para qualquer página**
Se tivermos:  
- `itens_por_página = 5`
- `página = X`

A consulta fica:  
```sql
SELECT * FROM produtos LIMIT 5 OFFSET (X - 1) * 5;
```

Se for página 4, por exemplo:  
```
OFFSET = (4 - 1) * 5 = 15
```
```sql
SELECT * FROM produtos LIMIT 5 OFFSET 15;
```

---

## ✅ **Resumo**
1. `LIMIT` define quantos itens você quer por página.  
2. `OFFSET` pula os itens das páginas anteriores.  
3. O cálculo do `OFFSET` é:  
   ```
   (página - 1) * itens_por_página
   ```
4. A query final para qualquer página é:  
   ```sql
   SELECT * FROM produtos LIMIT itens_por_página OFFSET (página - 1) * itens_por_página;
   ```

Agora ficou claro como fazer paginação? 🚀😃