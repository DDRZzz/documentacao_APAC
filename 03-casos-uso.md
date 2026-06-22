# Modelagem de Casos de Uso

## 1. Diagrama de Casos de Uso
*(Inserir imagem ou Mermaid)*
```mermaid
flowchart LR
    %% Definição dos Atores
    M((Médico))
    
    subgraph "Sistema Hospitalar"
        UC1([Realizar Consulta])
    end
    
    %% Relacionamentos
    M --- UC1
```
## 2. Especificação
### UC001 - Consulta
* **Ator**: Médico.
* **Fluxo**: Consultar paciente -> Avaliar necessidade de tratamento -> Emitir APAC -> Assinar APAC.

#### [CARE-UC001] Implementação da Consulta
* **Context**: Paciente identificado e autenticado.
* **Action**: Extrair dados do prontuário eletrônico e utilizar o dicionário para procurar as palavras chaves no campo de evolução da GHU.
* **Result**: Arquivo com as informações das APACs para ser importado pelo APAC magnético e dashboard para controle das APACs vigentes.
* **Evaluation**: Teste unitário para o validar tempo de resposta de criação das planilhas.
