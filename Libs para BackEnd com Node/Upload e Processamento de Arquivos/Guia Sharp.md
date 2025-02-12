# 🖼️ Tudo sobre **Sharp** – Manipulação de Imagens no Node.js  

Se você já precisou **redimensionar, converter, otimizar ou processar imagens** no **Node.js**, provavelmente já ouviu falar do **Sharp**. Ele é um dos pacotes mais rápidos e eficientes para manipulação de imagens, ideal para trabalhar com **uploads**, **redimensionamento automático**, **compressão** e muito mais.  

---

## 🚀 O que é o **Sharp**?
O **Sharp** é uma biblioteca para **manipulação de imagens em alta performance** no Node.js. Ele usa a **libvips** como backend, o que o torna **muito mais rápido e eficiente** do que soluções baseadas em ImageMagick ou GraphicsMagick.  

🔹 **Por que usar o Sharp?**  

✅ **Performance absurda** – Muito mais rápido que `Jimp`, `gm` e `ImageMagick`  
✅ **Baixo consumo de memória** – Perfeito para manipulação de imagens em larga escala  
✅ **Redimensionamento eficiente** – Ajuste imagens para web sem perda de qualidade  
✅ **Conversão de formatos** – JPEG, PNG, WebP, AVIF, TIFF e GIF  
✅ **Compressão avançada** – Reduza o tamanho das imagens sem perder qualidade  
✅ **Suporte a streams** – Manipulação eficiente sem salvar arquivos temporários  

Se você precisa **processar imagens em APIs, sistemas de upload ou geração de thumbnails**, o Sharp é uma das melhores opções! 🔥  

---

## 📦 Instalando o **Sharp**
Para instalar no seu projeto Node.js, rode:

```bash
npm install sharp
```
ou, se estiver usando **yarn**:  
```bash
yarn add sharp
```

Agora já podemos começar a brincar com imagens. 🚀  

---

## 📸 Redimensionando uma imagem  

O **Sharp** permite redimensionar imagens de forma super fácil.  

```js
const sharp = require('sharp');

sharp('imagem-original.jpg')
  .resize(300, 200) // Largura: 300px, Altura: 200px
  .toFile('imagem-redimensionada.jpg', (err, info) => {
    if (err) console.error('Erro ao redimensionar:', err);
    else console.log('Imagem redimensionada:', info);
  });
```

Isso cria um novo arquivo `imagem-redimensionada.jpg` com 300x200 pixels.

---

## 🔄 Convertendo formatos de imagem  

### 📌 Converter **JPEG → PNG**
```js
sharp('foto.jpg')
  .toFormat('png')
  .toFile('foto.png');
```

### 📌 Converter **PNG → WebP** (Formato otimizado para web)
```js
sharp('foto.png')
  .toFormat('webp')
  .toFile('foto.webp');
```

### 📌 Converter para **AVIF** (formato ultra eficiente)
```js
sharp('foto.jpg')
  .toFormat('avif')
  .toFile('foto.avif');
```

Isso reduz o tamanho da imagem sem perder muita qualidade. 🔥  

---

## 🎯 Melhorando a compressão de imagens  

Se quiser **reduzir o tamanho da imagem sem perder qualidade**, use `jpeg()` ou `png()` com configurações avançadas:

```js
sharp('foto.jpg')
  .resize(800) // Redimensiona mantendo a proporção
  .jpeg({ quality: 80 }) // Reduz a qualidade para 80%
  .toFile('foto-comprimida.jpg');
```

🔹 Para **PNG**:
```js
sharp('imagem.png')
  .png({ compressionLevel: 9 }) // Compressão máxima
  .toFile('imagem-otimizada.png');
```

🔹 Para **WebP**:
```js
sharp('imagem.png')
  .webp({ quality: 75 }) // Qualidade entre 0 e 100
  .toFile('imagem-otimizada.webp');
```

Isso reduz drasticamente o tamanho dos arquivos, ideal para **sites e APIs**!

---

## ✂️ Cortando (Crop) uma imagem  

