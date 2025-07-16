# Status Atual e Próximos Passos

## 📋 Status Atual do Projeto

### ✅ Concluído

#### A. Identificação do Problema e Solução
- [x] Análise das principais atividades realizadas no SEI pelas áreas da fundação
- [x] Mapeamento de oportunidades de automação
- [x] Definição do escopo inicial do projeto

#### B. Estudo do Manual SEI Web Services
- [x] Manual do SEI Web Services analisado
- [x] Compreensão da integração via SOAP
- [x] Identificação dos métodos necessários para automação

### 🔄 Em Andamento

#### C. Preparação da Infraestrutura
- [ ] Registro do "Sistema Externo" no SEI
- [ ] Geração da sigla do sistema (ex: SEI-AUTOMATE, SEI-BOT)
- [ ] Obtenção do ID da unidade responsável pela assinatura de documentos

### ⏳ Pendente

#### D. Desenvolvimento da API
- [ ] Desenvolvimento da API PHP para integração
- [ ] Implementação dos métodos SOAP do SEI
- [ ] Configuração de servidor HTTPS para deploy

#### E. Integração com Microsoft Teams
- [ ] Registro do bot no Azure
- [ ] Configuração de permissões no Azure
- [ ] Implementação do Microsoft Graph API
- [ ] Desenvolvimento da interface de comandos

---

## 🎯 Próximos Passos (Prioridades)

### **Prioridade Alta - Dependências Críticas**

1. **Registrar Sistema Externo no SEI**
   - Solicitar ao administrador do SEI o registro do sistema
   - Definir sigla final (sugestão: SEI-AUTOMATE)
   - Obter credenciais de acesso

2. **Obter Informações da Unidade**
   - Identificar ID da unidade responsável
   - Confirmar permissões necessárias

### **Prioridade Média - Desenvolvimento**

3. **Desenvolvimento da API PHP**
   - Criar estrutura básica da API
   - Implementar autenticação SOAP
   - Desenvolver métodos principais (`gerarBloco`, `disponibilizarBloco`)

4. **Configuração do Bot no Teams**
   - Registrar aplicação no Azure AD
   - Configurar Microsoft Graph API
   - Implementar webhook para receber comandos

### **Prioridade Baixa - Expansão**

5. **Implementar Funcionalidades Adicionais**
   - `gerarProcedimento` (criar processos)
   - `incluirDocumento` (adicionar documentos)
   - `enviarProcesso` (transferir entre unidades)
   - Consultas de dados

6. **Testes e Refinamento**
   - Testes em ambiente de desenvolvimento
   - Validação com usuários piloto
   - Ajustes e melhorias

---

## 🚧 Dependências e Bloqueios

### Dependências Externas
- **Administração do SEI**: Registro do sistema externo
- **Infraestrutura**: Servidor HTTPS para deploy da API
- **Azure/Microsoft**: Aprovações e configurações necessárias

### Recursos Necessários
- Acesso administrativo ao SEI
- Conta Azure com permissões adequadas
- Servidor web com suporte a PHP e HTTPS
- Ambiente de desenvolvimento e testes

---

## 📊 Cronograma Estimado

| Fase | Descrição | Prazo Estimado | Status |
|------|-----------|----------------|--------|
| 1 | Resolução de dependências | 2-3 semanas | 🔄 Em andamento |
| 2 | Desenvolvimento da API | 3-4 semanas | ⏳ Aguardando |
| 3 | Integração com Teams | 2-3 semanas | ⏳ Aguardando |
| 4 | Testes e ajustes | 1-2 semanas | ⏳ Aguardando |
| 5 | Deploy e produção | 1 semana | ⏳ Aguardando |

**Prazo total estimado**: 9-13 semanas (dependendo da resolução das dependências)
