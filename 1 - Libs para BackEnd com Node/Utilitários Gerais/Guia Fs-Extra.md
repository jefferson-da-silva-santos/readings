# 📂 Tudo sobre **fs-extra** – Manipulação Avançada de Arquivos no Node.js  

Se você já trabalhou com arquivos no **Node.js**, provavelmente usou o módulo **fs (File System)**. Mas ele tem algumas limitações, como **falta de suporte nativo para promises** e **operações mais avançadas**.  

É aí que entra o **fs-extra**! 🚀  

Com o **fs-extra**, você consegue **copiar, mover, apagar, ler e criar arquivos e diretórios** de maneira **mais simples, poderosa e assíncrona**.  

---

## 🚀 O que é o **fs-extra**?  
O **fs-extra** é um wrapper do módulo `fs` do Node.js, trazendo **funções adicionais e suporte a Promises**. Ele oferece:  

✅ **Suporte a Promises/async-await** (sem precisar de `fs.promises`)  
✅ **Copiar, mover e remover arquivos/pastas facilmente**  
✅ **Garantia de que diretórios serão criados antes de gravar arquivos**  
✅ **Leitura e escrita JSON de forma simplificada**  
✅ **Melhor performance em operações de arquivos**  

Agora, bora instalar e usar na prática! 🔥  

---

## 📦 Instalando o fs-extra  
Para instalar no seu projeto, rode:  

```bash
npm install fs-extra
```
ou, se estiver usando **yarn**:  
```bash
yarn add fs-extra
```

Depois, importe no seu código:

```js
const fs = require('fs-extra');
```

Agora podemos começar a manipular arquivos! 📂  

---

## 📄 Criando e Escrevendo em Arquivos  

### 📌 Criando e escrevendo em um arquivo de forma assíncrona  
```js
const fs = require('fs-extra');

(async () => {
  try {
    await fs.outputFile('dados.txt', 'Olá, este é um arquivo criado com fs-extra!');
    console.log('✅ Arquivo criado com sucesso!');
  } catch (error) {
    console.error('❌ Erro ao criar arquivo:', error);
  }
})();
```

Se a pasta **não existir**, o `fs.outputFile` **cria ela automaticamente** antes de escrever o arquivo! 😍  

---

## 📖 Lendo Arquivos  

### 📌 Lendo o conteúdo de um arquivo  
```js
(async () => {
  try {
    const conteudo = await fs.readFile('dados.txt', 'utf-8');
    console.log('📄 Conteúdo do arquivo:', conteudo);
  } catch (error) {
    console.error('❌ Erro ao ler arquivo:', error);
  }
})();
```

Isso lê o conteúdo do `dados.txt` e imprime no console.

---

## ✏️ Atualizando um Arquivo  

### 📌 Adicionando conteúdo sem sobrescrever  
Se quiser **adicionar** conteúdo em vez de sobrescrever, use `appendFile`:  

```js
(async () => {
  try {
    await fs.appendFile('dados.txt', '\nNova linha adicionada!');
    console.log('✅ Linha adicionada com sucesso!');
  } catch (error) {
    console.error('❌ Erro ao atualizar arquivo:', error);
  }
})();
```

Agora, `dados.txt` terá **o novo texto adicionado ao final**.  

---

## 🚀 Criando e Removendo Diretórios  

### 📌 Criando uma pasta  
Se precisar criar uma pasta, use `ensureDir`:  

```js
(async () => {
  try {
    await fs.ensureDir('meuDiretorio');
    console.log('✅ Diretório criado com sucesso!');
  } catch (error) {
    console.error('❌ Erro ao criar diretório:', error);
  }
})();
```

Se a pasta já existir, **ele não gera erro**.  

### 📌 Removendo uma pasta e todos os arquivos dentro dela  
Se quiser remover um diretório inteiro (mesmo se ele contiver arquivos), use `remove`:  

```js
(async () => {
  try {
    await fs.remove('meuDiretorio');
    console.log('✅ Diretório removido!');
  } catch (error) {
    console.error('❌ Erro ao remover diretório:', error);
  }
})();
```

Isso apaga a pasta e **tudo dentro dela**! ⚠️  

---

## 📂 Copiando e Movendo Arquivos/Pastas  

### 📌 Copiando um arquivo  
```js
(async () => {
  try {
    await fs.copy('dados.txt', 'copia.txt');
    console.log('✅ Arquivo copiado com sucesso!');
  } catch (error) {
    console.error('❌ Erro ao copiar arquivo:', error);
  }
})();
```

Isso copia `dados.txt` para `copia.txt`.  

### 📌 Copiando uma pasta inteira  
```js
(async () => {
  try {
    await fs.copy('minhaPasta', 'backupPasta');
    console.log('✅ Pasta copiada com sucesso!');
  } catch (error) {
    console.error('❌ Erro ao copiar pasta:', error);
  }
})();
```

Isso cria um **backup da pasta**, mantendo toda a estrutura de arquivos dentro dela! 😎  

### 📌 Movendo um arquivo  
Se quiser **mover um arquivo** para outra pasta:  

```js
(async () => {
  try {
    await fs.move('dados.txt', 'backup/dados.txt');
    console.log('✅ Arquivo movido com sucesso!');
  } catch (error) {
    console.error('❌ Erro ao mover arquivo:', error);
  }
})();
```

Isso move `dados.txt` para a pasta `backup/`.  

---

## 🔍 Verificando se um arquivo ou diretório existe  

Antes de tentar ler um arquivo, é bom verificar se ele existe:  

```js
(async () => {
  const existe = await fs.pathExists('dados.txt');

  if (existe) {
    console.log('✅ O arquivo existe!');
  } else {
    console.log('❌ O arquivo não existe.');
  }
})();
```

Isso evita **erros ao tentar acessar arquivos inexistentes**! 💡  

---

## 📜 Manipulando JSON com Facilidade  

O **fs-extra** facilita **ler e gravar arquivos JSON** sem precisar fazer `JSON.stringify` ou `JSON.parse` manualmente.  

### 📌 Criando um arquivo JSON  
```js
(async () => {
  const dados = { nome: 'João', idade: 25 };

  try {
    await fs.writeJson('dados.json', dados);
    console.log('✅ Arquivo JSON criado!');
  } catch (error) {
    console.error('❌ Erro ao criar JSON:', error);
  }
})();
```

### 📌 Lendo um JSON  
```js
(async () => {
  try {
    const dados = await fs.readJson('dados.json');
    console.log('📄 Dados do JSON:', dados);
  } catch (error) {
    console.error('❌ Erro ao ler JSON:', error);
  }
})();
```

Agora você pode manipular **arquivos JSON como objetos JavaScript** facilmente! 🎯  

---

## 🏁 Conclusão  

O **fs-extra** é um pacote **indispensável** para quem trabalha com arquivos e diretórios no **Node.js**. Ele simplifica a manipulação de arquivos e traz **recursos extras que o `fs` nativo não tem**.  

### 🎯 Resumo:  
✅ **Criação e leitura de arquivos com Promises/async-await**  
✅ **Criação e remoção automática de diretórios**  
✅ **Copiar, mover e excluir arquivos/pastas**  
✅ **Verificação de existência de arquivos**  
✅ **Manipulação fácil de JSON**  

Se você precisa **trabalhar com arquivos e pastas no Node.js**, o **fs-extra** é a solução perfeita! 🚀🔥