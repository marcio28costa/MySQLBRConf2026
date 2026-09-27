# Guia de Upgrade do MySQL

### MySQL BR Conf 2026

**Palestrante:** Márcio Costa  
**Tech Lead DBA — Citel Software**  
**OCP MySQL • Oracle ACE Associate**

---

## Sobre a apresentação

Um guia prático para planejar, executar e validar upgrades do MySQL com foco em **segurança, compatibilidade, disponibilidade, performance e redução de riscos**.

A proposta é tratar o upgrade como uma **mudança de plataforma**, e não simplesmente como a instalação de um novo pacote.

> **Upgrade = segurança + suporte + performance + evolução**

---

## O caminho da palestra

Do cenário atual ao ambiente validado:

1. **Diagnóstico**
2. **Estratégia**
3. **Segurança**
4. **Execução**
5. **Validação**
6. **Monitoramento**

Perguntas que orientam o processo:

- Onde estou?
- Para onde vou?
- Posso ir?
- Como vou?
- Deu certo?

---

## 1. Por que fazer upgrade?

Um upgrade pode estar relacionado a diferentes objetivos:

- **Segurança** — correções e redução de exposição.
- **Suporte** — manter-se em uma versão suportada.
- **Performance** — otimizações e melhor uso de recursos.
- **Funcionalidades** — novos recursos e evolução.

O ponto central é avaliar o upgrade como uma mudança de plataforma, considerando todo o ambiente envolvido.

---

## 2. Planejamento

Antes de executar, é importante saber exatamente **de onde o ambiente está saindo e onde pretende chegar**.

### Versão atual

- Qual versão?
- Qual suporte?
- Quais recursos estão em uso?
- Qual é o baseline de performance?

### Versão desejada

- LTS ou Innovation?
- Minor ou mudança de série?
- Compatibilidade
- Benefícios esperados

### Ambiente

- Sistema operacional e bibliotecas
- Aplicações e drivers
- Backup e janela de manutenção
- Plano de rollback

### Pergunta-chave

> **Qual é o menor risco para chegar ao destino?**

---

## 3. Upgrade Checker

O **MySQL Shell Upgrade Checker** ajuda a identificar problemas antes que eles sejam descobertos em produção.

### Fluxo

```text
MySQL atual
     ↓
MySQL Shell
     ↓
Upgrade Checker
     ↓
Relatório
     ↓
Correções
```

### O que procurar

- Incompatibilidades
- Configurações
- Recursos depreciados
- Objetos e comportamentos

### Depois do relatório

1. Corrigir
2. Testar
3. Reexecutar o checker
4. Documentar exceções

### Objetivo

> Chegar ao upgrade com problemas conhecidos, e não com surpresas.

---

## 4. Métodos de upgrade

A estratégia deve considerar principalmente **risco e downtime**.

### In-place

Execução no mesmo servidor.

**Características:**

- Simples
- Rápido
- Exige janela de manutenção

Pode ser adequado quando o downtime é aceitável.

### Replicação

Preparação de um novo servidor com a versão desejada.

**Características:**

- Preparação antecipada
- Validação antes do corte
- Possibilidade de minimizar o downtime

Pode ser utilizado em cenários de produção crítica.

### Outras estratégias

- Logical dump & load
- MySQL Clone
- Backup & Restore

> A estratégia deve ser escolhida de acordo com o cenário, os requisitos de disponibilidade e o risco aceitável.

---

## 5. Rollback

> **Se não existe plano de volta, o plano de upgrade está incompleto.**

### Antes

- Backup testado
- Snapshot ou servidor anterior
- Procedimento documentado

### Critérios

Definir antecipadamente:

- Quando abortar?
- Quem decide?
- Qual é a janela?
- Qual evidência será utilizada?

### Possibilidades de retorno

- Restore
- Retorno à réplica
- Reverter switch-over
- Comunicar o impacto

O rollback deve fazer parte do planejamento desde o início.

---

## 6. Upgrade com replicação

A ideia é **preparar a nova versão antes de mexer na produção**.

```text
Produção
MySQL atual
     ↓
Replicação
     ↓
Nova versão
     ↓
Validação
     ↓
SWITCHOVER
```

### Preparação

- Novo servidor
- Nova versão
- Configuração
- Segurança

### Sincronização

- Replicação em andamento
- Acompanhar lag
- Validar dados

### Troca

- Teste final
- Switch-over
- Monitoramento
- Rollback preparado

> **Objetivo: minimizar a indisponibilidade, não apenas reduzir o tempo do upgrade.**

---

## 7. Performance

A performance deve ser medida **antes, durante e depois** do upgrade.

### Antes

Estabelecer o baseline:

- CPU
- Memória
- I/O
- Latência
- Throughput
- Queries críticas
- Parâmetros e configuração

### Durante

Acompanhar:

- Recursos
- Erros e waits
- Replicação
- Comportamento da aplicação

### Depois

