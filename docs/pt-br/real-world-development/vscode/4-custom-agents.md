---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de inteligência artificial (IA) especializados em tarefas ou domínios específicos do fluxo de desenvolvimento. Defina agentes em arquivos Markdown na pasta `.github/agents` do repositório e forneça instruções, práticas recomendadas, padrões de código e conhecimento específico do domínio. Assim, você orienta o Copilot a realizar certos tipos de trabalho com mais eficiência. As equipes podem registrar seu conhecimento em agentes reutilizáveis — por exemplo, um agente de acessibilidade que aplica as [Diretrizes de Acessibilidade para Conteúdo Web (WCAG)][wcag], um agente de segurança que segue práticas de codificação segura ou um agente de testes que mantém padrões consistentes.

Defina agentes personalizados em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML (sigla em inglês para *YAML Ain't Markup Language*) com pelo menos os campos `name` e `description`, seguidos de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Comparar agentes personalizados e skills de agente

Há alguma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam uma inteligência artificial sobre como executar tarefas. A distinção mais simples é: o **agente personalizado** executa o trabalho, e as **skills** são ferramentas.

Os agentes personalizados têm uma janela de contexto própria e podem coordenar skills — e até outros agentes — durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade. Durante esse trabalho, ele poderá usar skills, como uma skill para o fluxo de pull requests ou para executar e gerenciar testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Como em qualquer trabalho com inteligência artificial, teste e itere para encontrar o que funciona nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer garantir que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades visuais ou preferências. Comentários recentes indicam que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver esse problema de acessibilidade, a equipe de design solicitou um modo de alto contraste que as pessoas possam ativar e desativar.

Como a acessibilidade é essencial, implemente a funcionalidade rapidamente com a ajuda de um agente personalizado.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar um modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Já existe um agente personalizado de acessibilidade. Revise o conteúdo para entender como ele orientará o Copilot.

Volte ao codespace, abra um terminal e mude para uma branch nova baseada na `main` para trabalhar na acessibilidade. Assim, o trabalho ficará separado do PR de filtragem do Exercício 3.

1. No terminal, execute os comandos a seguir para mudar para `main` e criar a branch:

   ```bash
   git checkout main
   git pull
   git checkout -b accessibility-vscode
   ```

2. Abra `.github/agents/accessibility.md`.
3. Observe o frontmatter YAML e os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

4. Leia as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código em HTML, CSS (folhas de estilo em cascata) e JavaScript.
   - armadilhas e erros comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code exibe todos os agentes personalizados definidos em `.github/agents` no seletor de agentes, na parte inferior da visualização Copilot Chat. Selecione um agente para limitar a conversa às instruções e ferramentas dele.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar as etapas abaixo, volte ao codespace, abra o painel do Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa especificará as opções relevantes.

1. Na visualização Chat, selecione **Agent** no menu suspenso de agentes, caso ainda não esteja selecionado.

   ![Seletor de agentes na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Na parte inferior da visualização Chat, selecione o menu suspenso de agentes. Ele mostra o agente ativo, que por padrão é **default**.
3. Na lista de agentes disponíveis, selecione **Accessibility agent**.
4. Envie este prompt ao agente de acessibilidade:

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. O Copilot Chat explorará o código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá na conversa com o caminho do arquivo e um diff para revisão.
6. À medida que as alterações forem propostas, o indicador **Files changed** será atualizado com os arquivos modificados pelo agente.

> [!NOTE]
> Esse processo pode levar alguns minutos. O Copilot fará alterações reais no repositório — editará componentes Astro, arquivos CSS e testes relacionados, conforme necessário.

Você revisará e orientará o trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents], assistentes especializados do GitHub Copilot para tarefas e domínios específicos. Eles permitem registrar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o Copilot.

Você explorou estes conceitos:

- como os agentes personalizados são definidos.
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, [monitore e oriente o trabalho do agente][next-lesson], revise as alterações enquanto são feitas e adicione um alternador para o tema claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no seu ambiente de desenvolvimento integrado (IDE)][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no Awesome Copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar a funcionalidade com o servidor MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
