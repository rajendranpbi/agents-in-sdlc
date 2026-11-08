---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Durante este laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse uma funcionalidade de jogos relacionados para a equipe de design explorar e implementasse recursos de acessibilidade, incluindo alternadores para os modos de alto contraste e claro. Revise as alterações e, se necessário, dê orientações ao Copilot para aprimorar o trabalho.

### Cenário

Os fundamentos do design de software e do desenvolvimento e operações (DevOps) não mudam com a inclusão de inteligência artificial generativa. Sempre revise o código gerado e siga o processo normal de DevOps. Revise as sugestões do GitHub Copilot para a documentação, os jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão, há restrições de segurança para proteger o ambiente:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch que usará para o código.
- O agente de nuvem é executado no GitHub Actions, em um ambiente separado e efêmero.
- Um ser humano precisa aprovar os fluxos de trabalho do GitHub Actions antes da execução.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo servidores do protocolo Model Context Protocol (MCP).

## Revisar a documentação gerada

Comece pelo primeiro pull request (PR) criado pelo agente de nuvem do GitHub Copilot, que adiciona documentação ao código. Use a interface padrão de PRs do GitHub.com.

> [!NOTE]
> Ao abrir o PR, você poderá ver um aviso de que o Copilot está bloqueado por um firewall. Isso é **esperado**: por padrão, o acesso a recursos externos, inclusive servidores MCP externos, é limitado. Se quiser, [personalize ou desabilite o firewall do agente de nuvem do Copilot][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de PRs.
3. Abra o pull request com um título semelhante a **Add missing documentation** ou outro título mais específico.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde a conclusão do trabalho. Isso pode levar alguns minutos; você pode fazer uma pausa ou refletir sobre o que aprendeu até agora.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Explore o código atualizado, que inclui comentários de documentação TSDoc e outros elementos de documentação. As alterações exatas variam.

   Ao revisar, procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentários. Eles foram definidos nos arquivos de instruções personalizadas examinados no Exercício 2.

6. Depois de revisar as alterações, volte à guia **Conversation** e role a tela para baixo.
7. Procure um indicador de que há fluxos de trabalho aguardando aprovação.
8. Se houver fluxos aguardando aprovação, selecione **Approve and run workflows**.

   ![Botão Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão entrar na fila da seção de verificações do PR. Se tudo correr bem, as verificações do aplicativo Astro deverão ser aprovadas. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

O trabalho com o Copilot em um pull request não é uma via de mão única. Você pode mencioná-lo em comentários gerais ou em comentários em linha no código. O Copilot verá esses comentários e iniciará outra sessão para resolvê-los. Como os resultados não são determinísticos, não há um texto único a usar. Por exemplo, você pode pedir ao Copilot que:

- adicione um cabeçalho com uma breve descrição no início de cada arquivo de código.
- adicione comentários TSDoc a arquivos TypeScript e Astro.
- crie um `README` que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e mencione **@copilot**, como faria com outra pessoa. Use uma das sugestões acima ou peça outra melhoria de documentação para o código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para aplicar as alterações.
3. Selecione **Back to pull request** para voltar ao PR.

    ![Botão Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, procure um novo commit no PR.
5. Selecione a guia **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, você pode converter o PR de rascunho em pronto para revisão e integrá-lo à branch principal.

![Opção para converter o PR em pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar a funcionalidade de jogos relacionados

Volte ao PR criado pelo Copilot para resolver a issue de exibir jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Show related games on the game details page** ou outro título mais específico.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois de revisar as alterações, volte à guia **Conversation** e role a tela para baixo.
6. Procure um indicador de que há fluxos de trabalho aguardando aprovação.
7. Se houver fluxos aguardando aprovação, selecione **Approve and run workflows**.

   ![Botão Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. Os fluxos de trabalho deverão entrar na fila da seção de verificações do PR. Se tudo correr bem, as verificações do aplicativo Astro deverão ser aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** faça um teste manual da funcionalidade de jogos relacionados no codespace. Abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, como **copilot/fix-8**:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou a funcionalidade de jogos relacionados. Assim como antes, você pode pedir alterações ao agente de nuvem de forma iterativa. Por exemplo, peça que ajuste a quantidade de jogos relacionados ou que adicione cabeçalhos de comentários e comentários TSDoc — lembre-se de que a tarefa foi atribuída **antes** da atualização das instruções personalizadas. Para solicitar alterações, adicione um comentário à guia **Conversation**. O Copilot o verá e iniciará outra sessão.

## Revisar os recursos de acessibilidade

Por fim, revise os recursos de acessibilidade criados com o agente personalizado de acessibilidade. Este PR deve incluir o modo de alto contraste atribuído no Exercício 3 e o tema claro solicitado na central de controle no Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Selecione o PR com um título semelhante a **Add high contrast mode to website** ou outro título mais específico.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde a conclusão do trabalho, que pode levar alguns minutos.

4. Selecione a guia **Files changed** para revisar o código gerado.
5. Revise a implementação, com atenção especial a:
   - componentes da interface do usuário (UI) para alternar entre os modos.
   - uso do armazenamento local para persistir as preferências.
   - alterações de folhas de estilo em cascata (CSS) para os modos de alto contraste e claro.
   - atributos de Accessible Rich Internet Applications (ARIA), navegação por teclado e outros recursos de acessibilidade.
   - código JavaScript ou TypeScript que controla a alternância entre os modos.
6. Depois de revisar as alterações, volte à guia **Conversation** e role a tela para baixo.
7. Procure um indicador de que há fluxos de trabalho aguardando aprovação.
8. Se houver fluxos aguardando aprovação, selecione **Approve and run workflows**.

   ![Botão Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. Os fluxos de trabalho deverão entrar na fila da seção de verificações do PR. Se tudo correr bem, as verificações do aplicativo Astro deverão ser aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** teste manualmente os recursos de acessibilidade no codespace. Abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os alternadores de alto contraste e tema claro no navegador. Confirme se funcionam e mantêm as preferências após recarregar a página.

Observe como o agente personalizado de acessibilidade orientou o Copilot a implementar os recursos seguindo práticas recomendadas de acessibilidade. Se encontrar problemas ou oportunidades de melhoria, mencione **@copilot** em um comentário para solicitar alterações, como nos PRs anteriores.

## Exercício opcional: continuar delegando

O agente de nuvem é mais útil quando recebe itens reais do backlog e você pode se dedicar a outras tarefas. Para desenvolver esse hábito, crie mais algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na lista de jogos.
- adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Ao longo dos exercícios, você:

- **inspecionou os arquivos de instruções personalizadas do repositório** para observar mais tarde o efeito deles no trabalho do agente de nuvem.
- **atribuiu issues ao agente de nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **criou e usou um agente personalizado** para acessibilidade, adicionando alternadores para os modos de alto contraste e claro.
- **usou a página de agentes do Copilot como central de controle** para monitorar e orientar a sessão de acessibilidade em andamento.
- **revisou e iterou sobre os pull requests do agente de nuvem**, mencionando `@copilot` para solicitar alterações e aprovando fluxos de trabalho.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Se quiser continuar explorando, os outros percursos complementam o que você praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com o MCP diretamente no ambiente de desenvolvimento integrado (IDE).
- 💻 **[Percurso da interface de linha de comando (CLI)](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente personalizado de QA, MCP do Playwright, Agent Merge e uma opção `/delegate` para voltar ao agente de nuvem.

No seu repositório, experimente estas ideias:

- atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra pilha tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o fluxo de configuração do agente de nuvem do Copilot][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem do Copilot][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
