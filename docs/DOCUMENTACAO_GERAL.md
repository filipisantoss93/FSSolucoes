# Documentação Geral do Ecossistema Portal FS

> Base oficial de conhecimento, arquitetura, decisões, produtos e pendências do ecossistema FS.

**Repositório mestre:** `filipisantoss93/PortalFS`  
**Última consolidação:** 5 de agosto de 2026  
**Responsável:** Filipi Aparecido dos Santos

---

## 1. Finalidade deste documento

O repositório PortalFS é a fonte central de documentação do ecossistema FS. O código de cada produto pode permanecer em repositórios independentes, mas decisões estratégicas, padrões compartilhados, auditorias, integrações, roadmap e pendências importantes devem ser preservados aqui.

O histórico do ChatGPT não deve ser tratado como fonte permanente de verdade. Informações relevantes precisam ser transferidas para arquivos Markdown neste repositório.

### Regra principal

Toda alteração que impacte arquitetura, banco de dados, autenticação, integrações, pagamentos, UX, identidade visual, segurança, deploy ou operação deve gerar atualização de documentação.

---

## 2. Fonte de verdade por assunto

| Assunto | Fonte principal |
|---|---|
| Código-fonte | Repositório específico do produto |
| Arquitetura e decisões | PortalFS `/docs` |
| Banco de dados | Supabase + documentação no PortalFS |
| Deploy e domínios | Vercel + documentação no PortalFS |
| Backlog e auditorias | PortalFS `/docs` e Issues |
| Histórico técnico | Git e `CHANGELOG.md` |
| Credenciais e segredos | Gerenciadores seguros e variáveis de ambiente |

Nunca registrar senhas, chaves privadas, tokens, service role keys ou dados bancários completos em arquivos versionados.

---

## 3. Visão do ecossistema

O Portal FS reúne sistemas digitais simples, profissionais e acessíveis para pequenos empreendedores, prestadores de serviço e empresas.

### Produtos principais em desenvolvimento

- **Portal FS:** portal institucional e base central de documentação.
- **FS Delivery:** gestão de restaurantes, pedidos, salão, cozinha e entregas.
- **FS Fit:** gestão para personal trainers e alunos.
- **FS Orçamentos:** criação e gestão de orçamentos profissionais em PDF.

### Produtos planejados ou apresentados no portal

- FS Guinchos
- FS Reserva
- FS Checklist
- FS Eventos
- FS Tasks
- FS Assinaturas
- FS Links

Produtos planejados não devem ser tratados como implementados sem validação no respectivo repositório.

---

## 4. Tecnologias e infraestrutura

### Padrão atual

- HTML5
- CSS3
- JavaScript
- Layout responsivo
- GitHub para versionamento
- Vercel para hospedagem e deploy
- Supabase para autenticação, banco PostgreSQL e políticas RLS quando aplicável

### Diretrizes

- Preferir soluções simples antes de introduzir frameworks ou dependências adicionais.
- Evitar duplicação de CSS e JavaScript.
- Preservar compatibilidade mobile.
- Manter variáveis de ambiente fora do código.
- Documentar qualquer integração externa antes de colocá-la em produção.

---

## 5. Padrões de desenvolvimento

### HTML

- Estrutura semântica.
- Acessibilidade básica com labels, textos alternativos e estados visíveis.
- Navegação consistente entre páginas do mesmo produto.
- Não duplicar fontes de dados ou versões paralelas do mesmo componente.

### CSS

- Consolidar o tema no arquivo principal existente do produto.
- Evitar CSS adicional criado apenas para sobrescrever regras antigas.
- Usar variáveis CSS para cores, espaçamentos, bordas e sombras.
- Manter cards, menus, botões e modais visualmente consistentes.
- Validar responsividade em celulares antes de concluir uma alteração.

### JavaScript

- Separar responsabilidades por módulo ou domínio funcional.
- Evitar funções globais duplicadas.
- Tratar erros e estados de carregamento.
- Não esconder falhas apenas com `try/catch` vazio.
- Consolidar observers, listeners e inicializações para evitar execução repetida.
- Manter uma única fonte de verdade para pedidos, clientes, cardápio e sessão.

### Banco de dados

- Usar identificadores estáveis.
- Definir relacionamentos e índices.
- Aplicar Row Level Security em dados de usuários e estabelecimentos.
- Documentar tabelas, colunas, funções, triggers e políticas.
- Não depender apenas de validação no frontend.

---

## 6. Identidade visual compartilhada

O ecossistema deve manter reconhecimento visual comum, sem impedir que cada produto tenha personalidade própria.

