# Glossário e Referências

## 1. Termos Técnicos e Siglas

### Siglas Médicas e de Faturamento
* **AGHU**: Aplicação de Gestão para Hospitais Universitários. Sistema principal de onde os dados clínicos são extraídos.
* **APAC**: Autorização de Procedimentos de Alta Complexidade. Documento/formulário padrão exigido pelo SUS para faturamento de procedimentos específicos (como oncologia).
* **CID**: Classificação Internacional de Doenças (frequentemente usado como CID Principal e CID Secundário).
* **PEP**: Prontuário Eletrônico do Paciente.
* **SIA/SUS**: Sistema de Informações Ambulatoriais do Sistema Único de Saúde. É o sistema governamental (APAC Magnético) que vai receber e processar o arquivo `.txt` gerado pelo QuickAPAC.
* **SIGTAP**: Sistema de Gerenciamento da Tabela de Procedimentos, Medicamentos e OPM do SUS. A tabela oficial de onde vêm os códigos de procedimentos que o Auditor mapeia no dicionário.
* **HIS/TISS**: *Hospital Information System* e Troca de Informação de Saúde Suplementar. Padrões gerais de sistemas hospitalares.

### Siglas de Arquitetura e Engenharia de Software
* **AD / LDAP**: *Active Directory* e *Lightweight Directory Access Protocol*. Tecnologias da Microsoft utilizadas pelo HC para centralizar o login e senhas dos funcionários.
* **ETL**: *Extract, Transform, Load* (Extrair, Transformar e Carregar). Refere-se à rotina do back-end que busca os dados no AGHU, processa os jargões e formata a saída.
* **HITL**: *Human-in-the-Loop*. Arquitetura central do QuickAPAC, onde o sistema tenta automatizar o máximo possível (varredura visual), mas coloca um humano (Auditor) no ciclo de decisão para ensinar novas palavras ao sistema.
* **LGPD**: Lei Geral de Proteção de Dados.
* **RBAC**: *Role-Based Access Control* (Controle de Acesso Baseado em Papéis). Garantia de que apenas usuários com a "role" de faturista/auditor acessem o sistema.
* **SPA**: *Single Page Application*. Arquitetura do front-end web (em Vue 3) em que o sistema não recarrega a página ao navegar.

## 2. Referências
* Manuais de Integração e Regras de Faturamento do SIA/SUS (Ministério da Saúde).
* Documentação de Layout do Arquivo Posicional de Exportação (APAC Magnético).
* Normas da ANS, CFM e manuais internos do Hospital das Clínicas (HC).
