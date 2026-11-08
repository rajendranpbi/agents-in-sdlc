---
title: "Lição 8 - Criar e integrar o pull request da funcionalidade"
description: "Revise em conjunto a filtragem, as instruções, a atualização da skill, o perfil de garantia de qualidade (QA) e os testes; depois, crie um pull request e use o Agent Merge."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

A implementação da filtragem, as atualizações de instruções e da skill, o perfil de garantia de qualidade (QA) e os testes estão salvos em uma única branch. É hora de revisar tudo em conjunto e abrir um pull request (PR). Você integrou manualmente o pull request de avaliações por estrelas; desta vez, permitirá que o **Agent Merge** gerencie o processo.

> [!NOTE]
> Normalmente, dividiríamos a funcionalidade, as atualizações de instruções e da skill e o agente de QA em alguns PRs separados. Para simplificar o workshop, você manteve todo o fluxo de filtragem e qualidade em uma única conversa e branch, e todo esse trabalho será incluído neste PR.

Nesta lição, você vai:

- aprender o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionar toda a alteração da funcionalidade e as evidências de verificação.
- criar o PR de filtragem.
- habilitar o Agent Merge somente após a revisão e confirmar que o PR foi integrado.

## Cenário

Durante o fluxo de filtragem, você usou o Copilot para planejar, implementar e verificar uma funcionalidade. Agora, a Tailspin Toys quer automatizar o trabalho restante do PR, mantendo a autorização do merge sob o controle do desenvolvedor.

## Apresentação do Agent Merge

O **Agent Merge** automatiza o trabalho restante para integrar um pull request. Quando você o habilita, o Copilot resolve o que estiver bloqueando o PR — corrige verificações de integração contínua (CI) com falha, responde aos comentários de revisão e faz rebase quando necessário — e, em seguida, habilita o merge automático do GitHub se o repositório permitir.

Até aqui, você selecionou **Merge pull request** por conta própria. O Agent Merge pode assumir essa responsabilidade. Revise o trabalho e confirme se está pronto antes de habilitar o Agent Merge.

## Usar o Agent Merge para gerenciar o PR

Com todo o código criado, revise-o, crie o PR e deixe o Agent Merge cuidar das etapas restantes.

1. Volte ao Codespace.
2. Digite `/agent` para abrir a caixa de diálogo de agentes.
3. Selecione **Default** na lista de opções e pressione <kbd>Enter</kbd>.
4. Crie um PR com o comando `/pr create`.
5. Ative o Agent Merge com `/pr agentmerge`.
6. O Copilot acompanhará o processo de integração contínua no PR. Quando tudo for aprovado, ele fará o merge.
7. Confirme se uma mensagem do Copilot informa algo semelhante a `PR #14 was squash-merged successfully.`

> [!IMPORTANT]
> O Agent Merge não ignora aprovações obrigatórias, proteções de branch, filas de merge, configurações do repositório nem permissões ausentes. Se houver um bloqueio, leia o motivo informado e conclua manualmente o merge revisado quando o repositório permitir.

## Resumo e próximos passos

Você automatizou várias etapas do desenvolvimento, incluindo a geração, o teste e a validação de código e, agora, o processo do pull request. Você:

- aprendeu o que é o Agent Merge e como ele automatiza o ciclo de vida do merge.
- inspecionou toda a alteração da funcionalidade e as evidências de verificação.
- criou o PR de filtragem.
- habilitou o Agent Merge somente após a revisão e confirmou que o PR foi integrado.

Em seguida, [explore outros comandos de barra úteis do Copilot CLI][next-lesson] para gerenciar o contexto, os modelos e o compartilhamento e conhecer a delegação opcional para a nuvem.

## Recursos

- [Gerenciar pull requests com o Copilot CLI][manage-prs]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