### Diretrizes gerais

- Tipografia legível e consistente.
- Cards com arredondamento moderado.
- Hierarquia clara entre ação principal, secundária, alerta e informação.
- Ícones reconhecíveis acompanhados de texto quando necessário.
- Menu mobile padronizado conforme as páginas disponíveis para cada perfil.

### FS Fit

O tema escuro foi consolidado diretamente em `css/style.css`, sem CSS paralelo ou regras de override adicionais.

Paleta registrada:

- fundo preto grafite;
- cards chumbo;
- texto cinza quase branco;
- verde para ações e progresso;
- azul para informações e ações secundárias;
- amarelo para destaques.

Alterações futuras devem preservar essa consolidação.

---

## 7. Produto: Portal FS

### Objetivo

Apresentar o ecossistema, direcionar usuários aos produtos e funcionar como repositório mestre de conhecimento.

### Responsabilidades

- Página institucional.
- Catálogo de soluções.
- Identidade central da marca.
- Documentação compartilhada.
- Registro de decisões técnicas.
- Roadmap geral.
- Auditorias e pendências interprodutos.

### Regra de documentação

Uma informação importante discutida em auditoria ou desenvolvimento deve ser registrada neste repositório antes de a conversa correspondente ser apagada.

---

## 8. Produto: FS Delivery

### Objetivo

Plataforma profissional para restaurantes, lanchonetes e estabelecimentos de alimentação.

### Módulos previstos ou em desenvolvimento

- Dashboard
- Estabelecimentos e configurações
- Cardápio
- Categorias e produtos
- Clientes
- Pedidos
- Mesas
- QR Code de mesa
- Garçom
- Balcão
- Cozinha
- Entregador
- Página pública do estabelecimento
- Pedidos online
- Assinaturas
- Integração de pagamento
- Notificações

### Regras funcionais de pedidos

#### Pedido na mesa

- Não exibir campos de endereço.
- Identificar mesa por número ou nome personalizado.
- O acesso por QR Code deve abrir diretamente o fluxo local com a mesa selecionada.

#### Pedido online

- Não exibir campo de mesa.
- Exibir apenas dados relacionados a entrega ou retirada, cliente e forma de pagamento.
- O pedido deve aguardar confirmação do restaurante quando necessário.

#### Garçom e balcão

Separar os fluxos:

- **Local:** seleção de mesa.
- **Retirada:** nome e WhatsApp do cliente.
- **Entrega:** dados do cliente, endereço, taxa e pagamento.

Botões de novo pedido das páginas correspondentes devem abrir o fluxo correto, sem redirecionar indevidamente para a página do garçom.

### Clientes

- O número do WhatsApp deve funcionar como identificador operacional do cliente quando adequado.
- Criar cadastro automaticamente ao realizar o primeiro pedido.
- Permitir mais de um endereço por cliente.
- Ao realizar novo pedido, carregar endereços já salvos.
- Evitar registros duplicados por diferenças de formatação do telefone.

### Página pública

Pendências registradas para validação:

- alinhar o card de prazo de entrega, pedido mínimo e WhatsApp;
- usar o símbolo correto do WhatsApp no botão;
- exibir campos de cliente e endereço antes da finalização;
- deixar a arquitetura pronta para integração de pagamento pelo aplicativo;
- garantir que o cardápio público e os fluxos internos usem a mesma fonte de dados.

### Entregador

Objetivos registrados:

- receber pedidos prontos para entrega;
- visualizar endereço e abrir navegação;
- confirmar entrega;
- organizar lista de entregas pendentes;
- avaliar roteirização estratégica sem duplicar fontes de status.

### Configurações

Pendências registradas:

- links de acesso para cozinha, entregador e garçom;
- compartilhamento desses links;
- status de loja aberta ou fechada;
- dados completos do estabelecimento;
- QR Codes das mesas;
- isolamento correto dos dados por estabelecimento.

### Multiestabelecimento

Dados operacionais devem ser isolados por estabelecimento, utilizando identificador como `establishment_id` e políticas RLS equivalentes. Essa arquitetura precisa ser validada no banco antes de ser considerada concluída.

### Assinaturas e pagamentos

- Auditar frontend, backend e integração bancária.
- Registrar claramente quem recebe cada pagamento.
- Cada estabelecimento deve operar com conta própria ou modelo juridicamente validado.
- Não armazenar dados bancários sensíveis no repositório.
- Documentar a integração com Efí Bank quando consolidada.

---

## 9. Produto: FS Fit

### Objetivo

