# 🔑 Tudo sobre **jsonwebtoken (JWT)** – Autenticação no Node.js

Se você precisa implementar **autenticação segura** em uma API no **Node.js**, provavelmente já ouviu falar do **JWT (JSON Web Token)**. O pacote `jsonwebtoken` é uma das formas mais populares de gerar e validar tokens no Node.js.

Aqui eu vou te mostrar **o que é JWT, como instalar, gerar e validar tokens de forma segura no seu projeto**. 🚀

---

## 🛠 O que é JSON Web Token (**JWT**)?
O **JWT (JSON Web Token)** é um padrão para **autenticação e troca segura de informações** entre sistemas. Ele é amplamente usado para:

✅ **Autenticação de usuários** (substitui sessões e cookies)  
✅ **Autorização** (controle de acesso a recursos)  
✅ **Troca segura de informações** (dados assinados digitalmente)  

O JWT é um **token assinado**, que pode ser **validado sem necessidade de um banco de dados**, tornando a autenticação mais rápida.

### 🟢 Como funciona um JWT?
1️⃣ O **usuário faz login** e o servidor gera um **token JWT** contendo informações como `id`, `email`, `role`, etc.  
2️⃣ O token é enviado para o **cliente (frontend)**, que o armazena (geralmente no **localStorage** ou **cookies**)  
3️⃣ Em cada requisição futura, o cliente envia o **token JWT no header Authorization**  
4️⃣ O servidor **valida o token** e permite o acesso ao recurso  

---

## 📦 Instalando o **jsonwebtoken**
Para usar no seu projeto **Node.js**, instale com:

```bash
npm install jsonwebtoken
```
ou, se estiver usando **yarn**:
```bash
yarn add jsonwebtoken
```

Depois, importe no seu código:

```js
const jwt = require('jsonwebtoken');
```

Agora bora ver **como gerar e validar tokens**. 🔥

---

## 🔐 Gerando um Token JWT
Vamos criar um **token JWT** usando `jsonwebtoken.sign()`:

```js
const jwt = require('jsonwebtoken');

const usuario = {
  id: 1,
  nome: 'John Doe',
  email: 'johndoe@example.com'
};

// Chave secreta (use uma mais segura em produção)
const segredo = 'minha_chave_secreta';

const token = jwt.sign(usuario, segredo, { expiresIn: '1h' });

console.log('Token gerado:', token);
```

### 🔹 O que acontece aqui?
- **`jwt.sign(payload, segredo, opções)`** → Cria um token JWT  
- O **payload** (`usuario`) contém os dados que queremos incluir no token  
- O **segredo** é usado para **assinar** o token (deve ser mantido seguro!)  
- **`expiresIn: '1h'`** define que o token expira em **1 hora**  

---

## 🔍 Estrutura de um Token JWT
O token gerado terá 3 partes separadas por `.` (ponto):

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.
eyJpZCI6MSwibm9tZSI6IkpvaG4gRG9lIiwiZW1haWwiOiJqb2huZG9lQGV4YW1wbGUuY29tIiwiaWF0IjoxNjkxMzMxNjAwLCJleHAiOjE2OTEzMzUyMDB9.
rCUrPb6eYMy9L2BYqFuFS4ql_6H54LpXZw9o06J3D8M
```

Cada parte significa:
1️⃣ **Header** → Define o algoritmo de criptografia  
2️⃣ **Payload** → Contém os dados do usuário (id, nome, email, expiração)  
3️⃣ **Assinatura** → Garante que o token **não foi alterado**  

⚠️ **IMPORTANTE:** O conteúdo do JWT pode ser lido facilmente (pois é só um **Base64**), então **NUNCA** coloque **senhas ou dados sensíveis no payload**!

---

## 🔑 Validando um Token JWT
Agora vamos **verificar se um token JWT é válido** com `jsonwebtoken.verify()`:

```js
const tokenRecebido = 'SEU_TOKEN_AQUI';

