---
title: "Lição 1 - Instalar o aplicativo GitHub Copilot"
description: "Instale o aplicativo GitHub Copilot, conecte o repositório criado a partir do modelo, conheça o espaço de trabalho e experimente um chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O [**aplicativo GitHub Copilot**][about-copilot-app] é um aplicativo para desktop voltado ao desenvolvimento orientado por agentes. Ele foi criado com base no GitHub Copilot CLI e se integra nativamente ao GitHub, para que seus repositórios, branches e pipelines de integração contínua (CI) funcionem sem configuração adicional. O aplicativo foi desenvolvido para fluxos de trabalho em que você orienta vários agentes em paralelo — cada um em um espaço de trabalho isolado — em vez de fazer todo o trabalho por conta própria, além de automatizar tarefas repetitivas. Com o Node.js instalado e sua cópia do projeto pronta, a próxima etapa é instalar o aplicativo e conectar o repositório.

Nesta lição, você vai:

- instalar o aplicativo GitHub Copilot e entrar na sua conta.
- adicionar seu projeto ao aplicativo a partir do repositório no GitHub.
- conhecer o espaço de trabalho, incluindo o backlog criado pelo modelo.
- experimentar um chat rápido para saber mais sobre o próprio aplicativo.

## Cenário

Sua equipe está adotando agentes de IA para trabalhar em um backlog crescente. O aplicativo Copilot oferece um único lugar para orientar esse trabalho: escolher issues, executar agentes, revisar alterações e fazer merge de pull requests. Nesta lição, você instalará e conectará o aplicativo e se familiarizará com o início de uma conversa sobre o projeto.

> [!NOTE]
> É necessário ter um plano elegível do Copilot: Copilot Free, Copilot Student ou qualquer plano pago (Pro, Pro+, Business ou Enterprise). Se você usa o Copilot Business ou o Copilot Enterprise, o administrador precisa manter habilitada a política **GitHub Copilot app**.

## Instalar e configurar o aplicativo GitHub Copilot

Para usar o aplicativo GitHub Copilot, primeiro instale-o. Há versões para Windows, macOS e Linux. Em seguida, autentique-se e adicione o repositório Tailspin Toys ao aplicativo.

1. Em um navegador, abra a [página inicial do aplicativo GitHub Copilot][download-app].
2. Baixe o aplicativo para sua plataforma e instale-o seguindo as instruções da página.
3. Abra o aplicativo depois de instalá-lo.
4. Selecione **Sign in to GitHub** e siga as instruções para se autenticar. Se você usa o GitHub Enterprise Cloud com residência de dados, selecione **Use GitHub Enterprise** e informe o nome do host `*.ghe.com` quando solicitado. O GitHub Enterprise Server não é compatível.
5. Se o processo inicial oferecer a seleção de repositório, selecione o repositório Tailspin Toys criado a partir do modelo. Você também pode pular essa etapa e adicionar o projeto depois.
6. Escolha um tema quando solicitado e conclua a configuração inicial.
7. Procure seu repositório em **Sessions**, na barra lateral. Se ele já estiver listado, não é necessário adicioná-lo novamente.
8. Se o repositório ainda não estiver conectado, selecione **+** ao lado de **Sessions** para criar uma sessão e, em seguida, selecione **Add GitHub repository**. Use o proprietário e o nome do repositório anotados na lição de pré-requisitos.
9. Confirme se o repositório aparece na barra lateral. No layout usado aqui, pastas de repositório como `tailspin-toys` aparecem agrupadas em **Sessions**, com as sessões individuais abaixo de cada repositório.

## Conhecer o espaço de trabalho

Com o projeto conectado, conheça as áreas da barra lateral. O aplicativo organiza o trabalho nestas seções:

