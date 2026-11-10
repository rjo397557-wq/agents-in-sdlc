---
title: "Exercício 4 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados em tarefas ou domínios específicos do fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você fornece instruções focadas, boas práticas, padrões de código e conhecimento de domínio. As equipes podem transformar sua experiência em agentes reutilizáveis, como um agente de acessibilidade que aplica as diretrizes [WCAG][wcag], um agente de segurança ou um agente de testes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que descreve o comportamento, a experiência e as instruções do agente.

### Agentes personalizados e skills de agente

Agentes personalizados e [skills de agente][agent-skills-concept] têm algumas semelhanças: ambos são definidos principalmente em arquivos Markdown e orientam uma IA sobre como executar tarefas. Uma distinção simples é: o **agente personalizado** é quem trabalha; as **skills** são ferramentas.

Os agentes personalizados têm uma janela de contexto própria e podem orquestrar skills e até outros agentes. Neste laboratório, o agente de acessibilidade revisará e atualizará o site conforme as diretrizes de acessibilidade; para isso, ele poderia usar uma skill de fluxo de pull request ou uma que execute e gerencie testes.

> [!NOTE]
> Não há uma única forma correta de criar um agente personalizado. Como em qualquer uso de IA, teste e itere para encontrar o que funciona em seu ambiente e cenário.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## Cenário

A Tailspin Toys quer garantir que sua plataforma de financiamento coletivo seja acessível a todas as pessoas, independentemente das capacidades ou preferências visuais. Comentários de usuários indicaram que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do baixo contraste entre o texto e o fundo. Para resolver isso, a equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é importante, você usará um agente personalizado para implementar a funcionalidade rapidamente.

Neste exercício, você vai:

- revisar um agente personalizado de acessibilidade existente.
- usar o agente de acessibilidade no Copilot Chat para implementar o modo de alto contraste.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado. Vamos revisar seu conteúdo para entender como ele orientará o Copilot.

Volte ao codespace e crie uma branch nova a partir de `main` para o trabalho de acessibilidade. O PR de filtragem do Exercício 3 continuará separado:

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> O frontmatter com `name` e `description` é obrigatório para agentes personalizados.

3. Examine as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível.
   - boas práticas de acessibilidade.
   - exemplos de HTML, CSS e JavaScript.
   - armadilhas e erros comuns.

## Usar o agente personalizado no Copilot Chat

O VS Code mostra todos os agentes personalizados de `.github/agents` no seletor de agentes na parte inferior da exibição Copilot Chat. Você pode selecionar um agente para aplicar suas instruções e ferramentas à conversa.

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam por exercício; cada etapa informa as opções relevantes.

1. Na exibição Chat, selecione **Agent** se ainda não estiver selecionado.

   ![Seletor de agente na exibição Chat](../../../_images/shared-chat-mode-selector.png)

2. Abra o seletor de agentes na parte inferior da exibição Chat. Ele mostra o agente ativo; por padrão, **default**.
3. Selecione **Accessibility agent**.
4. Envie ao agente de acessibilidade o prompt:

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. O Copilot Chat explorará a base de código, proporá alterações e editará os arquivos do projeto. Cada edição aparecerá no chat com o caminho e um diff para revisão.
6. O indicador **Files changed** será atualizado conforme o agente modificar arquivos.

> [!NOTE]
> Esse processo pode levar alguns minutos. O Copilot fará alterações reais no repositório — componentes Astro, CSS e testes relacionados.

Você revisará e orientará esse trabalho em andamento no próximo exercício.

## Resumo e próximos passos

Neste exercício, você conheceu os [agentes personalizados][custom-agents], assistentes especializados em tarefas e domínios específicos. Eles permitem transformar a experiência e os padrões da equipe em orientações reutilizáveis para o Copilot.

Você explorou:

- como os agentes personalizados são definidos.
- como usar um agente personalizado no modo de agente do Copilot Chat.

Em seguida, você [acompanhará e orientará o trabalho do agente][next-lesson], revisando as alterações em andamento e adicionando um controle de modo claro à mesma sessão.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Criar agentes personalizados no IDE][creating-custom-agents-ide]
- [Agentes personalizados no VS Code][custom-agents-vscode]
- [Configuração de agentes personalizados][custom-agents-config]
- [Agentes personalizados no awesome-copilot][awesome-copilot-agents]

---

| [← Lição anterior: Testar o recurso com o MCP do Playwright][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
