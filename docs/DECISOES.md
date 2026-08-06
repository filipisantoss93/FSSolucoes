# Registro de Decisões

## Decisões vigentes

### ADR-001 — PortalFS como repositório mestre

- Status: aceito
- Data: 2026-08-05
- Decisão: centralizar no PortalFS a documentação estratégica do ecossistema FS.
- Motivo: impedir que arquitetura, regras, auditorias e pendências dependam do histórico de conversas.
- Consequência: decisões relevantes devem gerar atualização em Markdown.

### ADR-002 — Código permanece nos repositórios dos produtos

- Status: aceito
- Data: 2026-08-05
- Decisão: o PortalFS não substitui os repositórios específicos de código.
- Motivo: preservar separação, deploy e histórico próprios de cada produto.

### ADR-003 — CSS consolidado

- Status: aceito
- Decisão: evitar arquivos paralelos e camadas de override sem necessidade.
- Motivo: reduzir inconsistência visual e dificuldade de manutenção.

### ADR-004 — Segredos fora do Git

- Status: aceito
- Decisão: senhas, tokens, service role keys, chaves privadas e dados bancários sensíveis não podem ser versionados.

## Modelo para novas decisões

```md
### ADR-XXX — Título

- Status: proposto | aceito | substituído | rejeitado
- Data:
- Contexto:
- Decisão:
- Motivo:
- Consequências:
- Evidências:
```