- **New** — inicie uma nova sessão de chat com o Copilot.
- **Pull requests** — navegue e filtre pull requests, confira o status da CI e revise pull requests sem sair do aplicativo.
- **Issues** — navegue e filtre issues e inicie uma sessão a partir de uma issue.
- **Automations** — tarefas de agente salvas que são executadas em uma agenda ou sob demanda. Elas podem ajudar a gerenciar listas de tarefas, fazer a manutenção regular do projeto ou delegar outras atividades repetitivas. O resumo apresenta as automações como próximo passo, não como outro exercício do workshop.
- **Customize** — adicione recursos ao aplicativo Copilot, como servidores do protocolo Model Context Protocol (MCP), plugins e skills. Você usará esta seção para configurar o MCP do Playwright.
- **Chats** — conversas rápidas para perguntas e brainstorming que não precisam de uma branch nem de um espaço de trabalho próprios. Esta seção aparece depois que você inicia seu primeiro chat. Se ela não estiver visível, selecione **+** ao lado de **Sessions** para iniciar um chat.
- **Sessions** — agrupe pastas de repositório e sessões de agentes. Selecione o nome de um repositório, como `tailspin-toys`, para abrir a visualização do repositório, ou selecione uma sessão abaixo dele para voltar à conversa. Cada sessão é executada em um espaço de trabalho isolado, permitindo executar várias sessões ao mesmo tempo sem que as alterações entrem em conflito. Você iniciará sua primeira sessão ao adicionar avaliações por estrelas.

À medida que avançar no workshop, você conhecerá melhor o espaço de trabalho.

> [!TIP]
> Em caso de dúvida, pergunte ao Copilot. Se não souber como fazer algo ou quiser saber se é possível, peça orientação ao Copilot.

### Localizar o backlog criado pelo modelo

Explore o backlog criado quando você gerou sua cópia a partir do modelo. Procure as issues pelo título, sem presumir quais são os números delas.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys, não uma sessão individual abaixo dela. Em seguida, selecione **Issues** na navegação superior do repositório.
2. Localize estas issues pelo título:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecione uma issue para ler os detalhes. Cada issue também pode iniciar uma sessão de agente. Você começará pela issue de filtragem depois de concluir uma primeira alteração rápida.

> [!NOTE]
> As listas de **Issues** e **Pull requests** são filtradas automaticamente para mostrar apenas itens dos repositórios adicionados ao aplicativo Copilot. Para ver itens de trabalho de outros repositórios, adicione-os ao aplicativo.

## Experimentar um chat rápido

Use um chat rápido para se familiarizar com o próprio aplicativo. Você pode fazer uma pergunta ou trocar ideias sem criar uma branch nem uma worktree, o que torna esse tipo de chat adequado para perguntas rápidas que não exigem uma sessão.

1. Na barra lateral, selecione **+** ao lado de **Chats** para abrir um chat. Se **Chats** ainda não estiver visível, selecione **+** ao lado de **Sessions** para iniciar seu primeiro chat; **Chats** aparecerá depois.
2. Pergunte ao aplicativo como as sessões dele funcionam:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Leia a resposta na visualização da conversa. Você verá que cada sessão é executada em sua própria worktree isolada do Git, o que permite executar vários agentes em paralelo sem que as alterações entrem em conflito. Você pode continuar a conversa ou iniciar outro chat a qualquer momento.

## Resumo e próximos passos

Você instalou o aplicativo GitHub Copilot, conectou o projeto e conheceu o espaço de trabalho. Você:

- instalou o aplicativo e entrou na sua conta do GitHub.
- adicionou um projeto a partir do repositório no GitHub.
- conheceu o espaço de trabalho e localizou o backlog criado pelo modelo em **Issues**.
- usou um chat rápido para fazer uma pergunta breve.

Em seguida, você [iniciará sua primeira sessão de agente][next-lesson] e a usará para exibir uma avaliação por estrelas nos cards dos jogos.

## Recursos

- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Introdução ao aplicativo GitHub Copilot][getting-started]
- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
