---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse ponto central, você pode ver as tarefas de agentes que têm pull requests (PRs) abertos, explorar as operações realizadas e orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para monitorar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar outra funcionalidade.

## Cenário

Depois que o agente recebeu a tarefa de criar um modo de alto contraste, a equipe percebeu que também seria um bom momento para adicionar um tema claro. Como o trabalho já atualiza o estilo do site e adiciona alternadores, faz sentido incluir os dois temas. Oriente o agente para que ele implemente também o tema claro.

## Revisar as tarefas do agente de nuvem do Copilot

Consulte o status das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe as tarefas listadas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação do código.
   - gerar interfaces de programação de aplicações (APIs) para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações executadas pelo Copilot. Elas podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores do protocolo Model Context Protocol (MCP).
   - executar várias etapas para concluir a tarefa atribuída.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel de pull requests à direita. Ele permite acompanhar o PR e os arquivos alterados.

## Orientar o agente de nuvem

Agora que você conhece as tarefas ativas, peça ao Copilot que inclua o alternador de tema claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão referente ao modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado atual do trabalho.

    ![Sessão de acessibilidade no ponto central de controle](../../../_images/ex6-accessibility-session.png)

2. Aguarde alguns minutos até que a sessão indique que a configuração terminou e o trabalho começou. Você verá mensagens semelhantes às mostradas no exemplo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, digite o prompt:

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Tarefa do agente de nuvem na página de agentes com a caixa de diálogo Steer active session while Copilot is working destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe o Copilot confirmar o recebimento do prompt e incluí-lo no fluxo de trabalho.

## Deixar o Copilot trabalhar

Assim como antes, o Copilot trabalhará na tarefa atualizada. Ele incorporará a nova solicitação depois de concluir a etapa em andamento quando você enviou a mensagem.

O processo pode levar vários minutos. Aproveite esse tempo para refletir sobre o que você aprendeu até agora.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, o ponto central para monitorar e orientar tarefas do agente de nuvem do GitHub Copilot. Você pode acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar tarefas em andamento ou fornecer orientações adicionais.

Você explorou estes conceitos:

- usar a página de agentes como ponto central para monitorar tarefas do agente de nuvem.
- redirecionar uma sessão em andamento para solicitar outra funcionalidade.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, [itere sobre os pull requests criados por ele][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
