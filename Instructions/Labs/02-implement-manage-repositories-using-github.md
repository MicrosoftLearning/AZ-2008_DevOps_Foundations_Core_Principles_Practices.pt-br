---
lab:
  title: Implementar o fluxo de trabalho com o GitHub
  module: Develop with DevOps
---

# Laboratório 2: Implementar o fluxo de trabalho com o GitHub

## Tempo estimado: 30 minutos

## Cenário

Lembre-se do cenário deste módulo em que você está trabalhando para uma empresa de desenvolvimento de software no setor de varejo que planeja migrar uma loja online de um aplicativo antigo para um novo aplicativo chamado eShopOnWeb. Como você decidiu usar o Git e o GitHub para facilitar o gerenciamento do ciclo de vida do aplicativo, este laboratório oferece a oportunidade de começar criando um fork de um repositório existente, configurando-o, criando um problema, criando um branch, atualizando arquivos no branch, criando e mesclando uma pull request, fechando o problema e validando as alterações.

## Objetivos

Neste laboratório, você vai:

- Implementar e gerenciar repositórios com o GitHub

> **Observação:** Para este e os laboratórios seguintes, use a mesma conta do GitHub que você criou para a finalidade do primeiro laboratório.

## Pré-requisitos

- Conclusão do [Laboratório 1: Planejamento e gerenciamento Agile com o GitHub](01-agile-planning-management-using-github.md)

## Exercício 1: implementar e gerenciar repositórios com o GitHub

Neste exercício, você vai criar um fork de um repositório Git e gerenciá-lo com o GitHub.

