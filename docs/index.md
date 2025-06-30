# Projeto SEI - Integração SEI

## 1. Objetivo do Projeto

Automatizar processos do SEI, podendo utilizar o Microsoft Teams como canal de integração, visando facilitar rotinas e reduzir tarefas manuais.

**Funcionalidades para os usuários:**
- Executar comandos diretamente no Teams
- Receber mensagens no Teams com confirmações e links para o SEI

## 2. Status Atual e Próximos Passos

### A. Identificação do Problema e Solução
- Análise das principais atividades realizadas no SEI pelas áreas da fundação
- Mapeamento de oportunidades de automação

### B. Estudo do Manual SEI Web Services
- Manual analisado
- Integração via SOAP, exigindo registro de sistema externo (pendência crucial para testes e execução)

**Dependências:**
- Registrar o "Sistema Externo" no SEI
- Gerar sigla do sistema (ex: SEI-AUTOMATE, SEI-BOT)
- Obter o ID da unidade responsável pela assinatura de documentos

### C. Primeira Ideia de Automação
- Gerar e disponibilizar blocos de assinatura a partir de comandos no Teams

**Fluxo:**
1. Usuário envia comando no Teams (bot)
2. API PHP recebe o comando
3. API PHP consome o Web Service SOAP do SEI para:
    - `gerarBloco`
    - `disponibilizarBloco`
4. API PHP responde ao Teams com mensagem de sucesso e link de verificação

**Outras oportunidades de automação:**
- Criar processos (`gerarProcedimento`)
- Incluir documentos (`incluirDocumento`)
- Enviar processos para outras unidades (`enviarProcesso`)
- Consultar dados de processos ou documentos

---

## Criação do Bot no Microsoft Teams

### Opção: Utilizar Microsoft Graph API

- Mais flexível e escalável para criação de processos
- Power Automate é mais simples, porém pode limitar a expansão futura

**Requisitos:**
- Registrar o bot no Azure (obter App ID e Secret)
- Configurar permissões necessárias no Azure (leitura e escrita de informações)
- Estudar o funcionamento do Microsoft Graph API para leitura e envio de mensagens

---

**Observação:**  
O deploy da API PHP deve ser realizado em servidor com HTTPS.
