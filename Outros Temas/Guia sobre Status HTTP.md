# 🌐 Guia Definitivo (e Divertido) dos **Status HTTP** 🚦  
**Ou: como o servidor fala com você quando tudo dá certo (ou não).**

---

## 🟢 **1xx – Informacionais (o servidor tá dizendo: "Calma, ainda tô pensando")**

| Código | Descrição técnica | Versão zoeira |
|--------|--------------------|----------------|
| 100 | Continue – A requisição começou bem, pode mandar o resto. | "Vai, continua! Tô ouvindo..." 🎧  
| 101 | Switching Protocols – O servidor vai mudar de protocolo a pedido do cliente. | "Troca o canal, vamos pra outro protocolo!" 📺  
| 102 | Processing (WebDAV) – Estou processando, mas calma, demora mesmo. | "Não travou, só tô pensando devagarzinho!" 🧠  
| 103 | Early Hints – Headers estão vindo antes do corpo. | "Spoiler alert! Aqui vai um gostinho antes do prato principal." 🍿

---

## ✅ **2xx – Sucesso (tudo certo, bebê!)**

| Código | Descrição técnica | Versão zoeira |
|--------|--------------------|----------------|
| 200 | OK – Requisição bem-sucedida. | "Show! Tudo lindo, toma aí a resposta." 😎  
| 201 | Created – Algo foi criado com sucesso. | "Parabéns, nasceu um novo recurso!" 👶  
| 202 | Accepted – Aceitei, mas ainda tô processando. | "Recebi, mas ainda vou mexer com isso depois, tá?" 🕒  
| 203 | Non-Authoritative Information – A resposta vem de uma fonte secundária. | "Essa info não veio direto da fonte... mas quebra o galho." 🕵️  
| 204 | No Content – Deu certo, mas não tem nada pra te mostrar. | "Tamo quites, mas não tenho nada pra te dar." 🤷  
| 205 | Reset Content – Pode limpar o formulário aí. | "Zera tudo e começa de novo, meu consagrado." 🧼  
| 206 | Partial Content – Só uma parte da resposta foi enviada. | "Toma só um pedaço por enquanto. Tá bom?" 🍰  
| 207 | Multi-Status (WebDAV) – Vários status em uma resposta. | "Resposta combo! Leva todos esses status aí." 🎁  
| 208 | Already Reported – Já te contei isso. Não vou repetir. | "Falei uma vez só, presta atenção!" 🔁  
| 226 | IM Used – Resposta manipulada usando uma extensão. | "Mexi no conteúdo antes de te mandar, beleza?" 🧪

---

## 🟡 **3xx – Redirecionamento (muda de rota, campeão!)**

| Código | Descrição técnica | Versão zoeira |
|--------|--------------------|----------------|
| 300 | Multiple Choices – Tem várias opções. Escolha aí. | "Menu de opções liberado, o que você vai querer?" 🍽️  
| 301 | Moved Permanently – Mudou pra sempre, atualiza os links. | "Mudou de casa e não volta mais. Atualiza aí!" 🏡  
| 302 | Found – Redirecionou temporariamente. | "Foi pra outro lugar rapidinho, mas ainda volta." 🚪  
| 303 | See Other – Vai olhar em outro lugar, irmão. | "Não olha pra mim, olha ali ó!" 👉  
| 304 | Not Modified – Nada mudou, usa o cache. | "Tá igualzinho da última vez. Reaproveita aí." 🗂️  
| 305 | Use Proxy – Use um proxy pra acessar isso. | "Não vem direto, passa por aquele corredor ali!" 🕵️‍♂️  
| 306 | Switch Proxy (Deprecated) – Não usa mais. | "Era moda antiga, esquece isso." 🧓  
| 307 | Temporary Redirect – Vá temporariamente para outro lugar, mesma requisição. | "Vai ali rapidinho, mas mantém o pedido." 📦  
| 308 | Permanent Redirect – Redirecionamento definitivo, mas com método intacto. | "Mudança oficial com tudo do jeito que tava." 🚚

---

## 🔴 **4xx – Erro do cliente (a culpa é sua, parça...)**

