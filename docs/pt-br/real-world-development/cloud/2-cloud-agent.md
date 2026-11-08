---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Poucas organizações estão livres de dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualização ou solicitações de funcionalidades que permanecem no backlog por falta de tempo para implementá-las. O agente de nuvem do GitHub Copilot pode atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria um pull request (PR) em rascunho para que um desenvolvedor revise o trabalho. Assim, você pode delegar tarefas repetitivas, acelerar o desenvolvimento e se concentrar em questões mais amplas.

Neste exercício, você vai explorar como usar o agente de nuvem do Copilot para:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- entender a importância de issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem dívidas técnicas para resolver. As pessoas contratadas para criar a primeira versão do site deixaram a documentação em um estado inadequado: ela está praticamente ausente. Como primeiro passo, a equipe quer adicionar comentários de documentação TSDoc a todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer facilitar a descoberta de jogos. Ela gostaria que a página de detalhes de cada jogo mostrasse outros títulos da mesma categoria, para que as pessoas pudessem continuar navegando. Por enquanto, a equipe não precisa de uma implementação refinada; quer algo que possa usar para testes de aceitação da experiência do usuário (UX). Embora essa tarefa seja um bloqueio, há outras issues de prioridade mais alta.

Esses são exemplos de tarefas que podem ser adiadas e que são adequadas para atribuir ao agente de nuvem do Copilot. Ele trabalha de forma assíncrona enquanto você se concentra em outras tarefas. Depois, você pode voltar para revisar o trabalho e confirmar se o resultado está correto.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de forma semelhante a um desenvolvedor. Assim como no trabalho com outra pessoa da equipe, há várias maneiras de iniciar uma tarefa, incluindo [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request em rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode acompanhar a sessão enquanto o trabalho está em andamento ou depois de concluído. Quando a solução estiver pronta para revisão, o Copilot marcará você no pull request.

## A importância de instruções com escopo adequado

O GitHub Copilot não é mágico: ainda é preciso pensar. Mesmo operações aparentemente simples ficam complexas quando examinadas de perto. Por isso, [defina com cuidado o escopo das tarefas para o agente de nuvem do Copilot][cloud-agent-best-practices]. Trate-o como um parceiro de programação com inteligência artificial: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inclusão de inteligência artificial generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram as instruções personalizadas e como elas orientam o Copilot. Se você já concluiu o exercício prático sobre instruções, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos de instruções incluídos no repositório sem alterá-los para que possa observar o efeito deles mais tarde.

Abra os arquivos a seguir na interface Web do GitHub ou em um codespace que já esteja em execução:

- `.github/copilot-instructions.md`: examine a seção **Code standards**, especialmente as convenções de TypeScript e os comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Na próxima seção, quando você atribuir ao agente de nuvem a issue `Code lacks documentation`, observe se o pull request resultante contém comentários TSDoc, segue as convenções de TypeScript e inclui cabeçalhos de comentários. Essas características vêm dos arquivos de instruções. Você voltará a elas ao revisar os PRs em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente de nuvem do Copilot

Criar código normalmente exige um ambiente específico e a execução de scripts de configuração para garantir que tudo esteja pronto. O mesmo vale para as tarefas atribuídas ao Copilot, que trabalha de forma semelhante a um profissional de engenharia de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente de trabalho. Você pode personalizá-lo com um [fluxo de trabalho de configuração especial][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, que é executado antes do início do trabalho. Isso disponibiliza as ferramentas de desenvolvimento e dependências necessárias. Esse fluxo já foi configurado para o laboratório. Ele fornece ao Copilot o Node.js, as dependências do projeto, os navegadores do Playwright para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido com dados para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

O arquivo segue o formato dos demais fluxos do GitHub. Ele contém alguns pontos importantes:

- Um único trabalho, chamado `copilot-setup-steps`, é executado no GitHub Actions antes de o Copilot começar a trabalhar no pull request.
- O gatilho `workflow_dispatch` permite executar o fluxo manualmente na guia **Actions** do repositório. Isso é útil para testar se a configuração funciona sem esperar uma execução do Copilot.

## Adicionar documentação

Embora a importância da documentação seja conhecida, muitos projetos têm informações desatualizadas ou não têm documentação. Essa dívida técnica pode reduzir a produtividade, dificultar a manutenção do código e tornar mais difícil integrar novos desenvolvedores à equipe. O Copilot pode ajudar a criar documentação, por isso esta é uma issue adequada para atribuir ao agente de nuvem. Ele trabalhará em segundo plano; você revisará o resultado em um exercício futuro.

1. Em uma nova guia do navegador, acesse o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. No campo **Title**, digite `Code lacks documentation`.
6. No campo **Description**, digite:

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

    ![Detalhes da atribuição ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o pull request gerado, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele ainda não aparecer na lista, aguarde um ou dois minutos e atualize a janela do navegador.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar vários minutos para aparecer no PR. O Copilot está configurando o ambiente com o fluxo de trabalho descrito anteriormente, analisando o projeto e definindo uma abordagem.

13. Revise a lista e as tarefas que o Copilot pretende executar.
14. Role a linha do tempo do pull request e procure uma atualização informando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

    ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Percorra a sessão em tempo real e observe como o Copilot resolve o problema: explora o código e o contexto, pausa para analisar e definir um plano e, em seguida, cria código.

Esse processo pode levar vários minutos. Um dos objetivos principais do agente de nuvem do Copilot é executar tarefas de forma assíncrona para que você possa se concentrar em outras atividades. Aproveite esse recurso atribuindo outra tarefa ao agente e, em seguida, desenvolvendo uma funcionalidade no aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma das vantagens do agente de nuvem do GitHub Copilot é permitir que o trabalho seja dividido: enquanto ele cuida de uma tarefa, você pode se concentrar em outra. A seção de jogos relacionados pode ser rápida de implementar, mas atribuí-la ao agente libera seu tempo para outras tarefas.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para usar o modelo em branco.
5. No campo **Title**, digite `Show related games on the game details page`.
6. No campo **Description**, digite:

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.

    ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, um emoji 👀 deverá aparecer no primeiro comentário da issue, indicando que o Copilot começou a trabalhar.

![Emoji de olhos indicando que o Copilot está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot agora está trabalhando na segunda solicitação. Como o agente de nuvem trabalha de forma semelhante a um profissional de engenharia de software, não é necessário monitorá-lo continuamente. Revise o resultado quando estiver pronto. Agora, conheça os agentes personalizados.

## Resumo e próximos passos

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Ele permite atribuir issues ao Copilot para que sejam resolvidas de forma assíncrona. Você pode usá-lo para resolver dívidas técnicas, criar funcionalidades ou ajudar a migrar código entre estruturas.

Você explorou estes conceitos:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- entender a importância de issues com escopo claro.
- atribuir issues ao Copilot.

Enquanto o agente de nuvem trabalha em segundo plano, [crie e use agentes personalizados][next-lesson]. O [agente de nuvem do Copilot também pode usar servidores do protocolo Model Context Protocol (MCP)][cloud-agent-mcp] e as instruções personalizadas exploradas nos módulos anteriores.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
