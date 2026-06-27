# Interfaces e Integrações

## 1. Protótipos 
Nenhum por enquanto

## 2. Integração de Hardware
*Nenhum hardware específico é exigido.* A operação depende apenas de teclado e mouse tradicionais para a funcionalidade de grifar texto.

## 3. Integração com Sistemas Externos (AGHU)
A comunicação com o AGHU ocorrerá estritamente em modo de leitura (Read-Only) para alimentar a base de transição (*staging*) do QuickAPAC.

### [SCHEMA] Extração do AGHU (TypeScript)
Contrato esperado da View ou API do hospital para a rotina de ETL.
```typescript
interface IExtracaoAGHU {
  // Busca apenas pacientes de clínicas de alta complexidade (ex: Oncologia)
  fetchPacientesAltaComplexidade(dataConsulta: string): Promise<PacienteExtrato[]>;
}

type PacienteExtrato = {
  cns_paciente: string;
  nome_paciente: string;
  data_atendimento: string;
  texto_evolucao: string; // Texto bruto
}
