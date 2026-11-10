---
title: "Exercício 2 - Agente de nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações não enfrentem problemas de dívida técnica. Pode ser uma questão de segurança não resolvida, código legado que precisa de atualização ou uma solicitação de funcionalidade que ficou esquecida no backlog por falta de tempo. O agente de nuvem do GitHub Copilot foi criado para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria um pull request (PR) de rascunho para que uma pessoa desenvolvedora revise o trabalho. Assim, é possível delegar tarefas tediosas, acelerar o desenvolvimento e se concentrar em objetivos maiores.

Neste exercício, você explorará como:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam realizadas com segurança.
- criar issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem dívidas técnicas para resolver. As pessoas contratadas inicialmente para criar o site deixaram a documentação em um estado inadequado — na verdade, ela está totalmente ausente. Como primeiro passo, a equipe quer comentários TSDoc em todas as funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. Ela quer mostrar jogos relacionados — outros títulos da mesma categoria — na página de detalhes de cada jogo para explorar a experiência e fazer testes de aceitação. Esse trabalho está bloqueado, mas há outras issues mais prioritárias.

Esses trabalhos podem ser adiados com facilidade e são bons candidatos para o agente de nuvem. Ele trabalha de forma assíncrona enquanto você cuida de outras tarefas; depois, você volta para revisar o trabalho.

## Apresentação do agente de nuvem do GitHub Copilot

O [agente de nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) executa tarefas em segundo plano de forma semelhante a uma pessoa desenvolvedora. Você pode iniciar o trabalho de várias formas, inclusive [atribuindo uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um PR de rascunho para acompanhar o progresso, configura o ambiente e começa a trabalhar. Você pode acompanhar a sessão enquanto ela está em andamento ou depois que termina. Quando a solução estiver pronta para revisão, o agente marcará você no pull request.

## A importância de instruções com escopo claro

O GitHub Copilot não é mágico; ainda é necessário pensar. Até tarefas aparentemente simples podem ter complexidades escondidas. Por isso, [defina o escopo do trabalho com cuidado][cloud-agent-best-practices]. Trate o Copilot como uma ferramenta de programação em par com IA: trabalhe em etapas, aprenda, experimente e adapte. Os fundamentos do desenvolvimento de software não mudam com a IA generativa.

## Instruções personalizadas neste repositório

Os exercícios anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Se você já fez um exercício prático sobre instruções, viu `.github/copilot-instructions.md` em ação; se ainda não, aproveite para examiná-lo. Antes de atribuir trabalho ao agente de nuvem, leia os arquivos de instruções existentes para observar seu efeito mais tarde.

Abra os arquivos a seguir na interface Web do GitHub ou em um codespace:

- `.github/copilot-instructions.md`: examine **Code standards**, principalmente as expectativas de convenções TypeScript e comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica as instruções aos arquivos `**/*.test.ts`.

Quando atribuir a issue `Code lacks documentation` ao agente de nuvem na próxima seção, observe no PR os comentários TSDoc, as convenções TypeScript e os cabeçalhos de arquivo, que vêm desses arquivos de instruções. Voltaremos a esse ponto ao revisar os PRs.

## Configurar o ambiente de desenvolvimento do agente de nuvem

Criar código normalmente exige um ambiente específico e scripts de configuração para instalar ferramentas e dependências. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de forma semelhante a um engenheiro de software.

O agente de nuvem usa o [GitHub Actions][github-actions] como ambiente. Você pode personalizá-lo com um [fluxo de configuração][setup-workflow] em `.github/workflows/copilot-setup-steps.yml`, executado antes do início do trabalho. Isso disponibiliza ferramentas e dependências necessárias. O fluxo já foi configurado para este laboratório: instala Node.js e dependências do projeto, navegadores para testes de ponta a ponta e configura o banco de dados SQLite migrado e preenchido para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões no menor nível necessário para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Para clonar o repositório durante a configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nas etapas de configuração, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do frontend/aplicativo: Node.js (o aplicativo inteiro agora usa Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migre e preencha o banco de dados SQLite local para que compilações e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

Esse arquivo se parece com qualquer fluxo do GitHub, mas tem alguns detalhes importantes:

- Ele contém um único job, `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no PR.
- O gatilho `workflow_dispatch` permite executar o fluxo manualmente na guia **Actions** do repositório. Isso é útil para testar a configuração sem esperar que o Copilot a execute.

## Adicionar documentação

Embora todo mundo saiba que a documentação é importante, muitos projetos têm informações desatualizadas ou não têm documentação. Essa dívida técnica reduz a produtividade e dificulta a manutenção do código e a integração de pessoas desenvolvedoras. O Copilot é capaz de criar documentação, por isso esta é uma boa tarefa para delegar ao agente de nuvem. Em um exercício futuro, você voltará para revisar o trabalho.

1. Abra uma nova guia do navegador e acesse o repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. Em **Title**, digite `Code lacks documentation`.
6. Em **Description**, digite:

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

   ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

   ![Detalhes da atribuição ao Copilot](../../../_images/ex4-assign-copilot-details.png)

10. Selecione a guia **Pull Requests**.
11. Abra o PR recém-criado, com um título semelhante a `[WIP]: Code lacks documentation`. Se ele não aparecer, aguarde e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista pode levar alguns minutos para aparecer. O Copilot está configurando o ambiente com o fluxo descrito anteriormente, analisando o projeto e definindo uma abordagem.

13. Revise as tarefas planejadas.
14. Role a linha do tempo do PR para encontrar a atualização que indica que o Copilot começou a trabalhar.
15. Selecione **View session**.

   ![Exibição da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a página para ver o indicador atualizado.

16. Observe a sessão em andamento: o Copilot explora o código, entende o estado do projeto, planeja e cria código.

Isso pode levar alguns minutos. Um dos objetivos do agente de nuvem é executar tarefas de forma assíncrona para que você possa cuidar de outros trabalhos. Vamos aproveitar esse recurso atribuindo outra tarefa e, em seguida, voltando a desenvolver o aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma vantagem do agente de nuvem é poder dividir o trabalho. Enquanto você cuida de uma tarefa, o agente pode adicionar uma seção de jogos relacionados para a equipe de design.

1. Volte ao repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue**.
4. Selecione **Blank issue**.
5. Em **Title**, digite `Show related games on the game details page`.
6. Em **Description**, digite:

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. Selecione **Create**.
8. À direita, selecione **Assign to Copilot**.

   ![Atribuir uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Logo depois, você deverá ver 👀 no primeiro comentário da issue, indicando que o Copilot está trabalhando.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O agente está trabalhando na segunda solicitação. Assim como uma pessoa engenheira de software, ele não exige acompanhamento contínuo; você pode revisar o resultado depois. Agora vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Neste exercício, você explorou o [agente de nuvem do GitHub Copilot][copilot-agents]. Você pode atribuir issues ao Copilot para execução assíncrona, por exemplo, para resolver dívida técnica, criar funcionalidades ou migrar código entre frameworks.

Você explorou:

- como personalizar o ambiente de geração de código.
- como garantir operações seguras.
- a importância de issues com escopo claro.
- como atribuir issues ao Copilot.

Enquanto o agente trabalha, você pode criar e usar agentes personalizados. O [agente de nuvem também pode usar servidores MCP][cloud-agent-mcp] e instruções personalizadas, que já foram abordados.

## Recursos

- [Sobre o agente de nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas para o fluxo de configuração do agente de nuvem][cloud-agent-best-practices]

| [Próxima lição: Agentes personalizados →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
