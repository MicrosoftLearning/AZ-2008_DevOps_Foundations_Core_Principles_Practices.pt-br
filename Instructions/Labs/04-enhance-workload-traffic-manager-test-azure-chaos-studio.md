---
lab:
  title: Aprimorar a resiliência da carga de trabalho usando o Gerenciador de Tráfego e testar a resiliência usando o Azure Chaos Studio
  module: Operate with DevOps
---

# Laboratório 04 – Aprimorar a resiliência da carga de trabalho usando o Gerenciador de Tráfego e testar a resiliência usando o Azure Chaos Studio

## Tempo estimado: 35 minutos

## Cenário

Lembre-se do cenário deste módulo, no qual você trabalha para uma empresa de desenvolvimento de software no setor de varejo que está enfrentando tempo de inatividade e problemas de desempenho frequentes com seu novo aplicativo de loja online. Como você decidiu por utilizar o Gerenciador de Tráfego e o Azure Chaos Studio para melhorar a carga de trabalho e testar a resiliência, respectivamente, este laboratório fornece a oportunidade de implementar um perfil do Gerenciador de Tráfego e validar a funcionalidade do Gerenciador de Tráfego, configurar o ambiente do Azure Chaos Studio e implementar e validar um experimento.

## Objetivos

Neste laboratório, você vai:

- Preparar a assinatura do Azure para o laboratório
- Aumente a resiliência da carga de trabalho usando o Gerenciador de Tráfego do Azure
- Testar a resiliência da carga de trabalho usando o Azure Chaos Studio
- Remova os recursos usados nos laboratórios

> **Observação:** No laboratório anterior, você implantou duas instâncias de um aplicativo Web .NET em duas regiões do Azure. Neste laboratório, você primeiro criará uma configuração resiliente que implementa a funcionalidade de balanceamento de carga do Gerenciador de Tráfego do Azure entre as duas instâncias do aplicativo Web. Em seguida, você usará o Azure Chaos Studio para disparar uma falha de um dos aplicativos para testar a resiliência da implementação de balanceamento de carga.

> **Observação:** Para este laboratório, use a mesma conta do GitHub que você tem usado em todos os laboratórios anteriores.

## Pré-requisitos

- Concluído [Laboratório 01 – Planejamento e Gerenciamento do Agile com o GitHub](01-agile-planning-management-using-github.md)
- Concluído [Laboratório 02 – Implementar o fluxo de trabalho com o GitHub](02-implement-manage-repositories-using-github.md)
- Concluído [Laboratório 03 – Implementar CI/CD com GitHub Actions e IaC com Bicep](03-implement-ci-cd-with-github-actions-and-iac-with-bicep.md)
- Uma assinatura do Azure à qual você tem o acesso no nível do proprietário

## Exercício 0: preparar a assinatura do Azure para o laboratório

> **Observação:** Se você estiver usando uma nova assinatura do Azure, alguns dos provedores de recursos do Azure poderão não estar registrados. Como o processo de registro pode levar alguns minutos, para minimizar o tempo de espera, você os registrará no início deste laboratório.

> **Observação:** Neste laboratório, você usará o Azure Chaos Studio. Se esta for a primeira vez que você implementa o Azure Chaos Studio em sua assinatura, primeiro registre o provedor de recursos correspondente.

1. Inicie um navegador da Web e navegue até o portal do Azure em `https://portal.azure.com`.
1. Se solicitado, entre usando sua conta do Microsoft Entra ID com o acesso do Proprietário à assinatura do Azure usada no laboratório anterior.
1. Na guia navegador da Web que exibe o portal do Azure, na caixa de texto de pesquisa na parte superior da página, insira **`Subscriptions`** e, na lista de resultados, selecione **Assinaturas**.
1. Na página assinaturas, no menu vertical à esquerda, selecione **Provedores de recursos**.
1. Na lista de provedores de recursos, pesquise e selecione **Microsoft.Chaos**.
1. Com o provedor de recursos **Microsoft.Chaos** selecionado, na barra de ferramentas, selecione **Registrar**.

   > **Observação:** Não aguarde até que o registro seja concluído, mas prossiga diretamente para o próximo exercício. O registro pode levar alguns minutos.

