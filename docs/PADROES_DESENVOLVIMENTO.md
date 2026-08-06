# Padrões de Desenvolvimento

## HTML

- Usar estrutura semântica.
- Manter navegação consistente.
- Incluir labels, estados de erro e acessibilidade básica.
- Evitar páginas paralelas para o mesmo fluxo.

## CSS

- Consolidar o tema no arquivo principal do produto.
- Evitar arquivos criados apenas para sobrescrever regras antigas.
- Usar variáveis CSS para cores, espaçamentos, bordas e sombras.
- Preservar responsividade mobile.
- Manter cards, botões, menus e modais visualmente consistentes.

## JavaScript

- Separar responsabilidades por domínio.
- Evitar funções globais duplicadas.
- Consolidar listeners, observers e inicializações.
- Tratar carregamento, vazio, sucesso e erro.
- Não ocultar falhas com blocos vazios.
- Manter uma única fonte de verdade por entidade.

## Banco de dados

- Documentar tabelas, colunas, índices, funções, triggers e políticas.
- Aplicar RLS em dados privados.
- Não depender apenas de validação do frontend.
- Usar identificadores estáveis.

## Git e deploy

- Agrupar alterações relacionadas em commits coerentes.
- Registrar mudanças importantes em Markdown.
- Validar console, responsividade, banco e fluxo principal antes do merge.
- Não versionar segredos.
