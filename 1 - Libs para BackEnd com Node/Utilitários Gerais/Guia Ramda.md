# 🧠 Tudo sobre **Ramda** – A Biblioteca de Programação Funcional para JavaScript  

Se você curte **JavaScript funcional**, mas acha que o **Lodash** não é funcional o suficiente, então **Ramda** pode ser a ferramenta perfeita para você! 🚀  

O **Ramda** é uma biblioteca que facilita a escrita de código **mais declarativo**, **imutável** e **composicional**, tornando seu código mais **limpo e reutilizável**.  

Bora ver **como instalar, usar e explorar os principais conceitos do Ramda** na prática! 💡  

---

## 🚀 O que é **Ramda**?  
O **Ramda** é uma biblioteca de utilitários para **programação funcional** em JavaScript. Ele segue princípios como:  

✅ **Funções puras** – Sem efeitos colaterais  
✅ **Imutabilidade** – Não modifica os dados originais  
✅ **Currying** – Funções retornam novas funções até receberem todos os argumentos  
✅ **Composição** – Permite combinar funções pequenas para criar soluções poderosas  

Se você já usou **Lodash**, mas quer algo **mais funcional**, o Ramda pode ser uma excelente alternativa. 🎯  

---

## 📦 Instalando o Ramda  

Para instalar no seu projeto Node.js, rode:  

```bash
npm install ramda
```
ou, se estiver usando **yarn**:  
```bash
yarn add ramda
```

Depois, importe no seu código:

```js
const R = require('ramda');
```

Se estiver usando **ESModules**:

```js
import * as R from 'ramda';
```

Agora bora ver o Ramda na prática! 🚀  

---

## 📌 Conceitos Básicos do Ramda  

### 🔹 1. Imutabilidade  
O Ramda sempre **retorna novos objetos** em vez de modificar os originais.

```js
const pessoa = { nome: 'João', idade: 25 };

// Sem Ramda (modificando o original)
pessoa.idade = 26; // 😡 Mutável

// Com Ramda (imutável)
const novaPessoa = R.assoc('idade', 26, pessoa);
console.log(novaPessoa); // { nome: 'João', idade: 26 }
console.log(pessoa); // { nome: 'João', idade: 25 } (não foi alterado!)
```

### 🔹 2. Currying  
No Ramda, as funções **aceitam argumentos de forma parcial**, permitindo reutilização fácil.

```js
const multiplicar = R.multiply(2);
console.log(multiplicar(5)); // 10

const saudacao = R.concat('Olá, ');
console.log(saudacao('João')); // Olá, João
```

Currying permite criar **funções altamente reutilizáveis**.  

---

## 🔄 **Transformação de Dados** com Ramda  

### 🟢 3. **Mapear e Filtrar Arrays**  
```js
const numeros = [1, 2, 3, 4, 5];

// Multiplicando cada número por 2
const dobrados = R.map(R.multiply(2), numeros);
console.log(dobrados); // [2, 4, 6, 8, 10]

// Pegando apenas os números pares
const pares = R.filter(R.even, numeros);
console.log(pares); // [2, 4]
```

### 🟡 4. **Reduzir Arrays**  
```js
const soma = R.reduce(R.add, 0, numeros);
console.log(soma); // 15
```

### 🔵 5. **Ordenação e Agrupamento**  
```js
const pessoas = [
  { nome: 'Ana', idade: 28 },
  { nome: 'Carlos', idade: 35 },
  { nome: 'Beatriz', idade: 22 }
];

// Ordenar por idade
const ordenarPorIdade = R.sortBy(R.prop('idade'));
console.log(ordenarPorIdade(pessoas));

/*
[
  { nome: 'Beatriz', idade: 22 },
  { nome: 'Ana', idade: 28 },
  { nome: 'Carlos', idade: 35 }
]
*/
```

---

## 🔗 **Composição de Funções**  
Uma das melhores coisas do Ramda é a **composição funcional**, onde podemos encadear operações de maneira elegante.

### 🏗️ 6. **Usando `compose` e `pipe`**
```js
const dobrar = R.multiply(2);
const somar10 = R.add(10);

// Composição da direita para a esquerda
const processarNumero = R.compose(somar10, dobrar);
console.log(processarNumero(5)); // (5 * 2) + 10 = 20

// Composição da esquerda para a direita
const processarNumeroPipe = R.pipe(dobrar, somar10);
console.log(processarNumeroPipe(5)); // Mesmo resultado
```

O `compose` lê a função de **trás para frente**, enquanto o `pipe` lê **de cima para baixo**.  

💡 **Dica:** Use `pipe` para facilitar a leitura! 🔥  

---

## 🧹 **Manipulação de Objetos**  

### 🟠 7. **Pegando propriedades de objetos (`prop`, `pluck`)**
```js
const pessoa = { nome: 'João', idade: 30 };

console.log(R.prop('nome', pessoa)); // 'João'

const pessoas = [
  { nome: 'Ana', idade: 28 },
  { nome: 'Carlos', idade: 35 }
];

console.log(R.pluck('nome', pessoas)); // ['Ana', 'Carlos']
```

### 🔹 8. **Atualizando objetos (`assoc`, `dissoc`)**  
```js
const pessoaAtualizada = R.assoc('idade', 31, pessoa);
console.log(pessoaAtualizada); // { nome: 'João', idade: 31 }

const pessoaSemIdade = R.dissoc('idade', pessoa);
console.log(pessoaSemIdade); // { nome: 'João' }
```

---

## 🎯 Exemplo Prático: Processamento de Usuários  

Imagine que temos uma **lista de usuários**, e queremos:  
1️⃣ Filtrar os **maiores de idade**  
2️⃣ Pegar **apenas os nomes**  
3️⃣ Ordenar **alfabeticamente**  

```js
const usuarios = [
  { nome: 'Carlos', idade: 17 },
  { nome: 'Ana', idade: 28 },
  { nome: 'Beatriz', idade: 22 }
];

// Definir funções
const maiorDeIdade = R.filter(R.propSatisfies(R.gte(R.__, 18), 'idade'));
const pegarNomes = R.pluck('nome');
const ordenar = R.sortBy(R.identity);

// Composição final
const processarUsuarios = R.pipe(maiorDeIdade, pegarNomes, ordenar);

console.log(processarUsuarios(usuarios)); // ['Ana', 'Beatriz']
```

🔥 Agora temos um **pipeline funcional** que processa a lista de usuários!  

---

## 🏁 Conclusão  

O **Ramda** é uma **ferramenta poderosa** para quem quer escrever **código mais funcional e imutável**.  

### 🎯 **Resumo**:
✅ **Imutabilidade e funções puras**  
✅ **Currying e composição de funções**  
✅ **Manipulação eficiente de arrays e objetos**  
✅ **Substitui `map`, `reduce`, `filter`, `sort`, etc., de forma mais elegante**  

Se você quer **escrever código mais declarativo e limpo**, **Ramda** é uma excelente escolha. Agora é só testar no seu projeto! 🚀🔥