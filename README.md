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

## Como utilizar

Clone o repositório:

git clone https://github.com/dbagustavoborges/gb-oracle-skills.git

Utilize:

- skills/oracle/SKILL.md como documento base de raciocínio
- skills/oracle/references/ como base técnica aprofundada

Configure sua ferramenta de IA para incluir esses arquivos como contexto permanente.

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
