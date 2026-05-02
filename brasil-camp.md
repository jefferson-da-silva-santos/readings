# 🥋 Brazil Camp — Guia de Testes da Plataforma

> **Ambiente:** Produção (Vercel) · **Versão:** Homologação 2025  
> **URL:** https://brasil-camp.vercel.app

---

## Sumário

1. [Credenciais de Acesso](#1-credenciais-de-acesso)
2. [Permissões por Tipo de Usuário](#2-permissões-por-tipo-de-usuário)
3. [Mapa de Rotas — Área Pública](#3-mapa-de-rotas--área-pública)
4. [Mapa de Rotas — Área do Atleta](#4-mapa-de-rotas--área-do-atleta)
5. [Mapa de Rotas — Admin / Organizador](#5-mapa-de-rotas--admin--organizador)
6. [Roteiro de Testes — Admin](#6-roteiro-de-testes--admin)
7. [Roteiro de Testes — Organizador](#7-roteiro-de-testes--organizador)
8. [Roteiro de Testes — Atleta](#8-roteiro-de-testes--atleta)
9. [Fluxo Completo — Inscrição e Pagamento](#9-fluxo-completo--inscrição-e-pagamento)
10. [Dicas e Pontos de Atenção](#10-dicas-e-pontos-de-atenção)

---

## 1. Credenciais de Acesso

A plataforma possui três usuários pré-cadastrados via seed do banco.

> 💡 **Dica:** Abra três perfis diferentes do Chrome (ou janelas anônimas) para testar os três fluxos simultaneamente.

### 🔴 Admin
| Campo  | Valor                        | Observação           |
|--------|------------------------------|----------------------|
| E-mail | admin@combateplus.com.br     | Acesso total ao sistema |
| Senha  | `Admin@123`                  | Sensível a maiúsculas |
| Perfil | `ADMIN`                      |                      |

🔗 [Abrir página de login](https://brasil-camp.vercel.app/login)

---

### 🟡 Organizador
| Campo  | Valor                  | Observação                    |
|--------|------------------------|-------------------------------|
| E-mail | org@nordestejj.com.br  | Gerencia seus próprios eventos |
| Senha  | `Org@123`              | Sensível a maiúsculas          |
| Perfil | `ORGANIZER`            |                               |

🔗 [Abrir página de login](https://brasil-camp.vercel.app/login)

---

### 🟢 Atleta
| Campo  | Valor                   | Observação                        |
|--------|-------------------------|-----------------------------------|
| E-mail | lucas@alliance.com.br   | Inscrições, pagamentos, loja      |
| Senha  | `Atleta@123`            | Sensível a maiúsculas              |
| Perfil | `ATHLETE`               |                                   |

🔗 [Abrir página de login](https://brasil-camp.vercel.app/login)

---

## 2. Permissões por Tipo de Usuário

| Funcionalidade                     | Admin | Org.    | Atleta | Público |
|------------------------------------|:-----:|:-------:|:------:|:-------:|
| Ver eventos publicados             | ✅    | ✅      | ✅     | ✅      |
| Ver notícias                       | ✅    | ✅      | ✅     | ✅      |
| Ver loja / produtos                | ✅    | ✅      | ✅     | ✅      |
| Inscrever-se em evento             | ✅    | ✅      | ✅     | ❌      |
| Enviar comprovante PIX             | ✅    | ✅      | ✅     | ❌      |
| Fazer pedido na loja               | ✅    | ✅      | ✅     | ❌      |
| Dashboard pessoal `/atleta`        | ✅    | ✅      | ✅     | ❌      |
| Criar / editar eventos             | ✅    | Próprios| ❌     | ❌      |
| Aprovar / rejeitar pagamentos      | ✅    | ✅      | ❌     | ❌      |
| Gerar chaveamento de bracket       | ✅    | ✅      | ❌     | ❌      |
| Iniciar e finalizar lutas          | ✅    | ✅      | ❌     | ❌      |
| Gerenciar destaques pagos          | ✅    | ✅      | ❌     | ❌      |
| Publicar / editar notícias         | ✅    | ❌      | ❌     | ❌      |
| Gerenciar produtos da loja         | ✅    | ❌      | ❌     | ❌      |
| Ver todos os pedidos da loja       | ✅    | ❌      | ❌     | ❌      |
| Gerenciar usuários / roles         | ✅    | ❌      | ❌     | ❌      |
| Desativar / reativar contas        | ✅    | ❌      | ❌     | ❌      |

> ⚠️ **Obs:** O Organizador só gerencia eventos que ele mesmo criou. Teste editar um evento do Admin logado como Organizador — deve retornar erro `403`.

---

## 3. Mapa de Rotas — Área Pública

Todas acessíveis sem autenticação.

| Rota | Descrição | Acesso |
|------|-----------|--------|
| [/](https://brasil-camp.vercel.app/) | Página inicial, ticker, eventos, notícias | Público |
| [/eventos](https://brasil-camp.vercel.app/eventos) | Listagem com filtros e paginação | Público |
| [/eventos/:slug](https://brasil-camp.vercel.app/eventos/copa-nordeste-bjj-2025) | Detalhe do evento, categorias, inscrição | Público |
| [/eventos/:slug/chaveamento](https://brasil-camp.vercel.app/eventos/copa-nordeste-bjj-2025/chaveamento) | Bracket / chaveamento do evento | Público |
| [/ao-vivo](https://brasil-camp.vercel.app/ao-vivo) | Lutas ao vivo — placar em tempo real (WebSocket) | Público |
| [/noticias](https://brasil-camp.vercel.app/noticias) | Feed de notícias | Público |
| [/noticias/:slug](https://brasil-camp.vercel.app/noticias/1) | Detalhe de notícia | Público |
| [/loja](https://brasil-camp.vercel.app/loja) | Marketplace — catálogo de produtos | Público |
| [/loja/:id](https://brasil-camp.vercel.app/loja/1) | Detalhe de produto | Público |
| [/login](https://brasil-camp.vercel.app/login) | Autenticação | Público |
| [/cadastro](https://brasil-camp.vercel.app/cadastro) | Criar nova conta | Público |

---

## 4. Mapa de Rotas — Área do Atleta

Requerem autenticação. Sem login, o sistema redireciona para `/login`.

| Rota | Descrição | Acesso |
|------|-----------|--------|
| [/atleta](https://brasil-camp.vercel.app/atleta) | Dashboard — stats, cashback, inscrições recentes | Atleta+ |
| [/atleta/perfil](https://brasil-camp.vercel.app/atleta/perfil) | Editar dados pessoais e alterar senha | Atleta+ |
| [/atleta/inscricoes](https://brasil-camp.vercel.app/atleta/inscricoes) | Todas as inscrições do usuário logado | Atleta+ |
| [/atleta/inscricoes/:id](https://brasil-camp.vercel.app/atleta/inscricoes/1) | Detalhe + upload de comprovante PIX | Atleta+ |
| [/atleta/pedidos](https://brasil-camp.vercel.app/atleta/pedidos) | Histórico de pedidos da loja | Atleta+ |
| [/atleta/pedidos/:id](https://brasil-camp.vercel.app/atleta/pedidos/1) | Detalhe de pedido + opção de cancelar | Atleta+ |

---

## 5. Mapa de Rotas — Admin / Organizador

Protegidas por role. Atleta é redirecionado ao tentar acessar qualquer rota `/admin`.

| Rota | Descrição | Quem acessa |
|------|-----------|-------------|
| [/admin](https://brasil-camp.vercel.app/admin) | Dashboard — KPIs, receita, pendências | Admin + Org. |
| [/admin/eventos](https://brasil-camp.vercel.app/admin/eventos) | Lista de eventos gerenciados | Admin + Org. |
| [/admin/eventos/novo](https://brasil-camp.vercel.app/admin/eventos/novo) | Criar novo evento | Admin + Org. |
| [/admin/eventos/:id/editar](https://brasil-camp.vercel.app/admin/eventos/1/editar) | Editar evento existente | Admin + Org. |
| [/admin/eventos/:id/atletas](https://brasil-camp.vercel.app/admin/eventos/1/atletas) | Checagem de atletas por categoria | Admin + Org. |
| [/admin/categorias](https://brasil-camp.vercel.app/admin/categorias) | Gerenciar categorias, faixas, pesos, preços | Admin + Org. |
| [/admin/pagamentos](https://brasil-camp.vercel.app/admin/pagamentos) | Aprovar / rejeitar comprovantes PIX | Admin + Org. |
| [/admin/chaveamentos](https://brasil-camp.vercel.app/admin/chaveamentos) | Gerar e visualizar chaveamentos | Admin + Org. |
| [/admin/lutas](https://brasil-camp.vercel.app/admin/lutas) | Gerenciar lutas — iniciar, pontuar, finalizar | Admin + Org. |
| [/admin/destaques](https://brasil-camp.vercel.app/admin/destaques) | Destaques pagos (ticker / área nobre) | Admin + Org. |
| [/admin/noticias](https://brasil-camp.vercel.app/admin/noticias) | Gerenciar notícias | Só Admin |
| [/admin/noticias/nova](https://brasil-camp.vercel.app/admin/noticias/nova) | Publicar nova notícia | Só Admin |
| [/admin/produtos](https://brasil-camp.vercel.app/admin/produtos) | Gerenciar catálogo da loja | Só Admin |
| [/admin/pedidos](https://brasil-camp.vercel.app/admin/pedidos) | Todos os pedidos da loja | Só Admin |
| [/admin/usuarios](https://brasil-camp.vercel.app/admin/usuarios) | Gerenciar usuários e roles | Só Admin |

---

## 6. Roteiro de Testes — Admin

**Login:** [https://brasil-camp.vercel.app/login](https://brasil-camp.vercel.app/login)  
**Credenciais:** `admin@combateplus.com.br` / `Admin@123`

### 6.1 Dashboard e KPIs

| # | Cenário de Teste | Resultado Esperado | OK? |
|---|------------------|--------------------|-----|
| 1 | Acessar `/admin` sem login | Redireciona para `/login` | ☐ |
| 2 | Acessar `/admin` como Admin | Dashboard com KPIs carrega corretamente | ☐ |
| 3 | KPIs exibem valores numéricos | Sem NaN ou campos em branco | ☐ |
| 4 | KPI Pendentes bate com `/admin/pagamentos` | Número idêntico nos dois locais | ☐ |

### 6.2 Gestão de Eventos

| # | Cenário de Teste | Resultado Esperado | OK? |
|---|------------------|--------------------|-----|
| 5 | Listar eventos em `/admin/eventos` | Lista carrega com status visíveis | ☐ |
| 6 | Criar evento com todos os dados válidos | Evento criado, slug gerado automaticamente | ☐ |
| 7 | Criar evento — prazo no passado | Erro de validação é exibido | ☐ |
| 8 | Editar título do evento | Alteração salva e refletida na listagem | ☐ |
| 9 | Publicar evento (`DRAFT → PUBLISHED`) | Aparece na home e em `/eventos` | ☐ |
| 10 | Upload de banner JPG / PNG | Banner exibido na página do evento | ☐ |
| 11 | Upload de banner com formato inválido | Erro — formato não suportado | ☐ |

### 6.3 Categorias

| # | Cenário de Teste | Resultado Esperado | OK? |
|---|------------------|--------------------|-----|
| 12 | Criar categoria com dados válidos | Salva e aparece no evento | ☐ |
| 13 | Criar categoria com preço R$ 0,00 | Erro de validação | ☐ |
| 14 | Excluir categoria sem inscrições | Removida com sucesso | ☐ |
| 15 | Excluir categoria COM inscrições ativas | Erro — não é possível excluir | ☐ |

### 6.4 Pagamentos

| # | Cenário de Teste | Resultado Esperado | OK? |
|---|------------------|--------------------|-----|
| 16 | Listar comprovantes pendentes | Tabela com dados do atleta carrega | ☐ |
| 17 | Visualizar comprovante (clicar no link) | Arquivo abre ou faz download | ☐ |
| 18 | Aprovar comprovante válido | Status `APPROVED`; cashback 10% gerado | ☐ |
| 19 | Rejeitar — preencher motivo | Status `REJECTED`; motivo salvo | ☐ |
| 20 | Tentar rejeitar sem preencher motivo | Botão desabilitado ou erro exibido | ☐ |

### 6.5 Chaveamento

| # | Cenário de Teste | Resultado Esperado | OK? |
|---|------------------|--------------------|-----|
| 21 | Gerar bracket com 2+ atletas aprovados | Chaves criadas, atletas distribuídos | ☐ |
| 22 | Gerar bracket com menos de 2 aprovados | Erro — mínimo 2 atletas necessário | ☐ |
| 23 | Verificar separação de equipes | Mesma equipe não se enfrenta na 1ª luta | ☐ |
| 24 | Visualizar bracket gerado | Árvore de lutas renderizada | ☐ |

### 6.6 Lutas ao Vivo

| # | Cenário de Teste | Resultado Esperado | OK? |
|---|------------------|--------------------|-----|
| 25 | Iniciar luta com status `SCHEDULED` | Status vira `ONGOING`, timer começa | ☐ |
| 26 | Adicionar ponto ao atleta A | Placar atualiza em tempo real | ☐ |
| 27 | Adicionar vantagem / penalidade | Contadores atualizam corretamente | ☐ |
| 28 | Finalizar luta — declarar vencedor | Vencedor avança no bracket | ☐ |
| 29 | Abrir `/ao-vivo` em outra aba | Placar sincroniza via WebSocket sem F5 | ☐ |

### 6.7 Usuários

| # | Cenário de Teste | Resultado Esperado | OK? |
|---|------------------|--------------------|-----|
| 30 | Listar usuários em `/admin/usuarios` | Lista com roles corretas | ☐ |
| 31 | Desativar usuário | Usuário não consegue mais fazer login | ☐ |
| 32 | Reativar usuário | Login volta a funcionar normalmente | ☐ |
| 33 | Atleta tenta acessar `/admin` pela URL | Redirecionamento — acesso negado | ☐ |

---

## 7. Roteiro de Testes — Organizador

**Credenciais:** `org@nordestejj.com.br` / `Org@123`

| # | Cenário de Teste | Resultado Esperado | OK? |
|---|------------------|--------------------|-----|
| 1 | Login como Organizador | Acessa `/admin` ou home | ☐ |
| 2 | Listar eventos | Apenas eventos do próprio organizador | ☐ |
| 3 | Editar evento de outro organizador | Erro `403` ou botão não aparece | ☐ |
| 4 | Criar novo evento | Evento vinculado ao organizador logado | ☐ |
| 5 | Aprovar pagamento de atleta | Funciona — permissão compartilhada | ☐ |
| 6 | Acessar `/admin/noticias/nova` | Redirecionamento — sem permissão | ☐ |
| 7 | Acessar `/admin/usuarios` | Redirecionamento — sem permissão | ☐ |
| 8 | Acessar `/admin/produtos` | Redirecionamento — sem permissão | ☐ |
| 9 | Gerar chaveamento do próprio evento | Chaveamento gerado com sucesso | ☐ |
| 10 | Gerenciar lutas ao vivo | Iniciar, pontuar e finalizar funciona | ☐ |

---

## 8. Roteiro de Testes — Atleta

**Credenciais:** `lucas@alliance.com.br` / `Atleta@123`

### 8.1 Inscrição e Pagamento

| # | Cenário de Teste | Resultado Esperado | OK? |
|---|------------------|--------------------|-----|
| 1 | Login como Atleta | Acessa `/atleta` (dashboard pessoal) | ☐ |
| 2 | Abrir detalhe de evento em `/eventos` | Categorias, datas e local exibidos | ☐ |
| 3 | Inscrever-se em categoria válida | Inscrição criada, payment `PENDING` | ☐ |
| 4 | Tentar inscrever após prazo encerrado | Erro — prazo de inscrições encerrado | ☐ |
| 5 | Inscrever na mesma categoria duas vezes | Erro — duplicidade detectada | ☐ |
| 6 | Enviar comprovante JPG / PNG | Status muda para `PAYMENT_SENT` | ☐ |
| 7 | Tentar enviar sem selecionar arquivo | Erro de validação no formulário | ☐ |
| 8 | Cancelar inscrição com status `PENDING` | Status vira `CANCELLED` | ☐ |
| 9 | Tentar cancelar inscrição já `APPROVED` | Erro — cancelamento não permitido | ☐ |

### 8.2 Perfil e Conta

| # | Cenário de Teste | Resultado Esperado | OK? |
|---|------------------|--------------------|-----|
| 10 | Editar nome e telefone em `/atleta/perfil` | Dados salvos corretamente | ☐ |
| 11 | Alterar senha com senha atual errada | Erro — senha atual incorreta | ☐ |
| 12 | Alterar senha com dados corretos | Senha salva com sucesso | ☐ |
| 13 | Ver stats em `/atleta` | Lutas, cashback e inscrições exibidos | ☐ |
| 14 | Tentar acessar `/admin` pela URL | Redirecionamento — sem permissão | ☐ |

### 8.3 Loja / Marketplace

| # | Cenário de Teste | Resultado Esperado | OK? |
|---|------------------|--------------------|-----|
| 15 | Listar produtos em `/loja` | Produtos ativos com preço correto | ☐ |
| 16 | Adicionar produto ao carrinho | Carrinho abre, item e subtotal exibidos | ☐ |
| 17 | Ajustar quantidade no carrinho | Subtotal recalcula corretamente | ☐ |
| 18 | Finalizar pedido | Pedido criado, estoque decrementado | ☐ |
| 19 | Pedir quantidade acima do estoque | Erro — estoque insuficiente | ☐ |
| 20 | Cancelar pedido com status `PENDING` | Cancelado, estoque devolvido | ☐ |
| 21 | Ver histórico em `/atleta/pedidos` | Lista com status e itens do pedido | ☐ |

---

## 9. Fluxo Completo — Inscrição e Pagamento

> Fluxo end-to-end mais importante. Execute com as três contas abertas simultaneamente. Valida a integração entre todos os módulos da plataforma.

| Passo | Usuário | Ação | O que verificar |
|-------|---------|------|-----------------|
| 1 | Atleta | Abrir [detalhe do evento](https://brasil-camp.vercel.app/eventos/copa-nordeste-bjj-2025) | Evento carrega com categorias e botão "Inscrever-se" |
| 2 | Atleta | Clicar em Inscrever-se, selecionar categoria e equipe, confirmar | Status da inscrição fica `PENDING` |
| 3 | Atleta | Realizar PIX e enviar comprovante em `/atleta/inscricoes` | Status muda para `PAYMENT_SENT` |
| 4 | Organizador | Acessar [/admin/pagamentos](https://brasil-camp.vercel.app/admin/pagamentos) | Comprovante do atleta aparece na fila |
| 5 | Organizador | Clicar em Aprovar | Status `APPROVED`; cashback de 10% gerado ao atleta |
| 6 | Organizador | Acessar [/admin/chaveamentos](https://brasil-camp.vercel.app/admin/chaveamentos) e gerar bracket | Bracket gerado com atletas distribuídos |
| 7 | Admin | Acessar [/admin/lutas](https://brasil-camp.vercel.app/admin/lutas) e iniciar luta | Status `ONGOING`, timer começa |
| 8 | Admin | Abrir [/ao-vivo](https://brasil-camp.vercel.app/ao-vivo) em outra aba | Placar atualiza em ambas as abas via WebSocket |
| 9 | Admin | Finalizar luta e declarar vencedor | Vencedor avança automaticamente no bracket |

---

## 10. Dicas e Pontos de Atenção

### 🔍 Console do Navegador (F12)
Monitore a aba **Network** e **Console** durante os testes. Qualquer erro `4xx` ou `5xx` inesperado deve ser registrado com print de tela.

### 📡 WebSocket — Placar ao Vivo
Abra [/ao-vivo](https://brasil-camp.vercel.app/ao-vivo) em duas abas ou dispositivos diferentes ao mesmo tempo. O placar deve sincronizar em menos de **200ms** sem precisar recarregar a página.

### 🚦 Rate Limiting
Tente fazer login com senha errada **cinco vezes seguidas**. A API deve bloquear temporariamente com resposta `429 Too Many Requests`.

### 🔄 Refresh Token Automático
Após **15 minutos** de inatividade o access token expira. O sistema deve renová-lo automaticamente em segundo plano sem solicitar login novamente.

### 📎 Upload de Arquivos
Teste os formatos `JPG`, `PNG`, `WEBP` e `PDF`. Teste também:
- Arquivo maior que **10MB** → deve retornar erro `413`
- Formatos inválidos como `.exe` ou `.zip` → deve retornar erro `415`

### 📱 Responsividade Mobile
Acesse a plataforma via celular ou use o DevTools com viewport de **375px**. Verifique navbar colapsável, formulários e cards de eventos.

### 🔎 Paginação e Filtros
Combine filtros em `/eventos`, `/noticias` e `/loja`. Verifique se o total de resultados exibido corresponde ao número real de itens retornados.

### 🚫 Página 404
Acesse https://brasil-camp.vercel.app/rota-que-nao-existe. Deve exibir a **página 404 personalizada** da plataforma.

### 🔐 Segurança por URL
Logado como Atleta, digite `/admin/usuarios` diretamente na barra de endereços. O sistema deve bloquear e redirecionar imediatamente.

### 👤 Duplicidade de Cadastro
Tente criar duas contas com o mesmo CPF ou o mesmo e-mail. Ambas as tentativas devem retornar erro `409` com mensagem clara.

### ⌨️ Acessibilidade por Teclado
Navegue pela plataforma usando apenas `Tab`, `Enter` e `Esc`. Formulários e modais devem ser completamente navegáveis sem mouse.

---

*Plataforma: https://brasil-camp.vercel.app · Login: /login · Admin: /admin*  
*© 2025 Brazil Camp · Documento de Homologação Interno*
