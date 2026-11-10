---
title: "Exercício 3 - Testar seu recurso com o servidor MCP do Playwright"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← Lição anterior: Modo de agente][previous-lesson] · [Próxima lição: Agentes personalizados →][next-lesson]

Você acabou de criar o recurso de filtragem com o modo de agente. Antes de abrir um pull request, confirme que ele funciona no navegador. Em vez de testar o aplicativo manualmente, conecte o **servidor MCP do Playwright** e deixe o Copilot controlar um navegador real. Depois, publique sua branch e abra o PR.

Neste exercício, você vai:

- aprender o que é o Model Context Protocol (MCP) e como os servidores MCP ampliam o Copilot com novas ferramentas.
- adicionar o servidor MCP do Playwright ao espaço de trabalho.
- pedir ao Copilot que use o servidor para testar a filtragem manualmente no navegador.
- publicar sua branch e abrir um pull request com o trabalho de filtragem.

## O que é o Model Context Protocol (MCP)?

O modo de agente fica muito mais útil quando consegue acessar recursos fora do editor. O Model Context Protocol (MCP) oferece essa possibilidade: é um padrão para o agente se comunicar com ferramentas e serviços externos.

![Diagrama do funcionamento interno do modo de agente e de sua interação com o contexto, o LLM e as ferramentas — incluindo ferramentas fornecidas por servidores MCP e extensões do VS Code](../../../_images/ex1-mcp-diagram.png)

O [Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) permite que agentes de IA se comuniquem com ferramentas e serviços externos em tempo real. Assim, eles podem acessar informações atualizadas por meio de recursos e executar ações em seu nome por meio de ferramentas.

Essas ferramentas e esses recursos são acessados por um servidor MCP, que funciona como uma ponte entre o agente de IA e as ferramentas e serviços externos, como APIs existentes ou ferramentas locais, por exemplo, pacotes NPM. Cada servidor MCP oferece ao agente um conjunto diferente de ferramentas e recursos.

Alguns servidores MCP conhecidos:

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: oferece acesso a APIs para gerenciar repositórios do GitHub, criar e atualizar repositórios e gerenciar issues e pull requests.
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: permite automatizar o navegador com Playwright, incluindo navegar por páginas, preencher formulários e selecionar botões.

