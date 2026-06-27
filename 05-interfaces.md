# Interfaces e Integrações

## 1. Protótipos 
Nenhum por enquanto

## 2. Integração de Hardware
*Nenhum hardware específico é exigido.* A operação depende apenas de teclado e mouse tradicionais para a funcionalidade de grifar texto.

## 3. Integração com Sistemas Externos (AGHU)
A comunicação com o AGHU ocorrerá estritamente em modo de leitura (Read-Only) arquitetada no Back-end via **SQL Templates**. O Front-end não interage com o AGHU.

### [SQL Template] Extração do AGHU
Contrato esperado da consulta SQL nativa (`src/providers/sql/aghu/extrair_pacientes_apac.sql`) executada pelo Provider.
```sql
SELECT 
    cns_paciente, 
    nome_paciente, 
    data_atendimento, 
    texto_evolucao 
FROM agh.evolucoes_alta_complexidade 
WHERE data_atendimento = :data_alvo;
```