Se precisar **cortar uma parte específica** da imagem, use `extract()`:

```js
sharp('foto.jpg')
  .extract({ width: 200, height: 200, left: 100, top: 50 }) // Corta uma área específica
  .toFile('foto-cortada.jpg');
```

Aqui, estamos pegando uma **área de 200x200 pixels** começando do ponto `(100, 50)`.

---

## 🎨 Aplicando Efeitos e Filtros  

O Sharp também permite **aplicar efeitos**, como **borrão, tons de cinza, etc.**  

### 📌 Aplicando **tons de cinza**:
```js
sharp('foto.jpg')
  .greyscale()
  .toFile('foto-preto-branco.jpg');
```

### 📌 Aplicando **borrão (blur)**:
```js
sharp('foto.jpg')
  .blur(5) // Quanto maior o número, mais borrado
  .toFile('foto-borrada.jpg');
```

### 📌 Ajustando **brilho e contraste**:
```js
sharp('foto.jpg')
  .modulate({ brightness: 1.2, saturation: 1.5 }) // 1.2x mais brilho, 1.5x mais saturação
  .toFile('foto-ajustada.jpg');
```

Isso é ótimo para **preparar imagens automaticamente** antes de exibir para os usuários.

---

## ⚡ Trabalhando com **Streams** para melhor performance  

Se estiver manipulando imagens dentro de um **servidor Node.js**, usar **streams** é muito mais eficiente do que salvar arquivos temporários.

```js
const express = require('express');
const sharp = require('sharp');
const fs = require('fs');

const app = express();

app.get('/imagem', (req, res) => {
  fs.createReadStream('foto.jpg')
    .pipe(sharp().resize(300, 300).jpeg({ quality: 80 }))
    .pipe(res);
});

app.listen(3000, () => console.log('🔥 Servidor rodando na porta 3000'));
```

Agora, quando acessar `http://localhost:3000/imagem`, o servidor **processa a imagem em tempo real e envia para o navegador** sem precisar salvar no disco!

---

## 📂 Integração com **Multer (Upload de Imagens)**  

Se sua aplicação precisa **receber imagens enviadas pelo usuário**, podemos integrar com o **Multer** para processar e salvar as imagens corretamente.

```js
const express = require('express');
const multer = require('multer');
const sharp = require('sharp');
const fs = require('fs');
const path = require('path');

const app = express();
const upload = multer({ dest: 'uploads/' });

app.post('/upload', upload.single('imagem'), async (req, res) => {
  const { path: caminhoTemp } = req.file;
  const novoCaminho = `uploads/${Date.now()}-processado.jpg`;

  await sharp(caminhoTemp)
    .resize(500, 500)
    .jpeg({ quality: 80 })
    .toFile(novoCaminho);

  fs.unlinkSync(caminhoTemp); // Remove o arquivo temporário

  res.json({ message: 'Imagem processada!', url: novoCaminho });
});

app.listen(3000, () => console.log('🚀 API rodando na porta 3000'));
```

Agora, se o usuário enviar uma imagem, o servidor:
1. **Recebe o upload** com Multer  
2. **Redimensiona e comprime com Sharp**  
3. **Salva a versão processada**  
4. **Retorna a URL da nova imagem**  

Perfeito para **upload de avatares, fotos de produtos e mais!** 🎯

---

## 🏁 Conclusão  

O **Sharp** é a melhor escolha para **manipulação de imagens no Node.js**, oferecendo **performance absurda e baixa utilização de memória**. 🚀  

### 🎯 O que aprendemos:
✅ **Redimensionar imagens automaticamente**  
✅ **Converter formatos (JPEG, PNG, WebP, AVIF)**  
✅ **Otimizar imagens para web (compressão avançada)**  
✅ **Aplicar filtros, cortes e ajustes de cores**  
✅ **Trabalhar com streams para performance máxima**  
✅ **Integrar com Multer para uploads processados**  

Se sua aplicação precisa manipular imagens de qualquer forma, **o Sharp é a escolha perfeita!** 🔥