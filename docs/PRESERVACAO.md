# Política de Preservação

## Objetivo

Permitir a limpeza do histórico de conversas sem perder informações essenciais dos projetos.

## Deve ser preservado

- Arquitetura vigente.
- Regras de negócio.
- Estrutura de banco e RLS.
- Integrações e fluxos de pagamento.
- Domínios, ambientes e estratégia de deploy.
- Auditorias ainda abertas.
- Decisões de UX e identidade visual.
- Roadmap e backlog.
- Procedimentos de recuperação e operação.

## Não deve ser preservado como conhecimento permanente

- Conversas casuais.
- Testes descartáveis.
- Erros já resolvidos sem valor histórico.
- Commits e deploys antigos sem relevância atual.
- Hipóteses substituídas.
- Dados pessoais ou credenciais.

## Antes de apagar uma conversa

1. Confirmar se houve decisão importante.
2. Registrar a decisão no arquivo adequado.
3. Registrar pendências no backlog.
4. Salvar evidências úteis no repositório correto.
5. Confirmar que nenhuma credencial foi incluída.

## Regra de qualidade

A documentação deve distinguir claramente:

- implementado e validado;
- implementado sem validação completa;
- planejado;
- hipótese;
- pendência.

Nunca apresentar como concluída uma funcionalidade baseada apenas em conversa ou intenção.
