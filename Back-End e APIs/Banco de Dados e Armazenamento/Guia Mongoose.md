# 🍃 Tudo sobre **Mongoose** – Trabalhando com MongoDB no Node.js

Se você está mexendo com **MongoDB** no **Node.js**, a melhor forma de interagir com o banco é usando o **Mongoose**. Ele é um **ODM (Object Data Modeling)** que facilita a criação de modelos, validações e interações com o MongoDB de forma estruturada.

Aqui, vou te mostrar **o que é, como instalar e usar o Mongoose no Node.js, desde a conexão até consultas complexas**.

---

## 🚀 O que é o Mongoose?
O **Mongoose** é um ODM para MongoDB no Node.js, que traz uma **camada de abstração sobre o banco**, permitindo:

✅ Criar **modelos de dados** com schemas  
✅ **Validação automática** de dados  
✅ Suporte a **relacionamentos** entre documentos  
✅ Métodos customizados para consultas mais fáceis  
✅ Uso de **middlewares** (hooks antes de salvar/deletar/etc.)  
✅ Melhor organização do código  

Ele transforma os **documentos JSON do MongoDB em objetos JavaScript manipuláveis**.

---

## 📦 Instalando o Mongoose
Primeiro, você precisa instalar o **Mongoose** no seu projeto:

```bash
npm install mongoose
```

Agora, bora conectar com o banco.

---

## 🔌 Conectando ao MongoDB
Antes de tudo, precisamos configurar a conexão. Se estiver rodando **MongoDB localmente**, a URL geralmente é:

```js
const mongoose = require('mongoose');

mongoose.connect('mongodb://localhost:27017/meubanco', {
  useNewUrlParser: true,
  useUnifiedTopology: true
})
  .then(() => console.log('🔥 Conectado ao MongoDB!'))
  .catch(err => console.error('❌ Erro ao conectar ao MongoDB:', err));
```

Se estiver usando o **MongoDB Atlas (nuvem)**, pegue a **URI de conexão** no painel do MongoDB e use assim:

```js
mongoose.connect('mongodb+srv://usuario:senha@cluster.mongodb.net/meubanco', {
  useNewUrlParser: true,
  useUnifiedTopology: true
});
```

---

## 📌 Criando um Schema e Model
No Mongoose, cada **coleção (tabela no Mongo)** é representada por um **Schema** e um **Model**.

Vamos criar um **schema de Usuário**:

```js
const { Schema, model } = mongoose;

const usuarioSchema = new Schema({
  nome: { type: String, required: true },
  email: { type: String, required: true, unique: true },
  idade: { type: Number, min: 0 }
}, { timestamps: true }); // Adiciona createdAt e updatedAt

const Usuario = model('Usuario', usuarioSchema);
```

Agora temos um **model** chamado `Usuario`, que representa a coleção **usuarios** no MongoDB.

---

## 🟢 Criando um usuário (Inserir documento)
Agora, bora salvar um usuário no banco:

```js
(async () => {
  try {
    const novoUsuario = await Usuario.create({
      nome: 'John Doe',
      email: 'johndoe@example.com',
      idade: 30
    });

    console.log('Usuário criado:', novoUsuario);
  } catch (error) {
    console.error('Erro ao criar usuário:', error);
  }
})();
```

Isso insere um novo documento na coleção **usuarios**.

---

## 🔵 Buscando usuários no banco (Read)
### Buscar **todos os usuários**:
```js
(async () => {
  const usuarios = await Usuario.find();
  console.log(usuarios);
})();
```

### Buscar **um usuário pelo email**:
```js
(async () => {
  const usuario = await Usuario.findOne({ email: 'johndoe@example.com' });
  console.log(usuario);
})();
```

### Buscar **pelo ID**:
```js
(async () => {
  const usuario = await Usuario.findById('65abcd1234ef567890123456');
  console.log(usuario);
})();
```

---

## 🟡 Atualizando um usuário
Para **alterar um usuário**, usamos `findByIdAndUpdate` ou `updateOne`:

```js
(async () => {
  await Usuario.findByIdAndUpdate('65abcd1234ef567890123456', { idade: 31 });
  console.log('Usuário atualizado!');
})();
```

Se quiser **retornar o usuário atualizado**, passe `{ new: true }`:

```js
(async () => {
  const usuarioAtualizado = await Usuario.findOneAndUpdate(
    { email: 'johndoe@example.com' },
    { idade: 35 },
    { new: true }
  );

  console.log('Usuário atualizado:', usuarioAtualizado);
})();
```

---

## 🔴 Deletando um usuário
Para excluir um usuário, usamos `deleteOne` ou `findByIdAndDelete`:

```js
(async () => {
  await Usuario.deleteOne({ email: 'johndoe@example.com' });
  console.log('Usuário deletado!');
})();
```

Ou por ID:

```js
(async () => {
  await Usuario.findByIdAndDelete('65abcd1234ef567890123456');
  console.log('Usuário removido!');
})();
```

---

## 🔗 Relacionamentos no Mongoose
O Mongoose permite **relacionar coleções** com **referências** (`ref`).

### 🏠 Criando um Schema de **Post** (Relacionamento 1:N)
Vamos criar um modelo de **Post**, onde **um usuário pode ter vários posts**:

```js
const postSchema = new Schema({
  titulo: { type: String, required: true },
  conteudo: { type: String, required: true },
  usuario: { type: Schema.Types.ObjectId, ref: 'Usuario', required: true }
});

const Post = model('Post', postSchema);
```

Agora, quando criamos um **post**, referenciamos o ID do usuário:

```js
(async () => {
  const usuario = await Usuario.findOne({ email: 'johndoe@example.com' });

  if (usuario) {
    const novoPost = await Post.create({
      titulo: 'Meu primeiro post',
      conteudo: 'Este é um post de teste no Mongoose!',
      usuario: usuario._id
    });

    console.log('Post criado:', novoPost);
  }
})();
```

E para buscar um usuário **com seus posts**:

```js
(async () => {
  const usuarioComPosts = await Usuario.findOne({ email: 'johndoe@example.com' }).populate('posts');
  console.log(usuarioComPosts);
})();
```

---

## 🎯 Validando dados automaticamente
O Mongoose permite validar os dados **antes de salvar**.

Exemplo de validações no **Schema de Usuário**:

```js
const usuarioSchema = new Schema({
  nome: { type: String, required: true, minlength: 3 },
  email: { type: String, required: true, unique: true, match: /.+\@.+\..+/ },
  idade: { type: Number, min: 18, max: 120 }
});
```

Agora, se tentar salvar um usuário com **idade abaixo de 18**, ele não será salvo.

---

## 🛠️ Middlewares (Hooks)
Os **hooks** permitem executar código antes ou depois de salvar, atualizar ou deletar um documento.

Exemplo de **middleware `pre`** (antes de salvar):

```js
usuarioSchema.pre('save', function (next) {
  console.log(`Usuário ${this.nome} será salvo no banco.`);
  next();
});
```

Agora, **toda vez que um usuário for salvo**, essa função será executada antes.

---

## 🏁 Conclusão
O **Mongoose** facilita a manipulação do **MongoDB** no Node.js, trazendo estrutura e validações para os dados.

### 🎯 Resumo:
✅ **Conexão com o MongoDB**  
✅ **Schemas e Models para estruturar os dados**  
✅ **CRUD (Criar, Buscar, Atualizar, Deletar)**  
✅ **Relacionamentos entre coleções**  
✅ **Validações automáticas**  
✅ **Hooks e middlewares para lógica antes/depois das operações**  

Agora é só testar e aplicar no seu projeto! 🚀🔥