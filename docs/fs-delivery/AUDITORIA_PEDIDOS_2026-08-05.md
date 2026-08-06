# FS Delivery — Auditoria unificada de pedidos

**Data:** 05/08/2026  
**Repositório operacional:** `filipisantoss93/fsdelivery`  
**Status:** concluída, validada e publicada em produção

## Escopo

A auditoria alinhou HTML, JavaScript, CSS operacional e Supabase nos seguintes canais:

- pedido do garçom;
- pedido público de entrega e retirada;
- pedido por QR Code da mesa;
- pedido criado no painel principal;
- pedido de balcão;
- venda rápida no caixa;
- acompanhamento, aprovação, cozinha, cobrança e conclusão.

## Contrato unificado

### Origens

- `publico`
- `qr_mesa`
- `painel`
- `garcom`
- `balcao`
- `caixa`

### Tipos

- `mesa`
- `retirada`
- `entrega`

O conceito visual `local` é convertido para `mesa` antes de chegar ao banco.

### Status iniciais

- entrega ou retirada pública: `aguardando_aprovacao`;
- QR da mesa e canais internos: `confirmado`.

### Status operacionais

1. `aguardando_aprovacao`
2. `confirmado`
3. `preparo`
4. `pronto`
5. `servido` para mesa
6. `saiu_entrega` para entrega
7. `finalizado`
8. `cancelado`

## Falhas críticas corrigidas

1. RPCs do garçom declaravam retorno `bigint`, mas o sistema gera códigos textuais como `PED-0009`.
2. A tabela de produção não possuía a coluna `pedidos.origem`.
3. O banco não distinguia pedido público de pedido interno ao definir o status inicial.
4. URLs limpas da Vercel não eram reconhecidas por módulos que esperavam `.html`.
5. O balcão não enviava CEP, endereço estruturado e troco conforme o contrato do Supabase.
6. Pedido de mesa era recusado por não informar pagamento antecipado.
7. O caixa abria o fluxo do garçom em vez de uma venda rápida própria.
8. Links legados com `.html` ainda apareciam no painel, loja, balcão e acesso do garçom.
9. O acompanhamento público usava polling contínuo, contrariando o padrão de estabilidade do frontend.
10. O histórico público podia ser consultado apenas com slug e WhatsApp.
11. O frontend chamava uma RPC de vínculo de dispositivo que ainda não existia no banco.
12. Estilos de pedidos eram duplicados ou injetados dinamicamente por JavaScript.
13. Funções internas de caixa e cozinha ainda herdavam permissões públicas desnecessárias.
14. Existiam políticas RLS redundantes, foreign keys sem índice e um índice duplicado em notificações.

## Alterações publicadas

### PR #17 — Unificar fluxos de pedidos e venda rápida

**Commit em `main`:** `5a62d2371b54a4679597ef0d521c94f3db58021c`

Incluiu:

- contrato único de rotas, tipos, origens e status;
- RPCs com retorno textual;
- endereço estruturado;
- venda rápida em `/balcao?origem=caixa`;
- segurança e índices;
- testes automáticos;
- documentação detalhada no repositório operacional.

### PR #18 — Normalizar URLs limpas nos fluxos de pedidos

**Commit em `main`:** `d2c79c11a65dcec9c3aceb5cb4eb8bc9f7f7ba81`

Incluiu:

- normalização central de links internos;
- limpeza de ações de formulário, `data-destination` e comandos `onclick`;
- preservação de links externos, telefone, e-mail e âncoras;
- cobertura automática contra regressão;
- implementação sem `MutationObserver` e sem polling.

### PR #19 — Endurecer histórico público e reduzir módulos legados

**Commit em `main`:** `a59b866414f2cd3f90648256867cfe652df43cff`

Incluiu:

- remoção de `js/balcao-fluxos.js` após absorção das correções em `js/balcao.js`;
- remoção de `js/cliente-submit-fix.js` após consolidação em `js/cliente.js`;
- substituição de `css/app-orders-operational.css` pela folha unificada `css/orders.css`;
- retirada de CSS injetado por `cliente.js` e `loja-pos-envio.js`;
- namespaces separados para timeline administrativa e acompanhamento público;
- retirada de polling contínuo no histórico público;
- vínculo obrigatório entre pedido, cliente e dispositivo;
- revisão de RLS, grants, funções e índices.

## Histórico público protegido por dispositivo

O histórico não é mais liberado somente pela combinação de slug e WhatsApp.

Fluxo atual:

1. o checkout público gera e envia um `checkout_token` único junto ao pedido;
2. após o pedido ser criado, o mesmo dispositivo apresenta esse comprovante à RPC `vincular_dispositivo_cliente`;
3. o banco valida slug, WhatsApp, cliente, pedido, origem e prazo de 24 horas;
4. o banco gera um token aleatório de 256 bits;
5. somente o hash SHA-256 do token é armazenado em `cliente_dispositivos`;
6. o histórico passa a exigir `slug + WhatsApp + token do dispositivo`;
7. o token expira em 180 dias, pode ser revogado e cada cliente mantém no máximo cinco dispositivos ativos.

A RPC antiga `consultar_pedidos_cliente(text, text)` foi removida. A consulta atual usa `consultar_pedidos_cliente(text, text, text)`.

