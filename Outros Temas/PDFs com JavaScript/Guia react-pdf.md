# 📄 Guia Completo do react-pdf

O `react-pdf` é uma biblioteca incrível que permite gerar documentos PDF diretamente em sua aplicação React, de forma simples e eficiente. Neste guia, vamos explorar tudo o que você precisa saber para começar a usar a biblioteca em seus projetos!

## 🚀 O que é o `react-pdf`?

O `react-pdf` é uma biblioteca que permite renderizar arquivos PDF diretamente no seu aplicativo React. Ele oferece uma maneira de criar e visualizar documentos PDF dentro do seu aplicativo, tornando a criação de relatórios, faturas, ou qualquer outro tipo de documento em PDF, uma tarefa bem simples.

A biblioteca fornece dois módulos principais:
- **react-pdf**: Usado para exibir PDFs dentro do seu aplicativo React.
- **@react-pdf/renderer**: Usado para gerar PDFs de forma dinâmica, utilizando componentes React para definir o layout.

## 📦 Instalando o `react-pdf`

Primeiro, instale o `react-pdf` usando o npm ou yarn:

```bash
npm install @react-pdf/renderer
```

ou

```bash
yarn add @react-pdf/renderer
```

Após a instalação, você já pode começar a utilizar a biblioteca no seu projeto React.

## 🔨 Como Usar o `@react-pdf/renderer` para Criar PDFs

O **@react-pdf/renderer** permite criar documentos PDF utilizando componentes React. Vamos ver um exemplo básico:

### Exemplo Básico de Geração de PDF:

```javascript
import React from 'react';
import ReactPDF from '@react-pdf/renderer';

// Definir o documento PDF
const MyDocument = () => (
  <ReactPDF.Document>
    <ReactPDF.Page size="A4">
      <ReactPDF.Text>Hello, World!</ReactPDF.Text>
    </ReactPDF.Page>
  </ReactPDF.Document>
);

// Gerar o PDF e salvar
ReactPDF.render(<MyDocument />, `${__dirname}/example.pdf`);
```

Nesse exemplo, criamos um documento simples com uma página de tamanho A4 e o texto "Hello, World!". O método `render()` é responsável por gerar e salvar o arquivo PDF.

### Customizando o Documento:

Você pode adicionar estilos aos seus componentes no PDF, como cores, fontes e tamanhos. Aqui está um exemplo mais avançado:

```javascript
import React from 'react';
import ReactPDF from '@react-pdf/renderer';

// Definir o documento PDF
const MyStyledDocument = () => (
  <ReactPDF.Document>
    <ReactPDF.Page size="A4">
      <ReactPDF.Text style={{ fontSize: 30, color: 'blue' }}>
        Hello, React-PDF!
      </ReactPDF.Text>
      <ReactPDF.Text style={{ fontSize: 20, marginTop: 20 }}>
        Este é um documento PDF gerado com React.
      </ReactPDF.Text>
    </ReactPDF.Page>
  </ReactPDF.Document>
);

// Gerar o PDF
ReactPDF.render(<MyStyledDocument />, `${__dirname}/styled-example.pdf`);
```

Neste exemplo, adicionamos estilos de texto, como o tamanho da fonte e a cor, para deixar o documento mais interessante.

## 🖨 Usando o `react-pdf` para Exibir PDFs no Browser

Além de gerar PDFs, o `react-pdf` também permite renderizar PDFs diretamente no seu aplicativo React, exibindo-os na interface do usuário. Aqui está um exemplo de como fazer isso:

### Exemplo de Exibição de PDF:

