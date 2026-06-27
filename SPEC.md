# SPEC.md - Contrato de Desenvolvimento (SDD)

## 1. Visão Geral e Resultados Esperados
Este documento é a ÚNICA fonte de verdade para a orquestração do desenvolvimento do **QuickAPAC**. O sistema visa automatizar a geração de APACs extraindo dados do AGHU via consultas SQL nativas, cruzando textos de evolução com um Dicionário de Termos local (SQLite) via busca determinística (CTRL+F), e provendo uma interface *Human-in-the-Loop* (Vue 3) para Auditores grifarem novos jargões.

### Objetivos de Alto Nível
- [ ] Implementar motor de ETL usando o padrão `Provider` e `SQL Templates` do framework do HC.
- [ ] Construir o motor determinístico no `Controller` do Back-end.
- [ ] Desenvolver a interface visual de marca-texto (Highlight) no Vue 3.
- [ ] Exportar arquivo posicional `.txt` do Datasus.

## 2. Contexto do Projeto (Documentação Imutável)
As definições detalhadas estão distribuídas nos seguintes documentos:
- [Visão](01-visao.md)
- [Requisitos](02-requisitos.md)
- [Casos de Uso](03-casos-uso.md)
- [Modelo de Dados](04-modelo-dados.md)
- [Interfaces](05-interfaces.md)
- [Arquitetura](06-arquitetura.md)
- [Glossário](07-glossario.md)

## 3. Limites de Escopo e Guardrails (Anti-Patterns)
**A IA DEVE:**
- Respeitar estritamente o fluxo unidirecional: `Router` -> `Controller` -> `Provider`.
- Usar SQLAlchemy APENAS para o banco local (`DICIONARIO_TERMOS` e `APAC_PROCESSAMENTO`).
- Usar consultas `.sql` puras (`src/providers/sql/...`) para acessar o AGHU.
- Proteger todas as rotas da API com o `auth_handler` existente no framework.

**A IA NÃO DEVE:**
- Criar rotas para "Cadastro de Pacientes" (a extração é 100% via AGHU).
- Sugerir o uso de bibliotecas de Inteligência Artificial ou NLP (como spaCy, NLTK).
- Colocar regras de negócio dentro do arquivo `Router` ou do `Provider`. Toda a lógica (o "CTRL+F") vive no `Controller`.

## 4. Task Breakdown (Plano de Implementação)

### Fase 1: Infraestrutura de Dados (Providers)
- [ ] **[TASK-001]** Criar o SQL Template (`extrair_evolucao_apac.sql`) e o respectivo `AghuProvider` para buscar pacientes de alta complexidade.
- [ ] **[TASK-002]** Criar os Models SQLAlchemy (`DicionarioTermo` e `ApacProcessamento`) e o `DicionarioProvider` para o banco de dados transacional (SQLite/Postgres interno).

### Fase 2: Motor de Regras (Controllers)
- [ ] **[TASK-003]** Desenvolver o `ApacController`, contendo a lógica de varredura: receber o texto do `AghuProvider`, cruzar com os termos do `DicionarioProvider` e retornar as posições (índices) das palavras mapeadas para o Front-end.
- [ ] **[TASK-004]** Desenvolver a lógica de formatação posicional (`zfill`, `ljust`) no Controller para gerar o arquivo de exportação `.txt`.

### Fase 3: APIs e Rotas (Routers)
- [ ] **[TASK-005]** Criar o `router/apac.py` expondo os endpoints para listar pacientes do dia, adicionar palavra ao dicionário e gerar o download do TXT. (Todas as rotas injetando o `Depends(auth_handler.decode_token)`).

### Fase 4: Interface de Usuário (Vue 3 Frontend)
- [ ] **[TASK-006]** Criar a Store no Pinia (`stores/apac.ts`) para gerenciar o estado do lote e a comunicação com a API (Axios).
- [ ] **[TASK-007]** Desenvolver a View de Dashboard com a tabela de pacientes (`status: PRONTA / EM_ANALISE`).
- [ ] **[TASK-008]** Desenvolver o Componente de *Highlight* (Marca-texto): Renderizar a evolução clínica, pintar de verde os termos conhecidos, e capturar o evento de seleção de texto (mouse) para abrir o Modal de inserção no Dicionário.

## 5. Critérios de Verificação Global
- [ ] A varredura de texto deve possuir cobertura de testes (`pytest`) validando cenários de extração exata das substrings.
- [ ] Zero dependências de IA instaladas no `requirements.txt`.
- [ ] Nenhum acesso de gravação (`INSERT`/`UPDATE`) no provider do AGHU.
