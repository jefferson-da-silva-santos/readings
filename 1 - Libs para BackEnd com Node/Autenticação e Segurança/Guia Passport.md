# 🔐 Tudo sobre **Passport.js** – Autenticação no Node.js  

Se você está desenvolvendo um sistema com **autenticação de usuários** no **Node.js**, usar **JWT, OAuth, ou autenticação local** pode ser um desafio. O **Passport.js** resolve isso de forma modular e flexível!  

Aqui vou te mostrar **como instalar, configurar e autenticar usuários com Passport.js usando JWT, Google, Facebook e autenticação local com senha**. Bora lá! 🚀  

---

## 🚀 O que é o **Passport.js**?  
O **Passport.js** é um middleware de autenticação para **Node.js** que permite:  

✅ **Autenticação com senha (usuário e senha)**  
✅ **Autenticação via JWT (token)**  
✅ **Login com Google, Facebook, GitHub, etc. (OAuth2)**  
✅ **Totalmente modular** – você usa só o que precisa  
✅ **Seguro e flexível**  

Se sua aplicação tem **login de usuários**, **tokens JWT**, ou **OAuth**, o Passport é uma excelente escolha! 🔥  

---

## 📦 Instalando o Passport.js  
Para adicionar ao seu projeto, rode:  

```bash
npm install passport
```
ou, se estiver usando **yarn**:  
```bash
yarn add passport
```

Agora, vamos configurar um método de autenticação! 🎯  

---

# 🟢 **1. Autenticação Local (Usuário e Senha)**
Se você quer autenticar usuários com **usuário e senha**, precisa instalar o **passport-local**:  

```bash
npm install passport-local bcryptjs
```

O **passport-local** permite autenticar usuários com **email e senha**, e o **bcryptjs** ajuda a criptografar as senhas.

### 📌 Criando a estratégia de autenticação
```js
const passport = require('passport');
const LocalStrategy = require('passport-local').Strategy;
const bcrypt = require('bcryptjs');

// Simulação de um banco de dados
const usuarios = [
  { id: 1, email: 'teste@example.com', senha: bcrypt.hashSync('123456', 10) }
];

// Configuração da estratégia
passport.use(new LocalStrategy({ usernameField: 'email' }, async (email, senha, done) => {
  const usuario = usuarios.find(u => u.email === email);
  if (!usuario) return done(null, false, { message: 'Usuário não encontrado' });

  const senhaCorreta = await bcrypt.compare(senha, usuario.senha);
  if (!senhaCorreta) return done(null, false, { message: 'Senha incorreta' });

  return done(null, usuario);
}));

// Serialização do usuário
passport.serializeUser((usuario, done) => done(null, usuario.id));
passport.deserializeUser((id, done) => {
  const usuario = usuarios.find(u => u.id === id);
  done(null, usuario);
});
```

### 📌 Criando uma rota de **login**
Agora, aplicamos a estratégia no **Express**:  

```js
const express = require('express');
const session = require('express-session');

const app = express();

app.use(express.json());
app.use(session({ secret: 'chavesecreta', resave: false, saveUninitialized: false }));
app.use(passport.initialize());
app.use(passport.session());

app.post('/login', passport.authenticate('local', {
  successRedirect: '/dashboard',
  failureRedirect: '/login-fail'
}));

app.get('/dashboard', (req, res) => {
  if (!req.isAuthenticated()) return res.status(401).send('Acesso negado');
  res.send('Bem-vindo ao painel!');
});

app.listen(3000, () => console.log('🔥 Servidor rodando na porta 3000'));
```

Agora, podemos testar enviando um **POST /login** com:
```json
{
  "email": "teste@example.com",
  "senha": "123456"
}
```

Se tudo estiver certo, o usuário será autenticado e redirecionado para o **dashboard**! 🚀  

---

# 🔵 **2. Autenticação com JWT (Token)**
Se quiser autenticar usuários via **token JWT**, instale o **passport-jwt**:

```bash
npm install passport-jwt jsonwebtoken
```