| Código | Descrição técnica | Versão zoeira |
|--------|--------------------|----------------|
| 400 | Bad Request – Pedido malformado. | "Não entendi nada do que você quis dizer." 🤯  
| 401 | Unauthorized – Precisa autenticar. | "Quem é você na fila do pão? Mostra a credencial aí!" 🪪  
| 402 | Payment Required – Reservado para uso futuro (pagamentos). | "Vai rolar, mas só se pagar!" 💸  
| 403 | Forbidden – Acesso negado. | "Nem adianta tentar. Aqui você não entra." 🚫  
| 404 | Not Found – Não encontrado. | "Procurou, procurou... e nada." 🔍  
| 405 | Method Not Allowed – Método HTTP não permitido. | "Você tentou usar uma ferramenta errada, tipo garfo na sopa." 🍴🥣  
| 406 | Not Acceptable – Não dá pra responder do jeito que você quer. | "Seu gosto é refinado demais, não tenho isso aqui não." 🧀  
| 407 | Proxy Authentication Required – Autentique-se no proxy. | "Primeiro convence o segurança do corredor!" 🛡️  
| 408 | Request Timeout – Demorou demais. | "Cansei de esperar, fui embora." ⏳  
| 409 | Conflict – Conflito de dados. | "Irmão, alguém mexeu nisso ao mesmo tempo. Deu ruim." 🤼  
| 410 | Gone – Recurso sumiu, não volta mais. | "Já era, nem adianta procurar. Sumiu de vez." 🧨  
| 411 | Length Required – Faltou informar o tamanho do corpo. | "Me diz o tamanho do pacote, senão não aceito." 📦  
| 412 | Precondition Failed – Condições não foram atendidas. | "Você fez promessas que não cumpriu!" 💔  
| 413 | Payload Too Large – Corpo muito grande. | "Isso aí tá pesado demais, não cabe aqui!" 🐘  
| 414 | URI Too Long – A URL é grande demais. | "Essa URL parece uma redação do ENEM. Encurta aí." ✍️  
| 415 | Unsupported Media Type – Tipo de mídia não aceito. | "Não sei o que fazer com esse tipo de conteúdo." 📼  
| 416 | Range Not Satisfiable – O intervalo pedido não faz sentido. | "Esse trecho que você pediu nem existe, meu rei!" 🧩  
| 417 | Expectation Failed – A expectativa era outra. | "Você me iludiu. Esperava mais." 😢  
| 418 | I'm a teapot – Eu sou um bule de chá. | "Não dá pra fazer café, porque... eu sou um bule, cara." 🫖 (Sim, é real!)  
| 421 | Misdirected Request – Foi pro servidor errado. | "Você mandou isso pra pessoa errada, amigão." 📬  
| 422 | Unprocessable Entity – Dados válidos, mas inaceitáveis. | "Entendi o que você quer... mas não dá pra aceitar." 😵  
| 423 | Locked – Recurso trancado. | "Tá trancado. Só com a chave certa!" 🔐  
| 424 | Failed Dependency – Algo que você depende falhou. | "A culpa nem é minha, é do outro sistema." 🙄  
| 425 | Too Early – Tá com pressa demais. | "Calma, nem comecei ainda!" 🐣  
| 426 | Upgrade Required – Precisa de uma versão melhor. | "Atualiza aí, você tá rodando versão 1995!" 💾  
| 428 | Precondition Required – Faltou condição na requisição. | "Fala os termos antes de entrar nesse papo!" 📝  
| 429 | Too Many Requests – Você exagerou no número de requisições. | "Calma, um de cada vez, por favor!" 🚨  
| 431 | Request Header Fields Too Large – Cabeçalhos grandes demais. | "Cheio de firula nos headers, não rola!" 🧠  
| 451 | Unavailable For Legal Reasons – Censurado. | "Removido por ordem judicial. Não discute." ⚖️

---

## 🔥 **5xx – Erro do servidor (a culpa agora é nossa 😬)**

| Código | Descrição técnica | Versão zoeira |
|--------|--------------------|----------------|
| 500 | Internal Server Error – Algo quebrou por dentro. | "Deu ruim aqui. Nem sei o que foi." 💥  
| 501 | Not Implemented – Ainda não implementei isso. | "Ideia boa... mas não botei em prática ainda." 📋  
| 502 | Bad Gateway – Gateway recebeu resposta inválida. | "Fui falar com outro servidor e ele só falou groselha." 📢  
| 503 | Service Unavailable – O serviço caiu. | "Tamo fora do ar. Volta depois, por favor!" 📴  
| 504 | Gateway Timeout – Outro servidor demorou demais. | "O vizinho não respondeu, desisti." 🧍‍♂️🕰️  
| 505 | HTTP Version Not Supported – Versão do HTTP não é suportada. | "Essa versão HTTP é tipo disquete, já era." 🧟‍♂️  
| 506 | Variant Also Negotiates – Loop infinito de escolha de versão. | "Nem eu sei qual versão usar. Socorro!" 🔄  
| 507 | Insufficient Storage – Falta espaço pra salvar. | "Tô cheio. Sem espaço nem pra mais um byte!" 🧱  
| 508 | Loop Detected – Detectei loop infinito. | "Tô preso num looping, chama ajuda!" 🔁😵  
| 510 | Not Extended – Faltam extensões na requisição. | "Você precisa incluir mais coisa nisso aí!" 🧩  
| 511 | Network Authentication Required – Autenticação de rede necessária. | "Loga no Wi-Fi primeiro, depois a gente conversa." 📡

---

## 🧾 Conclusão: HTTP Status não é só número, é mensagem com atitude 😎

Saber o significado de cada código te salva de muitos bugs, erros de integração, e tempo perdido caçando o porquê da resposta estar errada.  
E com esse guia, você também ganha uma risada no meio do caos. 😄

Se quiser, posso gerar uma versão **PDF, Markdown ou até um post bonitão pro LinkedIn**. Só avisar! 🚀