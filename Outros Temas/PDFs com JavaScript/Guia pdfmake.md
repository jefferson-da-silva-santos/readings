Aqui está um guia completo sobre o **pdfmake**, uma excelente biblioteca JavaScript para criar documentos PDF de maneira programática, com bastante flexibilidade e fácil integração.

---

# 📄 Guia Completo do `pdfmake`

O **pdfmake** é uma biblioteca JavaScript que permite criar documentos PDF diretamente no navegador ou no servidor. Com ela, você pode gerar PDFs de forma dinâmica e personalizar o conteúdo de acordo com suas necessidades. O `pdfmake` é altamente configurável e ideal para aplicações que precisam criar relatórios, faturas, contratos e outros tipos de documentos gerados pelo usuário.

## 🚀 O que é o `pdfmake`?

O `pdfmake` é uma biblioteca que permite criar documentos PDF utilizando JavaScript. Ele suporta textos, tabelas, imagens, gráficos e outros componentes dinâmicos em um formato de fácil personalização. Além disso, ele oferece uma API simples de usar, com recursos avançados como fontes personalizadas, layouts complexos e páginas múltiplas.

## 📦 Instalando o `pdfmake`

Para começar a usar o `pdfmake`, você pode instalar a biblioteca diretamente via npm ou baixá-la diretamente no seu projeto.

### Usando npm ou yarn:

```bash
npm install pdfmake --save
```

ou

```bash
yarn add pdfmake
```

### Usando diretamente em um arquivo HTML:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdfmake/0.1.70/pdfmake.min.js"></script>
```

Após a instalação, você pode começar a utilizar a biblioteca em seu código JavaScript.

## 🔨 Criando um Documento PDF Básico

O `pdfmake` permite criar um documento PDF a partir de um objeto JavaScript. O conteúdo do PDF é especificado em um formato de documento, que é basicamente um objeto com várias propriedades para definir o conteúdo, estilo e estrutura do PDF.

### Exemplo Básico de PDF

```javascript
const docDefinition = {
  content: [
    { text: 'Olá, Mundo!', fontSize: 20, alignment: 'center' }
  ]
};

// Gerando o PDF
pdfMake.createPdf(docDefinition).open();
```

No exemplo acima, estamos criando um PDF com o texto "Olá, Mundo!", com o tamanho da fonte 20 e alinhado ao centro.

## 🎨 Estilizando o Documento PDF

O `pdfmake` oferece um sistema de estilização muito flexível, onde você pode personalizar vários aspectos dos elementos, como fontes, tamanhos, cores, margens, etc.

### Exemplo de Estilos

```javascript
const docDefinition = {
  content: [
    { text: 'Texto em Negrito', fontSize: 20, bold: true },
    { text: 'Texto com Cor', fontSize: 16, color: 'blue' },
    { text: 'Texto com Margem', fontSize: 14, margin: [0, 10, 0, 10] }
  ]
};

// Gerando o PDF
pdfMake.createPdf(docDefinition).open();
```

Neste exemplo, temos três blocos de texto com diferentes estilos:
1. Texto em negrito.
2. Texto com cor azul.
3. Texto com uma margem superior e inferior de 10 unidades.

## 🖼 Adicionando Imagens

Você pode adicionar imagens ao seu PDF. O `pdfmake` suporta imagens tanto no formato base64 quanto em links para URLs externas.

### Exemplo de Adicionando Imagens

```javascript
const docDefinition = {
  content: [
    { text: 'Imagem no PDF', fontSize: 20, alignment: 'center' },
    { image: 'data:image/png;base64,iVBORw0K...', width: 150 }
  ]
};

// Gerando o PDF
pdfMake.createPdf(docDefinition).open();
```

Você pode inserir a imagem diretamente no formato base64 ou usar uma URL para uma imagem externa.

## 📊 Criando Tabelas

O `pdfmake` facilita a criação de tabelas com colunas e linhas, com suporte para estilos e alinhamento de texto.

### Exemplo de Tabela

```javascript
const docDefinition = {
  content: [
    { text: 'Tabela de Produtos', fontSize: 20, alignment: 'center' },
    {
      table: {
        body: [
          ['Produto', 'Preço'],
          ['Camiseta', 'R$ 50,00'],
          ['Calça', 'R$ 100,00']
        ]
      }
    }
  ]
};

