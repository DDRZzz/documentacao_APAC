# Arquitetura e Segurança

## 1. Padrões Arquiteturais
O QuickAPAC foi projetado com foco em alta previsibilidade, segurança de dados em saúde e baixo acoplamento com o sistema legado do hospital.

* **Arquitetura Determinística:** O motor de processamento de texto livre não utiliza modelos de inteligência artificial ou processamento de linguagem natural (NLP). A busca é baseada puramente em uma Tabela de Dispersão.
* **Human-in-the-Loop (HITL):** A inteligência de descoberta de novos termos é transferida do Back-end para o Front-end. O sistema aplica *highlight* (marca-texto) no que conhece, e o usuário alimenta o banco de dados selecionando manualmente as lacunas.
* **ETL (Staging Area):** O QuickAPAC consome os dados do AGHU (Extração), aplica os de-para de faturamento (Transformação) e gera o arquivo posicional (Carga/Exportação). Os dados clínicos da evolução ficam retidos no banco apenas durante o ciclo de vida do lote.

## 2. Stack Técnica
A stack definida para o projeto segue as diretrizes do framework base da instituição:
* **Back-end:** FastAPI (Python), orquestrando rotas, controladores e provedores.
* **Front-end:** Vue 3, gerenciamento de estado com Pinia e estilização via Tailwind CSS.
* **Banco de Dados:** Consultas nativas (.sql) para leitura do AGHU (PostgreSQL/Oracle) e ORM SQLAlchemy exclusivo para o banco transacional local da aplicação (SQLite/Postgres interno).

## 3. Segurança e Conformidade (LGPD)
Sendo um sistema de retaguarda hospitalar, o tráfego e acesso aos dados seguem regras estritas:

* **Minimização de Retenção:** O sistema não clona o Prontuário Eletrônico do Paciente (PEP). Os textos de evolução são armazenados temporariamente na tabela `APAC_PROCESSAMENTO` apenas até a exportação do lote, podendo ser expurgados ou arquivados na sequência.
* **Comunicação Read-Only:** A integração com o AGHU possui credenciais exclusivas de leitura, garantindo que o QuickAPAC não possa, sob nenhuma hipótese, alterar o prontuário original.
* **Autenticação e RBAC:** Acesso restrito via LDAP/AD corporativo. Apenas usuários do grupo de faturamento/oncologia podem emitir arquivos e retroalimentar o dicionário.
* **Criptografia:** Tráfego de dados protegido por TLS 1.2+ e criptografia AES-256 para os dados sensíveis em repouso no banco de dados.

## 4. Guardrails para IA (SDD)
### Escopo Positivo (O que fazer)
- **Fluxo Unidirecional:** Respeitar a arquitetura estrita: `Router -> Controller -> Provider -> SQL Template`. O Router não deve conter regras de negócio.
- **SQL Nativo para AGHU:** Utilizar exclusivamente arquivos `.sql` em `src/providers/sql/` para buscar dados de pacientes, nunca ORM.
- **Testes Backend:** Criar um arquivo de teste utilizando `pytest` (ex: `test_apac_controller.py`) para validar as regras de negócio em Python.

### Escopo Negativo (O que NÃO fazer - Anti-Patterns)
- **Proibido Sugerir Bibliotecas de IA:** Não adicionar dependências de NLP (como spaCy, NLTK) ou chamadas a APIs de LLMs. O escopo é 100% determinístico.
- **Proibido Duplicar Cadastro:** Não sugerir a criação de rotas de `POST /pacientes` para preenchimento manual. Toda a entrada de pacientes deve vir via ETL do AGHU.
- **No Secrets in Code:** Proibido salvar chaves, tokens do AD ou strings de conexão no código; utilizar variáveis de ambiente (`.env`).
