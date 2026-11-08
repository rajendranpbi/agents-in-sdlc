---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Explore os modos de agente: use Plan para definir uma abordagem, Autopilot para desenvolver a filtragem a partir de uma issue e Interactive para revisar e verificar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Começamos adicionando uma pequena funcionalidade ao projeto. Alterações maiores exigem um processo mais robusto. O aplicativo GitHub Copilot foi desenvolvido para se integrar ao fluxo de trabalho existente da organização e ajudar você a desenvolver as funcionalidades certas da maneira adequada. Nesta lição, você inicia um fluxo de desenvolvimento orientado por agentes: parte de uma issue para criar uma funcionalidade, verifica o código, valida o comportamento e, por fim, integra as alterações ao projeto.

> [!NOTE]
> Você usará a mesma sessão durante o fluxo de trabalho da funcionalidade. Em geral, equipes usam sessões ou pull requests (PRs) diferentes para cada tipo de arquivo alterado; neste workshop, vamos simplificar para manter o foco nos conceitos principais.

Nesta lição, você vai:

- iniciar uma sessão de agente a partir de uma issue do GitHub.
- definir os requisitos no modo **Plan**.
- implementar a funcionalidade no modo **Autopilot**.
- revisar o código.
- validar manualmente a funcionalidade em um canvas de navegador.

À medida que desenvolver essa funcionalidade, você atualizará as instruções do repositório, personalizará a skill `quality-checks`, adicionará validação com o protocolo Model Context Protocol (MCP), criará um agente de garantia de qualidade (QA) e abrirá o PR da funcionalidade.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e editora. A issue do backlog descreve a funcionalidade, mas detalhes como a combinação de categorias precisam ser definidos antes da implementação. Você usará o modo Plan para tomar essas decisões e, depois, autorizará uma implementação delimitada com o Autopilot.

## Contexto

Incorporar agentes de programação com inteligência artificial ao fluxo de desenvolvimento não muda os fundamentos — na verdade, torna-os ainda mais importantes. A maioria dos desenvolvedores segue um processo semelhante a este:

1. Abra uma issue registrada que descreva o trabalho necessário.
2. Crie um plano do que precisa ser desenvolvido.
3. Desenvolva e revise o código.
4. Execute os testes para validar o código.
5. Valide manualmente a nova funcionalidade.
6. Crie um pull request (PR).
7. Depois da revisão do código e da aprovação do processo de integração contínua, faça o merge.

> [!NOTE]
> Os detalhes variam de acordo com a equipe e a organização, mas a maioria dos processos segue uma variação dessas etapas.

Seguir essa abordagem ajuda a garantir que o código gerado por inteligência artificial atenda aos requisitos e passe pelo mesmo processo de revisão que o código escrito manualmente.

## Modos de sessão

O **modo de sessão** controla o nível de autonomia do agente. Você pode selecioná-lo no menu suspenso abaixo do campo de prompt e alterá-lo a qualquer momento:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua orientação antes de continuar.
- **Plan**: o agente cria um plano. Você o revisa e aprova antes de permitir que o agente o execute.
- **Autopilot**: o agente trabalha de forma autônoma — escreve código, executa testes e itera sem aguardar sua orientação.

Comece no modo Plan, revise o plano e, em seguida, use o Autopilot para implementá-lo.

## Iniciar uma sessão a partir da issue

Antes de começar, confirme se o PR de avaliações por estrelas foi integrado e se a `main` local está atualizada.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys e, na navegação superior do repositório, selecione **Issues**. Abra **Allow users to filter games by category and publisher**. Essa issue já foi criada a partir do modelo; não é necessário criar outra.
2. Selecione **New session** e escolha uma **new working tree** baseada na `main` atualizada.

    ![Visualização da issue no aplicativo GitHub Copilot com uma seta apontando para o botão New session](../../../_images/app-new-session-from-issue.png)

3. Confirme se a issue está anexada à sessão e selecione **Plan** no seletor de modo.

> [!NOTE]
> Se o Node.js ou o npm não for encontrado, peça ao agente que execute `node --version` e `npm --version`. Se os comandos funcionarem no terminal, mas não na sessão, verifique a configuração de `PATH` ou do gerenciador de versões e feche o aplicativo completamente antes de abri-lo novamente. Repita a operação e as verificações que falharam; não reinstale o npm apenas porque o agente não o encontrou.

## Planejar a funcionalidade de filtragem

