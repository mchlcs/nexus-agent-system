---
title: "Improve Agent — Lifecycle Loop"
version: 1.0.0
created: 2026-05-12
audience: nexus (executa), todos os agentes (alvo)
trigger: "Run improve-agent.md on [nome-do-agente]"
---

# Improve Agent — Loop de Melhoria Contínua

Este documento define o processo de melhoria iterativa de qualquer agente do sistema.
Inspirado no padrão Improve → Hill Climb da plataforma Agno.
Executado pelo Nexus. Máximo de 5 rounds por sessão.

## Quando executar

- Após um agente falhar em uma tarefa real
- Antes de um agente ser usado em contexto crítico pela primeira vez
- Semanalmente como manutenção preventiva (escolher 1 agente por semana)
- Após qualquer mudança no `docs/constitution.md` ou `docs/standards.md`

## O Loop

### Fase 1 — Leitura do agente
1. Ler o arquivo `.md` do agente alvo completamente
2. Identificar as **promessas** do agente: o que ele afirma fazer?
3. Listar as regras, outputs esperados e anti-padrões declarados

### Fase 2 — Derivação de probes (8–12 por sessão)
Criar casos de teste a partir das promessas. Distribuir entre:

| Tipo | Quantidade | Exemplos |
|------|-----------|---------|
| Golden path | 3–4 | Input perfeito, output esperado |
| Edge cases | 2–3 | Input incompleto, ambíguo, vazio |
| Tool selection | 1–2 | Tarefa que exige ferramenta específica |
| Adversarial | 1–2 | Prompt injection, input malformado, desvio de propósito |

### Fase 3 — Execução dos probes
Para cada probe:
1. Enviar o input ao agente
2. Ler o output
3. Julgar **PASS** ou **FAIL** contra o que as instruções prometem
4. Registrar: probe, resultado, motivo

### Fase 4 — Diagnóstico de falhas
Para cada FAIL, escolher o lever correto:

| Tipo de falha | Lever |
|---|---|
| Regra ausente nas instruções | Adicionar regra |
| Regra ambígua | Reescrever com critério objetivos |
| Ferramenta errada escolhida | Ajustar descrição de quando usar cada ferramenta |
| Output fora do formato | Adicionar exemplo de output ao arquivo |
| Agente saiu do escopo | Fortalecer boundary nas regras |
| Alucinação | Adicionar "se não encontrar X, declarar explicitamente" |

### Fase 5 — Edição e re-teste
1. Editar o arquivo `.md` do agente (cirurgicamente — uma mudança por falha)
2. Re-executar **apenas os probes que falharam**
3. Se passar → marcar como resolvido
4. Se ainda falhar → aplicar lever diferente
5. Repetir até PASS ou cap de 5 rounds

### Fase 6 — Regressão
Após todos os FAILs resolvirem, re-executar o **conjunto completo** de probes
para garantir que as correções não criaram regressões.

### Fase 7 — Registro
Chamar Ledger com:
- Nome do agente melhorado
- Probes executados (total, PASS, FAIL inicial, FAIL final)
- Mudanças feitas (lista de diffs resumidos)
- Próxima data sugerida para novo improve

## Template de registro de probe
Probe: [descrição do input]  
Tipo: [golden/edge/tool/adversarial]  
Input: [o que foi enviado]  
Output esperado: [o que as instruções prometem]  
Output recebido: [resumo do que veio]  
Resultado: PASS | FAIL  
Lever aplicado (se FAIL): [descrição da mudança]

## Critério de encerramento do loop

O loop encerra quando:
- Todos os probes passam, OU
- 5 rounds foram completados (registrar FAILs restantes como tech debt)

Nunca encerrar sem registrar no Ledger.