Plataforma para personal trainers organizarem alunos, treinos, exercícios, avaliações e assinaturas.

### Módulos conhecidos

- Personal trainer
- Alunos
- Treinos
- Exercícios
- Avaliações
- Página do aluno
- Página pública do personal
- Assinaturas

### Diretrizes preservadas

- Tema escuro consolidado em `css/style.css`.
- Não criar CSS paralelo para corrigir componentes isolados.
- Manter a experiência mobile como prioridade.
- Preservar a identidade visual já definida.
- Documentar alterações no sistema de assinatura e integrações financeiras.

### Pontos que exigem documentação específica no repositório do produto

- estrutura das tabelas;
- fluxo de criação e atribuição de treinos;
- autenticação e perfis;
- regras de acesso entre personal e aluno;
- integração de assinatura;
- domínio e deploy atual;
- pendências de UX e acessibilidade.

---

## 10. Produto: FS Orçamentos

### Objetivo

Gerar, gerenciar e exportar orçamentos profissionais em PDF.

### Tecnologias conhecidas

- HTML, CSS e JavaScript
- Supabase JavaScript v2
- Supabase Auth
- PostgreSQL
- `html2pdf.js`
- `localStorage` para dados auxiliares de interface

### Páginas e áreas conhecidas

- Página inicial e autenticação
- Painel de controle
- Orçamentos
- Sobre
- Contato
- Perfil da empresa
- Cabeçalho carregado dinamicamente

### Autenticação

- Login e cadastro por e-mail e senha.
- Sessão validada pelo Supabase.
- Páginas protegidas devem redirecionar usuários sem sessão.
- Logout deve limpar dados locais relacionados à sessão.
- Dados de perfil devem ser associados ao `auth.uid()`.

### Perfil

Campos conhecidos:

- nome;
- nome da empresa;
- WhatsApp ou telefone da empresa;
- plano;
- logotipo.

### Banco

Tabela conhecida: `perfis`.

Campos utilizados ou registrados:

- `id`;
- `nome`;
- `nome_empresa`;
- `telefone_empresa`;
- `plano`.

O cadastro passou a utilizar `upsert` para evitar falha quando o perfil já existe. Políticas RLS devem garantir que o usuário só grave e leia o próprio perfil.

### Pendência importante

O FS Orçamentos não possui fluxo completo de recuperação de senha nem link funcional “Esqueci a senha” no modal de login. Essa pendência deve ser tratada como funcional e de segurança operacional.

---

## 11. Autenticação e autorização

### Regras gerais

- Sessão deve ser verificada no carregamento de páginas protegidas.
- O frontend não substitui políticas RLS.
- Cada perfil deve acessar somente dados permitidos.
- Usuários de cozinha, garçom, entregador, personal, aluno e administrador precisam de permissões claras.
- Links públicos não podem expor operações administrativas.
- Mudanças de papel ou estabelecimento devem invalidar caches e permissões antigas.

### Dados locais

O `localStorage` pode auxiliar a interface, mas não deve ser fonte confiável para autorização ou dados financeiros.

---

## 12. Integrações e pagamentos

Toda integração deve possuir documentação com:

- finalidade;
- produto afetado;
- credenciais necessárias;
- variáveis de ambiente;
- endpoints;
- webhooks;
- estados de erro;
- idempotência;
- conciliação;
- política de reembolso;
- responsável pelo recebimento financeiro;
- ambiente de testes e produção.

### Efí Bank

A integração bancária deve ser documentada somente com identificadores públicos e nomes de variáveis. Segredos devem permanecer na Vercel, Supabase ou cofre seguro.

Antes de ativar cobranças para terceiros, validar o modelo jurídico, tributário e operacional. O Portal FS não deve assumir informalmente o papel de intermediador financeiro.

---

## 13. Deploy e operação

### Fluxo recomendado

1. Criar branch específica.
2. Implementar alterações relacionadas.
3. Executar testes locais ou verificações disponíveis.
4. Abrir Pull Request.
5. Validar preview da Vercel.
6. Fazer merge após validação.
7. Conferir produção.
8. Atualizar documentação e changelog.

### Checklist mínimo

- sem erros críticos no console;
- navegação funcional;
- responsividade validada;
- autenticação revisada;
- políticas RLS confirmadas;
- variáveis de ambiente presentes;
- build concluído;
- deploy acessível;
- fluxos principais testados;
- documentação atualizada.

---

## 14. Auditorias

Auditorias devem gerar arquivos próprios em futuras etapas.

Padrão de nome:

```text
docs/auditorias/AAAA-MM-DD-produto-assunto.md
```

