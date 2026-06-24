# Documento de Visão

## 1. Problema e Oportunidade
* **O Problema**: Atualmente, dentro do Hospital das Clínicas da UFPE, o processo de emissão/preechimento de APAC é caracterizado por um alto nível de retrabalho, sendo necessário o preenchimento de dados redundantes em diferentes tipos de documentos (Papel, Planilha, Sistemas Setélites), de forma manual. Nesse sentido, a principal dor identificada é o tempo gasto com atividades repetitivas de preenchimento de formulários.
* **Impacto**: Esse tipo de problema, impacta diretamente o profissional responsável, pois trata-se de um trabalho repetitivo e minucioso, onde qualquer pequeno erro pode causar uma perda para o Hospital, no caso da APAC perda financeira, visto que, se trata de uma das 3 formas que o HC tem de faturar.
* **Solução Proposta**: O QuickAPAC é uma solução de automação projetada para extrair dados do Prontuário Eletrônico do Paciente (PEP) no AGHU e acelerar a geração de formulários APAC. A arquitetura do sistema é baseada no modelo Human-in-the-Loop (HITL), o sistema trata dois tipos de informações.

  * Dados Estruturados: Informações já categorizadas no banco de dados do AGHU. O papel do sistema é apenas coletar, formatar e mapear esses dados diretamente para o layout de arquivo texto (fixed-width) exigido pelo padrão da APAC.

  * Dados Não Estruturados (Evolução Médica): O sistema realiza a leitura inteligente do texto livre escrito pelo médico. Ele consulta um dicionário interno para reconhecer termos e jargões, convertendo-os automaticamente nos Códigos de Procedimento, CID Principal e CID Secundário necessários para o faturamento. Caso uma palavra não exista previamente no dicionário, o sistema sinaliza a pendência e permite que um humano faça o mapeamento correto. Essa nova palavra é então incorporada, garantindo que o vocabulário do sistema cresça de forma orgânica e se torne cada vez mais autônomo com o passar do tempo.
  
## 2. Partes Interessadas (Stakeholders)
* Ernani (usuário final), Oncologia HC, Faturamento HC.

## 3. Escopo do Produto
* Extração de daods do AGHU.
* Preenchimento da APAC no formato de texto (fixed-width).
* Interface WEB para validação e aplicação de BI.
* Exportação para sistema satélite APAC magnético.

## 4. Metas e Objetivos de Negócio
* Reduzir o tempo de preenchimento de APAC.
