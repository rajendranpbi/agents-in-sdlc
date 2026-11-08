---
title: "Exercício 2 - Adicionar uma funcionalidade com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo podem exigir alterações em vários arquivos e tarefas como executar testes. Em geral, os desenvolvedores precisam localizar todos os arquivos necessários, fazer alterações, executar os testes, depurar, descobrir qual arquivo ficou faltando e fazer outra alteração. E assim por diante.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot foi desenvolvido para executar tarefas com mais autonomia no ambiente de desenvolvimento integrado (IDE). Ele age de forma semelhante a um desenvolvedor: começa explorando a estrutura do projeto, faz as alterações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Veja como usar o modo de agente para adicionar uma funcionalidade ao site.

> [!NOTE]
> Apesar dos nomes semelhantes, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente executa tarefas no IDE, o que permite ciclos de feedback rápidos e interação. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você verá como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- o modo de agente do GitHub Copilot pode implementar funcionalidades na interface do usuário (UI) e na camada de dados.
- revisar alterações e testes gerados pelo modo de agente antes de integrá-los ao código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. No exercício anterior, você adicionou um auxiliar para editoras; agora, concluirá o trabalho restante na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Executar o site Tailspin Toys

Antes de fazer alterações, explore o site Tailspin Toys para conhecer a funcionalidade atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que gera páginas da linguagem de marcação de hipertexto (HTML) estáticas durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter, usando o mapeador objeto-relacional (ORM) Drizzle. Não há uma interface de programação de aplicações (API) de back-end separada nem uma estrutura de interface no lado do cliente. Os auxiliares reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é implementada com scripts Astro `<script>` pequenos e específicos, usando interfaces de programação de aplicações (APIs) padrão do modelo de objetos de documento (DOM).

### Iniciar o site

O projeto inclui a tarefa `npm run dev`, que inicia o único servidor de desenvolvimento Astro. Execute-a no GitHub Codespaces:

1. Volte ao codespace. Você deve estar na branch `main`, após concluir o exercício anterior.
2. Abra um novo terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie uma branch para o trabalho de filtragem e mude para ela:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Execute o seguinte comando para iniciar o site:

   ```bash
   npm run dev
   ```

   Quando o servidor de desenvolvimento Astro estiver pronto, será exibido um banner com o endereço, semelhante a este:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo perguntando se você quer abrir uma janela do navegador para `http://localhost:4321`, feche-a selecionando **x**.

5. Para abrir o site, mantenha pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecione o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar o link local no terminal redireciona você para `https://<your-codespace-name>-4321.app.github.dev/`. Esse endereço é um túnel privado para o codespace, que agora hospeda o servidor Web.

### Explorar o site

Com o site em execução, explore os principais recursos:

- **Home Page**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Game Details Page**: ao selecionar um jogo, você acessa uma página de detalhes com informações como título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

O site já funciona, mas você vai ampliar seus recursos. Comece explorando o backlog de issues criado automaticamente quando você configurou o projeto a partir do modelo.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar as etapas abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa especificará as opções relevantes.

1. Na visualização Chat, selecione **Agent** no menu suspenso de agentes. O agente planeja e implementa alterações em vários arquivos, executa comandos de terminal e chama ferramentas.

   ![Seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos disponíveis, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> A inclusão de **Claude Sonnet 4.5** nestas instruções não indica preferência dos autores por um modelo. Esse modelo foi usado na criação do workshop para que as sugestões fossem relativamente consistentes. Como os modelos de linguagem de grande porte (LLMs) são probabilísticos, as sugestões podem ser diferentes das descritas no workshop; isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos modelos de linguagem de grande porte (LLMs), o Copilot pode usar um comando do protocolo Model Context Protocol (MCP) diferente, mas ainda deve conseguir concluir a tarefa.

3. Envie este prompt ao Copilot para consultar o backlog de issues:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista todas as issues.
5. Revise a lista gerada.

Observe que o Copilot também priorizou os itens de acordo com o que considera mais útil para as pessoas usuárias.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere código, revise o arquivo de instruções que fornecerá contexto ao Copilot. Você usará o arquivo de instruções de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, testabilidade e acessibilidade.
   - links para instruções específicas de tipos de arquivo, incluindo:
     - Astro.
     - Tailwind CSS.
     - Drizzle.

> [!TIP]
> Os arquivos de instruções podem referenciar outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao arquivo de instruções. Assim, é possível reutilizar orientações, dividir conjuntos complexos em partes menores e fornecer exemplos e modelos ao Copilot.

## Implementar a filtragem

Para concluir a filtragem, será necessário atualizar pelo menos três partes do aplicativo:

- adicionar ou aprimorar a lógica restante de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Antes de integrar as alterações, os testes também precisam ser executados e aprovados. O modo de agente do Copilot pode fazer esse trabalho.

1. Continue a conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções da interface do usuário](../../../_images/ex3-select-instructions-file.png)

3. Confirme se **Agent** continua selecionado no menu suspenso de agentes da visualização Chat.

   ![Seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme se **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade descrita na issue relacionada do backlog:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto, localizar os arquivos relacionados à funcionalidade, identificar os auxiliares da camada de dados, a interface e os testes, alterar os arquivos e executar os testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot explorará o projeto, modificará arquivos e executará testes. Isso pode levar alguns minutos, dependendo da complexidade da tarefa e do código. Durante o processo, os botões **Keep** e **Undo** podem aparecer no editor. Quando o trabalho terminar, haverá uma opção **Keep** ou **Undo** para cada alteração; não é necessário selecioná-las enquanto o agente ainda estiver trabalhando.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Caixa de diálogo no painel Copilot Chat pedindo confirmação para executar os testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Pode haver pausas e falhas em alguns testes durante o processo. O Copilot alterna entre gerar código e executar testes até concluir a tarefa sem detectar erros.

   ![Sessão concluída do Copilot Chat no modo de agente](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Explore o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Revise sempre o código gerado pelo Copilot ou por outras ferramentas de inteligência artificial.

10. Volte ao navegador onde o site está em execução e explore a nova funcionalidade.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela Copilot Chat.

## Resumo e próximos passos

Você usou o modo de agente do GitHub Copilot para adicionar recursos ao site Tailspin Toys. Você:

- explorou o projeto, identificou arquivos relevantes e coordenou alterações.
- implementou funcionalidades na interface do usuário e na camada de dados.
- revisou as alterações e os testes antes do merge.

Agora, [teste a funcionalidade com o servidor MCP do Playwright][next-lesson] e abra um pull request.

### Exercício extra: implementar paginação

À medida que a lista de jogos crescer, será necessário implementar paginação. Use as habilidades deste exercício e peça ao Copilot que atualize o site com esse recurso. Considere:

- seguir as práticas recomendadas existentes, inclusive usando os arquivos de instruções.
- decidir como implementar a paginação, por exemplo, se as pessoas poderão escolher o tamanho da página ou se ele será fixo.
- incluir no prompt as orientações necessárias para obter a implementação desejada.
- iterar com o GitHub Copilot, pedindo alterações e fornecendo contexto. Essa é uma parte normal do trabalho com o Copilot.

## Recursos

- [Copilot: modos ask, edit e agent — o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