- Comparar com o baseline
- Confirmar ganhos
- Detectar regressões
- Ajustar parâmetros
- Documentar o resultado

### Ferramentas

- PMM
- Performance Schema
- sys/schema
- Prometheus
- Grafana

---

## 8. Escolha pensando no ciclo de vida

A versão desejada precisa fazer sentido **hoje e também amanhã**.

A apresentação aborda a evolução das versões e o modelo **LTS / Innovation**, destacando a importância de considerar:

- Estabilidade e ciclo de vida
- Padronização de produção
- Novidades mais cedo
- Capacidade de testar e atualizar com frequência

> A escolha da versão deve fazer parte da estratégia de longo prazo do ambiente.

---

## 9. Validação pós-upgrade

> **Upgrade concluído não significa projeto concluído.**

A validação deve considerar quatro dimensões.

### Aplicação

- Conexão
- Queries críticas
- Drivers
- Funcionalidades

### Banco

- Erros
- Logs
- Objetos
- Replicação

### Performance

- Latência
- Throughput
- CPU / memória
- I/O

### Negócio

- Fluxos críticos
- SLA
- Usuários
- Aceite

> **Só finalize quando o ambiente estiver validado e monitorado.**

---

## 10. Boas práticas

Upgrade seguro é **processo, não comando**.

```text
Planejar
   ↓
Testar
   ↓
Executar
   ↓
Validar
   ↓
Monitorar
```

### Sistema operacional

Sempre que possível, alinhar o upgrade do MySQL ao ciclo de vida do sistema operacional, considerando:

- Bibliotecas
- Correções
- Segurança

### Homologação

- Repetir o processo antes da produção
- Testar aplicação + banco

### Documentação

Manter:

- Checklist
- Evidências
- Critérios de sucesso
- Plano de rollback

---

## Checklist resumido

Antes de iniciar:

- [ ] Versão atual identificada
- [ ] Versão desejada definida
- [ ] Suporte e ciclo de vida avaliados
- [ ] Compatibilidade analisada
- [ ] Upgrade Checker executado
- [ ] Problemas identificados e tratados
- [ ] Baseline de performance definido
- [ ] Estratégia de upgrade definida
- [ ] Backup testado
- [ ] Plano de rollback documentado
- [ ] Critérios de sucesso definidos
- [ ] Plano de validação preparado
- [ ] Monitoramento preparado

Durante:

- [ ] Execução acompanhada
- [ ] Replicação/lag monitorados quando aplicável
- [ ] Erros e comportamento da aplicação acompanhados
- [ ] Critérios de abortamento conhecidos

Depois:

- [ ] Aplicação validada
- [ ] Banco validado
- [ ] Performance comparada
- [ ] Fluxos de negócio validados
- [ ] Monitoramento ativo
- [ ] Evidências documentadas
- [ ] Aceite realizado

---

## Principais mensagens

> **Planeje antes de executar.**

> **Descubra os problemas antes de descobrir em produção.**

> **Escolha a estratégia de acordo com o risco e o downtime.**

> **Se não existe plano de volta, o plano de upgrade está incompleto.**

> **Meça antes, acompanhe durante e compare depois.**

> **Upgrade concluído não significa projeto concluído.**

> **Upgrade seguro é processo, não comando.**

---

## Agradecimento especial

### Apoio à apresentação — Citel Software

Agradeço à **Citel Software** pelo apoio à realização desta apresentação e por incentivar o compartilhamento de conhecimento técnico com a comunidade.

A Citel é uma empresa de tecnologia com atuação no desenvolvimento de sistemas para gestão do varejo, tendo como principal produto o **ERP Autcom**.

**Conheça a Citel Software:**

[https://www.citelsoftware.com.br/](https://www.citelsoftware.com.br/)

---

## Sobre o palestrante

**Márcio Costa**

- Tech Lead DBA — Citel Software
- OCP MySQL
- Oracle ACE Associate
- Especialista em bancos de dados e ambientes MySQL

---

## Referências

A apresentação utiliza como referências:

- MySQL Reference Manual
- Documentação de releases MySQL
- Documentação de Upgrade / Downgrade do MySQL
- Documentação do MySQL Shell Upgrade Checker
- Oracle Lifetime Support Policy
- Comunidade MySQL Brasil

### Links

- [MySQL Documentation](https://dev.mysql.com/doc/)
- [MySQL](https://www.mysql.com/)
- [Oracle](https://www.oracle.com/)
- [Comunidade MySQL Brasil](https://comunidademysql.com.br/)
- [Citel Software](https://www.citelsoftware.com.br/)

---

## Comunidade

**Conhecimento conecta pessoas.**

Obrigado pela presença, pelas perguntas e pela troca de experiências.

Vamos continuar:

**Conectar • Aprender • Compartilhar**

---

### MySQL BR Conf 2026

**Guia de Upgrade do MySQL**  
**Márcio Costa — Tech Lead DBA | Citel Software**

