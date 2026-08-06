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

## Supabase de produção

Foram aplicadas as migrations equivalentes a:

- `20260805_auditoria_pedidos_unificada.sql`;
- `20260805_corrigir_pagamento_pedido_mesa.sql`;
- `20260805_endurecer_seguranca_e_indices_pedidos.sql`.

Também foram:

- reduzidas permissões públicas desnecessárias;
- fixado `search_path` de funções auxiliares;
- criados índices para pedidos, itens, pagamentos, movimentações e notificações.

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

## CI e deploy

- auditoria estática dos pedidos: aprovada;
- auditoria de estabilidade frontend: aprovada;
- verificação de sintaxe JavaScript: aprovada;
- deploy de produção na Vercel: `READY`;
- domínio operacional: `delivery.portalfs.com.br`.

## Pendências não bloqueantes

1. remover gradualmente módulos legados mantidos por compatibilidade;
2. consolidar estilos específicos de pedidos para reduzir sobreposição de CSS;
3. tornar obrigatório o token de dispositivo no histórico público;
4. revisar RLS e grants de módulos fora do fluxo de pedidos;
5. eliminar índices antigos duplicados apontados pelos advisors do Supabase;
6. executar teste operacional real controlado em cada canal após futuras alterações relevantes.

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
- testes de regressão obrigatórios antes do merge.
