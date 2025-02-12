# ✅ Tudo sobre **Validator.js** – Validação de Dados no Node.js  

Se você precisa **validar e sanitizar entradas** em um projeto Node.js, o **Validator.js** é uma das melhores opções! Ele permite verificar **e-mails, URLs, números, textos, CPF, CNPJ e muito mais**.  

Aqui, vou te mostrar **como instalar, validar e evitar entradas inválidas ou maliciosas** no seu sistema. 🚀  

---

## 🚀 O que é o **Validator.js**?  
O **Validator.js** é uma biblioteca para **validar e limpar dados de entrada**, evitando problemas como:  

✅ **E-mails inválidos**  
✅ **URLs erradas ou maliciosas**  
✅ **Strings vazias ou contendo apenas espaços**  
✅ **SQL Injection ou ataques XSS**  
✅ **Números, datas e formatos específicos**  

Ele é **leve, rápido e muito fácil de usar**. Agora bora instalar e testar! 🔥  

---

## 📦 Instalando o Validator.js  

Para instalar, basta rodar:  

```bash
npm install validator
```
ou, se estiver usando **yarn**:  
```bash
yarn add validator
```

Agora, importe no seu código:

```js
const validator = require('validator');
```

---

## 📩 Validação de E-mail  

Para verificar se um **e-mail é válido**, use `isEmail()`:  

```js
const email = 'usuario@exemplo.com';

if (validator.isEmail(email)) {
  console.log('✅ E-mail válido!');
} else {
  console.log('❌ E-mail inválido!');
}
```

### 🔹 Exemplo de saída:
```bash
✅ E-mail válido!
```

Se tentarmos validar um e-mail errado:
```js
console.log(validator.isEmail('usuário@errado')); // false
```

---

## 🌍 Validação de URLs  

Se precisar validar **URLs**, use `isURL()`:  

```js
const url = 'https://www.meusite.com';

if (validator.isURL(url)) {
  console.log('✅ URL válida!');
} else {
  console.log('❌ URL inválida!');
}
```

### 🔹 Exemplo de saída:
```bash
✅ URL válida!
```

Se tentarmos validar um link errado:
```js
console.log(validator.isURL('meusite.com')); // false
```
Por padrão, o `isURL()` exige **http:// ou https://** no início.  

---

## 📆 Validação de Data  

Se precisar validar datas, use `isDate()`:  

```js
console.log(validator.isDate('2024-05-20')); // true
console.log(validator.isDate('31/02/2024')); // false
```

Isso evita problemas com **datas inválidas**, como "31 de fevereiro". ✅  

---

## 🔢 Validação de Números  

Se quiser verificar se um valor é **número**, use `isNumeric()`:  

```js
console.log(validator.isNumeric('12345')); // true
console.log(validator.isNumeric('12a45')); // false
```

Se precisar verificar se é um **inteiro**, use `isInt()`:  

```js
console.log(validator.isInt('50')); // true
console.log(validator.isInt('50.5')); // false
```

Se precisar validar um **float (decimal)**:  

```js
console.log(validator.isFloat('3.14')); // true
console.log(validator.isFloat('abc')); // false
```

---

## 💳 Validação de Cartão de Crédito  

Se precisar validar **números de cartão de crédito**, use `isCreditCard()`:  

```js
console.log(validator.isCreditCard('4111111111111111')); // true (Visa)
console.log(validator.isCreditCard('1234567890123456')); // false
```

Isso verifica **se o número segue os padrões reais de cartões de crédito**! 🔥  

---

## 📜 Validação de CPF e CNPJ (Brasil)  

Se quiser validar **CPF e CNPJ**, o Validator.js não tem suporte nativo, mas podemos usar outra lib chamada `cpf-cnpj-validator`:  

```bash
npm install cpf-cnpj-validator
```

Agora, no código:

```js
const { cpf, cnpj } = require('cpf-cnpj-validator');

console.log(cpf.isValid('123.456.789-09')); // false
console.log(cpf.isValid('111.444.777-35')); // true
console.log(cnpj.isValid('11.222.333/0001-81')); // false
console.log(cnpj.isValid('60.871.575/0001-25')); // true
```

Isso permite validar **CPFs e CNPJs reais**! ✅  

---

## 🚫 Validação de String Vazia ou Apenas Espaços  

Se quiser evitar entradas vazias ou com apenas espaços, use `isEmpty()`:  

```js
console.log(validator.isEmpty('')); // true
console.log(validator.isEmpty('    ')); // true
console.log(validator.isEmpty('texto')); // false
```

Isso é útil para **formularios**, evitando que o usuário envie valores vazios.  

---

## ✂️ **Sanitização** de Entradas  

Além de validar, o **Validator.js** também pode **sanitizar** entradas para remover espaços e caracteres indesejados.  

### 🔹 **Removendo espaços extras**
```js
console.log(validator.trim('  Olá Mundo!  ')); // "Olá Mundo!"
```

### 🔹 **Removendo tags HTML (XSS Protection)**
```js
console.log(validator.escape('<script>alert("ataque XSS")</script>'));
// "&lt;script&gt;alert(&quot;ataque XSS&quot;)&lt;/script&gt;"
```

Isso protege contra **ataques XSS**, onde hackers tentam injetar scripts no seu sistema. 🚨  

---

## 🔥 Integrando com **Express.js**  

Se você está validando **requisições em APIs Express**, pode usar Validator.js para proteger os dados da requisição:  

```js
const express = require('express');
const validator = require('validator');

const app = express();
app.use(express.json());

app.post('/cadastro', (req, res) => {
  const { nome, email, idade } = req.body;

  if (validator.isEmpty(nome) || !validator.isEmail(email) || !validator.isInt(idade)) {
    return res.status(400).json({ erro: 'Dados inválidos!' });
  }

  res.json({ mensagem: 'Cadastro realizado com sucesso!' });
});

app.listen(3000, () => console.log('🚀 Servidor rodando na porta 3000'));
```

Agora, se alguém tentar cadastrar um **e-mail inválido ou um campo vazio**, a API retorna um erro! 🔥  

---

## 🏁 Conclusão  

O **Validator.js** é uma ferramenta essencial para validar e limpar entradas no **Node.js**, protegendo seu sistema contra dados errados ou maliciosos.  

### 🎯 Resumo:  
✅ **Verificação de e-mails, URLs e números**  
✅ **Sanitização de strings e remoção de espaços**  
✅ **Proteção contra XSS e SQL Injection**  
✅ **Fácil integração com Express**  

Se você quer **melhorar a segurança e confiabilidade dos seus formulários e APIs**, o Validator.js é uma **excelente escolha**! 🚀🔥