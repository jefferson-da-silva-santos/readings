# **Parte 1: A Consultinha SQL - "Buscando os itens com LIMIT e OFFSET"** 🕵️‍♂️

Aqui começamos pelo backend, onde você está fazendo a consulta no banco de dados. Vamos entender o que está acontecendo:

```javascript
// O método recebe dois parâmetros: página e limite
static async getPagination(page = 1, limit = 10) {
  try {
    logger.info('Iniciando a busca de itens no banco de dados');
  
    // Cálculo do OFFSET
    const offset = (page - 1) * limit;
  
    // Buscar os itens com LIMIT e OFFSET
    const items = await ItemModel.findAll({
      where: { excluido: 0 }, // Traz apenas os itens não excluídos
      limit: limit,           // Limite de itens por página
      offset: offset,         // Pular os itens de acordo com a página atual
    });
  
    // Buscar o total de itens
    const totalItems = await ItemModel.count({
      where: { excluido: 0 },
    });
  
    // Calcular o número total de páginas
    const totalPages = Math.ceil(totalItems / limit);
  
    logger.info('Itens encontrados com sucesso', { itemCount: items.length, totalItems, totalPages });
  
    return {
      items: this.parseObject(items),  // Aqui você formata os itens antes de enviar
      totalItems,                      // Total de itens no banco
      totalPages,                      // Total de páginas
      currentPage: page,               // Página atual
    };
  } catch (error) {
    logger.error('Erro ao buscar itens', { error: error.message, stack: error.stack });
    throw new Error("Erro ao buscar itens");
  }
}
```

### Como funciona o que você fez aqui?

1. **Cálculo do OFFSET:** 🚀
   - `const offset = (page - 1) * limit;`
   - O `OFFSET` é o "pulo" que você dá nos itens para pegar a parte certa da lista. Se você está na **página 1**, começa do item 0, na **página 2** pula os primeiros 10, e assim por diante.
   - **Fórmula mágica:** (página - 1) * limite = OFFSET. Ou seja, se você estiver na página 3, será `(3 - 1) * 10 = 20`. Então ele vai pular os primeiros 20 itens.

2. **Buscando os Itens:** 🏃‍♂️
   - Com o `limit` e o `offset` definidos, você busca exatamente o número de itens que precisa para a página atual.
   - **LIMIT**: Quantos itens? (por exemplo, 10)
   - **OFFSET**: Onde começar a busca? (pule os itens das páginas anteriores)

3. **Contagem Total de Itens:** 📊
   - `ItemModel.count({ where: { excluido: 0 } })` vai contar todos os itens que não estão excluídos, sem aplicar o `LIMIT` e o `OFFSET`. Isso vai te dar a quantidade total de itens no banco.

4. **Total de Páginas:** 📚
   - Para calcular quantas páginas você precisa, basta dividir o total de itens pela quantidade de itens por página (`limit`).
   - E aqui vem a fórmula do **Math.ceil** (um truque para arredondar pra cima): `Math.ceil(totalItems / limit)`. Se o número de itens não for divisível de maneira exata, ele vai arredondar pra cima. Ou seja, se você tem 13 itens e o limite é 10, ele vai te dizer que são **2 páginas**.

---

### **Parte 2: O Controller - Recebendo a Requisição e Devolvendo os Resultados** 📡

Agora vamos pro **controller**! O controller é o responsável por responder as requisições HTTP, e ele vai chamar a função que fizemos no modelo.

```javascript
export const getPagination = async (req, res, next) => {
  const page = parseInt(req.params.page) || 1;  // Pega a página da URL, se não, coloca 1
  const limit = parseInt(req.params.limit) || 10;  // Pega o limite da URL, se não, coloca 10
  
  logger.info('Início da requisição para obter todos os itens', { method: req.method, url: req.originalUrl });

  try  {
    const result = await ItemService.getPagination(page, limit);

    if (!result || result.length === 0) {
      logger.error('Erro: Nenhum item encontrado no banco de dados.');
      return res.status(404).json({ error: "Nenhum item encontrado no banco de dados." });
    }

    logger.info('Itens encontrados com sucesso', { itemCount: result.length });
    res.status(200).json({
      items: result.items,        // Itens encontrados
      totalItems: result.totalItems, // Total de itens
      totalPages: result.totalPages, // Total de páginas
      currentPage: result.currentPage, // Página atual
    });
  } catch (error) {
    logger.error('Erro ao buscar itens', { error: error.message, stack: error.stack });
    next(error);
  }
};
```

