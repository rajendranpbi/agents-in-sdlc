---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de inteligência artificial (IA) especializados em tarefas ou domínios do fluxo de desenvolvimento. Defina agentes em arquivos Markdown na pasta `.github/agents` do repositório e forneça instruções, práticas recomendadas, padrões de código e conhecimento específico do domínio. Assim, você orienta o Copilot a realizar certos tipos de trabalho com mais eficiência. As equipes podem registrar seu conhecimento em agentes reutilizáveis — por exemplo, um agente de acessibilidade que aplica as [Diretrizes de Acessibilidade para Conteúdo Web (WCAG)][wcag], um agente de segurança que segue práticas de codificação segura ou um agente de testes que mantém padrões consistentes.

Defina agentes personalizados em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML (sigla em inglês para *YAML Ain't Markup Language*) com pelo menos os campos `name` e `description`, seguidos de um prompt Markdown que define o comportamento, a especialidade e as instruções do agente.

### Comparar agentes personalizados e skills de agente

Há alguma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam uma inteligência artificial sobre como executar tarefas. A distinção mais simples é: o **agente personalizado** executa o trabalho, e as **skills** são ferramentas.

Os agentes personalizados têm uma janela de contexto própria e podem coordenar skills — e até outros agentes — durante o trabalho. Neste laboratório, o agente personalizado de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade. Durante esse trabalho, ele poderá usar skills, como uma skill para o fluxo de pull requests ou para executar e gerenciar testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Como em qualquer trabalho com inteligência artificial, teste e itere para encontrar o que funciona nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Você explorará estes tópicos sobre agentes personalizados:

- como os agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys quer garantir que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente de suas capacidades visuais ou preferências. Comentários recentes indicam que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre as cores do texto e do plano de fundo. Para resolver esse problema de acessibilidade, a equipe de design solicitou um modo de alto contraste que as pessoas possam ativar e desativar.

Como a acessibilidade é essencial, implemente a funcionalidade rapidamente com a ajuda de um agente personalizado.

## Revisar o agente personalizado de acessibilidade

Já existe um agente personalizado de acessibilidade. Revise o conteúdo para entender como ele orientará o Copilot.

1. Volte ao codespace.
2. Abra `.github/agents/accessibility.md`.
3. Observe os campos `name` e `description` no frontmatter YAML.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

4. Leia as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código em HTML (linguagem de marcação de hipertexto), CSS (folhas de estilo em cascata) e JavaScript.
   - armadilhas e erros comuns.

## Criar e atribuir uma issue

A página de agentes é o ponto central para trabalhar com agentes no ambiente. Nela, você pode atribuir tarefas ao agente de nuvem do Copilot, monitorar o trabalho e redirecionar uma sessão ou fornecer orientações. Crie uma issue para o modo de alto contraste e atribua-a ao agente personalizado.

1. Acesse seu repositório.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de diálogo de nova issue.
4. Selecione **Blank issue** para criar uma issue em branco.
5. No campo **Title**, digite `Add high contrast mode to website`.
6. No campo **Description**, digite:

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de diálogo de atribuição.
9. Na lista de agentes personalizados, selecione **Accessibility agent**.

    ![Atribuição do agente de nuvem com o agente personalizado de acessibilidade selecionado](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. Aguarde enquanto o Copilot começa a trabalhar em segundo plano.

## Resumo e próximos passos

Neste exercício, você explorou os [agentes personalizados][custom-agents], assistentes especializados do GitHub Copilot para tarefas e domínios específicos. Eles permitem registrar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o Copilot.

Você explorou estes conceitos:

- como os agentes personalizados são definidos.
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, [monitore e oriente a sessão do agente][next-lesson] na página de agentes.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar-se para usar agentes personalizados na organização][org-custom-agents]
- [Preparar-se para usar agentes personalizados na empresa][enterprise-custom-agents]

---

| [← Lição anterior: Agente de nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