try {
  const usuarioDecodificado = jwt.verify(tokenRecebido, segredo);
  console.log('Token válido! Usuário:', usuarioDecodificado);
} catch (error) {
  console.error('Token inválido ou expirado:', error.message);
}
```

### 🔹 O que acontece aqui?
- `jwt.verify(token, segredo)` verifica se o token é **válido e não foi modificado**
- Se o token **for válido**, retorna os dados do usuário
- Se o token **for inválido ou expirado**, lança um erro

---

## 🏠 Guardando o Token no Frontend
No frontend, podemos armazenar o token de duas formas:

### 🔹 **LocalStorage (não recomendado para segurança)**
```js
localStorage.setItem('token', token);
```
⚠️ **Risco:** Vulnerável a ataques **XSS** (Cross-Site Scripting).

### 🔹 **Cookies (mais seguro)**
```js
document.cookie = `token=${token}; HttpOnly; Secure`;
```
✅ Mais seguro porque impede **acesso via JavaScript**.

---

## 🔒 Protegendo rotas com JWT (Middleware)
Podemos criar um **middleware no Express** para proteger **rotas privadas**:

```js
const express = require('express');
const jwt = require('jsonwebtoken');

const app = express();
const segredo = 'minha_chave_secreta';

// Middleware para verificar token
function autenticarToken(req, res, next) {
  const token = req.headers.authorization?.split(' ')[1];

  if (!token) {
    return res.status(401).json({ mensagem: 'Acesso negado! Token não fornecido.' });
  }

  try {
    const usuario = jwt.verify(token, segredo);
    req.usuario = usuario;
    next();
  } catch (error) {
    return res.status(403).json({ mensagem: 'Token inválido ou expirado.' });
  }
}

// Rota protegida
app.get('/api/protegida', autenticarToken, (req, res) => {
  res.json({ mensagem: 'Acesso permitido!', usuario: req.usuario });
});

app.listen(3000, () => console.log('🔥 Servidor rodando na porta 3000'));
```

### 🔹 Como funciona?
1️⃣ **O cliente faz login e recebe um token JWT**  
2️⃣ **Envia o token no header Authorization** nas próximas requisições  
3️⃣ **O middleware verifica o token antes de acessar a rota**  
4️⃣ **Se for válido, permite o acesso**; senão, retorna `401` ou `403`  

---

## 🔄 Renovação de Token JWT (Refresh Token)
O JWT **expira após um tempo**, então podemos criar um **Refresh Token** para renovar tokens expirados sem precisar de novo login.

### 🔹 Gerando um Refresh Token:
```js
const refreshToken = jwt.sign({ id: usuario.id }, segredo, { expiresIn: '7d' });
```

Armazene o Refresh Token no **banco de dados** e use para gerar **um novo access token** quando necessário.

---

## 🏁 Conclusão
O **jsonwebtoken (JWT)** é uma solução **rápida, segura e eficiente** para autenticação no Node.js, sem necessidade de sessões no banco.

### 🎯 Resumo:
✅ **Gera tokens seguros para autenticação**  
✅ **Verifica tokens recebidos do cliente**  
✅ **Protege rotas no Express** com middleware  
✅ **Pode ser combinado com Refresh Tokens** para sessões longas  
✅ **Funciona bem com cookies ou localStorage**  

---

## 🔥 Exemplo Completo de Login com JWT:
```js
const express = require('express');
const jwt = require('jsonwebtoken');
const app = express();

app.use(express.json());

const usuarios = [{ id: 1, email: 'teste@example.com', senha: '123456' }];
const segredo = 'minha_chave_secreta';

app.post('/login', (req, res) => {
  const { email, senha } = req.body;
  const usuario = usuarios.find(u => u.email === email && u.senha === senha);

  if (!usuario) return res.status(401).json({ mensagem: 'Credenciais inválidas' });

  const token = jwt.sign({ id: usuario.id, email }, segredo, { expiresIn: '1h' });

  res.json({ token });
});

app.listen(3000, () => console.log('🔥 Servidor rodando na porta 3000'));
```

Agora você já pode usar **JWT no seu projeto**! 🚀🔒