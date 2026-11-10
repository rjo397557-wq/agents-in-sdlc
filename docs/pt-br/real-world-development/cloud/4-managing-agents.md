---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você atribuiu três tarefas diferentes ao agente de nuvem do Copilot para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, é importante acompanhar as tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel central, você pode ver todas as tarefas com pull requests (PRs) abertos, explorar as operações executadas e orientar uma sessão em andamento.

Neste exercício, você vai:

- explorar a página de agentes para acompanhar as tarefas do agente de nuvem.
- orientar uma sessão em andamento para solicitar funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a implementação do modo de alto contraste, a equipe percebeu que seria útil adicionar também um modo claro. Como o trabalho já envolve atualizar os estilos do site e adicionar controles, faz sentido incluir essa funcionalidade. Você orientará o agente para garantir que ele implemente os dois modos.

## Revisar as tarefas do agente de nuvem

Vamos verificar o estado das tarefas atribuídas ao agente.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe as tarefas listadas nos painéis principal e esquerdo. Você deverá ver as tarefas atribuídas, incluindo:
   - atualizar a documentação do código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações do Copilot, que podem incluir:
   - obter o código do repositório.
   - configurar o ambiente de trabalho.
   - configurar servidores MCP.
   - executar etapas para concluir a tarefa.

> [!NOTE]
> As etapas exatas variam de acordo com o estado do trabalho e a abordagem do Copilot.

4. Observe também o painel de pull request à direita, que mostra o PR e os arquivos alterados.

## Orientar o agente de nuvem

Agora que você conhece as tarefas ativas, peça ao Copilot que adicione um controle de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão que está implementando o modo de alto contraste. O título exato depende do nome escolhido pelo Copilot e do estado do trabalho.

   ![Sessão de acessibilidade no painel central](../../../_images/ex6-accessibility-session.png)

2. Aguarde alguns minutos até que a configuração seja concluída e o trabalho comece. Você verá mensagens indicando isso.
3. Na caixa de diálogo **Steer active session while Copilot is working**, envie o prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Tarefa do agente na página de agentes com a caixa de diálogo para orientar uma sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe o Copilot reconhecer o prompt e incluí-lo no trabalho.

## Deixar o Copilot trabalhar

O Copilot incorporará a nova solicitação depois de concluir a etapa em andamento. Isso pode levar alguns minutos, então aproveite para refletir sobre o que aprendeu até aqui.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, o painel central para acompanhar e orientar tarefas do agente de nuvem. Você pode acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar uma tarefa em andamento para ajustar o escopo.

Você explorou:

- como usar a página de agentes para acompanhar tarefas do agente de nuvem.
- como orientar uma sessão em andamento para solicitar funcionalidade adicional.

Enquanto o Copilot conclui o trabalho de acessibilidade, vamos [iterar sobre os pull requests que ele criou][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