Consequência operacional: dispositivos antigos que nunca receberam token precisam realizar um novo pedido para serem vinculados com segurança.

## CSS consolidado dos pedidos

A folha `css/orders.css` passou a concentrar:

- cards, filtros e timeline do painel administrativo;
- cards e detalhes do histórico do cliente;
- acompanhamento exibido após o pedido público;
- responsividade dos componentes de pedidos.

Os seletores públicos usam prefixos próprios, como `fs-public-order-*`, para não disputar regras com a timeline administrativa.

## Supabase de produção

Foram aplicadas as migrations equivalentes a:

- `20260805_auditoria_pedidos_unificada.sql`;
- `20260805_corrigir_pagamento_pedido_mesa.sql`;
- `20260805_endurecer_seguranca_e_indices_pedidos.sql`;
- `20260805_fase2_hardening_pedidos.sql`;
- `20260805_index_cliente_dispositivos.sql`.

A fase 2 também:

- criou `cliente_dispositivos` com RLS e sem acesso direto para `anon` ou `authenticated`;
- removeu execução anônima de funções internas de caixa e cozinha;
- removeu políticas comprovadamente redundantes;
- alterou políticas sinalizadas para usar `(select auth.uid())`;
- criou índices ausentes para foreign keys;
- removeu a duplicata `idx_notificacoes_operacionais_destino` e preservou o índice equivalente;
- manteve índices apenas marcados como não usados até existir histórico suficiente para decidir sua remoção.

Após a migration, os advisors deixaram de apontar:

- foreign keys sem índice;
- `auth_rls_initplan` nas políticas revisadas;
- índice duplicado em notificações.

## Validação executada

Foi executada uma matriz reversível com seis cenários no banco de produção:

| Cenário | Origem | Tipo | Status inicial |
|---|---|---|---|
| Retirada pública | `publico` | `retirada` | `aguardando_aprovacao` |
| Entrega pública | `publico` | `entrega` | `aguardando_aprovacao` |
| QR da mesa | `qr_mesa` | `mesa` | `confirmado` |
| Garçom | `garcom` | `mesa` | `confirmado` |
| Balcão | `balcao` | `retirada` | `confirmado` |
| Venda rápida | `caixa` | `retirada` | `confirmado` |

Todos os asserts passaram, incluindo endereço, taxa de entrega, mesa e troco. A subtransação foi revertida e ficaram **zero pedidos** e **zero itens** de teste.

Na fase 2 também foram confirmados:

- existência da tabela `cliente_dispositivos`;
- assinatura das novas RPCs;
- remoção da RPC antiga de consulta por telefone;
- grants das funções internas e públicas;
- retirada do índice duplicado;
- preservação do índice correto;
- ausência de referências ativas aos módulos aposentados;
- funcionamento da página pública do histórico sem o script legado.

A troca completa de token por uma chamada RPC real não pôde ser automatizada pelas ferramentas conectadas, que bloquearam a execução do teste transacional e não permitiram conexão externa direta. O contrato, as funções, os grants, a sintaxe, os testes estáticos, o preview e o deploy foram validados; ainda é recomendado executar um pedido real controlado em dispositivo físico.

## CI e deploy

- auditoria estática dos pedidos: aprovada;
- auditoria de estabilidade frontend: aprovada;
- verificação de sintaxe JavaScript: aprovada;
- PR #19 mesclado na `main`;
- deploy de produção na Vercel: `READY`;
- página `/cliente?loja=fs-lanches`: HTTP 200;
- domínio operacional: `delivery.portalfs.com.br`.

## Pendências não bloqueantes

1. consolidar gradualmente `garcom-salao.js` em `garcom-core.js` e retirar os observers restantes;
2. avaliar a fusão futura de `app-orders-operational.js` e `app-orders-type-filters.js` no módulo principal do painel;
3. reduzir as três camadas públicas `loja-fluxos-pedido.js`, `loja-publica-consolidado.js` e `loja-pos-envio.js` após testes operacionais prolongados;
4. revisar as políticas permissivas duplicadas que representam acesso do proprietário e do administrador da plataforma, sem remover privilégios legítimos;
5. revisar a exposição ampla pelo Data API/GraphQL antes de migrar tabelas internas para um schema privado;
6. ativar no painel do Supabase a proteção contra senhas vazadas;
7. observar estatísticas de uso dos índices por um período real antes de excluir índices apenas classificados como `unused`;
8. executar pedido público real em dispositivo físico para confirmar o ciclo completo de criação, vínculo, armazenamento local e consulta protegida.

## Regra permanente

Toda evolução dos pedidos deve preservar:

- URLs sem `.html`;
- tipos, origens e status canônicos;
- preço calculado no banco;
- endereço apenas em entrega;
- pedido público aguardando aprovação;
- pedido interno e QR entrando confirmado;
- mesa sem pagamento antecipado;
- mesa finalizada somente após quitação integral;
- histórico público condicionado ao token do dispositivo;
- token bruto nunca armazenado no banco;
- funções internas sem execução anônima;
- estilos de pedidos concentrados em `css/orders.css`;
- testes de regressão obrigatórios antes do merge.
