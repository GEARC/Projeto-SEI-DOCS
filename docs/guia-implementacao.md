# Guia de Implementação

!!! info "Essas informações ainda não estão atualizadas!"
        **Teste**: Configurar os testes
        **env**: configurar .env
        **Dev**: pendente atualizar a estrutura de Documentos

## 🚀 Como Executar o Projeto Localmente

### 📋 Pré-requisitos

Antes de iniciar, certifique-se de ter os seguintes recursos instalados e configurados:

#### Software Necessário
- **Node.js** (LTS, v18 ou superior)
- **Visual Studio Code** (IDE recomendada)
- **Git** (para controle de versão)

#### Extensões e Ferramentas
- **Microsoft Teams Toolkit** (extensão do VS Code)
- **Bot Framework Emulator** (recomendado para testes)
- **TypeScript** (instalado globalmente via npm)

#### Acessos Necessários
- **Tenant Microsoft 365** com permissão para upload de aplicativos
- **Conta Azure** com permissões adequadas
- **Acesso ao SEI** da organização (para integração futura)

---

## 🛠️ Setup do Ambiente de Desenvolvimento

### 1. Preparação do Projeto

#### 1.1 Clone do Repositório
```bash
# Clonar o repositório do projeto
git clone [url-do-repositorio]
cd assistente-sei-teams
```

#### 1.2 Instalação de Dependências
```bash
# Instalar dependências do Node.js
npm install

# Instalar TypeScript globalmente (se necessário)
npm install -g typescript
```

#### 1.3 Configuração de Variáveis de Ambiente
```bash
# Criar arquivo de configuração
cp .env.example .env
```

**Configurar o arquivo `.env`**:
```env
# Microsoft Teams/Azure Configuration
MicrosoftAppId=your-app-id-here
MicrosoftAppPassword=your-app-password-here
MicrosoftAppTenantId=your-tenant-id-here

# Bot Configuration
BotName=AssistenteSEI
BotPort=3978

# SEI Configuration (para desenvolvimento futuro)
SEI_ENDPOINT=https://seu-sei.gov.br/sei/ws
SEI_SISTEMA=SEI-TEAMS-BOT
SEI_USUARIO=usuario-sistema
SEI_SENHA=senha-sistema

# Environment
NODE_ENV=development
LOG_LEVEL=debug
```

### 2. Compilação e Execução

#### 2.1 Compilar TypeScript
```bash
# Compilar código TypeScript para JavaScript
npx tsc

# Ou usar o comando de build do package.json
npm run build
```

#### 2.2 Iniciar o Servidor
```bash
# Iniciar o servidor do bot
node ./dist/index.js

# Ou usar o script de desenvolvimento com hot-reload
npm run dev
```

**Saída Esperada**:
```
[INFO] Bot Framework listening on port 3978
[INFO] restify listening to http://[::]:3978
[INFO] SEI Client initialized (mock mode)
```

---

## 🤖 Configuração do Bot Framework

### 3. Teste com Bot Framework Emulator

#### 3.1 Download e Instalação
- Baixe o [Bot Framework Emulator](https://github.com/Microsoft/BotFramework-Emulator/releases)
- Instale seguindo as instruções da plataforma

#### 3.2 Conexão com o Bot Local
1. Abra o Bot Framework Emulator
2. Clique em "Open Bot"
3. Configure a URL: `http://localhost:3978/api/messages`
4. Deixe App ID e Password em branco para teste local
5. Clique em "Connect"

#### 3.3 Testes Básicos
Execute os seguintes comandos no emulador:
```
oi
/help
/status
/login
```

---

## 🏢 Configuração do Microsoft Teams

### 4. Registro no Azure Portal

#### 4.1 Criar App Registration
1. Acesse [Azure Portal](https://portal.azure.com)
2. Navegue para **Azure Active Directory** → **App registrations**
3. Clique em **"New registration"**

**Configurações**:
```
Name: Assistente SEI para Teams
Supported account types: Single tenant
Redirect URI: https://token.botframework.com/.auth/web/redirect
```

#### 4.2 Gerar Credenciais
1. **Application (client) ID**: Copie e cole no `.env`
2. **Client Secret**: 
   - Vá para "Certificates & secrets"
   - Clique em "New client secret"
   - Copie o valor gerado e cole no `.env`
3. **Directory (tenant) ID**: Encontre na página Overview

#### 4.3 Configurar Permissões
Adicione as seguintes **Application permissions**:
- `User.Read.All`
- `TeamsActivity.Send`
- `Directory.Read.All`

**Importante**: Solicite aprovação do administrador para essas permissões.

### 5. Configuração no Teams

#### 5.1 Preparar Manifesto
O arquivo `appPackage/manifest.json` já está configurado. Apenas verifique se o `botId` corresponde ao seu App ID do Azure.

#### 5.2 Upload no Teams
1. Abra o Microsoft Teams
2. Vá para **Apps** → **Upload a custom app**
3. Selecione **"Upload for [sua organização]"**
4. Faça upload do arquivo `appPackage.zip` (ou crie o zip da pasta appPackage)

#### 5.3 Teste no Teams
1. Localize o bot na lista de apps da organização
2. Clique em **"Add"** para instalar
3. Inicie uma conversa com o bot
4. Teste comandos básicos

---

## 🔧 Estrutura de Desenvolvimento

### 6. Navegação no Código

#### 6.1 Arquivos Principais
```typescript
// src/index.ts - Ponto de entrada
import restify from 'restify';
import { TeamsBot } from './teamsBot';

// src/teamsBot.ts - Lógica principal
export class TeamsBot extends TeamsActivityHandler {
    constructor() {
        super();
        this.onMessage(async (context, next) => {
            // Processar mensagens do usuário
        });
    }
}

// src/ssoDialog.ts - Autenticação
export class SSODialog extends ComponentDialog {
    // Gerenciamento de login SSO
}

// src/seiClient.ts - Integração SEI
export class SEIClient {
    // Comunicação com APIs do SEI (mockado)
}
```

#### 6.2 Adaptive Cards
Os templates ficam em `src/adaptiveCards/`:
- `welcomeCard.json`: Card de boas-vindas
- `processCard.json`: Exibição de processos
- `signatureCard.json`: Interface de assinatura

#### 6.3 Configuração do Manifesto
```json
// appPackage/manifest.json
{
    "manifestVersion": "1.17",
    "version": "1.0.0",
    "id": "${{TEAMS_APP_ID}}",
    "bots": [{
        "botId": "${{BOT_ID}}",
        "scopes": ["personal", "team", "groupchat"]
    }]
}
```

---

## 🧪 Testes e Debug

### 7. Estratégias de Teste

#### 7.1 Testes Locais
```bash
# Executar testes unitários
npm test

# Executar com coverage
npm run test:coverage

# Lint do código
npm run lint
```

#### 7.2 Debug no VS Code
Configure o arquivo `.vscode/launch.json`:
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Debug Bot",
            "type": "node",
            "request": "launch",
            "program": "${workspaceFolder}/dist/index.js",
            "outFiles": ["${workspaceFolder}/dist/**/*.js"],
            "env": {
                "NODE_ENV": "development"
            }
        }
    ]
}
```

#### 7.3 Logs e Monitoramento
```typescript
// Configuração de logs estruturados
import { createLogger, format, transports } from 'winston';

