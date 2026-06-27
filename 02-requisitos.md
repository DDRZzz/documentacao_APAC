# Especificação de Requisitos

## 1. Requisitos Funcionais (RF)
| ID | Título | Descrição | Prioridade |
| :--- | :--- | :--- | :--- |
| RF001 | Autenticação | Login via LDAP/AD do hospital. | Baixa |
| RF002 | Extração do AGHU | Coletar os dados estruturados e texto de evolução do paciente no AGHU. | Essencial |
| RF003 | Varredura e mapeamento | Buscar no texto de evolução os termos que já existem no Dicionário local. | Essencial |
| RF004 | Marcação visual | Renderizar o texto da evolução na interface, destacando visualmente as palavras que foram mapeadas com sucesso. | Essencial |
| RF005 | Seleção de novos termos | Permitir que o usuário selecione com o mouse trechos do texto livre que não foram destacados pelo sistema. | Essencial |
| RF006 | Enriquecimento do Dicionário | Exibir um formulário ao selecionar um novo termo, vinculando-o ao CID e Procedimento, e salvar a nova relação no Dicionário. | Essencial |
| RF007 | Interface BI | Fornecer dashboard semelhante à planilha de faturamento para controle de APACs. | Média |
| RF008 | Exportação | Formatar o documento (.txt posicional) para exportação ao APAC Magnético. | Média |

## 2. Requisitos Não Funcionais (RNF)
| ID | Categoria | Descrição |
| :--- | :--- | :--- |
| RNF001 | Segurança | Criptografia AES-256 para dados sensíveis. |
| RNF002 | LGPD | Auditoria de acesso (logs) a dados sensíveis. |
| RNF003 | Desempenho | A varredura de texto e a busca na Tabela Hash do dicionário devem ocorrer de forma instantânea (O(1)). |
| RNF004 | Usabilidade | A funcionalidade de "grifar texto" (RF005) deve operar de forma fluida nos navegadores padrão das estações de trabalho do HC. |

## 3. Detalhamento SDD (CARE)

### [CARE-RF001] Autenticação LDAP
* **Context (Contexto)**: Módulo de autenticação (`auth_handler.py`) existente no framework.
* **Action (Ação)**: Proteger as rotas do QuickAPAC utilizando a injeção de dependência `Depends(auth_handler.decode_token)`.
* **Result (Resultado)**: Acesso negado (401) sem token válido.
* **Evaluation (Avaliação)**: Requisições HTTP para os endpoints da APAC sem o Header de Autorização devem falhar com HTTP Status 401.

### [CARE-RF002] Extração AGHU
* **Context (Contexto)**: Permissões de leitura concedidas para as tabelas ou visualizações do AGHU.
* **Action (Ação)**: Desenvolver rotinas de extração (ETL) para puxar os dados de cadastro e a evolução dos pacientes.
* **Result (Resultado)**: Retorno de payload JSON estruturado contendo as informações e o texto bruto.
* **Evaluation (Avaliação)**: Validar o payload extraído contra um JSON Schema esperado.

### [CARE-RF003] Varredura e Mapeamento
* **Context (Contexto)**: Texto recebido do AGHU e Dicionário carregado.
* **Action (Ação)**: Realizar busca de substrings no texto baseando-se nas chaves do Dicionário.
* **Result (Resultado)**: Associação dos termos aos códigos e mapeamento das posições das palavras no texto para o Front-end.
* **Evaluation (Avaliação)**: Teste unitário injetando termos conhecidos e garantindo o retorno dos códigos e posições exatas na string.

### [CARE-RF004] Marcação Visual
* **Context (Contexto)**: Front-end recebe o texto da evolução e a lista de palavras mapeadas.
* **Action (Ação)**: Aplicar tags HTML (ex: `<mark>`) ou componentes de destaque ao redor das palavras conhecidas na renderização do texto.
* **Result (Resultado)**: Usuário visualiza o texto com os jargões destacados em cor.
* **Evaluation (Avaliação)**: Teste de componente verificando se a tag de destaque foi aplicada corretamente na palavra mapeada.

### [CARE-RF005] Seleção de Novos Termos
* **Context (Contexto)**: O Auditor lê o texto e encontra um jargão médico que o sistema não destacou.
* **Action (Ação)**: Capturar o evento de seleção de texto (mouse dragging) na interface.
* **Result (Resultado)**: O trecho selecionado é armazenado temporariamente em uma variável de estado e o modal de cadastro é acionado.
* **Evaluation (Avaliação)**: Teste E2E simulando a seleção de uma string no DOM e verificando se o evento de abertura do modal é disparado com a string correta.

### [CARE-RF006] Enriquecimento do Dicionário
* **Context (Contexto)**: Modal aberto contendo o trecho selecionado pelo usuário no RF005.
* **Action (Ação)**: Receber o *input* do CID e Procedimento, e enviar um POST/PUT para a API.
* **Result (Resultado)**: Nova relação termo-código salva no banco. O Front-end re-renderiza o texto e a palavra recém-adicionada passa a ficar destacada (RF004).
* **Evaluation (Avaliação)**: Inserir termo via API, validar persistência no banco e conferir se o status da APAC do paciente foi atualizado para PRONTA.

### [CARE-RF007] Interface BI
* **Context (Contexto)**: Dados de APACs consolidados no banco de dados.
* **Action (Ação)**: Desenvolver componentes de visualização consumindo endpoints analíticos.
* **Result (Resultado)**: Dashboard renderizado na tela dos gestores para acompanhamento em tempo real.
* **Evaluation (Avaliação)**: Testes E2E para validar a montagem da tabela.

### [CARE-RF008] Exportação
* **Context (Contexto)**: Lote de APACs 100% resolvidas.
* **Action (Ação)**: Executar script de formatação posicional (*fixed-width*).
* **Result (Resultado)**: Geração e download do arquivo `.txt` para uso no APAC Magnético.
* **Evaluation (Avaliação)**: Validar arquivo gerado em ambiente de teste confirmando o número exato de bytes por linha.