// Gerando o PDF
pdfMake.createPdf(docDefinition).open();
```

No exemplo acima, estamos criando uma tabela simples com duas colunas: Produto e Preço. Você pode adicionar quantas linhas quiser e personalizar os estilos.

## 🖋 Fontes Personalizadas

Se você quiser usar fontes personalizadas em seu documento PDF, pode registrá-las e aplicá-las de maneira simples. Aqui está um exemplo de como fazer isso.

### Exemplo de Fontes Personalizadas

```javascript
pdfMake.fonts = {
  Roboto: {
    normal: 'fonts/Roboto-Regular.ttf',
    bold: 'fonts/Roboto-Bold.ttf',
    italics: 'fonts/Roboto-Italic.ttf',
    bolditalics: 'fonts/Roboto-BoldItalic.ttf'
  }
};

const docDefinition = {
  content: [
    { text: 'Texto com Fonte Personalizada', font: 'Roboto', fontSize: 20 }
  ]
};

// Gerando o PDF
pdfMake.createPdf(docDefinition).open();
```

Aqui, estamos registrando as fontes personalizadas e usando uma delas em nosso texto.

## 📄 Páginas Múltiplas

Você pode criar documentos com múltiplas páginas e definir o conteúdo de cada página separadamente. Basta adicionar novas entradas no array `content`.

### Exemplo de Páginas Múltiplas

```javascript
const docDefinition = {
  content: [
    { text: 'Página 1', fontSize: 20, pageBreak: 'after' },
    { text: 'Página 2', fontSize: 20 }
  ]
};

// Gerando o PDF
pdfMake.createPdf(docDefinition).open();
```

O `pageBreak: 'after'` indica que o conteúdo seguinte deve ir para a próxima página. Isso é útil quando você quer controlar onde cada nova página começa.

## 📏 Definindo o Layout do Documento

Você pode controlar o layout do documento, incluindo margens, tamanhos e orientações das páginas. O `pdfmake` também suporta diferentes tamanhos de página, como A4, carta, entre outros.

### Exemplo de Layout

```javascript
const docDefinition = {
  pageSize: 'A4',
  pageOrientation: 'landscape',
  content: [
    { text: 'Documento em Paisagem', fontSize: 20, alignment: 'center' }
  ]
};

// Gerando o PDF
pdfMake.createPdf(docDefinition).open();
```

Neste exemplo, a página foi configurada para ter o tamanho A4 e orientação paisagem (`landscape`).

## 🛠 Funcionalidades Avançadas

### 1. **Links e Marcadores**

Você pode adicionar links clicáveis e até mesmo links internos para diferentes seções do PDF.

```javascript
const docDefinition = {
  content: [
    { text: 'Clique aqui para visitar o Google', link: 'http://www.google.com' },
    { text: 'Ir para a próxima seção', link: '#nextSection' }
  ]
};

// Gerando o PDF
pdfMake.createPdf(docDefinition).open();
```

### 2. **Anexando PDFs Existentes**

Você pode anexar um PDF existente a outro, usando a função `pdfMake.addPDF()`.

### 3. **Proteção de Documento**

É possível proteger um documento PDF com senha. Aqui está um exemplo básico de como fazer isso.

```javascript
const docDefinition = {
  content: [
    { text: 'Este é um documento protegido por senha' }
  ],
  password: '12345'
};

pdfMake.createPdf(docDefinition).open();
```

### 4. **Gerar PDF no Servidor com Node.js**

O `pdfmake` pode ser usado também no lado do servidor com Node.js. Aqui está um exemplo de como você pode gerar o PDF diretamente no servidor e enviá-lo para o cliente.

```javascript
const fs = require('fs');
const pdfMake = require('pdfmake');

const docDefinition = {
  content: [
    { text: 'Relatório gerado no servidor', fontSize: 20 }
  ]
};

const pdfDoc = pdfMake.createPdf(docDefinition);

