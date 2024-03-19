---
lab:
  title: "Implementar CI/CD com\_o GitHub Actions e IaC com o Bicep"
  module: Deliver with DevOps
---

# Laboratório 3- Implementar CI/CD com o GitHub Actions e IaC com o Bicep

## Tempo estimado: 40 minutos

## Cenário

Lembre-se do cenário deste módulo, no qual você trabalha para uma empresa de desenvolvimento de software no setor de varejo que deseja garantir que o processo de lançamento de uma nova versão do aplicativo da loja online seja eficiente e confiável, minimizando o risco de erros. Como você decidiu usar o GitHub para facilitar o gerenciamento do ciclo de vida do aplicativo, este laboratório oferece a oportunidade de bifurcar e revisar o repositório do GitHub que contém o código-fonte de um aplicativo Web, um fluxo de trabalho do GitHub Actions e um modelo do Bicep. Além disso, você poderá configurar o ambiente de destino e validar a Infraestrutura como Código (IaC) e a funcionalidade de CI/CD.

## Objetivos

Neste laboratório, você vai:

- Preparar a assinatura do Azure para o laboratório
- Implemente a Infraestrutura como Código (IaC) e CI/CD com o GitHub Actions e um modelo do Bicep

> **Observação:** No laboratório anterior, você configurou o GitHub Pages, o que significa que você já implementou a implantação contínua (potencialmente sem sequer perceber). O GitHub Pages usa (em segundo plano) o GitHub Actions para executar a implantação automatizada após qualquer confirmação na ramificação principal. Você pode verificar isso navegando até a guia **Ações** na página principal do repositório bifurcado, **Spoon-Knife**. Agora, você aprimorará essa funcionalidade. Em particular, você usará um fluxo de trabalho do GitHub Actions desenvolvido sob medida com um modelo Bicep para provisionar alguns aplicativos Web do Serviço de Aplicativo do Azure em diferentes regiões do Azure e implantar um aplicativo Web .NET personalizado em ambas.

> **Observação:** Para este e os laboratórios seguintes, use a mesma conta do GitHub que você criou para a finalidade do primeiro laboratório.

## Pré-requisitos