## Exercício 1: Aumente a resiliência da carga de trabalho usando o Gerenciador de Tráfego do Azure

Neste exercício, você implementará uma configuração resiliente que distribui solicitações entre as duas instâncias do aplicativo Web .NET em duas regiões diferentes do Azure usando o Gerenciador de Tráfego do Azure.

> **Observação:** Para fins de nosso laboratório, consideraremos a implantação do aplicativo Web baseado em .NET eShopOnWeb na região Leste dos EUA como a instância primária. Embora, nesse caso, essa consideração seja puramente arbitrária (e usada apenas para fins de demonstração), pode haver cenários em que pode ser benéfico priorizar um dos pontos de extremidade.

O exercício é composto pelas seguintes tarefas:

- Tarefa 1: Implementar um perfil do Gerenciador de Tráfego
- Tarefa 2: Validar a funcionalidade do Gerenciador de Tráfego

### Tarefa 1: Implementar um perfil do Gerenciador de Tráfego

1. Na guia navegador da Web que exibe o portal do Azure, na caixa de texto de pesquisa na parte superior da página, insira **`Traffic Manager profiles`** e, na lista de resultados, selecione **perfis do Gerenciador de Tráfego**.
1. Na página **Serviços de Balanceamento de Carga \| Gerenciador de Tráfego**, selecione **+ Criar**.
1. Na página **Criar perfil do Gerenciador de Tráfego**, execute as seguintes ações:

   - Na caixa de texto **Nome**, insira **`devopsfoundationstmprofile`**.

       > **Observação:** O nome do perfil do Gerenciador de Tráfego deve ser globalmente exclusivo. Se você receber uma mensagem de erro indicando que o nome já está em uso, tente um nome diferente e registre-o. Você vai precisar dele durante este laboratório.

   - Na lista suspensa **Método de roteamento**, selecione **Prioridade**.

   > **Observação:** Escolhemos o método de roteamento de prioridade para refletir a suposição um tanto arbitrária de que todas as solicitações devem ser tratadas pelo aplicativo Web do Serviço de Aplicativo do Azure no Leste dos EUA.

   - Verifique se sua assinatura do **Azure aparece na lista suspensa assinatura**
   - Clique no link **Criar novo** abaixo da lista suspensa **Grupo de Recursos**; na caixa de texto **Nome**, insira **`rg-devops-foundations`** e clique em **OK**.
   - Na lista suspensa **Local do grupo de recursos**, selecione a mesma região do Azure escolhida nos laboratórios anteriores deste curso.

1. Selecione **Criar** para iniciar o processo de provisionamento.

   > **Observação**: aguarde até que a implantação seja concluída. Isso deve ser concluído em um minuto.

1. Na página **Serviços de Balanceamento de Carga \| Gerenciador de Tráfego**, se necessário, selecione **Atualizar** e **devopsfoundationstmprofile**.
1. Na página **devopsfoundationstmprofile**, na seção **Essentials**, copie o valor da configuração **nome DNS** e registre-o. Você vai precisar dele durante este laboratório.
1. Na página **devopsfoundationstmprofile**, no menu de navegação à esquerda, na seção **Configurações**, selecione **Configuração**.
1. Examine o conteúdo da página **devopsfoundationstmprofile \| Configuração**. Observe que, por padrão, o **TTL (tempo de vida útil) do DNS** é definido como **60** segundos. Altere o valor para **5** segundos.

   > **Observação:** A redução do valor de TTL acelerará o tempo de failover. 

   > **Observação:** Além do valor de TTL, o intervalo de investigação, o tempo limite da investigação e o número tolerado de falhas afetam o tempo em que uma falha de ponto de extremidade é detectada.