Conteúdo mínimo:

- escopo;
- ambiente auditado;
- problemas encontrados;
- impacto;
- prioridade;
- arquivos envolvidos;
- recomendação;
- status da correção;
- PR ou commit relacionado;
- validação final.

### Prioridades

- **Crítica:** segurança, perda de dados, indisponibilidade ou cobrança incorreta.
- **Alta:** fluxo principal quebrado ou bloqueio de operação.
- **Média:** falha relevante de UX ou inconsistência funcional.
- **Baixa:** melhoria visual, organização ou otimização sem bloqueio.

---

## 15. Registro de decisões técnicas

Decisões estruturais devem usar ADRs.

Padrão:

```text
docs/decisoes/ADR-000-titulo.md
```

Conteúdo:

- contexto;
- problema;
- opções avaliadas;
- decisão;
- consequências positivas;
- riscos;
- data;
- status.

### Decisões já consolidadas

1. PortalFS é o repositório mestre de documentação.
2. O histórico de conversas não é fonte permanente de verdade.
3. CSS deve ser consolidado, evitando camadas de override.
4. Produtos podem ter repositórios próprios, mas decisões compartilhadas ficam no PortalFS.
5. Dados de usuários e estabelecimentos devem ser protegidos por autenticação e RLS.

---

## 16. Backlog inicial consolidado

### Prioridade crítica ou alta

- Validar isolamento multiestabelecimento no FS Delivery.
- Corrigir fluxos de pedidos que exibem campos incompatíveis com mesa, retirada ou entrega.
- Garantir cadastro de cliente e endereço no checkout público.
- Auditar assinaturas e pagamentos antes de produção.
- Criar recuperação de senha no FS Orçamentos.
- Validar QR Code de mesas e seleção automática da mesa.
- Revisar regressões provocadas por CSS, JavaScript e observers.

### Prioridade média

- Padronizar menus inferiores por perfil.
- Consolidar status de pedidos em todas as telas.
- Criar links compartilháveis para cozinha, garçom e entregador.
- Melhorar cards e alinhamentos da página pública.
- Consolidar notificações.
- Documentar tabelas e políticas RLS por produto.

### Prioridade baixa ou futura

- Analytics e relatórios avançados.
- Roteirização de entregas.
- Aplicativos móveis.
- APIs públicas.
- Marketplace de integrações.

O backlog deve ser revisado nos repositórios reais antes de marcar itens como concluídos.

---

## 17. Política de preservação antes de apagar conversas

Antes de excluir conversas antigas, confirmar:

- decisões importantes registradas;
- auditorias transferidas para Markdown;
- código enviado ao GitHub;
- deploy funcional;
- variáveis de ambiente preservadas;
- estrutura do banco documentada;
- pendências no backlog;
- links, domínios e integrações registrados;
- nenhuma credencial presente na conversa sem cópia segura em local apropriado.

Conversas concluídas podem ser apagadas depois que o conteúdo relevante estiver no GitHub.

---

## 18. Próxima organização recomendada

Este documento é a consolidação inicial. A evolução deve dividir o conteúdo sem perder o índice central:

```text
docs/
├── README.md
├── DOCUMENTACAO_GERAL.md
├── ARQUITETURA.md
├── PADROES_DESENVOLVIMENTO.md
├── IDENTIDADE_VISUAL.md
├── SEGURANCA.md
├── DEPLOY.md
├── BACKLOG.md
├── ROADMAP.md
├── auditorias/
├── decisoes/
└── produtos/
    ├── portal-fs.md
    ├── fs-delivery.md
    ├── fs-fit.md
    └── fs-orcamentos.md
```

A separação deve ocorrer gradualmente, evitando documentos vazios ou conteúdo duplicado.

---

## 19. Critério de atualização

Atualizar este documento quando houver:

- novo produto;
- mudança de arquitetura;
- nova integração;
- alteração de banco;
- novo fluxo de autenticação;
- mudança em pagamentos;
- auditoria relevante;
- alteração de padrão visual compartilhado;
- mudança de domínio ou hospedagem;
- decisão que afete mais de um produto.

---

## 20. Estado da documentação

Esta é uma consolidação inicial baseada nas decisões e informações preservadas até 5 de agosto de 2026. Alguns detalhes ainda precisam ser confirmados diretamente nos repositórios de cada produto e nos ambientes Supabase e Vercel.

Nenhum item deve ser considerado tecnicamente concluído apenas por aparecer neste documento. A confirmação final depende de inspeção do código, banco, configurações e deploy atual.