const logger = createLogger({
    level: process.env.LOG_LEVEL || 'info',
    format: format.combine(
        format.timestamp(),
        format.json()
    ),
    transports: [
        new transports.Console(),
        new transports.File({ filename: 'bot.log' })
    ]
});
```

---

## 🚀 Deploy e Produção

### 8. Preparação para Deploy

#### 8.1 Build de Produção
```bash
# Limpar build anterior
npm run clean

# Build otimizado para produção
npm run build:prod

# Verificar se não há erros
npm run lint
npm test
```

#### 8.2 Configuração do Azure

**Azure App Service**:
```bash
# Criar resource group
az group create --name rg-assistente-sei --location brazilsouth

# Criar app service plan
az appservice plan create --name plan-assistente-sei --resource-group rg-assistente-sei --sku S1

# Criar web app
az webapp create --name assistente-sei-teams --resource-group rg-assistente-sei --plan plan-assistente-sei --runtime "NODE|18-lts"
```

#### 8.3 CI/CD Pipeline
Configure GitHub Actions ou Azure DevOps para deploy automatizado:

```yaml
# .github/workflows/deploy.yml
name: Deploy to Azure
on:
  push:
    branches: [ main ]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Setup Node.js
      uses: actions/setup-node@v2
      with:
        node-version: '18'
    - run: npm install
    - run: npm run build
    - run: npm test
    - name: Deploy to Azure
      uses: azure/webapps-deploy@v2
      with:
        app-name: assistente-sei-teams
        publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
```

---

## 📋 Checklist de Desenvolvimento

### ✅ Setup Inicial
- [ ] Node.js v18+ instalado
- [ ] VS Code com Teams Toolkit
- [ ] Repositório clonado
- [ ] Dependências instaladas (`npm install`)
- [ ] Arquivo `.env` configurado

### ✅ Configuração Azure/Teams
- [ ] App Registration criado no Azure
- [ ] Client ID e Secret configurados
- [ ] Permissões aprovadas pelo admin
- [ ] Bot registrado no Bot Framework
- [ ] App uploadado no Teams

### ✅ Desenvolvimento
- [ ] Código compila sem erros (`npx tsc`)
- [ ] Bot inicia corretamente (`npm start`)
- [ ] Teste no Bot Emulator funcionando
- [ ] Teste no Teams funcionando
- [ ] Logs estruturados implementados

### ✅ Testes
- [ ] Testes unitários passando
- [ ] Teste de integração com Teams
- [ ] Teste de fluxo de autenticação
- [ ] Validação de Adaptive Cards
- [ ] Teste de comandos principais

### ✅ Deploy
- [ ] Build de produção criado
- [ ] Variáveis de ambiente configuradas
- [ ] Azure App Service provisionado
- [ ] Pipeline de CI/CD configurado
- [ ] Monitoramento ativo

---

## 🆘 Troubleshooting Comum

### Problemas Frequentes

#### Bot não responde no Teams
1. Verificar se App ID no manifesto está correto
2. Confirmar se bot está rodando na porta 3978
3. Verificar logs do servidor para erros
4. Testar primeiro no Bot Emulator

#### Erro de autenticação
1. Verificar se Client Secret não expirou
2. Confirmar permissões no Azure AD
3. Verificar se Tenant ID está correto
4. Confirmar aprovação do administrador

#### Adaptive Cards não renderizam
1. Validar JSON no [Adaptive Cards Designer](https://adaptivecards.io/designer/)
2. Verificar versão do schema
3. Testar no Bot Emulator primeiro
4. Verificar logs de renderização

#### Build do TypeScript falha
1. Executar `npm run clean` antes do build
2. Verificar se todas as dependências estão instaladas
3. Confirmar versão do TypeScript
4. Verificar `tsconfig.json` para configurações corretas