```javascript
import React, { useState } from 'react';
import { Document, Page } from 'react-pdf';

const MyPDFViewer = ({ file }) => {
  const [numPages, setNumPages] = useState(null);
  const [pageNumber, setPageNumber] = useState(1);

  // Função para lidar com o carregamento do documento
  function onDocumentLoadSuccess({ numPages }) {
    setNumPages(numPages);
  }

  return (
    <div>
      <Document
        file={file}
        onLoadSuccess={onDocumentLoadSuccess}
      >
        <Page pageNumber={pageNumber} />
      </Document>

      <div>
        Página {pageNumber} de {numPages}
      </div>

      <button
        disabled={pageNumber <= 1}
        onClick={() => setPageNumber(pageNumber - 1)}
      >
        Anterior
      </button>
      <button
        disabled={pageNumber >= numPages}
        onClick={() => setPageNumber(pageNumber + 1)}
      >
        Próxima
      </button>
    </div>
  );
};

export default MyPDFViewer;
```

### Passos no código:

1. **Document**: Representa o PDF. Você pode passar o caminho do arquivo como propriedade `file`.
2. **Page**: Cada página do PDF. A propriedade `pageNumber` define qual página será exibida.
3. **Controles de Navegação**: Botões para navegar entre as páginas do PDF.

Este exemplo permite ao usuário visualizar o PDF no navegador e navegar entre as páginas.

## 🌍 Personalizando a Renderização do PDF

A biblioteca oferece uma série de opções para personalizar a renderização do seu PDF, como layout de página, margens, tamanhos de fonte, fontes personalizadas, e até mesmo suporte para componentes complexos como tabelas e gráficos.

### Exemplo de Tabela no PDF:

```javascript
import React from 'react';
import ReactPDF from '@react-pdf/renderer';

const MyTableDocument = () => (
  <ReactPDF.Document>
    <ReactPDF.Page size="A4">
      <ReactPDF.View style={{ margin: 10 }}>
        <ReactPDF.Text style={{ fontSize: 18, marginBottom: 10 }}>
          Tabela de Exemplo
        </ReactPDF.Text>

        <ReactPDF.View style={{ flexDirection: 'row', marginBottom: 5 }}>
          <ReactPDF.Text style={{ width: '33%', fontWeight: 'bold' }}>Nome</ReactPDF.Text>
          <ReactPDF.Text style={{ width: '33%', fontWeight: 'bold' }}>Idade</ReactPDF.Text>
          <ReactPDF.Text style={{ width: '33%', fontWeight: 'bold' }}>Cidade</ReactPDF.Text>
        </ReactPDF.View>

        <ReactPDF.View style={{ flexDirection: 'row' }}>
          <ReactPDF.Text style={{ width: '33%' }}>Alice</ReactPDF.Text>
          <ReactPDF.Text style={{ width: '33%' }}>25</ReactPDF.Text>
          <ReactPDF.Text style={{ width: '33%' }}>São Paulo</ReactPDF.Text>
        </ReactPDF.View>

        <ReactPDF.View style={{ flexDirection: 'row' }}>
          <ReactPDF.Text style={{ width: '33%' }}>Bob</ReactPDF.Text>
          <ReactPDF.Text style={{ width: '33%' }}>30</ReactPDF.Text>
          <ReactPDF.Text style={{ width: '33%' }}>Rio de Janeiro</ReactPDF.Text>
        </ReactPDF.View>
      </ReactPDF.View>
    </ReactPDF.Page>
  </ReactPDF.Document>
);

ReactPDF.render(<MyTableDocument />, `${__dirname}/table-example.pdf`);
```

Neste exemplo, usamos a propriedade `flexDirection: 'row'` para criar uma tabela simples com nome, idade e cidade.

## 🌟 Conclusão

O `react-pdf` é uma ferramenta poderosa para trabalhar com PDFs no React, tanto para visualização quanto para geração dinâmica de documentos. Agora você tem um guia básico para começar a usar a biblioteca e criar PDFs no seu aplicativo. Desde simples textos até tabelas e gráficos complexos, as possibilidades são enormes!

Continue explorando e personalizando seus PDFs para melhorar ainda mais sua aplicação! 🎉

---

Esse guia fornece uma introdução abrangente ao `react-pdf`, com exemplos para começar a gerar e exibir PDFs diretamente no seu aplicativo React. O que acha?