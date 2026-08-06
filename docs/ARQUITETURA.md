# Arquitetura do Ecossistema FS

## Princípio central

O PortalFS é o repositório mestre de documentação. Cada produto pode manter seu próprio código-fonte em repositório separado.

## Fontes de verdade

- Código: repositório específico do produto.
- Decisões e padrões compartilhados: PortalFS.
- Banco e autenticação: Supabase e documentação correspondente.
- Deploy e domínios: Vercel e documentação correspondente.
- Pendências: backlog, auditorias e Issues.

## Tecnologias predominantes

- HTML5
- CSS3
- JavaScript
- Supabase/PostgreSQL
- GitHub
- Vercel

## Diretrizes

- Preferir arquitetura simples e verificável.
- Evitar duplicação de CSS, JavaScript e fontes de dados.
- Separar dados por usuário ou estabelecimento com RLS.
- Manter segredos apenas em variáveis de ambiente.
- Documentar integrações antes da produção.
- Validar fluxos completos: interface, banco, regras e deploy.

## Produtos

- Portal FS
- FS Delivery
- FS Fit
- FS Orçamentos

Produtos adicionais devem ser classificados como planejados até existir validação no repositório correspondente.