Há muitos outros servidores MCP. O GitHub mantém um [registro MCP](https://github.com/mcp) para facilitar a descoberta e a contribuição ao ecossistema.

> [!CAUTION]
> Trate os servidores MCP como qualquer outra dependência do projeto. Antes de usar um servidor, revise o código-fonte, verifique quem o publicou e considere as implicações de segurança. Use servidores confiáveis e tenha cuidado ao conceder acesso a recursos ou operações confidenciais.

## Revisar a configuração do MCP

O arquivo `.vscode/mcp.json` configura os servidores MCP disponíveis neste espaço de trabalho do VS Code.

1. Abra `.vscode/mcp.json` no codespace.
2. Você verá um servidor `github` já configurado:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

Essa entrada vem do template do projeto e permitiu que o Copilot lesse o backlog de issues no exercício anterior. Ela usa o [servidor MCP remoto do GitHub][remote-github-mcp-server], portanto, não é necessário instalá-lo localmente. O VS Code conecta-se por HTTP, e você se autentica no GitHub na primeira vez que o Copilot usa uma ferramenta.

## Adicionar o servidor MCP do Playwright

Agora você adicionará outro servidor. O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador que ele pode controlar.

1. Em `.vscode/mcp.json`, adicione uma entrada `playwright` ao lado de `github`:

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. Salve o arquivo.

Ao contrário do servidor remoto `github`, `playwright` é local: o VS Code o inicia executando `npx @playwright/mcp@latest`. A opção `--headless` executa o navegador sem uma janela visível, o que é necessário no codespace, que não tem uma área de trabalho gráfica.

> [!NOTE]
> O projeto Tailspin Toys já usa Playwright nos testes de ponta a ponta, então o navegador necessário geralmente já está instalado. Se o Copilot informar que não há um navegador, peça que execute `npx playwright install chromium` e tente novamente.

## Iniciar e confiar no servidor

O VS Code inicia servidores MCP sob demanda: quando o Copilot precisa das ferramentas de um servidor pela primeira vez, o VS Code o inicia e pede que você confirme que confia nele.

1. Em `.vscode/mcp.json`, selecione a ação **Start** que aparece acima da entrada `playwright` para iniciar o servidor e confirmar que a configuração está correta.
2. Se o VS Code pedir que você confirme a confiança no servidor, revise a configuração e escolha confiar nele.

Iniciá-lo manualmente é opcional. Se você não fizer isso, o VS Code iniciará o servidor e pedirá sua confirmação quando o Copilot precisar das ferramentas na etapa seguinte.

## Testar o recurso de filtragem

O [servidor MCP do Playwright][playwright-mcp-server] fornece ao Copilot um navegador real. Em vez de navegar pelo aplicativo para verificar o trabalho, o agente pode abrir uma página, navegar, aplicar filtros, ler o resultado e resumir o que observou.

Internamente, o servidor MCP do Playwright usa a [árvore de acessibilidade][playwright-mcp-server] da página em vez de capturas de tela. Assim, o agente raciocina sobre elementos estruturados e rotulados — botões, links e itens de lista — da mesma forma que a tecnologia assistiva. Uma verificação funcional rápida também pode funcionar como uma verificação básica de acessibilidade.

Com o servidor conectado e o aplicativo em execução, peça ao Copilot que teste o recurso:

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

O Copilot abrirá um navegador pelo servidor MCP do Playwright, seguirá as etapas e apresentará os resultados. Compare o resumo com os critérios de aceitação da issue. Se algo parecer errado, faça perguntas ou peça que o Copilot corrija o código antes de abrir o PR.

> [!NOTE]
> O aplicativo precisa estar em execução em `http://localhost:4321`. Se você tiver parado o servidor de desenvolvimento, inicie-o antes de enviar o prompt. Na primeira utilização do Playwright MCP, o Copilot talvez precise baixar um navegador. Se isso acontecer, peça que execute `npx playwright install chromium` e tente novamente.

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## Publicar a branch e criar um pull request

Depois de confirmar que o recurso funciona, abra um pull request (PR) para que a equipe possa revisá-lo. Primeiro, publique a branch `filtering-vscode`.

1. No codespace, abra o painel **Source Control** e revise as alterações feitas pelo Copilot.
2. Prepare as alterações selecionando o ícone **+**.
3. Gere uma mensagem de commit usando o botão **Sparkle**.

   ![Painel Source Control com as alterações feitas pelo Copilot](../../../_images/ex3-source-control-changes.png)

4. Selecione **Publish** para enviar a branch ao repositório.

## Criar o pull request

Há várias formas de criar um pull request, inclusive pelo github.com e pela interface de linha de comando do GitHub. Como você já está usando o GitHub Copilot, peça que ele crie o PR e o associe à issue relevante.

1. Abra o painel Copilot Chat e selecione **New Chat** para iniciar uma sessão.
2. Confirme que **Agent** está selecionado para que o Copilot possa usar as ferramentas do GitHub.
3. Envie o prompt:

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. Quando necessário, selecione **Continue** para permitir que o Copilot obtenha informações e execute ações. Na primeira utilização de uma ferramenta do GitHub, talvez seja solicitado que você se autentique.
5. Observe o Copilot procurar a issue correta e criar o PR.
6. Selecione o link para revisar o pull request, mas **não faça o merge ainda**.

## Resumo e próximos passos

Neste exercício, você:

- aprendeu o que é o MCP e como os servidores MCP ampliam o Copilot.
- adicionou o servidor MCP do Playwright ao espaço de trabalho.
- usou o servidor para testar manualmente a filtragem em um navegador antes de publicar o recurso.
- publicou a branch e abriu um pull request.

Você usou um servidor MCP para *testar* um recurso, mas o MCP é apenas uma das formas de ampliar o Copilot. Em seguida, vamos [criar um agente personalizado][next-lesson] para tarefas específicas, como revisões de acessibilidade.

## Recursos

- [O que é MCP e por que todo mundo está falando sobre isso?][mcp-blog-post]
- [Servidor MCP do Microsoft Playwright][playwright-mcp-server]
- [Servidor MCP do GitHub][github-mcp-server]
- [Registro de servidores MCP do GitHub][mcp-registry]
- [Servidores MCP no VS Code][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
