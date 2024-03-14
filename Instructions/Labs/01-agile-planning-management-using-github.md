---
lab:
  title: Planejamento e gerenciamento do Agile usando o GitHub
  module: Plan with DevOps
---

# Laboratório 01 – Planejamento e gerenciamento Agile usando o GitHub

## Tempo estimado: 40 minutos

## Cenário

Lembre-se do cenário deste módulo, no qual você está trabalhando para uma empresa de desenvolvimento de software no setor de varejo que planeja migrar sua loja online para um novo aplicativo, mas está tendo dificuldades para planejar o projeto devido à pouca colaboração e comunicação entre as equipes de desenvolvimento e operações. Já que você decidiu usar o GitHub para planejamento e gerenciamento Ágil, este laboratório oferece a oportunidade de criar um repositório no GitHub, marcos e problemas associados, um projeto e um quadro de projeto. Além disso, você poderá adicionar um item de rascunho ao quadro de projeto e um item com base em um problema e revisar as configurações de automação.

## Objetivos

Neste laboratório, você vai:

- Criar um repositório, projeto e quadro de projeto do GitHub
- Criar e gerenciar itens do quadro de projetos

> **Observação:** Para esse laboratório e os subsequentes você vai precisar de uma conta do GitHub. Recomendamos fortemente que você crie uma conta separada para usar nos laboratórios. Para criar uma conta no GitHub, siga as instruções no artigo [Como criar uma conta no GitHub](https://docs.github.com/get-started/quickstart/creating-an-account-on-github).

## Exercício 1: criar um repositório, um projeto e um quadro de projetos no GitHub

Neste exercício, você vai criar um repositório, um projeto e um quadro de projeto do GitHub.

> **Observação:** De acordo com a documentação do GitHub, o recurso Projetos oferece *uma ferramenta flexível e adaptável para planejar e acompanhar o trabalho no GitHub*. Os projetos fornecem acesso a *quadros*, que cumprem a função de quadros *Kanban*. Kanban é uma estrutura comum e amplamente utilizada em um ambiente Agile para representar o estado do trabalho do projeto.

O exercício é composto pelas seguintes tarefas:

- Tarefa 1: Criar um repositório do GitHub
- Tarefa 2: Criar marcos de referência e problemas do GitHub
- Tarefa 3: Criar um projeto do GitHub
- Tarefa 4: Criar um quadro de projetos do GitHub

### Tarefa 1: Criar um repositório do GitHub

1. Abra um navegador da web e navegue até a página inicial do [GitHub](https://github.com).
1. Quando solicitado a se autenticar, faça login usando sua conta de usuário do GitHub.
1. Na página inicial do GitHub, selecione a guia **Repositórios** e, a seguir, selecione **Novo**.
1. Na página **Criar um novo repositório**, execute as seguintes ações:

   - Na lista suspensa **Proprietário**, selecione o nome da conta de usuário do GitHub.
   - Na caixa de texto **Nome do repositório**, insira **DevOpsCoreIntroRepo**.
   - Altere a visibilidade do repositório para **Privado**.
   - Habilite a caixa de seleção **Adicionar um arquivo README**.
   - Na lista suspensa **Adicionar .gitignore**, selecione **Visual Studio**.

   > **Observação:** Para saber mais sobre .gitignore, confira [Como ignorar arquivos](https://docs.github.com/get-started/getting-started-with-git/ignoring-files).

   - Na lista suspensa **Escolher uma licença**, selecione **MIT**.

   > **Observação:** Para saber mais sobre seleções de licença, confira [Como licenciar um repositório](https://docs.github.com/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/licensing-a-repository).

1. Selecione **Criar repositório**.

### Tarefa 2: Criar marcos de referência e problemas do GitHub

1. Na página **DevOpsCoreIntroRepo**, selecione a guia **Problemas**.

   > **Observação:** você vai criar um marco de referência e um problema associados ao repositório recém-criado, que será usado posteriormente quando trabalhar com o quadro de projetos.

1. Na página **Problemas**, à esquerda do botão **Novo problema**, selecione **Marcos de Referência**.
1. Na página **Marcos de Referência**, selecione **Novo marco de referência**.
1. Na página **Novo marco de referência**, execute as seguintes ações:

   - Na caixa de texto **Título**, insira **versão alfa**.
   - Na caixa de texto **Data de conclusão (opcional)**, insira uma data uma semana após a data atual.
   - Na caixa de texto **Descrição**, insira **Conclusão da versão alfa**.

1. Selecione **Criar marco de referência**.
1. Repita as últimas três etapas para criar um marco de referência de **versão beta** com a data de conclusão duas semanas após a data atual. Na caixa de texto **Descrição**, insira **Conclusão da versão beta**.
1. Navegue de volta para a página **Problemas** e selecione **Novo problema**.
1. Na caixa de texto **Adicionar um título**, insira **A página README do repositório está vazia**.
1. Na caixa de texto **Adicionar uma descrição**, insira **A brevidade pode ser uma virtude, mas essa página README seria realmente melhor se tivesse um texto**.
1. Selecione o ícone de engrenagem ao lado da entrada **Marco de Referência** e, na lista suspensa, selecione **versão alfa**.
1. Selecione o ícone de engrenagem ao lado da entrada **Rótulos** e, na lista suspensa, selecione **bug**.
1. Selecione **Enviar novo problema**. Observe que o **n.º 1** foi atribuído ao problema automaticamente.

### Tarefa 3: Criar um projeto do GitHub

1. Na página principal do GitHub, selecione o ícone de avatar no lado direito da barra de ferramentas e, no menu suspenso, selecione **Seus projetos**.
1. Selecione **Novo projeto**.
1. No painel **Selecionar um modelo**, selecione **Planejamento em equipe** e, em seguida, selecione **Criar projeto**.  

   > **Observação:** alternativamente, você pode começar do zero e exibir o projeto no formato de tabela, quadro ou roteiro.

1. Na página do novo projeto, selecione o nome do projeto gerado automaticamente. Isso exibirá automaticamente a página **Configurações do Projeto**.
1. Na caixa de texto **Nome do Projeto**, insira **Projeto de Introdução de Itens Básicos do DevOps**.
1. Na caixa de texto **Descrição curta**, insira **Introdução aos Projetos do GitHub** e selecione **Salvar**.
1. Na seção **README**, insira o texto a seguir

   > **Observação:** a seção **README** inclui um editor de Markdown simplificado que ajuda você a criar uma página README visualmente atraente para o projeto. Você pode usar os ícones da barra de ferramentas para formatar o texto e usar a guia **Visualização** para revisar as alterações resultantes. Copie e cole o seguinte texto na seção README do editor:

   ```text
   ### Welcome to DevOps Core Intro Project ###

   **Projects are a customizable, flexible tool for planning and tracking your work.**

   To find out more, refer to GitHub documentation [about Projects](https://docs.github.com/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects).
   ```

1. Selecione **Visualização** para revisar a página resultante.
1. Selecione **Salvar**.
1. Role para baixo até a **Zona de Perigo** e **observe** a opção que permite alternar a visibilidade do projeto entre **Privado** e **Público**, feche o projeto e o exclua.

   > **Observação:** Não feche nem exclua o projeto nesta altura. Apenas observe as opções disponíveis para você.

1. Role até a parte superior da página e observe que você tem as opções de **Gerenciar o acesso** ao projeto e criar campos personalizados na interface do projeto.
1. Selecione a seta para a esquerda no rótulo **<- Configurações** para sair da página **Configurações**.

### Tarefa 4: Criar um quadro de projetos do GitHub

1. Na página **Projeto de Introdução de Itens Básicos do DevOps**, selecione o acento circunflexo voltado para baixo ao lado da guia **Lista de pendências** e, na seção **Layout**, selecione **Quadro**.

   > **Observação:** você pode usar essa opção para alternar facilmente entre as exibições de tabela, quadro e roteiro.

1. Revise a página resultante, que consiste de três colunas predefinidas com os seguintes rótulos:

   - **Pendentes** — contendo os que ainda não foram iniciados
   - **Em andamento** — contendo os que estão sendo trabalhados ativamente
   - **Concluídos** — contendo os que foram concluídos

   > **Observação:** esse layout representa um quadro Kanban muito básico. Em cada coluna, você pode adicionar itens individuais. Você também pode adicionar colunas extras.

1. Para adicionar uma coluna extra, selecione o ícone **+** à direita da coluna**Concluídos** e, a seguir, selecione **+ Nova Coluna**.
1. Na janela **Nova opção**, na caixa de texto **Texto de Rótulo**, insira **Revisar Em Andamento** e selecione uma cor que você queira atribuir à coluna. Na caixa de texto **Descrição**, insira **Este item está sendo revisado** e selecione **Salvar**.
1. Selecione o pequeno círculo ao lado do rótulo **Revisão em Andamento** da coluna recém-adicionada e use-o para arrastá-lo entre a coluna **Em Andamento** e a coluna **Concluídos**.

## Exercício 2: criar e gerenciar itens do quadro de projetos

Neste exercício, você vai criará e gerenciar itens do quadro de projetos

> **Observação:** existem duas maneiras básicas de adicionar itens a um quadro de projeto. Você pode criar um rascunho de item ou adicionar um item que represente um problema existente em um repositório GitHub.

O exercício é composto pelas seguintes tarefas:

- Tarefa 1: Adicionar um rascunho de item
- Tarefa 2: Adicionar um item baseado em um problema
- Tarefa 3: Revisar as configurações de automação

### Tarefa 1: Adicionar um rascunho de item

1. Na página **Projeto de Introdução de Itens Básicos do DevOps**, na coluna **Pendentes**, selecione **+ Adicionar item**.

   > **Observação:** Na caixa de texto exibida automaticamente, você pode começar a digitar para criar um rascunho ou digitar **#** para mencionar um problema existente em qualquer repositório do GitHub. Começaremos com a primeira dessas duas técnicas.

1. Na caixa de texto, insira **Wiki ausente** e pressione **Enter** no teclado. Isso adicionará um novo rascunho de item à coluna **Pendentes**.
1. No rascunho de item recém-adicionado, selecione o símbolo de reticências e, no menu suspenso, selecione **Converter em um problema**.
1. Na lista suspensa **Selecionar um item**, selecione **DevOpsCoreIntroRepo** para adicionar o item ao repositório criado no exercício anterior. Observe que o problema foi rotulado automaticamente com **n.° 2**.
1. Selecione o problema **Wiki ausente**.
1. No painel **Wiki ausente n.º 2**, observe que você tem configurações adicionais disponíveis nesta altura, incluindo rótulos e marcos de referência.
1. Selecione **Adicionar rótulos** e, na lista suspensa **Selecionar itens**, selecione **aprimoramento**.
1. Selecione **Adicionar marco de referência** e, na lista suspensa **Selecionar um item**, selecione **versão alfa**.
1. Feche o painel **Wiki ausente n.º 2**.

   > **Observação:** Agora, você vai adicionar outro rascunho de item e convertê-lo em um problema.

1. Na página **Projeto de Introdução de Itens Básicos do DevOps**, na coluna **Pendentes**, selecione **+ Adicionar item**.
1. Na caixa de texto, insira **Colaboradores adicionais necessários** e pressione **Enter** no teclado. Isso adicionará um novo rascunho de item à coluna **Pendentes**.

### Tarefa 2: Adicionar um item baseado em um problema

1. Na página **Projeto de Introdução de Itens Básicos do DevOps**, na coluna **Pendentes**, selecione **+ Adicionar item**.
1. Na caixa de texto, insira **#** para exibir uma lista de repositórios existentes. Na lista, selecione **DevOpsCoreIntroRepo**.
1. Em seguida, na lista de problemas existentes, selecione **A página README do repositório está vazia n.º 1**. Isso adicionará automaticamente esse problema à coluna **Pendentes**.
1. Selecione **A página README do repositório está vazia** para exibir o painel correspondente.
1. No painel **A página README do repositório está vazia**, na seção Pessoas Alocadas, selecione **Adicionar pessoas alocadas...**; na lista suspensa **Selecionar itens**, selecione seu nome de usuário do GitHub e feche o painel **A página README do repositório está vazia**.
1. Arraste o item **A página README do repositório está vazia** da coluna **Pendentes** e solte-o na coluna **Em Andamento**.

### Tarefa 3: Revisar as configurações de automação

1. Na página **Projeto de Introdução de Itens Básicos do DevOps**, logo abaixo do ícone de avatar, selecione o ícone de reticências.
1. Na lista suspensa, selecione **Fluxos de Trabalho**.
1. Na página **Fluxos de Trabalho**, no menu do lado esquerdo, revise a lista de fluxos de trabalho padrão. '

   > **Observação:** os fluxos de trabalho **Item encerrado** e **Solicitações de pull mescladas** estão habilitados por padrão.

1. Selecione **Item encerrado** e revise o fluxo de trabalho. O fluxo de trabalho define automaticamente o status de problemas como **Concluídos** sempre que o item em questão é encerrado.
1. Selecione o ícone de avatar no canto superior direito da página e, no menu suspenso, selecione **Seus repositórios**.
1. Na lista de repositórios, selecione **DevOpsCoreIntroRepo**.
1. **Na seção README**, selecione o ícone de lápis para abrir o arquivo no modo de edição.
1. Substitua o conteúdo existente pelo seguinte texto:

   ```text
   ### Welcome to DevOps Core Intro Project Repository ###

   **Projects are a customizable, flexible tool for planning and tracking your work.**

   To find out more, refer to GitHub documentation [about Projects](https://docs.github.com/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects).
   ```

1. Selecione **Confirmar alterações**.
1. Na janela **Confirmar alterações**, aceite as configurações padrão e selecione **Confirmar alterações** novamente.
1. Selecione a guia **Problemas**.
1. Selecione **A página README do repositório está vazia**.
1. Na página **A página README do repositório está vazia n.º 1**, selecione **Encerrar o problema**.
1. Observe que o encerramento do item resultou nas seguintes ações:

   - O status do item foi alterado automaticamente para **Concluído**, conforme indicado por um comentário extra informando que o bot **github-project-automation** migrou o item de **Em andamento** para **Concluídos** no**Projeto de Introdução de Itens Básicos do DevOps**.
   - O marco de referência **versão alfa** foi marcado como concluído, conforme indicado pela barra horizontal verde na seção **Marco de referência** da página.

   > **Observação:** Caso não esteja vendo as alterações, atualize a página.

1. Para verificar isso, navegue de volta para o modo de exibição de quadro do **Projeto de Introdução de Itens Básicos do DevOps** e observe que o item **A página README do repositório está vazia** aparece na coluna **Concluídos**.
