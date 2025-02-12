# 📂 Tudo sobre **Multer** – Upload de Arquivos no Node.js com Express  

Se você já precisou lidar com **upload de arquivos no Node.js**, provavelmente já encontrou algumas dificuldades. O **Multer** é o **middleware mais usado para lidar com upload de arquivos no Express**, facilitando o **processamento de imagens, PDFs, vídeos e outros arquivos**.  

Aqui vou te mostrar **o que é, como instalar e como usar na prática**, incluindo **upload de múltiplos arquivos, armazenar em disco e até upload para serviços externos (como Amazon S3 e Cloudinary).**  

---

## 🚀 O que é o **Multer**?
O **Multer** é um middleware para **Express.js** que facilita o upload de arquivos, trabalhando com **multipart/form-data**, que é o tipo de dados usado nos formulários HTML para envio de arquivos.

✅ **Salva arquivos no servidor (ou na memória)**  
✅ **Suporte a múltiplos arquivos**  
✅ **Filtragem por tipo de arquivo**  
✅ **Definição de tamanho máximo**  
✅ **Integração fácil com armazenamento na nuvem (S3, Cloudinary, Firebase, etc.)**  

Se você precisa permitir que **usuários façam upload de imagens, PDFs ou qualquer outro tipo de arquivo**, o Multer é a escolha certa! 🔥  

---

## 📦 Instalando o Multer  

Para adicionar ao seu projeto Express, rode:  

```bash
npm install multer
```
ou, se estiver usando **yarn**:  
```bash
yarn add multer
```

Agora, bora configurar o upload de arquivos! 🚀  

---

## 📂 Criando um **upload básico** (Armazenamento local)  

Primeiro, criamos um **configurador do Multer** para salvar os arquivos na pasta `uploads/`:

```js
const express = require('express');
const multer = require('multer');
const path = require('path');

const app = express();

// Configuração do armazenamento
const storage = multer.diskStorage({
  destination: (req, file, cb) => {
    cb(null, 'uploads/'); // Define a pasta onde os arquivos serão salvos
  },
  filename: (req, file, cb) => {
    cb(null, Date.now() + path.extname(file.originalname)); // Renomeia o arquivo
  }
});

// Criando um middleware Multer
const upload = multer({ storage });

// Criando uma rota para upload
app.post('/upload', upload.single('arquivo'), (req, res) => {
  res.json({ message: '✅ Upload feito com sucesso!', file: req.file });
});

app.listen(3000, () => console.log('🔥 Servidor rodando na porta 3000'));
```

Agora, podemos testar o upload enviando um arquivo via **Postman** ou um formulário HTML.  

---

## 🟢 Enviando arquivos via formulário HTML  
Podemos testar a API com um formulário básico:  

```html
<form action="http://localhost:3000/upload" method="POST" enctype="multipart/form-data">
  <input type="file" name="arquivo">
  <button type="submit">Enviar</button>
</form>
```

Isso envia o arquivo para a **rota `/upload`**, e o Multer salva na pasta `uploads/`.  

---

## 📂 Upload de **múltiplos arquivos**  

Se precisar permitir o **upload de vários arquivos de uma vez**, podemos usar `upload.array()`:

```js
app.post('/uploads', upload.array('arquivos', 5), (req, res) => {
  res.json({ message: '✅ Upload feito com sucesso!', files: req.files });
});
```

Agora podemos enviar **até 5 arquivos de uma vez**, e o Multer vai salvá-los na pasta `uploads/`.  

---

## ⚠️ Limitando tamanho e tipo de arquivo  

Podemos impedir que arquivos **muito grandes** ou com formatos errados sejam enviados.

### 🔹 Definir tamanho máximo do arquivo  
```js
const upload = multer({
  storage,
  limits: { fileSize: 2 * 1024 * 1024 } // 2MB
});
```

Se um arquivo for maior que 2MB, o Multer **bloqueia o upload automaticamente**.  

---

### 🔹 Restringindo o tipo de arquivo  
Podemos criar um **filtro de tipo de arquivo**, permitindo **apenas imagens (PNG, JPG, JPEG)**:

