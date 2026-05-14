---
title: "Standards & Anti-Patterns"
version: 1.0.0
created: 2026-05-12
updated: 2026-05-12
audience: forge, shield, pixel
---

# Padrões do Sistema

Critérios objetivos de qualidade para código, documentação e outputs dos agentes.
Shield usa este arquivo como rubrica de revisão.
Forge usa como checklist antes de entregar.

## Código

### Obrigatório
- Tipagem estrita em todo código novo (TypeScript strict mode / Python type hints)
- Testes junto com implementação — nunca depois
- Cobertura mínima: 80% em código novo, 60% em código legado tocado
- Funções: máximo 80 linhas, máximo 3 níveis de aninhamento
- Nomes: descritivos, em inglês, sem abreviações obscuras
- Sem `TODO` sem issue linkada
- Sem segredos hardcoded — usar variáveis de ambiente

### Proibido
- Lógica duplicada — refatorar antes de duplicar
- `any` em TypeScript sem comentário justificando
- Queries SQL sem parametrização
- `console.log` em produção — usar logger estruturado
- Commits diretos em `main` sem PR

### Formatação
- Prettier para JS/TS, Black para Python
- Lint passa antes de qualquer entrega
- Imports organizados: stdlib → third-party → local

## Documentação

### Obrigatório
- Todo arquivo de agente tem front matter completo (name, role, model, version, triggers)
- Todo ADR tem: data, status, contexto, decisão, alternativas rejeitadas, consequências
- READMEs têm: propósito, como rodar localmente, variáveis de ambiente, links
- Changelogs seguem Keep a Changelog (Added / Changed / Fixed / Removed / Deprecated)

### Anti-padrões de documentação
- ❌ "Melhora a performance" sem número
- ❌ "Refatoração" sem descrever o que mudou
- ❌ Doc desatualizada — se o código mudou, o doc muda junto
- ❌ ADR sem alternativas rejeitadas (decisão sem contexto não é rastreável)

## Outputs de agentes

### Todo output de agente deve ter
- Critério de done cumprido (sim/não + evidência)
- Arquivos alterados listados
- Próxima ação sugerida
- Flag se requer Shield review

### Níveis de revisão

| Situação | Revisão necessária |
|---|---|
| Novo endpoint público | Shield obrigatório |
| Mudança em auth/permissões | Shield obrigatório |
| Novo ADR proposto | Shield obrigatório |
| Refactor sem mudança de comportamento | Forge auto-verifica |
| Componente UI novo | Pixel entrega + Herald valida comunicação |
| Documentação interna | Herald entrega direto |

## Nomenclatura de arquivos

- Agentes: `kebab-case.md` (ex: `forge.md`)
- ADRs: `NNNN-titulo-em-kebab.md` (ex: `0001-escolha-de-orm.md`)
- Sessions logs: `YYYY-MM-DD-agente.md` (ex: `2026-05-12-forge.md`)
- Componentes: `PascalCase.tsx` (ex: `UserCard.tsx`)
- Utilitários: `camelCase.ts` (ex: `formatDate.ts`)