1. Na página **devopsfoundationstmprofile \| Configuração**, na lista suspensa **Protocolo**, selecione **HTTPS** e, na caixa de texto **Porta**, insira **443**.

   > **Observação:** Ambos os aplicativos Web do Serviço de Aplicativo do Azure que serão configurados como pontos de extremidade podem ser acessados por HTTPS na porta TCP padrão 443.

1. Na página **devopsfoundationstmprofile \| Configuração**, selecione **Salvar**.
1. Na página **devopsfoundationstmprofile**, no menu de navegação à esquerda, na seção **Configurações**, selecione **Pontos de Extremidade**.
1. Na página **devopsfoundationstmprofile \| Pontos de Extremidade****, selecione **+ Adicionar**.

   > **Observação:** Primeiro, você adicionará um ponto de extremidade que representa o aplicativo Web do Serviço de Aplicativo do Azure.

1. No painel **Adicionar ponto de extremidade**, execute as seguintes ações:

   - Verifique se **ponto de extremidade do Azure** aparece na lista suspensa **Tipo**.
   - Na caixa de texto **Nome**, insira **`DevOps Foundations web app - East US`**.
   - Verifique se a caixa de seleção **Habilitar Ponto de Extremidade** está selecionada.
   - Na lista suspensa **Tipo de recurso de destino**, selecione **Serviço de Aplicativo**.
   - Na lista suspensa **Recurso de destino**, na seção **rg-eshoponweb-eastus**, selecione o nome do aplicativo Web do Serviço de Aplicativo na região Leste dos EUA do Azure.
   - Na caixa de texto **Prioridade**, insira **1**.

   > **Observação:** Um valor de prioridade mais baixo designa uma precedência mais alta. Nesse caso, todas as solicitações serão enviadas para a instância do aplicativo Web na região Leste dos EUA, desde que essa instância permaneça disponível.

   - Verifique se as **Verificações de Integridade** estão habilitadas.

1. Selecione **Adicionar**.

1. De volta à página **devopsfoundationstmprofile \| Pontos de Extremidade****, selecione **+ Adicionar**.

   > **Observação:** Em seguida, você adicionará um ponto de extremidade representando o outro aplicativo Web do Serviço de Aplicativo do Azure implantado na região Oeste da Europa do Azure.

1. No painel **Adicionar ponto de extremidade**, execute as seguintes ações:

   - Verifique se **ponto de extremidade do Azure** aparece na lista suspensa **Tipo**.
   - Na caixa de texto **Nome**, insira **`DevOps Foundations web app - West Europe`**.
   - Verifique se a caixa de seleção **Habilitar Ponto de Extremidade** está selecionada.
   - Na lista suspensa **Tipo de recurso de destino**, selecione **Serviço de Aplicativo**.
   - Na lista suspensa **Recurso de destino**, na seção **rg-eshoponweb-westeurope**, selecione o nome do aplicativo Web do Serviço de Aplicativo na região Oeste da Europa do Azure.
   - Na caixa de texto **Prioridade**, insira **2**.
   - Verifique se as **Verificações de Integridade** estão habilitadas.

1. Selecione **Adicionar**.
1. De volta à página **devopsfoundationstmprofile \| Pontos de Extremidade****, verifique se ambos os pontos de extremidade estão listados com a entrada **Online** na coluna **Monitorar status**.

   > **Observação:** Talvez seja necessário aguardar alguns minutos e selecionar **Atualizar** antes que a entrada de status esteja atualizada.

### Tarefa 2: Validar a funcionalidade do Gerenciador de Tráfego

1. Inicie uma janela do navegador da Web e navegue até o nome DNS associado ao perfil do Gerenciador de Tráfego.
1. Verifique se o navegador da Web exibe a página familiar do site hospedado pelo aplicativo Web do Serviço de Aplicativo do Azure implantado no laboratório anterior.
1. Feche a janela do navegador da Web recém-aberta.
1. Alterne para a janela do navegador da Web exibindo o portal do Azure.
1. No portal do Azure, selecione o ícone do **Azure Cloud Shell** à direita da caixa de texto de pesquisa.
1. Se necessário, selecione **Bash** no menu suspenso no canto superior esquerdo do painel do Cloud Shell.
1. Na sessão Bash no painel do Cloud Shell, execute o seguinte comando para resolver o nome DNS do perfil do Gerenciador de Tráfego criado na tarefa anterior para um dos dois pontos de extremidade correspondentes (substitua o espaço reservado `<tm_profile>` pelo nome DNS do perfil do Gerenciador de Tráfego, por exemplo, `devopsfoundationstmprofile.trafficmanager.net`), mas certifique-se de remover `https:\\` à esquerda:

   ```cli
   nslookup <tm_profile>
   ```

