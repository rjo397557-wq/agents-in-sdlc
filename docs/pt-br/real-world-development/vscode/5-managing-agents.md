---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

Quando você usa o GitHub Copilot no modo de agente, ele trabalha de forma autônoma: explora a base de código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho ocorre em tempo real na sua máquina, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo. Você pode revisar cada diff, aceitar ou rejeitar alterações e orientar a conversa com prompts de acompanhamento sem sair do editor.

Neste exercício, você vai:

- acompanhar o trabalho do agente de acessibilidade em tempo real.
- revisar e aceitar as edições propostas.
- orientar o agente durante a sessão para adicionar um controle de modo claro junto ao alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho está em andamento, a equipe de design também solicita um controle de modo claro para as pessoas que consideram o tema padrão escuro demais. Em vez de iniciar uma conversa nova, você orientará a sessão existente para ampliar o trabalho.

## Acompanhar o progresso do Copilot

A exibição Copilot Chat mostra cada etapa do agente: os arquivos que explora, os comandos que executa e as edições propostas. Cada edição aparece no chat com um diff e os controles **Keep** e **Undo**.

1. Volte à exibição Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, acompanhe as chamadas de ferramentas, leituras e edições de arquivos e os comandos de terminal que ele pede para executar.
3. Quando o agente propuser uma edição, o diff aparecerá no chat:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para revertê-la e permitir que o agente tente novamente.
4. Abra **Files changed** na parte superior do chat para ver todos os arquivos modificados na sessão.

> [!NOTE]
> Se uma chamada de ferramenta solicitar permissão (por exemplo, para executar um comando de terminal), o agente pausará e exibirá **Allow**. Aprove apenas comandos com os quais você concorde.

5. Aguarde o agente concluir o modo de alto contraste. Ele apresentará uma mensagem final que resume as alterações.

## Orientar o agente para adicionar o modo claro

Agora que o modo de alto contraste está implementado, amplie a mesma conversa para adicionar um controle de modo claro. Como você continua a sessão, o agente mantém o contexto das alterações, dos componentes, dos estilos e do padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie o prompt:

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. O agente examinará as alterações anteriores e proporá adições para o modo claro.
3. Revise cada nova edição e use **Keep** ou **Undo**.
4. Se o agente seguir uma direção indesejada — por exemplo, tentar refatorar os controles de forma que você não queira — interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem para esclarecer.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Use prompts de acompanhamento para pedir ajustes, corrigir o rumo ou ampliar o escopo. Por exemplo: "simplifique o CSS e use variáveis CSS em vez de duplicar seletores", "integre o modo claro ao painel de configurações existente" ou "adicione atalhos de teclado para os dois controles".

## Revisar o conjunto de alterações

Antes de fazer commit, revise o trabalho do agente.

1. Abra a exibição **Source Control** no VS Code.
2. Revise a lista de arquivos alterados. Você deverá ver atualizações nos componentes Astro, nos estilos e nos testes relacionados.
3. Abra alguns arquivos alterados e examine os diffs. Confirme o uso dos padrões de acessibilidade do agente personalizado: atributos ARIA, navegação por teclado, HTML semântico e persistência no armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para acompanhar e orientar uma sessão em andamento, revisando cada edição, aceitando ou rejeitando alterações e ampliando o trabalho com prompts de acompanhamento.

Neste exercício, você:

- acompanhou o trabalho do agente em tempo real.
- aceitou e rejeitou edições individuais.
- orientou a sessão durante o trabalho com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade em um pull request e revisará as alterações produzidas pelo agente personalizado local.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