> **Importante:** O uso do recurso GitHub Copilot para pull request é totalmente opcional. Para usar esse recurso, você precisa ser membro de uma empresa com uma **assinatura do Copilot Enterprise**. Ignore as etapas que envolvem o recurso GitHub Copilot para pull request caso você não tenha acesso a ele. Se você quiser saber mais sobre o recurso GitHub Copilot para pull request, veja [Sobre os resumos de pull requests do Copilot](https://docs.github.com/en/enterprise-cloud@latest/copilot/github-copilot-enterprise/copilot-pull-request-summaries/about-copilot-pull-request-summaries).

> **Observação:** Você criou um repositório no primeiro laboratório do nosso curso. Neste laboratório, você começará criando um fork de um repositório existente. Um fork é um repositório que compartilha as configurações de código e visibilidade com um repositório upstream existente. Essa abordagem é frequentemente usada durante o desenvolvimento de atualizações para projetos de código aberto ou em cenários em que o acesso de gravação ao repositório upstream não está disponível. Para obter mais informações, veja [Como trabalhar com forks](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks).

O exercício é composto pelas seguintes tarefas:

- Tarefa 1: Criar um fork de um repositório do GitHub
- Tarefa 2: Configurar um repositório do GitHub
- Tarefa 3: Criar um problema
- Tarefa 4: Criar um branch
- Tarefa 5: Atualizar os arquivos no branch
- Tarefa 6: Criar e mesclar uma pull request
- Tarefa 7: Fechar o problema
- Tarefa 8: Validar as alterações

### Tarefa 1: Criar um fork de um repositório do GitHub

1. Inicie um navegador da Web e navegue até a home page do [GitHub](https://github.com).
1. Quando precisar se autenticar, conecte-se usando sua conta de usuário do GitHub.
1. Abra outra guia na mesma janela do navegador e navegue até o repositório [Spoon-Knife](https://github.com/octocat/Spoon-Knife).
1. Na página do repositório **Spoon-Knife**, selecione **Fork**.
1. Na página **Criar um fork**, verifique se a entrada da lista suspensa **Proprietário** exibe seu nome de usuário do GitHub, aceite a entrada padrão **Spoon-Knife** na caixa de texto **Nome do repositório**, mantenha a caixa de seleção **Copiar somente a ramificação principal** habilitada e escolha **Criar fork**.

   > **Observação:** A sessão do navegador será redirecionada automaticamente para o repositório com fork recém-criado.

### Tarefa 2: Configurar um repositório do GitHub

1. Na página do repositório **Spoon-Knife** com fork, na barra de ferramentas, selecione **Configurações**.
1. Na seção **Geral** da guia **Configurações**, observe que o branch padrão está definido como **principal*.
1. Navegue até a área **Recursos** da seção **Geral** e habilite a caixa de seleção **Problemas**.
1. No menu de navegação do lado esquerdo, no agrupamento **Código e automação**, selecione a entrada **Páginas**.
1. No painel do **GitHub Pages**, na seção Branch, altere a entrada **Nenhum** na lista suspensa para **principal** e selecione **Salvar**.

   > **Observação:** O GitHub Pages publicará automaticamente o conteúdo do repositório em um site acessível por meio da URL `https://<your_GitHub_username>.github.io/Spoon-Knife/`.

1. No painel do **GitHub Pages**, selecione o botão **Visitar site**. Isso abrirá automaticamente outra guia do navegador da Web e exibirá a página que representa o conteúdo atual do arquivo index.html.

   > **Observação:** Talvez seja necessário aguardar alguns minutos antes que o botão **Visitar site** e a página fiquem disponíveis.

   > **Observação:** Realize as etapas restantes desta tarefa se você concluiu o primeiro laboratório.

1. De volta à página do repositório **Spoon-Knife** com fork, na barra de ferramentas, selecione **Projetos**.
1. No painel **Boas-vindas ao novo projeto**, selecione **Vincular um projeto** e, no menu suspenso, selecione **Vincular um projeto existente**.
1. Na lista de projetos existentes, selecione **Projeto de Introdução ao DevOps Core**.

### Tarefa 3: Criar um problema

1. Na página **Spoon-Knife** com fork, selecione a guia **Problemas**.
1. Na página **Boas-vindas aos problemas!**, selecione **Novo problema**.
1. Na caixa de texto **Adicionar título**, insira **`index.html looks rather austere`**.
1. Na caixa de texto **Adicionar descrição**, insira **`index.html file can use a modern touch`**.
1. No painel atual, na seção Destinatários, selecione **Adicionar destinatários…** e, na seção **Sugestões**, escolha o nome de usuário do GitHub.
1. Selecione o ícone de engrenagem ao lado da entrada **Rótulos** e, na lista suspensa, selecione **aprimoramento**.
1. Selecione o ícone de engrenagem ao lado da entrada **Projetos** e, na lista suspensa, escolha **Projeto de Introdução do DevOps Core**.
1. Selecione **Enviar novo problema**.
1. No painel **O index.html parece bem simples**, na seção **Projetos**, defina **Status** como **Em andamento**.
1. Na página **Spoon-Knife** com fork, selecione a guia **Projetos**.
1. Na página **Boas-vindas aos novos projetos**, escolha **Projeto de Introdução do DevOps Core**.
1. Na exibição de quadros do **Projeto de Introdução do DevOps Core**, dê uma olhada na coluna **Em andamento** e observe que ela inclui o problema recém-criado.

### Tarefa 4: Criar um branch

1. Volte à guia **Código**.
1. No canto superior esquerdo da página, selecione a entrada **principal** para exibir a lista suspensa **Alternar branches/tags**.
1. Na caixa de texto **Localizar ou criar um branch…**, insira **`update index.html`** e selecione a entrada **Criar branch: atualizar index.html por meio de 'principal'** para criar um branch.

   > **Observação:** Isso fará com que o branch recém-criado seja automaticamente o atual, conforme indicado pelo conteúdo da lista suspensa.

### Tarefa 5: Atualizar os arquivos no branch

1. Na página do repositório **Spoon-Knife** com fork, na listagem de arquivos, selecione **index.html**.
1. Na página **Spoon-Knife/index.html**, no lado direito da barra de ferramentas do editor de código, selecione o ícone de lápis para alternar para o modo do editor.
1. No painel do editor, substitua todo o elemento do corpo da página (linhas 12 a 17) pelo seguinte código HTML:

   ```html
   <div id="octocat">
     <img src="https://octodex.github.com/images/NUX_Octodex.gif" alt="Octocat Image">
   </div>

   <p>
     Ready to team up? Let's collaborate, @octocat!
   </p>
   ```

1. No canto superior direito da página do editor, selecione **Fazer commit das alterações…**.
1. Na janela **Fazer commit das alterações**, na caixa de texto **Descrição estendida**, insira **`Modified the image and paragraph text`**, aceite a mensagem de commit padrão e clique em **Fazer commit das alterações**.

   > **Observação:** Neste momento, você também tem a opção de criar um branch para o commit.

1. Na listagem de arquivos do repositório à esquerda, selecione **styles.css**.
1. Na página **Spoon-Knife/styles.css**, no lado direito da barra de ferramentas do editor de código, selecione o ícone de lápis para alternar para o modo do editor.
1. No painel do editor, substitua a linha 17 por todo o seguinte código HTML:

   ```css
     color: #333;
     line-height: 1.5;
     text-align: center;
   }

   body {
     font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
     background-color: #f8f9fa;
   }

   #octocat img {
     display: block;
     width: 100%;
     height: auto;
   }
   ```

1. No canto superior direito da página do editor, selecione **Fazer commit das alterações…**.
1. Na janela **Fazer commit das alterações**, na caixa de texto **Descrição estendida**, insira **`Modified CSS tags and selectors`**, aceite a mensagem de commit padrão e clique em **Fazer commit das alterações** para fazer commit das alterações no branch update-index.html.

### Tarefa 6: Criar e mesclar uma pull request

1. Volte à página do repositório **Spoon-Knife** com fork.
1. Verifique se o branch **update-index.html** está sendo exibido, conforme indicado pelo rótulo que aparece na lista suspensa no canto superior esquerdo da página. Se esse rótulo exibir **principal**, selecione-o primeiro e, depois, na lista suspensa que contém a lista de branches, selecione **update-index.html**.
1. Na página do repositório **Spoon-Knife** com fork, selecione **Contribuir** e escolha **Abrir pull request**.
1. Na página **Abrir uma pull request**, selecione a entrada **repositório base: octocat/Spoon-Knife**.
1. Na lista suspensa **Escolher um Repositório Base**, selecione o nome do repositório com fork criado no início do laboratório.

   > **Observação:** O nome começará com o seu nome do GitHub, seguido por uma barra "/" e, depois, por **Spoon-Knife**. Depois que você selecioná-lo, a entrada deverá ser alterada para **base: principal**.

   > **Observação:** Isso é necessário porque queremos atualizar a ramificação principal no repositório com fork, em vez da ramificação principal no repositório do qual você criou o fork.

1. Na caixa de texto **Adicionar um título**, substitua **Atualização de index.html** por **Atualização de index.html e styles.css**.

1. (Opcional) Se você tiver acesso ao recurso GitHub Copilot para pull request, na caixa de texto **Adicionar uma descrição**, clique no botão **Ação do Copilot** e escolha **Resumo** (gerar um resumo das alterações na pull request).

   1. O recurso GitHub Copilot para pull request vai gerar um resumo das alterações na pull request.

       ![Recurso GitHub Copilot para pull request](media/github-pull-request-action.png)

   1. Leia o resumo gerado pelo recurso GitHub Copilot para pull request.

   1. Depois que o resumo for gerado, insira **`Addressing #1`** em **Adicionar uma descrição** na primeira linha e clique em **Criar pull request**.

       ![Resumo do GitHub Copilot](media/github-pull-request-copilot-summary.png)

   > **Observação:** Caso você não tenha acesso ao recurso GitHub Copilot para pull request, ignore esta etapa.

   > **Observação:** se você optar por usar o recurso GitHub Copilot para pull request, ignore a próxima etapa.

1. Na caixa de texto **Adicionar descrição** insira **`Addressing #1`** e clique em **Criar pull request**.

   > **Observação:** Ao incluir **nº 1**, você poderá referenciar o primeiro problema associado a essa pull request.

1. Verifique se o branch atual não tem nenhum conflito com o branch base, selecione **Mesclar pull request** e escolha **Confirmar mesclagem**.
1. Verifique se a pull request foi mesclada e fechada com êxito e selecione **Excluir branch**.

### Tarefa 7: Fechar o problema

1. Na barra de ferramentas da página do GitHub, selecione a guia **Problemas**.
1. Marque a caixa de seleção à esquerda do primeiro problema **O index.html parece bem simples**, selecione **Marcar como** e, na lista suspensa, selecione **Fechado**.
1. Volte à exibição de quadros do **Projeto de Introdução do DevOps Core** e observe que o problema agora é exibido na coluna **Concluído**.

### Tarefa 8: Validar as alterações

1. Na janela do navegador da Web, volte à página do repositório **Spoon-Knife** com fork, selecione a guia **Configurações** e, no menu de navegação do lado esquerdo, no agrupamento **Código e automação**, escolha **Pages** para exibir o painel **GitHub Pages**.
1. No painel **GitHub Pages**, selecione **Visitar site** para abrir outra guia do navegador exibindo o conteúdo atualizado do arquivo index.html.
1. Verifique se a página foi atualizada para incluir os elementos visuais referenciados nos arquivos HTML e CSS.

> **Observação:** Neste ponto, você pode enviar as alterações na ramificação principal do fork de volta ao repositório original. Normalmente, essa é a próxima etapa no desenvolvimento de atualizações e colaboração em projetos de código aberto. No entanto, como o repositório original não tem manutenção, esta etapa não é aplicável aqui.
