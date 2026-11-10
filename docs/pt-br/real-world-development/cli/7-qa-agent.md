---
title: "Lição 7 - Criar e usar um agente de QA"
description: "Crie um agente personalizado de QA que combine os requisitos da issue, a skill quality-checks e o MCP do Playwright."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Você usou a skill `quality-checks` para executar verificações automatizadas e o MCP do Playwright para observar a experiência de filtragem em um navegador. Agora, reunirá essas capacidades em um agente personalizado com um processo de QA claramente definido.

Nesta lição, você vai:

- explorar como um agente personalizado trabalha com instruções, skills e ferramentas MCP.
- criar e examinar um perfil reutilizável de garantia de qualidade.
- selecionar o agente de QA e revisar suas conclusões em relação à issue de filtragem.

## Cenário

A Tailspin Toys quer uma revisão consistente dos requisitos, da qualidade do código, das verificações automatizadas, da cobertura de testes e do comportamento no navegador antes de abrir um pull request (PR). Um agente personalizado pode coordenar esse processo de QA e fornecer um relatório reutilizável.

## O que é um agente personalizado?

Um agente personalizado é uma versão especializada do Copilot definida em um perfil Markdown. O perfil descreve a finalidade, as instruções e as ferramentas disponíveis. Neste workshop, você definirá um papel de QA em `.github/agents/qa.agent.md` e o selecionará no Copilot CLI.

As personalizações têm funções diferentes. As instruções do repositório descrevem os padrões da equipe. A skill `quality-checks` agrupa verificações repetíveis. O MCP do Playwright fornece ferramentas de navegador. O perfil de QA informa ao Copilot como usar esses recursos para avaliar requisitos e relatar conclusões. Ele não substitui as outras personalizações nem exige outra conversa.

## Criar o perfil de QA

Antes de abrir o PR do recurso, peça ao Copilot que crie um perfil de QA reutilizável. O perfil definirá as verificações de QA e os limites que o agente deve respeitar.

1. Volte ao Codespace e confirme que a conversa de filtragem está no modo Interactive.
2. Peça ao agente padrão que crie o agente personalizado:

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## Examinar o perfil

Antes de usar o novo agente, revise o perfil para confirmar que o Copilot registrou o fluxo e os limites de autoridade desejados. Isso evita que um agente incompleto ou amplo demais altere o recurso quando você quer apenas verificá-lo.

1. Digite `/diff` e abra `.github/agents/qa.agent.md`.
2. Leia o frontmatter. `description` é obrigatório; `name` é opcional, mas fornece um nome de exibição claro.
3. Leia as instruções do perfil e confirme que o QA começa pelos requisitos, segue as instruções do repositório, executa a skill `quality-checks` e usa o MCP do Playwright.
4. Confirme que o QA apresenta evidências, pergunta antes de alterar o código da implementação e não faz commits nem abre pull requests.
5. Se o perfil não incluir alguma dessas responsabilidades ou limites, peça ao agente padrão que o revise antes de continuar.

## Executar QA em relação à issue

O Copilot CLI carrega os agentes do projeto quando uma conversa começa. Depois de criar o perfil, retome a mesma conversa de filtragem e selecione QA para que o agente possa usar a issue e as decisões de planejamento já presentes no contexto.

1. Ative o agente com o prompt:

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> Como você acabou de criar o agente, ele talvez não apareça na lista de agentes disponíveis. Ainda assim, o comando acima o ativa.

2. Peça ao agente de QA que revise o recurso:

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. Aguarde o agente concluir o trabalho.
4. Leia o relatório e confirme que as conclusões têm evidências de apoio.

## Resumo e próximos passos

Você adicionou um papel especializado reutilizável ao fluxo e revisou o trabalho dele. Nesta lição, você:

- explorou como um agente personalizado usa instruções, skills e ferramentas MCP.
- criou e examinou um perfil reutilizável de QA.
- selecionou o agente de QA e revisou suas conclusões em relação à issue.

Agora você tem a implementação, a atualização da skill, o perfil de QA, os testes e o relatório de verificação prontos para revisão. Em seguida, você [reunirá esses itens em um PR do recurso e usará o Agent Merge][next-lesson].

## Recursos

- [Criar agentes personalizados para o Copilot CLI][create-agents]
- [Configuração de agentes personalizados][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