1. Execute novamente o mesmo comando algumas vezes, aguardando um pouco mais de 5 segundos entre cada invocação (para eliminar a possibilidade de cache de DNS) e observe que a saída em cada caso faz referência ao nome DNS do aplicativo Web na região Leste dos EUA do Azure.

## Exercício 2: Testar a resiliência da carga de trabalho usando o Azure Chaos Studio

Neste exercício, você testará a resiliência da carga de trabalho usando o Azure Chaos Studio.

> **Observação:** Este exercício ilustra o uso do Azure Chaos Studio. A finalidade do Azure Chaos Studio é ajudar a medir, entender e criar resiliência de aplicativos e serviços. Isso é feito interrompendo intencionalmente cargas de trabalho a fim de identificar lacunas de resiliência e implementar a mitigação correspondente proativamente, em vez de reativamente.

O exercício é composto pelas seguintes tarefas:

- Tarefa 1: Configurar o ambiente do Azure Chaos Studio
- Tarefa 2: Implementar um experimento
- Tarefa 3: Validar o experimento

### Tarefa 1: Configurar o ambiente do Azure Chaos Studio

1. Na guia navegador da Web que exibe o portal do Azure, na caixa de texto de pesquisa na parte superior da página, insira **`Chaos Studio`** e, na lista de resultados, selecione **Chaos Studio**.
1. Na página **Chaos Studio**, selecione **Destinos**.
1. Na página **Chaos Studio \| Destinos**, selecione a instância do aplicativo Web do Serviço de Aplicativo do Azure no grupo de recursos **rg-eshoponweb-eastus** na região Leste dos EUA implantada no laboratório anterior.
1. Na barra de ferramentas, selecione o cabeçalho da lista suspensa **Habilitar destinos** e, na lista suspensa, selecione **Habilitar destinos diretos do serviço (Todos os recursos)**.
1. Na página **Habilitar destinos diretos do serviço**, verifique se a instância correta do aplicativo Web do Serviço de Aplicativo está listada, selecione **Examinar + Habilitar** e, em seguida, selecione **Habilitar**.

   > **Observação:** Aguarde até que o destino seja habilitado. Isso deve levar menos de um minuto.

### Tarefa 2: Implementar um experimento

1. No portal do Azure, na caixa de texto de pesquisa na parte superior da página, insira **`Chaos Studio`** e, na lista de resultados, selecione **Chaos Studio**.
1. Na página **Chaos Studio**, selecione **Experimentos**.
1. Na página **Experimentos**, selecione **+ Criar** e, na lista suspensa, selecione **Novo experimento**.
1. Na guia **Noções básicas** da página **Criar um experimento**, execute as seguintes ações:

   - Verifique se sua assinatura do Azure aparece na lista suspensa **Assinatura**.
   - Clique no link **Criar novo** abaixo da lista suspensa **Grupo de Recursos**; na caixa de texto **Nome**, insira **`rg-devops-foundations`** e clique em **OK**.
   - Na seção **Detalhes do experimento**, na caixa de texto **Nome**, insira **`DevOps_Foundations_Labs_Experiment_01`**.
   - Na lista suspensa **Região**, selecione a região do Azure **Oeste da Europa**.

   > **Observação:** Você pode escolher qualquer região do Azure, mas considerando que está testando falhas em um recurso na região Oeste da Europa, qualquer região que não seja o Leste dos EUA parece mais apropriada.

