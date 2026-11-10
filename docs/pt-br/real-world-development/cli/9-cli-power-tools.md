---
title: "Lição 9 - Comandos de barra no GitHub Copilot CLI"
description: "Explore comandos para gerenciar o contexto, escolher modelos, compartilhar sessões e delegar tarefas opcionalmente ao agente de nuvem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Como toda boa ferramenta de CLI, o GitHub Copilot CLI inclui vários comandos de barra. Eles dão acesso a recursos avançados, informações internas e opções de configuração. Você já usou comandos como `/diff`, `/mcp`, `/skills`, `/agent` e `/pr`. Vamos explorar alguns outros comandos úteis.

Nesta lição, você vai:

- usar `/context` e `/compact` para explorar como o Copilot gerencia o contexto da conversa.
- usar `/model` para conhecer os modelos disponíveis.
- aprender como `/share` exporta ou compartilha uma sessão.
- explorar comandos opcionais para trabalho paralelo, worktrees e delegação ao agente de nuvem.

## Cenário

Você concluiu o fluxo principal da CLI. Agora vamos conhecer outros recursos: gerenciamento de contexto, troca de modelo, compartilhamento de sessões e delegação opcional ao [agente de nuvem do Copilot][about-cloud-agent].

## Explorar o contexto do Copilot CLI

Em tarefas maiores ou mais complexas, você pode chegar ao limite de contexto do modelo. O Copilot CLI compacta a conversa automaticamente quando necessário, mas você também pode inspecionar e compactar o contexto com comandos de barra.

1. Volte ao Codespace e inicie o Copilot CLI na raiz do repositório, caso ele ainda não esteja aberto.
2. Digite:

   ```plaintext
   /context
   ```

3. Observe o modelo, o uso atual de tokens e a divisão do contexto entre instruções do sistema, ferramentas, mensagens e espaço livre.
4. Compacte a conversa:

   ```plaintext
   /compact
   ```

5. Digite `/context` novamente e compare os resultados. A diferença pode ser pequena se a conversa já for curta.

> [!NOTE]
> O Copilot CLI compacta o contexto automaticamente conforme a janela se enche. Use `/compact` quando quiser escolher o momento. Use `/clear` ou `/new` ao mudar para uma tarefa sem relação e iniciar uma conversa limpa.

## Escolher um modelo

Os modelos têm pontos fortes diferentes, e cada pessoa desenvolvedora tem suas preferências. O Copilot CLI permite listar e selecionar o modelo desejado.

1. Digite:

   ```plaintext
   /model
   ```

2. Explore os modelos disponíveis e as informações de uso.
3. Mantenha o modelo atual, selecione outro ou pressione <kbd>Esc</kbd> para fechar a lista.

## Compartilhar uma sessão

Trabalhar em equipe e compartilhar aprendizados ajuda todo mundo a aproveitar melhor as ferramentas de IA. O comando `/share` pode exportar uma sessão para um arquivo Markdown ou HTML, criar um link compartilhável ou publicar um GitHub gist.

1. Digite `/help` e confira as opções de `/share` na versão instalada.
2. Se quiser compartilhar a sessão, escolha o destino adequado, como `/share file` para exportar um arquivo Markdown local.
3. Revise o conteúdo exportado antes de enviá-lo ou publicá-lo. As exportações podem incluir prompts, respostas e detalhes do projeto.

Publicar um link ou gist é opcional. Não publique conteúdo do repositório ou da conversa que a equipe não tenha autorizado compartilhar.

## Opcional: ampliar o trabalho ou delegar

O fluxo principal do workshop foi concluído. O Copilot CLI também oferece comandos para tarefas maiores:

- `/fleet` pode dividir subtarefas independentes entre subagentes e executá-las em paralelo.
- `/worktree` pode criar um worktree isolado para outra tarefa.
- `/delegate` pode enviar uma tarefa ao agente de nuvem do Copilot, que trabalha de forma assíncrona e pode abrir um pull request.

Esses comandos são opcionais porque podem criar worktrees adicionais ou trabalho remoto. Inicie uma tarefa nova e bem delimitada antes de experimentá-los e revise os resultados pelo fluxo normal. Para conhecer melhor o trabalho assíncrono dos agentes, continue com o [workshop do agente de nuvem][cloud-workshop].

## Resumo e próximos passos

Os comandos de barra permitem configurar o Copilot CLI, compartilhar sessões e acompanhar o que acontece nos bastidores. Nesta lição, você:

- usou `/context` e `/compact` para explorar como o Copilot gerencia o contexto.
- usou `/model` para conhecer os modelos disponíveis.
- aprendeu como `/share` exporta ou compartilha sessões.
- explorou comandos opcionais para trabalho paralelo, worktrees e delegação ao agente de nuvem.

Há muitos outros comandos de barra e recursos para conhecer. Vamos encerrar a jornada [revisando o que aprendemos][next-lesson] e explorando os próximos passos.

## Recursos

- [Referência de comandos do Copilot CLI][cli-reference]
- [Gerenciamento de contexto no Copilot CLI][context-management]
- [Sobre o agente de nuvem do Copilot][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
