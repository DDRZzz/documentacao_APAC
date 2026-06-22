# Documento de Visão

## 1. Problema e Oportunidade
* **O Problema**: Atualmente, o processo de preenchimento dos formulários APAC (Autorização de Procedimentos de Alta Complexidade) caracteriza-se por ser altamente manual, suscetível a erros operacionais e com elevado índice de retrabalho.
* **Impacto**: O alto grau de retrabalho gera desperdício de tempo, fazendo com que um processo "simples" seja extremamente repetitivo sem necessidade.
* **Solução Proposta**: O QuickAPAC é uma solução baseada em HITL (Human in the loop). Ele extrai os dados necessários do AGHU, preenche os campos estruturados da APAC como: Identificação do paciente, e logo após usa um dicionário para traduzir texto da evolução médica (campo não-estruturado) para campos estruturados necessários para emissão da APAC, como código do procedimento e CID principal e secundário.
  
## 2. Partes Interessadas (Stakeholders)
* Setor de Oncologia (HC), Ernani (Profissional do faturamento).

## 3. Escopo do Produto
* Extração de dados de prontuário do módulo Ambulatório do AGHU (campos estruturados).
* Geração da APAC com auxílio humano.
* Interface WEB para validação e aplicação de BI.

## 4. Metas e Objetivos de Negócio
* Maximizar o faturamento mensal efetivo do HC, reduzindo o índice de glosas por inconsistências de preenchimento.
* Reduzir o tempo médio de ciclo desde a solicitação médica até a emissão da APAC pronta para faturamento.