```js
const upload = multer({
  storage,
  fileFilter: (req, file, cb) => {
    const tiposPermitidos = /jpeg|jpg|png/;
    const extname = tiposPermitidos.test(path.extname(file.originalname).toLowerCase());
    const mimetype = tiposPermitidos.test(file.mimetype);

    if (extname && mimetype) {
      return cb(null, true);
    } else {
      return cb(new Error('⛔ Apenas imagens PNG, JPG e JPEG são permitidas!'));
    }
  }
});
```

Agora, **arquivos de tipos diferentes (como PDFs, vídeos, etc.) serão bloqueados!** 🚫  

---

## 📦 Upload de Arquivo **na Memória** (Sem salvar no Disco)  
Se não quiser salvar no **servidor** e precisar processar os arquivos diretamente na **memória RAM** (por exemplo, para enviar para o **Cloudinary ou Amazon S3**), usamos `storage: multer.memoryStorage()`:

```js
const upload = multer({ storage: multer.memoryStorage() });

app.post('/upload-memoria', upload.single('arquivo'), (req, res) => {
  console.log(req.file.buffer); // Buffer do arquivo na memória
  res.json({ message: '✅ Upload feito na memória!' });
});
```

Isso faz com que os arquivos **não sejam salvos no disco**, mas fiquem disponíveis em `req.file.buffer`.  

---

## ☁️ Enviando arquivos para o **Amazon S3**
Se quiser salvar **diretamente na nuvem (S3, Cloudinary, Firebase, etc.)**, basta usar `multer.memoryStorage()` e depois enviar o arquivo para o serviço desejado.

Exemplo com **AWS S3**:

```js
const AWS = require('aws-sdk');
const multer = require('multer');
const s3 = new AWS.S3();

const upload = multer({ storage: multer.memoryStorage() });

app.post('/upload-s3', upload.single('arquivo'), async (req, res) => {
  const params = {
    Bucket: 'seu-bucket-s3',
    Key: `uploads/${Date.now()}_${req.file.originalname}`,
    Body: req.file.buffer,
    ContentType: req.file.mimetype
  };

  const uploadS3 = await s3.upload(params).promise();
  res.json({ message: '✅ Upload feito no S3!', url: uploadS3.Location });
});
```

Agora, os arquivos são enviados **diretamente para o S3**, sem passar pelo disco do servidor. 🚀  

---

## 🛠 Configuração Completa do Multer  

Aqui está um **setup completo** com:  
✅ **Armazenamento local e memória**  
✅ **Upload único e múltiplo**  
✅ **Limite de tamanho e tipos permitidos**  
✅ **Upload para S3 (pode adaptar para Cloudinary ou Firebase)**  

```js
const express = require('express');
const multer = require('multer');
const AWS = require('aws-sdk');
const path = require('path');

const app = express();
const s3 = new AWS.S3();

// Configuração do Multer
const storage = multer.diskStorage({
  destination: 'uploads/',
  filename: (req, file, cb) => {
    cb(null, Date.now() + path.extname(file.originalname));
  }
});

const fileFilter = (req, file, cb) => {
  const tiposPermitidos = /jpeg|jpg|png/;
  const extname = tiposPermitidos.test(path.extname(file.originalname).toLowerCase());
  const mimetype = tiposPermitidos.test(file.mimetype);

  if (extname && mimetype) {
    return cb(null, true);
  } else {
    return cb(new Error('⛔ Apenas imagens PNG, JPG e JPEG são permitidas!'));
  }
};

const upload = multer({
  storage,
  limits: { fileSize: 2 * 1024 * 1024 }, // 2MB
  fileFilter
});

app.post('/upload', upload.single('arquivo'), (req, res) => {
  res.json({ message: '✅ Upload feito com sucesso!', file: req.file });
});

app.listen(3000, () => console.log('🔥 Servidor rodando na porta 3000'));
```

---

## 🏁 Conclusão  
O **Multer** é a melhor opção para **upload de arquivos no Express**, seja **armazenando localmente** ou enviando para **nuvem (AWS S3, Cloudinary, Firebase, etc.)**.

### 🎯 Resumo:  
✅ **Fácil de usar com Express**  
✅ **Armazena localmente ou na memória**  
✅ **Filtra tipos de arquivos**  
✅ **Define limite de tamanho**  
✅ **Compatível com S3, Cloudinary, Firebase**  

Agora é só testar no seu projeto! 🚀🔥