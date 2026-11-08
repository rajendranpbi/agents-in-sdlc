---
title: "Exercício 1 - Instruções personalizadas (agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com inteligência artificial generativa. Se uma tarefa precisa ser realizada de uma forma específica — ou há informações de contexto que o Copilot deve conhecer — garanta que essas orientações estejam disponíveis. Os [arquivos de instruções][instruction-files] fornecem essas orientações para que o Copilot entenda não só *o que* você quer, mas também *como* realizar o trabalho.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio de instruções personalizadas do repositório e de arquivos de instruções com escopo de caminho.
- adicionar um novo padrão para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos do VS Code e da CLI, aqui você não fará uma comparação de prompts *antes e depois*. O agente de nuvem do Copilot trabalha de forma assíncrona em issues do GitHub, então é mais difícil demonstrar a influência das instruções em tempo real. Você verá esse efeito mais adiante, ao revisar os pull requests produzidos pelo agente.

## Arquivos de instruções

### Cenário

Como toda boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Eles incluem:

- A camada de dados sempre precisa de testes de unidade.
- A interface do usuário (UI) deve usar o modo escuro e ter uma aparência moderna.
- O código deve incluir comentários de documentação TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever sua finalidade.

Com os arquivos de instruções, você pode fornecer ao Copilot as orientações necessárias para realizar o trabalho de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele possa entender melhor seu estilo de programação e seus requisitos. Elas ajudam a orientar sugestões e trechos de código. Você pode especificar convenções de codificação, bibliotecas e até o tipo de comentário que prefere. Crie instruções para todo o repositório ou para tipos específicos de arquivo e tarefas.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações feitas no repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações feitas ao Copilot pelo chat ou pela linha de comando. Por exemplo, a pilha tecnológica, uma visão geral do que está sendo desenvolvido, práticas recomendadas e outras orientações globais.
- `.github/instructions/*.instructions.md`: arquivos que podem ser criados para tarefas ou tipos de arquivo específicos. Use-os para fornecer diretrizes para linguagens específicas, como TypeScript ou Astro, ou para tarefas, como criar um componente de interface ou um novo conjunto de testes de unidade.

> [!NOTE]
> No ambiente de desenvolvimento integrado (IDE), os arquivos de instruções são usados apenas para gerar código no Copilot Chat — não para conclusões de código nem sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam arquivos de instruções do repositório e arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também [oferece suporte a arquivos de instruções em outros padrões][custom-instructions-support], incluindo arquivos AGENTS.md e CLAUDE.md.

### Práticas recomendadas para gerenciar arquivos de instruções

Criar arquivos de instruções está além do escopo deste workshop, mas os exemplos do projeto mostram uma abordagem representativa. Em linhas gerais:

- Mantenha `copilot-instructions.md` focado nas orientações do projeto, como uma descrição do que está sendo desenvolvido, a estrutura do projeto e os padrões globais de codificação.
- Use arquivos `*.instructions.md` para instruções específicas de tipos de arquivo, como testes de unidade, componentes Astro e a camada de dados, ou para tarefas específicas.
- Use linguagem natural e mantenha as orientações claras. Inclua exemplos de como o código deve e não deve ser escrito.

Não há uma única maneira correta de criar arquivos de instruções nem de usar inteligência artificial. Faça experiências para descobrir o que funciona melhor no seu projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter um conjunto sólido de arquivos de instruções. Ao explorar os arquivos deste projeto, você verá instruções para vários tipos de tarefa, incluindo [atualizações de interface][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface oferece essa opção de uma forma diferente — por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. O exercício referente à interface em uso explicará quando aplicável.
>
> Para encontrar modelos ou um ponto de partida, explore o [Awesome Copilot][awesome-copilot], um repositório com arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas deste projeto

Leia os arquivos de instruções incluídos no repositório: há um `copilot-instructions.md` principal e vários arquivos `*.instructions.md` para tarefas diferentes. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe as orientações aninhadas de **GitHub Actions Workflows**. Essas instruções se aplicam a todas as interações com o Copilot neste repositório.
3. Abra a pasta `.github/instructions` e explore os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e outros tópicos.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo` no início do arquivo. Ele define um padrão glob relativo à raiz do repositório que determina a quais arquivos as instruções se aplicam. Neste projeto, ele corresponde a arquivos de teste TypeScript, como os que seguem o padrão `**/*.test.ts`.
5. Observe as instruções específicas para criar testes de unidade neste projeto.
6. Abra `.github/instructions/drizzle.instructions.md` e role até o final. Observe os links para outros arquivos de instruções, como `unit-tests.instructions.md`, e para arquivos existentes do projeto. Assim, você pode dividir conjuntos maiores de instruções em arquivos menores e reutilizáveis e indicar ao Copilot exemplos a seguir. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` documenta os padrões de codificação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de comentários nos arquivos.

## Adicionar um novo padrão para o repositório

A próxima etapa é a única edição deste exercício: adicionar uma regra global para que a documentação seja incluída no código por meio de comentários TSDoc e de um cabeçalho de comentários no arquivo. O agente de nuvem usará essa regra quando trabalhar nas issues que você atribuir mais adiante.

Antes de editar o arquivo, crie uma branch. O agente de nuvem lerá as instruções da branch de destino da issue, mas você fará commit das alterações de forma organizada.

1. No terminal do codespace, crie uma branch e mude para ela:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

Agora, atualize `.github/copilot-instructions.md` para documentar os padrões de codificação do repositório e melhorar as sugestões de código.

2. Abra novamente `.github/copilot-instructions.md`.
3. Localize a seção **Code formatting requirements**, que deve estar perto da linha 27. Ela documenta os padrões de codificação, mas ainda não exige documentação no código — por isso, o auxiliar gerado não tinha comentários.
4. Logo abaixo dos padrões existentes, adicione estas linhas de Markdown para orientar o Copilot a incluir cabeçalhos de comentários nos arquivos e comentários TSDoc:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu no exercício anterior, os arquivos de instruções podem fornecer orientações globais em `.github/copilot-instructions.md` ou instruções específicas para linguagens, tipos de arquivo ou tarefas em arquivos `*.instructions.md`. O arquivo do repositório é adequado para padrões globais, como a regra de comentários de documentação que você acabou de adicionar.

## Fazer commit, enviar e integrar a atualização das instruções

O agente de nuvem lê os arquivos de instruções da branch de destino da issue. No próximo exercício, o Copilot criará uma branch a partir de `main`; portanto, as alterações nas instruções precisam ser integradas a `main` para que o agente possa usá-las.

1. Prepare as alterações e faça commit:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e faça o merge. Assim, o agente de nuvem poderá usar as instruções ao trabalhar nas issues atribuídas no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente na `main` neste workshop, ignore a etapa da branch e faça commit diretamente nela. A etapa da branch demonstra um processo mais próximo do que você usaria em um projeto real.

## Resumo e próximos passos

Você:

- explorou como o Copilot obtém contexto dos arquivos de instruções do projeto.
- adicionou um novo padrão para todo o repositório em `.github/copilot-instructions.md`.

Você verá esse padrão nos pull requests que o agente de nuvem gerar no restante deste percurso.

Em seguida, [atribua sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Cinco dicas para escrever instruções melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
