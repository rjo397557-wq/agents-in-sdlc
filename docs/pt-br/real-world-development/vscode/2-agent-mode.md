---
title: "Exercício 2 - Adicionar funcionalidade com o Agent Mode do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até as atualizações mais simples de um aplicativo normalmente exigem alterações em vários arquivos e operações como executar testes. O fluxo de desenvolvimento costuma envolver localizar todos os arquivos necessários, fazer as alterações, executar testes, depurar, descobrir qual arquivo ficou faltando e fazer mais alterações. A lista continua.

É aí que entra o Agent Mode do Copilot.

O Agent Mode foi criado para atuar com mais autonomia no IDE. Ele explora a estrutura do projeto, faz as alterações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos ver como usá-lo para adicionar funcionalidade ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente de nuvem oferecem experiências diferentes. O modo de agente trabalha no IDE e permite ciclos de feedback rápidos. O agente de nuvem trabalha de forma assíncrona, como uma pessoa da equipe, e interage com você por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o Agent Mode do Copilot explora o projeto, identifica arquivos relevantes e coordena alterações.
- o Agent Mode implementa recursos na interface e na camada de dados.
- revisar alterações e testes gerados pelo Agent Mode antes do merge.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. Você já adicionou um helper de editoras no exercício anterior. Agora concluirá as alterações restantes na camada de dados, na interface e nos testes com o Agent Mode.

## Executar o site Tailspin Toys

Antes de fazer alterações, explore o site para conhecer sua funcionalidade atual.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com tema de desenvolvimento. Ele permite listar jogos e exibir detalhes. O site é um único aplicativo Astro, que renderiza páginas como HTML estático durante a compilação. As páginas consultam diretamente um banco de dados SQLite local por meio do Drizzle ORM; não há uma API de backend nem um framework de interface executado no cliente. Os helpers reutilizáveis de acesso a dados ficam em `src/lib/`, e a interatividade é implementada com um `<script>` Astro pequeno e localizado que usa as APIs padrão do DOM.

### Iniciar o site

O comando `npm run dev` inicia o servidor de desenvolvimento Astro.

1. Volte ao codespace. Você deve estar na branch `main` depois do exercício anterior.
2. Abra um terminal pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, você verá uma mensagem semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se aparecer uma caixa de diálogo perguntando se você quer abrir `http://localhost:4321` em um navegador, feche-a selecionando **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro no terminal.

> [!NOTE]
> Em um codespace, selecionar o link `localhost` no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`, um túnel privado para o servidor do codespace.

### Explorar o site

O site tem estas áreas principais:

- **Página inicial**: lista os jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: exibe mais informações, como título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar, abra o painel Copilot Chat no codespace e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa informa as opções relevantes.

O site já funciona, mas queremos ampliar seus recursos. Vamos começar consultando o backlog de issues criado automaticamente quando você configurou o projeto.

1. No seletor de agentes da exibição Chat, selecione **Agent**. Esse agente planeja e implementa alterações em vários arquivos, executa comandos de terminal e chama ferramentas.

   ![Seletor de agente na exibição Chat](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> Os autores deste workshop não estão recomendando um modelo específico. Usamos Claude Sonnet 4.5 ao criá-lo e o incluímos para tentar manter as sugestões relativamente consistentes. Como os LLMs são probabilísticos, os resultados podem ser diferentes; isso é normal.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente e ainda concluir a tarefa.

3. Peça ao Copilot que mostre o backlog:

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. Selecione **Continue** para executar o comando que lista as issues.
5. Revise a lista gerada e a priorização sugerida.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere código, examine o arquivo de instruções de interface do usuário (UI), que fornece contexto sobre como adicionar funcionalidade ao site.

1. No codespace, abra `.github/instructions/ui.instructions.md`.
2. Observe as orientações: arquitetura, princípios de componentes, testabilidade e acessibilidade, além de links para instruções de tipos de arquivo como Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem referenciar outros arquivos de instruções e arquivos do projeto. Os caminhos são relativos ao arquivo de instruções, o que permite reutilizar conteúdo e dividir instruções complexas em partes menores.

## Implementar a filtragem

Para concluir a filtragem, o aplicativo precisa de pelo menos três alterações:

- adicionar ou ajustar a lógica de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos com a interface de filtragem.

Os testes também precisam passar antes do merge. O Agent Mode pode fazer esse trabalho.

1. Continue a conversa atual ou selecione **New Chat**.
2. Selecione **Add Context**, **Instructions** e **ui** como arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. Confirme que **Agent** continua selecionado.
4. Confirme que **Claude Sonnet 4.5** continua selecionado.
5. Peça ao Copilot que implemente o recurso de acordo com a issue:

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Observe o Copilot explorar o projeto, localizar a camada de dados, a interface e os testes e, em seguida, alterar arquivos e executar testes.

   ![Copilot explorando arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot pode levar alguns minutos para explorar, editar arquivos e executar testes. Enquanto trabalha, talvez apareçam os botões **Keep** e **Undo** no editor. Aguarde a conclusão antes de aceitar ou desfazer as alterações.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.
8. Alguns testes podem falhar durante o processo; o Copilot pode alternar entre gerar código e executar testes até não detectar erros.

   ![Sessão concluída do Copilot Agent Mode](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examine o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por outras ferramentas de IA.

10. Volte ao navegador em que o site está em execução e teste a nova funcionalidade.
11. Depois de confirmar o funcionamento e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Neste exercício, você usou o Agent Mode para adicionar recursos ao site Tailspin Toys. Você aprendeu como:

- o Agent Mode implementa recursos na interface e na camada de dados.
- o Copilot explora o projeto, identifica arquivos relevantes e coordena alterações.
- revisar alterações e testes gerados pelo Agent Mode antes do merge.

Agora, vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício bônus – Implementar paginação

À medida que a lista de jogos cresce, será necessário implementar paginação. Use o que aprendeu neste exercício para pedir ao Copilot que faça isso. Considere:

- seguir as práticas existentes e usar os arquivos de instruções.
- decidir se a pessoa usuária poderá escolher o tamanho da página ou se ele será fixo.
- fornecer no prompt orientações suficientes para a implementação desejada.
- iterar com o GitHub Copilot, pedindo alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