// Salvar o PDF no sistema de arquivos
pdfDoc.getBuffer(buffer => {
  fs.writeFileSync('relatorio.pdf', buffer);
});
```

Neste exemplo, estamos gerando e salvando um PDF no servidor.

## 🚀 Funcionalidades Avançadas do **pdfmake**

### 1. **Paginação e Cabeçalhos/Rodapés Dinâmicos**

Você pode adicionar cabeçalhos e rodapés que se repetem em todas as páginas ou até mesmo personalizar o conteúdo com base na página em que você está. Isso é útil, por exemplo, para relatórios e faturas, onde você precisa de um cabeçalho fixo com o título do documento ou uma numeração de página.

#### Exemplo de Cabeçalhos e Rodapés Dinâmicos

```javascript
const docDefinition = {
  content: [
    { text: 'Relatório Financeiro', fontSize: 18, bold: true }
  ],
  footer: (currentPage, pageCount, pageSize) => {
    return [
      {
        text: `Página ${currentPage} de ${pageCount}`,
        alignment: 'center',
        margin: [0, 0, 0, 20]
      }
    ];
  }
};

// Gerar o PDF
pdfMake.createPdf(docDefinition).open();
```

Neste exemplo, o rodapé é dinâmico e exibe o número da página atual em relação ao total de páginas.

### 2. **Quebra de Página Manual**

Se você quiser controlar a quebra de páginas em locais específicos, você pode usar a propriedade `pageBreak`. Por exemplo, você pode forçar uma quebra de página depois de um parágrafo ou em outro ponto específico.

#### Exemplo de Quebra de Página

```javascript
const docDefinition = {
  content: [
    { text: 'Introdução', fontSize: 18, bold: true },
    { text: 'Texto de introdução...', fontSize: 12 },
    { text: 'Parte 2: Detalhamento', fontSize: 18, bold: true, pageBreak: 'before' },
    { text: 'Texto de detalhamento...', fontSize: 12 }
  ]
};

// Gerar o PDF
pdfMake.createPdf(docDefinition).open();
```

A propriedade `pageBreak: 'before'` faz com que a seção "Parte 2: Detalhamento" inicie em uma nova página.

### 3. **Adicionando Links Internos ao PDF**

Você pode criar links internos que direcionam o usuário para diferentes seções dentro do mesmo PDF. Isso é útil para documentos interativos, como manuais ou relatórios que contêm várias seções.

#### Exemplo de Link Interno

```javascript
const docDefinition = {
  content: [
    { text: 'Ir para Seção 2', link: '#secao2' },
    { text: 'Texto da Seção 1', id: 'secao1' },
    { text: 'Texto da Seção 2', id: 'secao2' }
  ]
};

// Gerar o PDF
pdfMake.createPdf(docDefinition).open();
```

Neste exemplo, o texto "Ir para Seção 2" é um link que levará o usuário para o texto "Texto da Seção 2" dentro do mesmo PDF. O link está vinculado ao `id` da seção desejada.

### 4. **Gráficos no PDF com pdfmake**

Embora o `pdfmake` não tenha suporte direto para gráficos, você pode usar bibliotecas externas, como **chart.js**, para gerar gráficos e depois incorporar esses gráficos como imagens no seu PDF. Isso é muito útil quando você precisa gerar relatórios visuais com gráficos dinâmicos.

#### Exemplo de Gráfico com `chart.js` e pdfmake

1. **Gerar o gráfico com `chart.js`**:

```javascript
const ctx = document.getElementById('myChart').getContext('2d');
const chart = new Chart(ctx, {
  type: 'bar',
  data: {
    labels: ['A', 'B', 'C'],
    datasets: [{
      label: 'Exemplo de Dados',
      data: [10, 20, 30],
      backgroundColor: 'rgba(0, 123, 255, 0.5)'
    }]
  }
});
```

2. **Converter o gráfico para uma imagem (base64)**:

```javascript
const chartImage = chart.toBase64Image();
```

3. **Incorporar a imagem no PDF**:

```javascript
const docDefinition = {
  content: [
    { text: 'Gráfico de Vendas', fontSize: 18 },
    { image: chartImage, width: 500 }
  ]
};