1. Na guia **Noções básicas** da página **Criar um experimento**, selecione **Avançar: Permissões >**.
1. Na guia **Permissões**, aceite a opção padrão **Identidade atribuída pelo sistema** e selecione **Avançar: Designer de experimentos >**.
1. Na guia **Designer de Experimentos**, execute as seguintes ações:

   - Na caixa de texto **Etapa**, insira **`Step 1: Failover an App Service web app`**.
   - Na caixa de texto **Ramificação**, insira **`Branch 1: Emulate an App Service failure`**.
   - Selecione **+ Adicionar ação** e, na lista suspensa, selecione **Adicionar falha**.

1. Na guia **Detalhes de falha** do painel **Adicionar falha**, na lista suspensa **Falhas**, selecione **Parar Serviço de Aplicativo** e defina o valor de **Duração (minutos)** como 10.
1. Na guia **Detalhes de falha** do painel **Adicionar falha**, selecione **Avançar: Recursos de destino >**.
1. Na guia **Recursos de destino** do painel **Adicionar falha**, verifique se a opção **Selecionar manualmente em uma lista** está selecionada, selecione o aplicativo Web do Serviço de Aplicativo que você adicionou como destino ao ambiente do Chaos Studio e selecione **Adicionar**.
1. De volta à guia **Designer de experimentos** da página **Criar um experimento**, selecione **Examinar + criar**.
1. Na guia **Revisar + criar** da página **Criar um experimento**, selecione **Criar**.

   > **Observação:** Aguarde até que o experimento seja criado. Isso deve levar menos de um minuto.

   > **Observação:** Para que o experimento seja bem-sucedido, você também precisa conceder permissões de conta de serviço gerenciada recém-criadas suficientes para interromper o aplicativo Web do Serviço de Aplicativo do Azure. Aproveitaremos para essa finalidade a função de Colaborador interno do Azure, mas você poderá criar uma função personalizada se quiser seguir o princípio de privilégios mínimos.

1. No portal do Azure, na caixa de texto de pesquisa na parte superior da página, insira **`App Services`** e selecione **Serviços de Aplicativos** na lista de resultados.
1. Na página **Serviços de Aplicativo**, selecione o aplicativo Web do Serviço de Aplicativo do Azure na região Leste dos EUA que você implantou no laboratório anterior.
1. Na página do aplicativo Web, no menu vertical no lado esquerdo, selecione **Controle de acesso (IAM)**.
1. Na página **IAM (controle de acesso)** do aplicativo Web, selecione **+ Adicionar** e, na lista suspensa, selecione **Adicionar atribuição de função**.
1. Na guia **Função** da página **Adicionar atribuição de função**, selecione **funções de administrador com privilégios**, na lista de funções, selecione **Colaborador** e então selecione **Avançar**.
1. Na guia **Membros** da página **Adicionar atribuição de função**, defina a opção **Acesso atribuído a** como **Identidade Gerenciada** e selecione **+ Selecionar Membros**
1. No painel **Selecionar identidades gerenciadas**, na lista suspensa **Identidades gerenciadas**, selecione **Experimento do Caos**, na lista de identidades gerenciadas, selecione a que você criou anteriormente nesta tarefa e selecione **Selecionar**.
1. De volta à guia **Membros** da página **Adicionar atribuição de função**, selecione **Examinar + atribuir** e, em seguida, selecione **Examinar + atribuir** novamente.

### Tarefa 3: Validar o experimento

1. No portal do Azure, navegue de volta para a página **Chaos Studio** e, no menu vertical no lado esquerdo, selecione **Experimentos**.
1. Na lista de experimentos, selecione **DevOps_Foundations_Labs_Experiment_01**.
1. Na página **DevOps_Foundations_Labs_Experiment_01**, selecione **Iniciar** e, quando solicitado para confirmação, selecione **OK**.
1. Aguarde até que o status do experimento seja alterado para **Em execução**.
1. Na página **DevOps_Foundations_Labs_Experiment_01**, na seção **Histórico**, selecione o link **Detalhes** na entrada que representa a execução do experimento atual.
1. Na página de detalhes do experimento **DevOps_Foundations_Labs_Experiment_01**, examine a listagem que contém a etapa, branch e a ação associadas ao experimento.

   > **Observação:** Talvez seja necessário selecionar a entrada **Atualizar** da barra de ferramentas para atualizar o status da listagem.

