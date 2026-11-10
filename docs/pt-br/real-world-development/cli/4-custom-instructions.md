---
title: "Lição 4 - Orientar o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é essencial ao trabalhar com IA generativa. Se uma tarefa precisar ser feita de uma maneira específica, você deve garantir que essa orientação esteja disponível para o Copilot. Os [arquivos de instruções][instruction-files] descrevem não apenas *qual* código você quer, mas também *como* ele deve ser estruturado. Agora que você criou a filtragem, explorará as instruções usadas pelo Copilot, adicionará um padrão de documentação e o aplicará ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos de instruções com escopo de caminho chegam ao agente.
- atualizar o arquivo de instruções para que os padrões de codificação sejam seguidos.
- observar o impacto dos arquivos de instruções no código.

## Cenário

Como toda boa equipe de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para suas práticas de desenvolvimento. Entre eles:

- Os comentários devem explicar a intenção e as decisões que não são óbvias, em vez de repetir o código.
- As funções exportadas em `db/` e `src/lib/` devem documentar sua finalidade, seus parâmetros e os valores retornados com TSDoc/JSDoc, incluindo um argumento `db` injetável quando houver.
- Os componentes reutilizáveis do Astro devem documentar seus contratos `Props`, e os comentários devem continuar atualizados quando o código relacionado mudar.
- As orientações existentes de formatação e lint devem ser preservadas.

Com os arquivos de instruções, você garantirá que o Copilot tenha as informações necessárias para executar as tarefas de acordo com essas práticas.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele entenda melhor seu estilo de codificação e seus requisitos. Elas ajudam a orientar o Copilot para gerar sugestões e trechos de código mais relevantes. Você pode especificar convenções de codificação, bibliotecas e os tipos de comentários que deseja incluir no código. As instruções podem valer para todo o repositório ou para tipos específicos de arquivos.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`, um único arquivo enviado ao Copilot em **todas** as solicitações do repositório. Ele deve conter informações de projeto relevantes para a maioria das solicitações feitas pelo chat ou pela CLI, como a pilha de tecnologia, uma visão geral do que está sendo criado, boas práticas e outras orientações globais.
- `.github/instructions/*.instructions.md`, arquivos que fornecem orientações para linguagens, tipos de arquivo ou tarefas específicos.

> [!NOTE]
> O suporte a outros formatos de instruções varia de acordo com o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explorar os arquivos de instruções personalizadas do projeto

O projeto inicial já inclui arquivos de instruções. Vamos explorá-los para observar o impacto antes de fazer uma alteração.

1. Volte ao Codespace.
2. No editor do Codespaces, e não no terminal, abra `.github/copilot-instructions.md`.
3. Explore o arquivo e observe a breve descrição do projeto e as orientações de codificação. Elas se aplicam a todas as interações com o Copilot neste repositório.
4. Abra `.github/instructions` e explore os arquivos. Há instruções para arquivos Astro, a camada de dados Drizzle, testes e muito mais.
5. Abra `.github/instructions/unit-tests.instructions.md` e observe o campo `applyTo` no início. Ele define um glob que determina a quais arquivos as instruções se aplicam.
6. Abra `.github/instructions/drizzle.instructions.md` e examine os links para outros arquivos de instruções e para arquivos existentes no projeto. Isso permite dividir instruções maiores em arquivos menores e reutilizáveis e indicar exemplos que o Copilot pode seguir.

> [!NOTE]
> A seção **Code formatting guidance** em `copilot-instructions.md` documenta os padrões de codificação do projeto, mas ainda não exige documentação no código. Nas próximas etapas, você adicionará regras para comentários TSDoc.

## Atualizar as instruções de acordo com as orientações da equipe

Vamos modificar o arquivo principal `copilot-instructions.md` para exigir comentários TSDoc no TypeScript gerado.

1. Abra novamente `.github/copilot-instructions.md`.
2. Localize a seção **Code formatting guidance**.
3. Adicione o item a seguir no final dessa seção:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente.

## Usar as orientações atualizadas

O Copilot CLI carrega as instruções do repositório quando uma conversa começa. Depois de salvar as instruções, saia e retome a conversa de filtragem para que o Copilot carregue as novas orientações sem perder o contexto do recurso.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit`. O Copilot CLI salva conversas automaticamente.
2. Na raiz do repositório e na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a branch `game-filters-cli` está selecionada.
3. Peça ao Copilot que aplique as orientações atualizadas:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Digite `/diff` e examine os arquivos TypeScript alterados. Observe os comentários TSDoc recém-gerados e confirme que descrevem o código corretamente.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções e aplicou um novo padrão ao recurso. Especificamente, você:

- explorou como as instruções do repositório e os arquivos com escopo de caminho chegam ao agente.
- atualizou o arquivo de instruções para que os padrões de codificação sejam seguidos.
- observou o impacto dos arquivos de instruções no código.

Em seguida, você [personalizará e executará a skill reutilizável quality-checks][next-lesson] para garantir que lint e testes sejam executados de forma consistente.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Boas práticas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — uma coleção de arquivos de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