- Concluído [Laboratório 01 – Planejamento e Gerenciamento do Agile com o GitHub](01-agile-planning-management-using-github.md)
- Concluído [Laboratório 02 – Implementar o fluxo de trabalho com o GitHub](02-implement-manage-repositories-using-github.md)
- Uma assinatura do Azure na qual você tenha pelo menos o acesso de nível Colaborador. Se você não tiver uma assinatura do Azure, poderá se inscrever para uma [avaliação gratuita](https://azure.microsoft.com/free).

## Exercício 0: preparar a assinatura do Azure para o laboratório

> **Observação:** Se você estiver usando uma nova assinatura do Azure, alguns dos provedores de recursos do Azure poderão não estar registrados. Como o processo de registro pode levar alguns minutos, para minimizar o tempo de espera, você os registrará no início deste laboratório.

> **Observação:** Neste laboratório, você usará o Azure Cloud Shell. Se for a primeira vez que você estiver usando o Azure Cloud Shell em sua assinatura, será necessário registrar o provedor de recursos correspondente. 

1. Inicie um navegador da Web e navegue até o portal do Azure em `https://portal.azure.com`.
1. Se solicitado, entre usando sua conta do Microsoft Entra ID com o acesso do Proprietário à assinatura do Azure disponível para você.
1. Na guia navegador da Web que exibe o portal do Azure, na caixa de texto de pesquisa na parte superior da página, insira **Assinaturas** e, na lista de resultados, selecione **Assinaturas**.
1. Na página **Assinaturas**, selecione a assinatura que você deseja usar nesse laboratório.
1. Na página assinaturas, no menu vertical à esquerda, selecione **Provedores de recursos**.
1. Na lista de provedores de recursos, pesquise e selecione **Microsoft.CloudShell**.
1. Com o provedor de recursos **Microsoft.CloudShell** selecionado, na barra de ferramentas, selecione **Registrar**.

   > **Observação:** Não aguarde até que o registro seja concluído, mas prossiga diretamente para o próximo exercício. O registro pode levar alguns minutos.

## Exercício 1: implementar q Infraestrutura como Código (IaC) e CI/CD com GitHub Actions e um modelo Bicep

Nesse exercício, você implementará IaC com CI/CD para aplicativos Web do Serviço de Aplicativo do Azure com o GitHub Actions e um modelo Bicep.

> **Observação:** Para simplificar a configuração inicial, você usará uma bifurcação de um repositório existente do GitHub que contém o código-fonte do aplicativo .NET, o fluxo de trabalho do GitHub Actions e o modelo Bicep.

O exercício é composto pelas seguintes tarefas:

- Tarefa 1: Bifurque e examine o repositório GitHub que contém o código-fonte de um aplicativo Web, um fluxo de trabalho do GitHub Actions e um modelo Bicep
- Tarefa 2: Configurar o ambiente de destino
- Tarefa 3: Validar a funcionalidade de IaC e CI/CD

### Tarefa 1: Bifurque e examine o repositório GitHub que contém o código-fonte de um aplicativo Web, um fluxo de trabalho do GitHub Actions e um modelo Bicep

1. Alterne para a janela do navegador exibindo sua conta do GitHub e verifique se você ainda está autenticado (caso contrário, entre usando sua conta de usuário do GitHub).
1. Abra outra guia na mesma janela do navegador e navegue até o repositório [eShopOnWeb](https://github.com/MicrosoftLearning/eShopOnWeb), que hospeda a versão do .NET de um site de exemplo, um fluxo de trabalho do GitHub Actions e um modelo Bicep.
1. Na página de repositório **eShopOnWeb**, selecione **Bifurcação**.
1. Na página **Criar uma bifurcação**, verifique se a entrada da lista suspensa **Proprietário** exibe seu nome de usuário do GitHub, aceite a entrada padrão **eShopOnWeb** na caixa de texto **Nome do repositório**, confirme **Copiar somente a ramificação principal** habilitada e escolha **Criar bifurcação**.

   > **Observação:** A sessão do navegador será redirecionada automaticamente para o repositório com a bifurcação recém-criada.

1. Na página **eShopOnWeb**, confirme se a lista suspensa exibe a ramificação **principal** atual.
1. Examine a estrutura do repositório e observe que ela inclui os seguintes componentes:

   - O diretório **.github/workflows** que contém vários fluxos de trabalho baseados em YAML, incluindo um chamado **eshoponweb-cicd.yml**
   - o diretório **infra** que contém o modelo Bicep chamado **webapp.bicep**
   - o diretório **src/Web** que contém o código .NET do aplicativo Web de origem

1. Abra o diretório **.github/workflows** e selecione o arquivo **eshoponweb-cicd.yml** para exibir seu conteúdo. Observe que este fluxo de trabalho do GitHub Actions contém os trabalhos de **buildandtest** e **implantação**, que consistem nas seguintes etapas:

   - buildandtest
      - fazer check-out do repositório
      - preparar o executor para a versão .NET desejada do SDK
      - compilar, testar e publicar o projeto .NET
      - carregar os artefatos de código do site publicados
      - carregar o modelo bicep como um artefato para o próximo trabalho
   - deploy
      - baixar os artefatos publicados criados no trabalho anterior
      - baixar o modelo bicep carregado no trabalho anterior
      - fazer logon na assinatura do Azure de destino usando uma entidade de serviço
      - implantar um aplicativo Web do Serviço de Aplicativo do Azure usando o modelo bicep
      - publicar os artefatos do site no aplicativo Web do Serviço de Aplicativo do Azure

   > **Observação:** Não se concentre nos detalhes de cada trabalho. Nesse momento, é muito mais importante simplesmente entender a finalidade do fluxo de trabalho.

1. Observe que o processo de provisionamento de recursos do Azure tem como destino um único grupo de recursos que é definido no início do fluxo de trabalho como uma variável de ambiente `RESOURCE-GROUP`.

   > **Observação:** Você precisará criar o grupo de recursos antes de executar o fluxo de trabalho.

1. Observe que o fluxo de trabalho depende de um segredo para se autenticar na assinatura do Azure de destino durante a etapa Configurar a CLI do Azure, conforme evidenciado pela instrução `creds: ${{ secrets.AZURE_CREDENTIALS }}`.

   > **Observação:** Você precisará configurar esse segredo antes de executar o fluxo de trabalho.

1. Além disso, observe que esse fluxo de trabalho pode ser configurado para ser disparado manualmente, conforme indicado pela sintaxe `on: workflow_dispatch:`.

### Tarefa 2: Configurar o ambiente de destino

> **Observação:** Você começará criando os grupos de recursos. Você executará o fluxo de trabalho duas vezes para implantar duas instâncias do site em duas regiões diferentes do Azure.

1. Alterne para a guia do navegador da Web que está exibindo o portal do Azure em `https://portal.azure.com`.
1. No portal do Azure, na caixa de texto de pesquisa na parte superior da página, insira **Grupos de recursos** e selecione **Grupos de recursos** na lista de resultados.
1. Na página **Grupos de recursos**, selecione **+ Criar**.
1. Na caixa de texto **Grupos de recursos**, insira **rg-eshoponweb-westeurope**.
1. Na lista suspensa **Região**, selecione **(Europa) Europa Ocidental**.
1. Selecione **Examinar + criar** e, em seguida, em **Revisar + criar**, selecione **Criar**.
1. Na página **Grupos de recursos**, selecione **+ Criar**.
1. Na caixa de texto **Grupos de recursos**, insira **rg-eshoponweb-eastus**.
1. Na lista suspensa **Região**, selecione **Leste dos EUA**.
1. Selecione **Examinar + criar** e, em seguida, em **Revisar + criar**, selecione **Criar**.

    > **Observação:** Em seguida, você criará uma entidade de serviço que será usada para autenticar do fluxo de trabalho do GitHub Actions para a assinatura de destino do Azure e atribuirá a ela a função de Colaborador na assinatura.

1. No portal do Azure, selecione o ícone do **Cloud Shell** à direita da caixa de texto de pesquisa.
1. Se necessário, selecione **Bash** no menu suspenso no canto superior esquerdo do painel do Cloud Shell.
1. Na sessão Bash no painel do Cloud Shell, execute o seguinte comando para armazenar o valor da ID da assinatura do Azure em uma variável:

   ```cli
   SUBCRIPTION_ID=$(az account show --query id --output tsv) 
   echo $SUBCRIPTION_ID
   ```

1. Copie o valor da ID da Assinatura retornada pelo segundo comando e registre-o. Você precisará dele posteriormente no exercício.
1. Na sessão Bash no painel do Cloud Shell, execute o seguinte comando para criar uma entidade de serviço do Microsoft Entra ID e atribuir a ela a função de Colaborador no escopo da assinatura:

   ```cli
   az ad sp create-for-rbac --name "devopsfoundationslabsp" --role contributor --scopes /subscriptions/$SUBCRIPTION_ID --json-auth
   ```

1. Copie toda a saída formatada em JSON do comando e registre-a. Você precisará dele em breve. A saída deve ter o formato semelhante ao seguinte texto:

   ```json
   {
     "clientId": "cccccccc-cccc-cccc-cccc-cccccccccccc",
     "clientSecret": "xxxxx~xxxxxxxxxxxx-xxxxxxxxxxxxx_xxxxxxx",
     "subscriptionId": "yyyyyyyy-yyyy-yyyy-yyyy-yyyyyyyyyyyy",
     "tenantId": "zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz",
     "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
     "resourceManagerEndpointUrl": "https://management.azure.com/",
     "activeDirectoryGraphResourceId": "https://graph.windows.net/",
     "sqlManagementEndpointUrl": "https://management.core.windows.net:8443/",
     "galleryEndpointUrl": "https://gallery.azure.com/",
     "managementEndpointUrl": "https://management.core.windows.net/"
   }
   ```

1. Alterne para a janela do navegador da Web exibindo a página dp repositório bifucrado do GitHub do **eShopOnWeb** bifurcada e, na barra de ferramentas, selecione **Configurações**.
1. No menu vertical à esquerda, na seção **Segurança**, selecione **Segredos e variáveis** e, na lista suspensa, selecione **Ações**.
1. Na página **Segredos e variáveis da ação**, selecione **Novo segredo do repositório**.
1. Na página **Segredos/novo segredo das ações**, na caixa de texto **Nome**, digite **AZURE_CREDENTIALS**.
1. Na caixa de texto **Segredo**, cole o texto formatado em JSON que você registrou anteriormente nesta tarefa e selecione **Adicionar segredo**.
1. Volte para o navegador da Web exibindo a sessão Bash no painel do Cloud Shell e execute o seguinte comando para gerar o nome do primeiro aplicativo Web do Serviço de Aplicativo que você implantará:

   ```cli
   echo devops-webapp-westeurope-$RANDOM$RANDOM
   ```

1. Copie o valor retornado pelo comando e registre-o. Você o usará posteriormente neste exercício.
1. Na sessão Bash no painel do Cloud Shell, execute o seguinte comando para gerar o nome do segundo aplicativo Web do Serviço de Aplicativo que você implantará:

   ```cli
   echo devops-webapp-eastus-$RANDOM$RANDOM
   ```

1. Copie o valor retornado pelo comando e registre-o. Você o usará posteriormente neste exercício.

### Tarefa 3: Validar a funcionalidade de IaC e CI/CD

1. Mude para a janela do navegador da web que exibe a página do repositório bifurcado do GitHub do **eShopOnWeb** e, se necessário, na página **eShopOnWeb**, clique na guia **Código** e, na lista suspensa, confirme se a ramificação atual é **principal**.
1. Na janela do navegador da Web exibindo a ramificação **principal** da página do repositório bifurcado do GitHub do **eShopOnWeb**, navegue até a pasta **.github/workflows** e selecione **eshoponweb-cicd.yml**.
1. No painel **.github/workflows/eshoponweb-cicd.yml**, selecione o ícone de lápis para editar o fluxo de trabalho.
1. No painel **Editar**, substitua a linha 4 pelo seguinte texto:

   ```yaml
   on: workflow_dispatch
   ```

    >**Observação**: Verifique se você usa o recuo adequado.

1. No painel **Editar**, substitua a linha 8 pelo seguinte texto:

   ```yaml
    RESOURCE-GROUP: rg-eshoponweb-westeurope
   ```

1. No painel **Editar**, substitua o espaço reservado `YOUR-SUBS-ID` na linha 11 pelo valor da ID da assinatura do Azure que você registrou anteriormente neste exercício:
1. No painel **Editar**, substitua o espaço reservado `eshoponweb-webapp-NAME` na linha 11 pelo nome do **primeiro** aplicativo Web do Serviço de Aplicativo do Azure gerado anteriormente neste exercício.
1. No painel **.github/fluxos de trabalho/eshoponweb-cicd.yml**, selecione **Confirmar alterações** e selecione **Confirmar alterações** novamente.
1. Na janela do navegador da Web exibindo a ramificação **principal** da página do repositório bifurcado do GitHub do **eShopOnWeb**, navegue até a pasta **infra** e selecione **webapp.bicep**.
1. No painel **infra/webapp.bicep**, selecione o ícone de lápis para editar o fluxo de trabalho.
1. No painel **Editar**, substitua a linha 2 pelo seguinte texto:

   ```yaml
   param sku string = 'S1' // The SKU of App Service Plan
   ```

1. No painel **infra/webapp.bicep**, selecione **Confirmar alterações** e selecione **Confirmar alterações** novamente.
1. Na janela do navegador da Web exibindo a página do repositório bifurcado do GitHub do **eShopOnWeb**, selecione **Ações**.
1. Se for solicitado que você habilite o GitHub Actions, selecione **Eu entendo meus fluxos de trabalho, vá em frente e habilite-os**.
    >**Observação**: Isso é esperado, pois, por padrão, o GitHub desabilitará fluxos de trabalho em um repositório bifurcado para sua própria proteção.
1. Na seção **Todos os fluxos de trabalho** no lado esquerdo, selecione **Compilação e Teste do eShopOnWeb**.
1. No painel **Compilação e Teste do eShopOnWeb**, selecione **Executar fluxo de trabalho**, na lista suspensa, confirme se a opção **Ramificação: principal** está selecionada e selecione **Executar fluxo de trabalho** novamente.

    >**Observação**: Isso deve disparar uma execução de fluxo de trabalho.

1. Selecione a execução do fluxo de trabalho de **Compilação e Teste do eShopOnWeb**.

    >**Observação**: Se necessário, atualize a página para ver a execução mais recente do fluxo de trabalho.

1. No painel **Compilar e Testar nº 1 do eShopOnWeb**, selecione **buildandtest**.
1. Monitore o progresso do fluxo de trabalho e verifique se todas as etapas do trabalho **buildandtest** foram concluídas com êxito.
1. Depois que este trabalho for concluído, na seção **Resumo**, selecione **implantar**.
1. Monitore o progresso do fluxo de trabalho e verifique se todas as etapas do trabalho de **implantação** foram concluídas com êxito.

    >**Observação**: Caso alguma das etapas falhe, na mesma página que exibe o progresso do fluxo de trabalho, no canto superior direito, selecione **Executar novamente todos os trabalhos** e, no painel **Executar novamente todos os trabalhos**, selecione **Executar trabalhos**novamente.

1. Alterne para a janela do navegador da Web exibindo a página do repositório bifurcado do GitHub do **eShopOnWeb** e, se necessário, na página **do eShopOnWeb**, na lista suspensa, confirme se a ramificação atual é **principal**.
1. Na janela do navegador da Web exibindo a ramificação **principal** da página do repositório bifurcado do GitHub do **eShopOnWeb**, navegue até a pasta **.github/workflows** e selecione **eshoponweb-cicd.yml**.
1. No painel **.github/workflows/eshoponweb-cicd.yml**, selecione o ícone de lápis para editar o fluxo de trabalho.
1. No painel **Editar**, substitua a linha 8 pelo seguinte texto:

   ```yaml
    RESOURCE-GROUP: rg-eshoponweb-eastus
   ```

1. No painel **Editar**, substitua o espaço reservado `eshoponweb-webapp-NAME` na linha 11 pelo nome do **segundo** aplicativo Web do Serviço de Aplicativo do Azure gerado anteriormente neste exercício.
1. No painel **.github/fluxos de trabalho/eshoponweb-cicd.yml**, selecione **Confirmar alterações** e selecione **Confirmar alterações** novamente.
1. Na janela do navegador da Web exibindo a página do repositório bifurcado do GitHub do **eShopOnWeb**, selecione **Ações**.
1. Na seção **Todos os fluxos de trabalho** no lado esquerdo, selecione **Compilação e Teste do eShopOnWeb**.
1. No painel **Compilação e Teste do eShopOnWeb**, selecione **Executar fluxo de trabalho**, na lista suspensa, confirme se a opção **Ramificação: principal** está selecionada e selecione **Executar fluxo de trabalho** novamente.

    >**Observação**: Isso deve disparar uma execução de fluxo de trabalho.

1. Selecione a execução do fluxo de trabalho de **Compilação e Teste do eShopOnWeb**.
1. No painel **Compilar e Testar nº 2 do eShopOnWeb**, selecione **buildandtest**.
1. Monitore o progresso do fluxo de trabalho e verifique se todas as etapas do trabalho **buildandtest** foram concluídas com êxito.
1. Depois que este trabalho for concluído, na seção **Resumo**, selecione **implantar**.
1. Monitore o progresso do fluxo de trabalho e verifique se todas as etapas do trabalho de **implantação** foram concluídas com êxito.

    >**Observação**: Caso alguma das etapas falhe, na mesma página que exibe o progresso do fluxo de trabalho, no canto superior direito, selecione **Executar novamente todos os trabalhos** e, no painel **Executar novamente todos os trabalhos**, selecione **Executar trabalhos**novamente.

1. Na janela do navegador da Web que exibe o portal do Azure, na caixa de texto de pesquisa na parte superior da página, insira os **Serviços de Aplicativos** e selecione **Serviços de Aplicativos** na lista de resultados.
1. Na página **Serviços de Aplicativos**, na lista de Serviços de Aplicativo, selecione o serviço **devops-webapp-westeurope-app** criado anteriormente neste exercício.
1. Na página **devops-webapp-westeurope**, na seção **Essentials**, verifique se o valor de **domínio Padrão** é exibido e selecione-o para abrir o aplicativo Web em uma nova guia do navegador.
1. Na nova guia do navegador, verifique se o aplicativo Web é exibido e se ele está funcional. Você também pode verificar o segundo aplicativo Web na região **Leste dos EUA** da mesma maneira.
    >**Observação**: Deixe os recursos implantados do Azure em execução. Você precisará deles no próximo laboratório.
