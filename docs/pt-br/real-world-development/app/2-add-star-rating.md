---
title: "Lição 2 - Adicionar avaliações por estrelas: uma melhoria rápida"
description: "Inicie sua primeira sessão de agente no aplicativo GitHub Copilot, faça uma pequena alteração nos cards dos jogos e integre-a como seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Agora que você conheceu o espaço de trabalho e experimentou um chat rápido, inicie uma **sessão de agente** para fazer sua primeira alteração no projeto. Ela será pequena: os dados dos jogos já incluem uma avaliação por estrelas, mas os cards da página inicial ainda não a exibem. Você pedirá ao agente que mostre a avaliação, revisará a alteração e a integrará como seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma sessão de agente e conhecer sua estrutura.
- pedir ao agente que faça uma alteração pequena e específica no projeto.
- revisar a alteração na visualização de diff do espaço de trabalho.
- executar o aplicativo localmente e confirmar a alteração no navegador.
- abrir e integrar seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes do jogo. Os cards da página inicial, porém, mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, você pedirá ao agente que exiba a avaliação existente em cada card — uma alteração pequena e independente, ideal para sua primeira sessão.

## Estrutura de uma sessão

Uma **sessão** é uma conversa com um agente executada em um espaço de trabalho isolado. Cada sessão tem uma **worktree do Git e uma branch próprias**, o que permite executar várias sessões ao mesmo tempo — uma para adicionar uma funcionalidade, outra para corrigir um bug — sem que as alterações entrem em conflito. As sessões aparecem na barra lateral, agrupadas por repositório. Selecione uma sessão para alternar para ela.

Em uma sessão, você verá três elementos: a **conversa** com o agente, a **atividade das ferramentas** enquanto ele explora e edita arquivos e a lista de **arquivos alterados** com os respectivos diffs.

## Iniciar uma sessão e solicitar a alteração

Inicie uma sessão para explorar o projeto e implementar a funcionalidade. Durante a [configuração do aplicativo][prior-lesson], você adicionou o projeto a partir do repositório no GitHub. Agora, crie uma sessão para esse repositório e solicite a alteração.

1. Volte ao aplicativo GitHub Copilot ou abra-o.
2. Na parte superior da barra lateral, selecione **+** para iniciar uma sessão.
3. Selecione `tailspin-toys` como repositório.
4. Abaixo da caixa de prompt, selecione uma **new working tree** e o modo **Interactive**. Use este prompt para solicitar a alteração:

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. Pressione <kbd>Enter</kbd> para enviar o prompt ao Copilot.

O aplicativo Copilot começa criando uma nova worktree, uma cópia isolada do projeto. Em seguida, explora o projeto, localiza os arquivos que precisam ser atualizados e cria o código necessário. Você acabou de adicionar uma funcionalidade com o aplicativo Copilot.

## Revisar o diff

Revise todas as alterações geradas por inteligência artificial antes de integrá-las, mesmo quando forem pequenas. Inspecione as alterações diretamente no aplicativo Copilot.

1. No canto superior direito do aplicativo, selecione **Toggle review panel**. No painel que abrir, selecione **Changes** para ver os arquivos alterados e seus diffs.

    ![Barra de ferramentas superior do aplicativo GitHub Copilot com uma seta apontando para o botão Toggle review panel à direita de Create PR](../../../_images/app-2-review-panel.png)

2. Procure o código adicionado a `GameCard.astro`, o arquivo principal usado para exibir os detalhes do jogo. Ele deve ser semelhante ao exemplo a seguir: um bloco que mostra a avaliação quando ela está disponível e usa `No rating yet` quando `starRating` é `null`:

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Como todas as ferramentas de inteligência artificial generativa, o Copilot é probabilístico, não determinístico. O código exato pode variar, mas deve ser relativamente semelhante ao exemplo.

## Verificar as alterações

Revise os resultados das verificações automatizadas do agente antes de abrir o navegador. Confirme se os testes cobrem uma avaliação numérica e a alternativa para `null`. Um pré-requisito ausente ou uma verificação ignorada não conta como aprovação; revise qualquer solicitação de instalação antes de aprová-la.

Não basta ler o código e presumir que funciona. Peça ao Copilot que abra o site em um canvas de navegador para examinar a interface atualizada.

> [!TIP]
> Um canvas é um widget interativo disponível no aplicativo Copilot. Mais adiante, você explorará canvases personalizados e criará um; por enquanto, use o canvas de navegador integrado.

1. Use este prompt para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Aguarde o início do aplicativo e a abertura de uma janela do navegador dentro do aplicativo Copilot.
3. Confirme se os cards dos jogos com avaliação exibem a nota de um total de cinco.
4. Quando terminar, peça ao Copilot que pare o servidor de desenvolvimento iniciado para esta sessão:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Abrir e integrar seu primeiro pull request

Você criou a funcionalidade. Agora, crie um pull request (PR) para integrar o novo código à base de código existente.

1. No canto superior direito, selecione **Create PR**.
2. Se solicitado, selecione **Sign in with your browser** e siga as instruções para autenticar-se.
3. Se o Copilot explicar que a política do repositório exige uma issue correspondente antes de criar o PR, confirme que ele deve seguir esse processo.
4. Aguarde o Copilot criar o PR.
5. Selecione o indicador **PR** logo acima do chat para abrir o pull request no painel de revisão. Revise-o conforme necessário.
6. Quando estiver tudo pronto, selecione **Ready to merge**.
7. Na nova caixa de diálogo, selecione **Merge pull request** para integrar o pull request.

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração usando o aplicativo GitHub Copilot. Você:

- iniciou uma sessão de agente e conheceu a estrutura das sessões.
- orientou o agente a fazer uma alteração pequena e específica nos cards dos jogos.
- revisou a alteração na visualização de diff do espaço de trabalho.
- executou o aplicativo localmente e confirmou a avaliação por estrelas no navegador.
- abriu o PR 1, revisou as verificações e fez o merge explicitamente.

Em seguida, [comece pela issue de filtragem e use os modos Plan e Autopilot][next-lesson] para desenvolver uma funcionalidade maior.

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Gerenciar issues e pull requests com o aplicativo GitHub Copilot][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#instalar-e-configurar-o-aplicativo-github-copilot
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