1. Selecione a entrada **Ação** para exibir o painel **Detalhes de falha**.
1. Na seção **Destinos em execução**, selecione a entrada que representa o aplicativo Web do Serviço de Aplicativo do Azure.
1. Na página do aplicativo Web, na seção **Essential**, observe que o status do aplicativo Web está listado como **Parado**.

   > **Observação:** Talvez seja necessário selecionar a entrada **Atualizar** da barra de ferramentas para atualizar o status do Aplicativo Web.

   > **Observação:** Agora vamos verificar se o Gerenciador de Tráfego está redirecionando com êxito as solicitações direcionadas ao seu perfil para o ponto de extremidade que representa o aplicativo Web do Serviço de Aplicativo na região Oeste da Europa.

1. Inicie um navegador da Web e navegue até o nome DNS associado ao perfil do Gerenciador de Tráfego.
1. Verifique se o navegador da Web exibe a página familiar do site hospedado pelo aplicativo Web do Serviço de Aplicativo do Azure implantado no laboratório anterior.
1. Alterne para a janela do navegador da Web exibindo o portal do Azure.
1. No portal do Azure, selecione o ícone do **Azure Cloud Shell** à direita da caixa de texto de pesquisa.
1. Na sessão Bash no painel do Cloud Shell, execute o seguinte comando para resolver o nome DNS do perfil do Gerenciador de Tráfego que você criou no exercício anterior para um dos dois pontos de extremidade correspondentes (substitua o espaço reservado `<tm_profile>` pelo nome DNS do perfil do Gerenciador de Tráfego, mas certifique-se de remover o `https:\\` à esquerda):

   ```cli
   nslookup <tm_profile>
   ```

1. Execute novamente o mesmo comando algumas vezes, aguardando um pouco mais de 5 segundos entre cada invocação (para eliminar a possibilidade de cache DNS) e verifique se a saída em cada caso faz referência ao nome DNS do aplicativo Web na região Oeste da Europa do Azure.

> **Observação:** Embora este exemplo possa parecer um pouco simplista, o objetivo principal do exercício era ilustrar o processo de implementação de testes baseados no Azure Chaos Studio e seus componentes primários. Você usaria a abordagem equivalente se um aplicativo Web do Serviço de Aplicativo do Azure fosse parte de um ambiente mais complexo, em que o impacto de sua falha poderia ser muito mais desafiador de prever.

### Exercício 3: Remover os recursos usados nos laboratórios

Neste exercício, você removerá os recursos usados nos laboratórios.

1. Na guia navegador da Web que exibe o portal do Azure, na caixa de texto de pesquisa na parte superior da página, insira **`Resource groups`** e, na lista de resultados, selecione **Grupos de recursos**.
1. Na página **Grupos de recursos**, na lista de grupos de recursos, selecione o grupo de recursos que você criou no laboratório atual.
1. Na página Grupo de recursos, selecione **Excluir grupo de recursos**.
1. Na caixa de texto **Inserir nome do grupo de recursos para confirmar a exclusão**, insira o nome do grupo de recursos que você está prestes a excluir e clique em **Excluir**.

   > **Observação:** Aguarde até que o grupo de recursos seja excluído. Isso deve levar menos de um minuto.

1. Repita a etapa anterior do grupo de recursos que você criou nos laboratórios anteriores deste curso.

    > **Observação:** Excluir o grupo de recursos removerá todos os recursos associados a ele, incluindo o perfil do Gerenciador de Tráfego e o ambiente do Azure Chaos Studio.

    > **Observação:** Não é necessário excluir a conta do GitHub e os repositórios criados nos laboratórios anteriores deste curso. Você pode continuar usando-os como um portfólio do seu trabalho.