// Gerar o PDF
pdfMake.createPdf(docDefinition).open();
```

Nesse exemplo, estamos criando um gráfico de barras com **chart.js** e inserindo o gráfico gerado diretamente no PDF.

### 5. **Assinaturas Digitais e Segurança**

Você pode adicionar uma camada extra de segurança ao seu documento, como proteger o PDF com uma senha. Embora o `pdfmake` não tenha suporte nativo para assinaturas digitais, você pode integrá-lo com outras bibliotecas de assinatura ou usar ferramentas externas para adicionar essas assinaturas posteriormente.

#### Exemplo de PDF Protegido por Senha

```javascript
const docDefinition = {
  content: [
    { text: 'Documento Protegido', fontSize: 18 }
  ],
  password: 'senhaSegura123'
};

// Gerar o PDF
pdfMake.createPdf(docDefinition).open();
```

Este PDF estará protegido por senha, e o usuário precisará fornecê-la para acessar o conteúdo.

## 🛠 Customização Avançada de Layout

### 1. **Usando o `stacked` para Layouts Verticais**

O `stacked` é um tipo de layout usado para empilhar os elementos de forma ordenada, útil quando você tem muitos componentes verticais (como textos ou imagens).

#### Exemplo de Layout Stacked

```javascript
const docDefinition = {
  content: [
    { text: 'Relatório de Vendas', fontSize: 18, bold: true },
    {
      stack: [
        { text: 'Produto 1', fontSize: 14 },
        { text: 'Preço: R$ 50,00', fontSize: 12 }
      ]
    },
    {
      stack: [
        { text: 'Produto 2', fontSize: 14 },
        { text: 'Preço: R$ 100,00', fontSize: 12 }
      ]
    }
  ]
};

// Gerar o PDF
pdfMake.createPdf(docDefinition).open();
```

Aqui, estamos utilizando o layout `stacked` para agrupar o nome e o preço de cada produto em uma pilha, o que ajuda a criar um layout mais organizado e legível.

### 2. **Alterando o Layout para Diferentes Dispositivos**

Se você precisar gerar PDFs responsivos ou adaptar o layout para diferentes tamanhos de página, o `pdfmake` permite ajustar facilmente os tamanhos de página, como A4, carta ou tamanho personalizado. Para relatórios que serão impressos, você pode precisar ajustar as margens, orientações e tamanhos para se adequar ao formato de papel.

#### Exemplo de Layout Responsivo

```javascript
const docDefinition = {
  pageSize: 'A4',
  pageOrientation: 'landscape',
  content: [
    { text: 'Relatório em Paisagem', fontSize: 18, alignment: 'center' }
  ]
};

// Gerar o PDF
pdfMake.createPdf(docDefinition).open();
```

### 3. **Suporte a Orientação Retrato/Paisagem**

Se seu conteúdo se encaixa melhor em uma orientação de paisagem ou retrato, você pode facilmente configurar isso no `pdfmake`.

```javascript
const docDefinition = {
  pageSize: 'A4',
  pageOrientation: 'portrait', // ou 'landscape' para paisagem
  content: [
    { text: 'Conteúdo em orientação retrato', fontSize: 18, alignment: 'center' }
  ]
};

// Gerar o PDF
pdfMake.createPdf(docDefinition).open();
```

## 🌍 Usos Avançados e Casos de Aplicação

- **Faturas e Recibos**: O `pdfmake` pode ser usado para criar faturas, recibos e outros documentos financeiros personalizados, incluindo campos dinâmicos como totais, impostos, descontos e itens de linha.
  
- **Relatórios de Dados**: Para relatórios que contêm tabelas dinâmicas, gráficos, e dados financeiros, o `pdfmake` permite inserir dados facilmente e também ajustar o layout conforme necessário.

- **Documentos de Marketing**: É possível criar apresentações em PDF, como catálogos e brochuras, com imagens e textos formatados.

- **Assinaturas e Documentos Jurídicos**: Embora não tenha suporte nativo para assinaturas digitais, o `pdfmake` pode ser combinado com outras bibliotecas para criar contratos e documentos legais.

---

## 🎉 Conclusão

O **pdfmake** é uma biblioteca poderosa e flexível para a criação de PDFs personalizados em JavaScript. Sua capacidade de lidar com layouts complexos, fontes personalizadas, tabelas dinâmicas e imagens, além de permitir a proteção do documento, faz dela uma excelente escolha para quem precisa gerar PDFs de maneira programática e personalizada.

Com todas essas funcionalidades, você pode criar PDFs interativos, seguros e totalmente sob seu controle. Aproveite essas dicas avançadas para criar documentos incríveis!