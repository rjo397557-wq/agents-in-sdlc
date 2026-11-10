---
title: "Exercício 1 - Instruções personalizadas (Agente de nuvem)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Pré-requisitos][previous-lesson] · [Próxima lição: Agente de nuvem do Copilot →][next-lesson]

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisa ser feita de uma maneira específica — ou há informações de contexto que o Copilot deve conhecer — você precisa garantir que elas estejam disponíveis. Os [arquivos de instruções][instruction-files] fornecem orientações para que o Copilot entenda não apenas *o que* você quer, mas também *como* o trabalho deve ser feito.

Neste exercício, você vai:

- explorar como o contexto do projeto, as diretrizes de codificação e os padrões de documentação chegam ao Copilot por meio das instruções do repositório e de arquivos com escopo de caminho.
- adicionar um novo padrão válido para todo o repositório em `.github/copilot-instructions.md`.

> [!NOTE]
> Diferentemente dos percursos de VS Code e CLI, você não verá aqui uma comparação de prompt *antes e depois*. O agente de nuvem trabalha de forma assíncrona em issues do GitHub, então é mais difícil demonstrar o impacto lado a lado. Você verá a influência do arquivo de instruções mais adiante, ao revisar os pull requests criados pelo agente.

## Arquivos de instruções

### Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para suas práticas:

- A camada de dados sempre precisa de testes unitários.
- A interface deve usar modo escuro e ter uma aparência moderna.
- A documentação deve ser adicionada ao código como comentários TSDoc.
- Um bloco de comentários no início de cada arquivo deve descrever o que ele faz.

Com os arquivos de instruções, você dará ao Copilot as informações necessárias para executar as tarefas de acordo com essas práticas.

### Instruções personalizadas

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de codificação e seus requisitos. Você pode especificar convenções de codificação, bibliotecas e os tipos de comentários que deseja incluir no código. As instruções podem valer para todo o repositório ou para tipos específicos de arquivos.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações sobre o projeto relevantes para a maioria das solicitações feitas pelo chat ou pela CLI, como a pilha de tecnologia, uma visão geral do que está sendo criado e orientações globais.
- `.github/instructions/*.instructions.md`, arquivos que fornecem orientações para linguagens, tipos de arquivo ou tarefas específicos.

> [!NOTE]
> No IDE, os arquivos de instruções são usados para gerar código no Copilot Chat, não para autocompletar nem para sugestões de próxima edição.
>
> O Copilot Chat, o Copilot CLI e o agente de nuvem do Copilot usam as instruções do repositório e os arquivos `*.instructions.md` com frontmatter `applyTo` ao gerar código.
>
> O Copilot também oferece suporte a outros padrões, como `AGENTS.md` e `CLAUDE.md`; consulte a [referência de suporte a instruções personalizadas][custom-instructions-support].

## Boas práticas para gerenciar arquivos de instruções

Uma discussão completa sobre a criação de arquivos de instruções está além do escopo deste workshop. Os exemplos do projeto mostram uma abordagem representativa:

- Mantenha `copilot-instructions.md` focado nas orientações do projeto, como descrição, estrutura e padrões globais de codificação.
- Use `*.instructions.md` para tipos de arquivo (testes unitários, componentes Astro, camada de dados) ou tarefas específicos.
- Use linguagem natural e orientações claras. Inclua exemplos do que fazer e do que evitar.

Não há uma única forma de criar arquivos de instruções nem de usar IA. Experimente para descobrir o que funciona melhor para o projeto.

> [!TIP]
> Todo projeto que usa o GitHub Copilot deve ter uma coleção robusta de arquivos de instruções. Neste projeto, há orientações para vários tipos de tarefas, incluindo [atualizações de UI][ui-instructions] e [Astro][astro-instructions].
>
> O Copilot também pode ajudar a gerar arquivos de instruções. Cada interface apresenta esse recurso de forma diferente, por exemplo, **Configure Chat → Generate Agent Instructions** no VS Code ou `/init` no Copilot CLI. A lição do ambiente atual explicará o recurso quando for relevante.
>
> Para encontrar modelos ou um ponto de partida, explore o [awesome-copilot][awesome-copilot], que reúne arquivos de instruções, agentes personalizados e outros recursos.

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## Explorar os arquivos de instruções personalizadas do projeto

Leia os arquivos de instruções incluídos no repositório — um `copilot-instructions.md` principal e vários arquivos `*.instructions.md` para tarefas específicas. Abra-os no editor ou na interface Web do GitHub.

1. Abra `.github/copilot-instructions.md`.
2. Explore o arquivo e observe a breve descrição do projeto e seções como **Agent notes**, **Code standards**, **Scripts** e **Repository Structure**. Em **Code standards**, observe a orientação aninhada de **GitHub Actions Workflows**. Ela se aplica às interações com o Copilot.
3. Abra `.github/instructions` e explore seus arquivos. Há instruções para Astro, a camada de dados Drizzle, testes e muito mais.
4. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo`, que define um glob relativo à raiz do repositório. Neste caso, ele corresponde a arquivos de teste TypeScript, como `**/*.test.ts`.
5. Observe as instruções específicas para criar testes de unidade.
6. Abra `.github/instructions/drizzle.instructions.md` e examine os links para outros arquivos de instruções e arquivos do projeto. Assim, você pode dividir instruções maiores em arquivos menores e reutilizáveis. Os caminhos são relativos ao arquivo de instruções, não à raiz do repositório.

> [!NOTE]
> A seção **Code formatting requirements** em `copilot-instructions.md` descreve os padrões do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc e cabeçalhos de arquivo.

## Adicionar um novo padrão do repositório

Agora você adicionará uma regra global para incluir comentários TSDoc e cabeçalhos de arquivo no código.

1. No terminal do codespace, crie e selecione uma branch nova:

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. Abra `.github/copilot-instructions.md`.
3. Localize a seção **Code formatting requirements**, perto da linha 27.
4. Adicione estes itens Markdown logo abaixo dos padrões existentes:

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

5. Salve `copilot-instructions.md`.

> [!TIP]
> Como você viu anteriormente, instruções no nível do repositório (`.github/copilot-instructions.md`) são o lugar certo para padrões globais, como a regra de comentários de documentação. Use `*.instructions.md` para linguagens, tipos de arquivo ou tarefas específicos.

## Fazer commit, enviar e integrar as instruções

O agente de nuvem lê os arquivos de instruções da branch de destino da issue. No próximo exercício, ele criará uma branch a partir de `main`; portanto, as instruções precisam estar em `main` para serem aplicadas.

1. Prepare e faça commit da alteração:

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. Envie a branch:

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. No github.com, abra um pull request de `custom-instructions-cloud` para `main` e faça o merge. Assim, o agente de nuvem poderá ler as instruções nas issues atribuídas no próximo exercício.

> [!TIP]
> Se preferir trabalhar diretamente em `main` neste workshop, você pode ignorar a etapa da branch e fazer commit direto em `main`. A branch serve para reproduzir o fluxo de um projeto real.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções e adicionou um padrão global a `.github/copilot-instructions.md`. Você verá esse padrão nos pull requests que o agente de nuvem criar nos exercícios seguintes.

Em seguida, você [atribuirá sua primeira issue ao agente de nuvem do Copilot][next-lesson].

## Recursos

- [Arquivos de instruções para personalizar o GitHub Copilot][instruction-files]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [5 dicas para escrever instruções personalizadas melhores para o Copilot][copilot-instructions-five-tips]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
