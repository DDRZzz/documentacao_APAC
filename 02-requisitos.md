# Especificação de Requisitos

## 1. Requisitos Funcionais (RF)
| ID | Título | Descrição | Prioridade |
| :--- | :--- | :--- | :--- |
| RF001 | Autenticação | Login via LDAP/AD do hospital. | Baixa |
| RF002 |	Extração do AGHU | Coletar os dados estruturados e não estruturados do AGHU |	Essencial |
| RF003 |	Varredura do texto | Ler o campo de evolução e isolar palavras identificadas |	Essencial |
| RF004 | Mapeamento semântico |	Fazer a correlação de palvras encontradas com os respctivos Código de procedimento, CID e CID secundário | Essencial |
| RF005 |	Sinalização de erro | Retornar nulo caso uma palavra não seja encontrada no seu dicionário |	Essencial |
| RF006 |	Solicitação de preenchimento | Caso uma palavra não seja encontrada, fornecer interface para preenchimento, salvando automaticamente as alterações no dicionário | Essencial |
| RF007 |	Interface BI | Fornecer dashboard semelhante a planilha de faturamento para controle | Média |
| RF008 | Exportação | Formatar o documento para exportação para o APAC Magnético | Média |

## 2. Requisitos Não Funcionais (RNF)
| ID | Categoria | Descrição |
| :--- | :--- | :--- |
| RNF001 | Segurança | Criptografia AES-256. |
| RNF002 | LGPD | Auditoria de acesso a dados sensíveis. |
| RNF003 |	Desempenho | A identificação do Cid e o retorno da planilha devem ocorrer em tempo hábil. |
| RNF005 | Usabilidade | O dashboard deve operar de forma fluida nos navegadores e resoluções padrão das estações de trabalho da Oncologia. |

## 3. Detalhamento SDD (CARE)
Para cada requisito, a implementação deve seguir o padrão:

### [CARE-RF001] Autenticação LDAP
* **Context (Contexto)**: Servidor LDAP configurado e credenciais de serviço disponíveis.
* **Action (Ação)**: Criar middleware de autenticação que consulte o AD.
* **Result (Resultado)**: Token JWT gerado após sucesso; Código 401 em falha.
* **Evaluation (Avaliação)**: Executar `npm test tests/auth.spec.ts` (deve passar com 100% de sucesso).

### [CARE-RF002] Extração AGHU
* **Context (Contexto)**: Permissões de leitura concedidas para as tabelas ou visualizações do módulo Ambulatório do AGHU.
* **Action (Ação)**: Desenvolver rotas ou rotinas de extração (ETL) para puxar os dados de cadastro e o texto de evolução dos pacientes agendados.
* **Result (Resultado)**: Retorno de um payload JSON estruturado contendo as informações do paciente e o texto clínico bruto prontos para processamento.
* **Evaluation (Avaliação)**: Validar o payload extraído contra um JSON Schema esperado garantindo que campos vitais (CNS, Nome) não retornem vazios.

### [CARE-RF003] Varredura do texto
* **Context (Contexto)**: Texto longo e não estruturado da evolução médica recebido pela aplicação.
* **Action (Ação)**: Implementar rotina de normalização (remoção de caracteres especiais, conversão para minúsculas) e tokenização para isolar blocos de texto.
* **Result (Resultado)**: Uma lista limpa contendo as expressões e jargões extraídos do prontuário.
* **Evaluation (Avaliação)**: Passar blocos de texto clínico complexo na suíte de testes e validar se a saída é exatamente o array de strings esperado.

### [CARE-RF004] Mapeamento semântico
* **Context (Contexto)**: Array de expressões médicas pronto e dicionário de equivalências carregada em memória.
* **Action (Ação)**: Iterar sobre o array e realizar busca na tabela do dicionário para cada termo.
* **Result (Resultado)**: Associação dos termos encontrados com seus respectivos Códigos de Procedimento, CID Principal e Secundário.
* **Evaluation (Avaliação)**: Teste unitário injetando termos conhecidos e garantindo o retorno do objeto com o CID exato.

### [CARE-RF005] Sinalização de erro
* **Context (Contexto)**: O laço de repetição (RF004) não encontrou uma palavra correspondente no seu dicionário.
* **Action (Ação)**: Interromper a tentativa de associação automática e acionar uma flag de erro/pendência.
* **Result (Resultado)**: Retorno de valor null, sinalizando pendência no processamento final da APAC.
* **Evaluation (Avaliação)**: Injetar termos fictícios nos testes da API para garantir que o response retorne null.

### [CARE-RF006] Solicitação de preenchimento
* **Context (Contexto)**: O front-end identificou pacientes com termos null pendentes de validação humana.
* **Action (Ação)**: Fornecer formulário e um endpoint POST/PUT para receber o jargão e o código oficial correspondente apontado pelo analista.
* **Result (Resultado)**: A pendencia é resolvida para o paciente atual e a nova relação termo → código é salva no banco de dados do dicionário.
* **Evaluation (Avaliação)**: Inserir um termo novo via requisição, checar a inserção no banco e rodar o RF004 imediatamente após para garantir que a busca já encontra a nova palavra.

### [CARE-RF007] Interface BI
* **Context (Contexto)**: Dados de APACs pendentes, resolvidas e faturadas consolidados no banco de dados.
* **Action (Ação)**: Desenvolver componentes de visualização de dados em Vue3 (tabelas, contadores, filtros por data e status) consumindo endpoints analíticos da aplicação.
* **Result (Resultado)**: Dashboard de faturamento renderizado na tela dos gestores para acompanhamento em tempo real da volumetria.
* **Evaluation (Avaliação)**: Testes end-to-end para validar a montagem e a ordenação dos dados na tabela exibida ao usuário final.

### [CARE-RF008] Exportação
* **Context (Contexto)**: Pacientes sem pendências de dicionário e com todos os dados estruturados preenchidos.
* **Action (Ação)**: Executar script de formatação posicional (fixed-width), aplicando ljust() e zfill() conforme as regras de bytes.
* **Result (Resultado)**: Geração e download do arquivo .txt contendo as linhas padronizadas para uso no APAC Magnético.
* **Evaluation (Avaliação)**: Gerar um arquivo falso em ambiente de teste e submetê-lo a um script de validação que verifica se o tamanho total de cada linha bate exatamente com o exigido e se quebras de linha usam o padrão correto.