### O que acontece aqui?

1. **Pega os parâmetros da URL:** 🌍
   - A página (`page`) e o limite (`limit`) vêm da URL! Se não forem passados, ele assume valores padrões (1 e 10, respectivamente).
   - Tipo de requisição: `GET /items/page/1/10`. Nesse exemplo, está pedindo para pegar a **página 1** e **10 itens por página**.

2. **Chama a função do Model:** 🛠️
   - A função `ItemService.getPagination(page, limit)` é chamada para buscar os dados com base na página e no limite fornecidos.

3. **Retorna os dados:** 📬
   - Se tudo der certo, você manda de volta os itens, o total de itens, o total de páginas e a página atual. Isso tudo em formato JSON para o frontend consumir.

---

### **Parte 3: A Rota - Conectando Tudo** 🌐

Agora, temos que mapear a rota que vai chamar o controller:

```javascript
router.get('/items/page/:page/:limit', getPagination);
```

Aqui, a URL que vai chamar essa função de paginação é definida. Ou seja, se alguém acessar `GET /items/page/1/10`, a página 1 e limite 10 serão passados para o backend, que irá buscar os itens.

---

### **Parte 4: O Frontend - "Mostrando os Itens na Tela"** 🖥️

Agora, chegamos no frontend! Aqui você usa o React e o componente `Pagination` do Material UI. Vamos ver como você busca os itens e exibe a paginação:

```javascript
function App() {
  const [items, setItems] = useState([]);  // Itens que vão ser exibidos
  const [numeroPaginaAtual, setNumeroPaginaAtual] = useState(1);  // Página atual
  const [qtdItensPorPagina] = useState(10);  // Limite de itens por página
  const [totalPaginas, setTotalPaginas] = useState(1);  // Total de páginas

  async function fetchItems(page = 1, limit = 10) {
    try {
      const result = await fetch(
        `http://localhost:3000/api/gic/items/page/${page}/${limit}`
      );
      const data = await result.json();
      if (result.status === 200) {
        setItems(data.items);   // Atualiza a lista de itens
        setTotalPaginas(data.totalPages);  // Atualiza o total de páginas
      } else {
        console.log("Erro na busca");
      }
    } catch (error) {
      console.log("Erro no request: ", error);
    }
  }

  useEffect(() => {
    fetchItems(numeroPaginaAtual, qtdItensPorPagina);  // Chama a busca assim que a página muda
  }, [numeroPaginaAtual, qtdItensPorPagina]);

  const handlePageChange = (event, value) => {
    setNumeroPaginaAtual(value);  // Atualiza a página quando o usuário clica
  };

  return (
    <div>
      <ul className='list'>
        {items.map((item) => (
          <li className="item" key={item.id}>
            {item.id} - {item.descricao} - {item.valor_unitario}
          </li>
        ))}
      </ul>
      <Stack spacing={2}>
        <Pagination
          count={totalPaginas}
          page={numeroPaginaAtual}
          onChange={handlePageChange}
          color="primary"
        />
      </Stack>
    </div>
  );
}
```

### O que acontece aqui?

1. **Buscando os Itens com a `fetchItems`:** 🔄
   - Toda vez que a página mudar, o frontend faz uma requisição HTTP para o backend. Ele pega a página e o limite da URL e vai lá buscar os dados.

2. **Exibindo os Itens:** 🖼️
   - Após a busca, os itens são exibidos numa lista. Cada item tem um `id`, `descricao`, e `valor_unitario`.

3. **Controle de Página:** 📅
   - Você usa o componente `Pagination` para mostrar os números das páginas. Quando o usuário clica numa página, o `handlePageChange` é acionado e atualiza a página atual.

---

### **Resumo Final: Como a Magia Acontece?** ✨

1. O backend recebe a página e o limite, calcula o `OFFSET` para saber de onde começar a busca.
2. A consulta SQL retorna os itens daquela página específica.
3. O total de itens é contado para calcular quantas páginas são necessárias.
4. O frontend solicita esses dados e os exibe, com a opção de navegar entre as páginas usando a paginador.

E é isso! Agora você tem uma paginação cheia de estilo e funcionando direitinho. Sempre que você precisar implementar algo parecido, é só lembrar desses passos! 😎