### 📌 Configurando autenticação com JWT
```js
const passportJWT = require('passport-jwt');
const JWTStrategy = passportJWT.Strategy;
const ExtractJWT = passportJWT.ExtractJwt;
const jwt = require('jsonwebtoken');

const chaveSecreta = 'minhaChaveSecreta';

// Estratégia JWT
passport.use(new JWTStrategy({
  jwtFromRequest: ExtractJWT.fromAuthHeaderAsBearerToken(),
  secretOrKey: chaveSecreta
}, (jwtPayload, done) => {
  const usuario = usuarios.find(u => u.id === jwtPayload.id);
  return usuario ? done(null, usuario) : done(null, false);
}));

// Gerando um token JWT no login
app.post('/login-jwt', (req, res) => {
  const { email, senha } = req.body;
  const usuario = usuarios.find(u => u.email === email);
  
  if (!usuario || !bcrypt.compareSync(senha, usuario.senha)) {
    return res.status(401).send('Usuário ou senha incorretos');
  }

  const token = jwt.sign({ id: usuario.id, email: usuario.email }, chaveSecreta, { expiresIn: '1h' });
  res.json({ token });
});

// Rota protegida com JWT
app.get('/protegido', passport.authenticate('jwt', { session: false }), (req, res) => {
  res.send(`Bem-vindo, ${req.user.email}`);
});
```

Agora, ao fazer login via **POST /login-jwt**, você recebe um **token JWT**.  
Para acessar `/protegido`, basta enviar o **token no header**:  
```
Authorization: Bearer SEU_TOKEN
```

---

# 🔥 **3. Login com Google (OAuth)**
Se quiser login via **Google**, instale o **passport-google-oauth20**:

```bash
npm install passport-google-oauth20
```

### 📌 Configurando o Google OAuth2
Crie um app no [Google Developer Console](https://console.cloud.google.com/) e obtenha **Client ID** e **Client Secret**.

Agora, configure o Passport:

```js
const GoogleStrategy = require('passport-google-oauth20').Strategy;

passport.use(new GoogleStrategy({
  clientID: 'SEU_CLIENT_ID',
  clientSecret: 'SEU_CLIENT_SECRET',
  callbackURL: '/auth/google/callback'
}, (accessToken, refreshToken, profile, done) => {
  let usuario = usuarios.find(u => u.email === profile.emails[0].value);
  if (!usuario) {
    usuario = { id: usuarios.length + 1, email: profile.emails[0].value };
    usuarios.push(usuario);
  }
  return done(null, usuario);
}));

// Rota de login Google
app.get('/auth/google', passport.authenticate('google', { scope: ['profile', 'email'] }));

// Callback do Google
app.get('/auth/google/callback', passport.authenticate('google', { failureRedirect: '/' }),
  (req, res) => res.send('Login com Google bem-sucedido!')
);
```

Agora, ao acessar **/auth/google**, você é redirecionado para o login do Google e autenticado! 🔥  

---

# 🔐 **Protegendo Rotas**
Para proteger qualquer rota, basta adicionar `passport.authenticate()`.

### 🔹 Protegendo rota **com autenticação de sessão**:
```js
app.get('/dashboard', (req, res) => {
  if (!req.isAuthenticated()) return res.status(401).send('Acesso negado');
  res.send('Painel do usuário autenticado');
});
```

### 🔹 Protegendo rota **com JWT**:
```js
app.get('/api-secreta', passport.authenticate('jwt', { session: false }), (req, res) => {
  res.json({ message: 'Acesso autorizado!', user: req.user });
});
```

---

# 🏁 Conclusão  

O **Passport.js** facilita a autenticação em **Node.js**, oferecendo suporte para:  

✅ **Login com senha (LocalStrategy)**  
✅ **Autenticação via JWT (passport-jwt)**  
✅ **Login com Google, Facebook, GitHub e outros (OAuth)**  
✅ **Suporte a sessões e cookies**  
✅ **Segurança e modularidade**  

Se você precisa de **autenticação segura e escalável**, o **Passport.js** é uma das melhores opções! Agora é só testar no seu projeto! 🚀🔥