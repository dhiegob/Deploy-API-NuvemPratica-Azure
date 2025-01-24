# Deploy-API-NuvemPratica-Azure
Projeto realizado com base no curso feito na DIO Bootcamp Microsoft Certification Challenge #2 AZ-204


Para fazer o deploy de uma API no Azure com escalabilidade, segurança e CI/CD, siga os passos abaixo:

### 1. Configurar o Ambiente

1. **Criar uma Conta no Azure**:
   - Se você ainda não tem uma conta no Azure, crie uma em [azure.microsoft.com](https://azure.microsoft.com).

2. **Instalar o Azure CLI**:
   - Baixe e instale o Azure CLI a partir de [docs.microsoft.com](https://docs.microsoft.com/cli/azure/install-azure-cli).

3. **Login no Azure CLI**:

   az login


4. **Criar um Resource Group**:
  
   az group create --name MeuResourceGroup --location eastus
 

5. **Criar um App Service Plan**:
  
   az appservice plan create --name MeuAppServicePlan --resource-group MeuResourceGroup --sku B1


6. **Criar um Web App**:

   az webapp create --name MinhaAPI --resource-group MeuResourceGroup --plan MeuAppServicePlan


### 2. Configurar CI/CD com GitHub Actions

1. **Adicionar o Repositório no GitHub**:
   - Crie um repositório no GitHub e faça o push do código da sua API.

2. **Configurar GitHub Actions**:
   - No repositório do GitHub, vá para a aba "Actions" e configure um novo workflow.

3. **Criar um Workflow de Deploy**:
   - Adicione um arquivo `.github/workflows/deploy.yml` no seu repositório com o seguinte conteúdo:

 
   name: Deploy API to Azure

   on:
     push:
       branches:
         - main

   jobs:
     build-and-deploy:
       runs-on: ubuntu-latest

       steps:
         - name: Checkout code
           uses: actions/checkout@v2

         - name: Set up Node.js
           uses: actions/setup-node@v2
           with:
             node-version: '14'

         - name: Install dependencies
           run: npm install

         - name: Build the project
           run: npm run build

         - name: Deploy to Azure Web App
           uses: azure/webapps-deploy@v2
           with:
             app-name: 'MinhaAPI'
             slot-name: 'production'
             publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
  

4. **Adicionar o Publish Profile no GitHub Secrets**:
   - No portal do Azure, vá para o seu Web App, baixe o Publish Profile e adicione-o como um segredo no GitHub (`AZURE_WEBAPP_PUBLISH_PROFILE`).

### 3. Configurar Segurança

1. **Configurar HTTPS**:
   - No portal do Azure, vá para o seu Web App e habilite HTTPS.

2. **Configurar Autenticação e Autorização**:
   - No portal do Azure, vá para o seu Web App, selecione "Authentication/Authorization" e configure a autenticação com Azure AD ou outro provedor.

### 4. Monitorar a Performance

1. **Configurar Application Insights**:
   - No portal do Azure, vá para o seu Web App e habilite o Application Insights para monitorar a performance da sua API.

2. **Adicionar Telemetria no Código**:
   - Adicione o SDK do Application Insights no seu código para enviar telemetria personalizada.

   ```javascript
   const appInsights = require("applicationinsights");
   appInsights.setup("<INSTRUMENTATION_KEY>").start();
   ```

3. **Monitorar no Portal do Azure**:
   - Use o portal do Azure para visualizar métricas, logs e criar alertas personalizados.

### 5. Escalabilidade

1. **Configurar Autoescalonamento**:
   - No portal do Azure, vá para o seu App Service Plan e configure o autoescalonamento baseado em métricas como CPU e memória.

2. **Usar Azure API Management**:
   - Considere usar o Azure API Management para gerenciar, proteger e escalar sua API.

Seguindo esses passos, você terá uma API no Azure com um ambiente configurado para escalabilidade, segurança e CI/CD, além de monitoramento de performance.
