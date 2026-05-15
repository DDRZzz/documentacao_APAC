# Interfaces e Integrações

## 1. Protótipos
* https://gemini.google.com/share/9fa65150c245

## 2. Hardware
* Impressoras térmicas e leitores de código de barras.

## 3. Software
* Integração com AGHU. 

### [SCHEMA] Interface de Integração (TypeScript)
```typescript
interface IHospitalApi {
  getPatientData(id: string): Promise<PatientRecord>;
  syncProntuario(data: ProntuarioUpdate): Promise<SyncResponse>;
  checkLdapAuth(credentials: AuthInfo): Promise<AuthStatus>;
}
```
