# 📏 Tudo sobre **Joi** – Validação de Dados no Node.js  

Se você já precisou validar dados em uma **API no Node.js**, sabe o quanto isso pode ser chato e repetitivo. **E se existisse uma forma fácil, rápida e flexível para validar inputs?** 🤔  

É aí que entra o **Joi**, uma biblioteca poderosa para validação de dados **sem sofrimento**! 🚀  

Aqui, vou te mostrar **como instalar, usar e criar validações poderosas** para inputs no Node.js e Express.  

---

## 🔎 O que é o **Joi**?  
O **Joi** é uma biblioteca para **validar dados de forma declarativa**, garantindo que os inputs recebidos estejam no formato correto **antes de processá-los**.  

🔹 **Por que usar Joi?**  

✅ **Evita bugs e crashes** causados por dados inválidos  
✅ **Melhora a segurança** prevenindo ataques com inputs maliciosos  
✅ **Sintaxe clara e intuitiva**  
✅ **Valida dados automaticamente** sem precisar de ifs gigantes  
✅ **Perfeito para APIs Express/Fastify**  
✅ **Totalmente personalizável**  

Se sua API recebe **inputs de usuários, formulários ou JSONs**, o Joi pode **salvar seu código** de verificações manuais cansativas! 🔥  

---

## 📦 Instalando o Joi  
Para adicionar ao seu projeto, basta rodar:  

```bash
npm install joi
```
ou, se estiver usando **yarn**:  
```bash
yarn add joi
```

Depois, importa no código:

```js
const Joi = require('joi');
```

Agora, bora começar a **validar dados como um mestre**! 💪  

---

## 🛠 Criando uma validação básica
O Joi permite **definir um esquema de validação** e aplicar nos dados recebidos.

### Exemplo: Validando um usuário
```js
const schemaUsuario = Joi.object({
  nome: Joi.string().min(3).max(30).required(),
  email: Joi.string().email().required(),
  idade: Joi.number().integer().min(18).max(100),
});

const usuario = {
  nome: 'John Doe',
  email: 'john.doe@example.com',
  idade: 25
};

const { error, value } = schemaUsuario.validate(usuario);

if (error) {
  console.log('❌ Erro de validação:', error.details[0].message);
} else {
  console.log('✅ Dados válidos:', value);
}
```

Se o usuário enviar **dados inválidos**, ele recebe um erro amigável! 🛑  

```json
{
  "error": "❌ nome deve ter no mínimo 3 caracteres"
}
```

Mas se tudo estiver certo, o Joi retorna os dados **já validados**! ✅  

---

## 🎯 Tipos de validação do Joi
O Joi permite validar **vários tipos de dados**:

### 🔹 **Strings**
```js
Joi.string().min(3).max(30).required();  // Entre 3 e 30 caracteres
Joi.string().email();                     // Validação de e-mail
Joi.string().pattern(/^[A-Za-z0-9]+$/);  // Apenas letras e números
```

### 🔹 **Números**
```js
Joi.number().integer().min(0).max(100);   // Número inteiro entre 0 e 100
Joi.number().positive();                  // Apenas números positivos
Joi.number().precision(2);                // 2 casas decimais
```

### 🔹 **Booleanos**
```js
Joi.boolean();
```

### 🔹 **Arrays**
```js
Joi.array().items(Joi.string());         // Array de strings
Joi.array().min(2).max(5);               // Mínimo 2 e máximo 5 itens
```

### 🔹 **Objetos**
```js
Joi.object({
  nome: Joi.string().required(),
  idade: Joi.number().required()
});
```

### 🔹 **Datas**
```js
Joi.date().iso();                       // Formato ISO 8601 (YYYY-MM-DD)
Joi.date().greater('1-1-2000');         // Maior que o ano 2000
```

Agora que você já conhece os tipos, bora aplicar no **Express**! 🚀  

---

## 🔥 Usando Joi no **Express**
Se sua API usa **Express**, você pode validar os inputs direto nas rotas.  

### Exemplo: Validando um cadastro de usuário  
```js
const express = require('express');
const Joi = require('joi');

const app = express();
app.use(express.json());

const schemaCadastro = Joi.object({
  nome: Joi.string().min(3).required(),
  email: Joi.string().email().required(),
  senha: Joi.string().min(6).required()
});

app.post('/cadastro', (req, res) => {
  const { error } = schemaCadastro.validate(req.body);

  if (error) {
    return res.status(400).json({ erro: error.details[0].message });
  }

  res.json({ message: '✅ Cadastro realizado com sucesso!' });
});

app.listen(3000, () => console.log('🔥 Servidor rodando na porta 3000'));
```

Agora, se um usuário tentar cadastrar com **dados inválidos**, recebe um erro:  

```json
{
  "erro": "❌ senha deve ter no mínimo 6 caracteres"
}
```

Isso evita que dados inválidos entrem no banco! 🔥  

---

## 🛡️ Validação de senha forte (regex)
Se quiser validar **senhas mais fortes**, pode usar **expressões regulares**:

```js
const schemaSenha = Joi.object({
  senha: Joi.string()
    .pattern(new RegExp('^(?=.*[a-z])(?=.*[A-Z])(?=.*\\d)[A-Za-z\\d@$!%*?&]{8,}$'))
    .required()
    .messages({
      'string.pattern.base': 'Senha deve ter pelo menos 8 caracteres, 1 letra maiúscula, 1 minúscula e 1 número.'
    })
});
```

Agora, só aceita senhas **seguras**:

```
Senha fraca: "abc123" ❌ REJEITADO
Senha forte: "SenhaSegura123!" ✅ ACEITO
```

---

## ⏳ Validando parâmetros da URL (req.params)
Se sua API recebe **parâmetros na URL**, também dá pra validar:

```js
const schemaParams = Joi.object({
  id: Joi.number().integer().required()
});

app.get('/usuario/:id', (req, res) => {
  const { error } = schemaParams.validate(req.params);

  if (error) return res.status(400).json({ erro: error.details[0].message });

  res.json({ message: `✅ Usuário ${req.params.id} encontrado!` });
});
```

Agora, se o usuário enviar um `id` inválido, recebe um erro **antes da API processar**! 🛑  

---

## 🎛️ Criando um Middleware de validação (Reutilizável)
Se quiser **reutilizar a validação**, crie um **middleware**:

```js
const validarDados = (schema) => (req, res, next) => {
  const { error } = schema.validate(req.body);
  if (error) return res.status(400).json({ erro: error.details[0].message });
  next();
};

app.post('/cadastro', validarDados(schemaCadastro), (req, res) => {
  res.json({ message: '✅ Cadastro realizado!' });
});
```

Agora, **toda rota** pode usar o middleware sem repetir código! 🎯  

---

## 🏁 Conclusão
O **Joi** torna a validação de dados **simples, segura e poderosa**!  

### 🔥 Resumo:
✅ **Evita bugs e crashes com inputs inválidos**  
✅ **Validação clara e intuitiva**  
✅ **Evita ataques (injection, brute-force, etc.)**  
✅ **Perfeito para APIs REST no Express**  
✅ **Criação de middlewares reutilizáveis**  

Se sua API recebe dados de usuários, **use o Joi e durma tranquilo**! 😴🔥