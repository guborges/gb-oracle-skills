# GB Oracle Skills

Oracle não é só escrever `SELECT`.

É entender versão, contexto, RAC, Data Guard, impacto em produção, cardinalidade e plano de execução.

Este repositório organiza conhecimento real de campo em um formato simples (Markdown) para orientar agentes de IA e pessoas a produzirem SQL/PLSQL Oracle correto, seguro e performático — no padrão de um DBA Oracle que trabalha em ambiente crítico.

Sem mágica.  
Sem chute.  
Com responsabilidade.

---

## O que é isso?

Um pacote estruturado em:

- `skills/oracle/SKILL.md` → o mapa de decisão
- `skills/oracle/references/` → referências por tema (padrões, tuning, RAC/DG, RMAN, segurança)

A ideia é você apontar o agente para este repositório e ele usar como base para:

- escrever queries melhores
- evitar anti-patterns
- explicar trade-offs
- sugerir caminhos seguros para produção

---

## Como usar

Quando o assunto for Oracle:

1. Entender o objetivo (consulta, manutenção, tuning, crise)
2. Confirmar contexto mínimo:
   - Versão (19c, 23ai, 26ai?)
   - CDB/PDB?
   - RAC?
   - Data Guard?
   - Volume estimado?
3. Só então gerar SQL/procedimento
4. Sempre explicar impacto em produção quando houver risco

---

## Instalação

Exemplo de comando:

    npx skills add dbagustavoborges/gb-oracle-skills

Se você não usa tooling, basta clonar o repositório e utilizar os arquivos `SKILL.md` e `references/` como base de conhecimento.

---

## Filosofia

- Oracle é ambiente crítico
- Performance não é opcional
- Contexto vem antes da solução
- Toda query tem impacto
- RAC e Data Guard mudam decisões
- Em produção: sempre pensar em rollback e janela

---

## Estrutura do Projeto

    gb-oracle-skills/
    ├── README.md
    └── skills/
        └── oracle/
            ├── SKILL.md
            └── references/
                ├── version-awareness.md
                ├── sql-writing-standards.md
                ├── performance-mindset.md
                ├── rac-dg-asm.md
                ├── rman-restore-duplicate.md
                ├── production-safety.md
                └── troubleshooting-checklist.md

---

## Roadmap

- Referências aprofundadas por cenário real (AWR/ASH, SPM, stats, GC waits)
- Conteúdo específico para 19c / 23ai / 26ai
- Playbooks de crise (checklists rápidos)
- Anti-patterns comuns em ambientes Oracle

---

## Autor

Gustavo Borges  
Database Engineer / DBA Oracle  
https://dbagustavoborges.com.br