O planejamento permite revisar a abordagem antes de o Copilot escrever código. Como você começou pela issue, o Copilot já tem a solicitação como contexto. Envie o prompt a seguir e use as perguntas e o plano resultante para definir o comportamento esperado.

1. Envie este prompt:

   ```plaintext
   Build this feature.
   ```

2. Responda às perguntas do Copilot e compare o plano com os critérios de aceitação da issue. Confirme se ele aborda a filtragem por categoria e editora, os controles acessíveis, as alterações no acesso a dados e os testes. Esclareça comportamentos como a combinação de várias categorias ou o resultado quando nenhum jogo corresponde.
3. Confirme se o plano inclui lint, testes de unidade, testes de ponta a ponta (E2E) e verificação de tipos com as ferramentas existentes do projeto. Mantenha-o focado na implementação e nos testes da filtragem; você criará o PR depois de concluir o fluxo de qualidade.
4. Peça alterações no plano antes de aprová-lo e anote o endereço da issue e quaisquer esclarecimentos acordados para usar na validação posterior.

## Aprovar explicitamente o Autopilot

Quando estiver satisfeito com o plano, selecione **Approve and implement this plan** ou a opção equivalente na sua versão. Confirme o modo ativo em vez de presumir que a aprovação o alterou.

1. Selecione **Approve and implement this plan** ou a opção equivalente.
2. Confirme se o indicador de modo mostra **Autopilot**.
3. Observe o Copilot seguir o plano aprovado, gerar código e executar testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano primeiro. Se o Copilot informar que faltam dependências ou que há um conflito de portas, resolva o problema de configuração antes de considerar as verificações concluídas. Pare somente os servidores que você iniciou.

## Revisar e verificar a implementação

Assim como qualquer outro código, o código gerado precisa ser revisado antes do merge. Inspecione as alterações e execute o site para confirmar o resultado.

1. Abra **Changes** e inspecione a implementação da filtragem e os testes.
2. Compare o resultado com a issue e os esclarecimentos aprovados, incluindo as combinações de categorias e editoras. Confirme se as alterações seguem as instruções existentes do repositório.
3. Inspecione os resultados de lint, testes de unidade, testes E2E e verificação de tipos. Uma verificação ignorada não conta como aprovada.
4. Resolva as falhas e execute novamente as verificações afetadas antes de aceitar a implementação. A configuração E2E do Playwright cria e disponibiliza uma pré-visualização e pode reutilizar um servidor local. Confirme se o servidor testado pertence a esta worktree, não a uma lição anterior.

## Explorar a nova funcionalidade

Agora que o código foi revisado, inicie o aplicativo e abra o site em um canvas de navegador para confirmar se ele funciona.

1. Peça ao Copilot para iniciar o aplicativo e abrir a página no canvas de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Aguarde o início do aplicativo e a abertura de uma janela do navegador dentro do aplicativo Copilot.
3. Selecione uma categoria e confirme se os resultados correspondem a ela. Limpe o filtro, selecione uma editora e confirme os resultados.
4. Combine filtros de categoria e editora e compare os resultados com a issue e o plano aprovado. Se houver suporte a várias categorias, verifique o comportamento acordado.
5. Teste uma combinação sem jogos correspondentes e confira o estado vazio. Limpe ou redefina os filtros e confirme se a lista completa volta a aparecer.
6. Quando terminar, peça ao Copilot que pare o servidor de desenvolvimento iniciado para esta sessão:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Manter esta sessão para as próximas lições

Não crie nem integre o PR da filtragem ainda. Mantenha a sessão, a worktree e a branch para as Lições 4 a 8. Você aplicará instruções personalizadas e adicionará melhorias à skill e ao perfil de QA antes de criar e integrar o PR combinado na Lição 8.

1. Depois de parar o servidor de desenvolvimento, deixe a sessão aberta e sem atividade para retomá-la nas próximas lições.

## Resumo e próximos passos

Você usou diferentes modos de agente para desenvolver e revisar uma funcionalidade. Nesta lição, você:

- iniciou uma sessão de agente a partir de uma issue do GitHub.
- definiu os requisitos no modo **Plan**.
- implementou a funcionalidade no modo **Autopilot**.
- revisou o código.
- validou manualmente a funcionalidade em um canvas de navegador.

Em seguida, [use instruções personalizadas][next-lesson] para conhecer melhor como o código é gerado e garantir que siga as práticas documentadas.

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
