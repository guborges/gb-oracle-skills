# GB Oracle Skills

Oracle não é só escrever `SELECT`.

É entender versão, contexto, RAC, Data Guard, impacto em produção, cardinalidade e plano de execução.

Este repositório organiza conhecimento **real de campo** em um formato simples (Markdown) para orientar agentes de IA e pessoas a produzirem SQL/PLSQL Oracle **correto, seguro e performático** — no padrão de um DBA Oracle que trabalha em ambiente crítico.

Sem mágica.  
Sem chute.  
Com responsabilidade.

---

## O que é isso?

Um pacote estruturado em:

- `skills/oracle/SKILL.md` → o “mapa” do skill (como pensar / como decidir)
- `skills/oracle/references/` → referências por tema (padrões, tuning, RAC/DG, RMAN, segurança)

A ideia é você apontar o agente para este repositório e ele usar como “fonte” para:
- escrever queries melhores
- evitar anti-patterns
- explicar trade-offs
- sugerir caminhos seguros para produção

---

## Como usar (na prática)

Quando o assunto for Oracle, use este repositório como base e siga a ordem:

1) Entender o objetivo (consulta, manutenção, tuning, crise)
2) Confirmar contexto mínimo (versão, CDB/PDB, RAC/DG, volume)
3) Só então gerar SQL/procedimento com cautela
4) Sempre explicar impacto em produção quando houver risco

---

## Instalação

> Exemplo de comando para instalar como “skill” via tooling externo:

```bash
npx skills add dbagustavoborges/gb-oracle-skills
