---
title: "Lição 10 - Revisão e próximos passos"
description: "Recapitule o fluxo do Copilot CLI, os dois pull requests, as personalizações reutilizáveis e outros recursos."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Você usou o GitHub Copilot CLI em um fluxo contínuo de trabalho na Tailspin Toys. Você:

- preparou um Codespace, instalou o Copilot CLI, explorou o projeto e encontrou a issue de filtragem criada pelo modelo.
- adicionou avaliações por estrelas, revisou o resultado em um navegador com porta encaminhada e integrou manualmente seu primeiro pull request (PR).
- começou pela issue de filtragem, definiu a abordagem no modo Plan, desenvolveu no modo Autopilot e revisou no modo Interactive.
- orientou o agente com instruções personalizadas e personalizou a skill `quality-checks` existente para executar testes de unidade, lint e verificações de tipos.
- adicionou o servidor Model Context Protocol (MCP) do Playwright e o usou para explorar a filtragem em um navegador real.
- criou e selecionou um agente personalizado de garantia de qualidade (QA) para avaliar requisitos, cobertura, resultados da skill e evidências do navegador.
- revisou toda a alteração de filtragem e autorizou o Agent Merge para o PR da funcionalidade.
- explorou comandos de barra para gerenciar o contexto, os modelos e o compartilhamento e delegar tarefas opcionalmente à nuvem.

## O que você entregou

O workshop tem dois marcos de PR, cada um em sua própria branch baseada na `main` atualizada:

1. **Avaliações por estrelas:** exiba o `starRating` existente e um estado explícito para jogos sem avaliação nos cards.
2. **Filtragem e fluxo de qualidade:** implemente a filtragem, atualize as instruções e aplique-as à funcionalidade, personalize o relatório de `quality-checks`, crie um perfil de QA e inclua os testes relacionados.

Do planejamento da filtragem até a abertura do PR, você usou a mesma conversa e branch. Esse trabalho foi reunido em um único PR para simplificar o workshop.

## Diferentes tipos de verificação

Você verificou o código de várias formas: com testes automatizados, sua própria inspeção no navegador e a exploração do navegador pelo Copilot via MCP. A skill `quality-checks` executou testes de unidade, lint e verificações de tipos e apresentou os resultados no novo formato. Antes do PR, o agente de QA reuniu esses resultados com a revisão dos requisitos e da cobertura de testes.

Os testes adicionados devem cobrir lacunas reais; uma execução de QA que não precisa de novos testes pode estar correta. Revise o código e as evidências antes de autorizar o merge e atualize as evidências afetadas depois de qualquer alteração.

## Boas práticas

O contexto e as ferramentas que você fornece orientam o trabalho do Copilot. Neste workshop, você atualizou instruções, personalizou uma skill, criou um perfil de QA e configurou um servidor MCP. Reutilize essas personalizações entre conversas e ajuste-as conforme as necessidades da equipe mudarem. As instruções definem padrões, as skills descrevem tarefas repetíveis, os agentes personalizados definem funções especializadas e os servidores MCP conectam ferramentas externas. Revise as alterações reais e os resultados das ferramentas, não apenas o resumo do agente.

Associe o **modo e o modelo** à tarefa. Use **Plan** para avaliar uma abordagem antes de desenvolver, **Interactive** para acompanhar alterações específicas e **Autopilot** em tarefas bem delimitadas. Escolha um modelo mais rápido para edições rotineiras e um modelo mais avançado para trabalhos complexos.

O contexto continua tão importante quanto a infraestrutura. Descrever com clareza *o que* você quer desenvolver, *por que* e *como* muda significativamente o resultado.

## Mais recursos para explorar

Você concluiu o fluxo de trabalho principal. Veja outros recursos da CLI:

- `/review` para pedir ao agente de revisão de código que analise as alterações.
- `/rubber-duck` para conversar sobre um problema e obter outra perspectiva.
- `/fleet` para coordenar tarefas independentes em paralelo.
- `/worktree` para isolar outra tarefa.
- `/delegate` para enviar uma tarefa ao agente de nuvem do Copilot.

## Próximos passos

A melhor maneira de melhorar com qualquer ferramenta é continuar usando-a. Use-a em código de produção, em projetos pessoais ou naquele pequeno aplicativo que você planeja criar há anos. Compartilhe o que aprendeu com a equipe e aprenda com as experiências dela. E, como sempre, explore a documentação.

Se quiser ampliar a Tailspin Toys pelo terminal, continue com a série opcional [Foundry Backer Concierge][foundry]. Para comparar outros ambientes, explore o [workshop do VS Code][vscode], o [workshop do aplicativo GitHub Copilot][app] ou o [workshop do agente de nuvem do Copilot][cloud].

## Recursos

- [Sobre o GitHub Copilot CLI][about-cli]
- [Referência de comandos do Copilot CLI][cli-reference]
- [Personalizar o Copilot CLI][customize-cli]
- [Gerenciar pull requests com o Copilot CLI][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
