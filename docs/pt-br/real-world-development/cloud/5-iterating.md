---
title: "Exercício 5: Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, implementasse uma seção de jogos relacionados e criasse recursos de acessibilidade com modos de alto contraste e claro. Vamos revisar as alterações e, se necessário, orientar o Copilot para aprimorá-las antes de solicitar a revisão da equipe.

### Cenário

Os fundamentos do design de software e do DevOps não mudam com a IA generativa. É importante revisar o código gerado e seguir o processo normal de DevOps. Vamos revisar as sugestões do GitHub Copilot para a documentação, os jogos relacionados e os recursos de acessibilidade antes de solicitar a revisão da equipe.

## Segurança e agente de nuvem do GitHub Copilot

Como o agente de nuvem trabalha de forma assíncrona e sem supervisão contínua, há restrições de segurança:

- O Copilot tem acesso somente de leitura ao repositório e acesso de escrita **apenas** à branch que usa para o código.
- O agente de nuvem é executado no GitHub Actions em um ambiente separado e temporário.
- Os fluxos do GitHub Actions precisam de aprovação humana antes de serem executados.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo servidores MCP.

## Revisar a documentação gerada

Vamos começar pelo primeiro pull request (PR) criado pelo agente de nuvem: a documentação do código. Usaremos a interface padrão de PR do GitHub.com.

> [!NOTE]
> Talvez você veja um aviso de que o Copilot foi bloqueado por um firewall. Isso é **esperado**, pois o acesso a recursos externos, inclusive servidores MCP externos, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente de nuvem][agent-firewall].

1. Volte ao repositório no github.com.
2. Selecione **Pull Requests**.
3. Abra o PR com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde a conclusão, que pode levar alguns minutos.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

   ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que inclui comentários TSDoc e outra documentação. As alterações exatas podem variar. Procure comentários TSDoc, convenções TypeScript e cabeçalhos de arquivo, orientados pelos arquivos de instruções vistos no Exercício 2.
6. Quando terminar a revisão, volte à guia **Conversation** e role para baixo.
7. Você deverá ver um indicador de que alguns fluxos estão aguardando aprovação.
8. Se houver fluxos aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Os fluxos deverão entrar na fila da seção de verificações do PR. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

Você pode mencionar o Copilot em comentários do PR ou comentários em linha, assim como faria com outra pessoa da equipe. O Copilot verá o comentário e iniciará uma sessão para atender à solicitação. Como o resultado não é determinístico, não há um texto único a usar. Algumas ideias:

- adicionar cabeçalhos de comentário no início de cada arquivo com uma breve descrição.
- adicionar comentários TSDoc aos arquivos TypeScript e Astro.
- criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração na documentação gerada e marque **@copilot**. Use uma das ideias acima ou outra sugestão.
2. Selecione **View Session** para acompanhar o Copilot. Ele iniciará uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao PR.

   ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, deverá aparecer um novo commit no PR.
5. Selecione **Files changed** para revisar as alterações.

Continue iterando até ficar satisfeito. Depois, marque o PR como pronto para revisão e faça o merge.

![Marcar PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Agora vamos revisar o PR criado pelo Copilot para mostrar jogos relacionados na página de detalhes.

1. Volte ao repositório no GitHub.com.
2. Selecione **Pull Requests**.
3. Abra o PR com um título semelhante a **Show related games on the game details page**.
4. Selecione **Files changed** e revise o código.
5. Quando estiver tudo certo, volte à guia **Conversation**.
6. Verifique se há fluxos aguardando aprovação.
7. Se houver, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

8. Aguarde as verificações do aplicativo Astro serem concluídas.
9. **Opcional:** faça checkout da branch em seu Codespace para testar o recurso manualmente. Substitua `<branch-name>` pela branch criada pelo Copilot, por exemplo, `copilot/fix-8`:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

O Copilot criou o recurso de jogos relacionados. Você pode pedir novas alterações por meio de comentários na guia **Conversation**, como ajustar quantos jogos são exibidos ou adicionar cabeçalhos de comentário e comentários TSDoc. Observe que a tarefa foi atribuída antes de atualizar as instruções personalizadas.

## Revisar os recursos de acessibilidade

Por fim, vamos revisar os recursos de acessibilidade implementados pelo agente personalizado. Esse PR deve incluir o modo de alto contraste solicitado no Exercício 3 e o modo claro solicitado no painel central do Exercício 4.

1. Volte ao repositório no GitHub.com.
2. Selecione **Pull Requests**.
3. Abra o PR com um título semelhante a **Add high contrast mode to website**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Aguarde a conclusão.

4. Selecione **Files changed** para revisar o código.
5. Examine a implementação, principalmente:
   - os controles de interface para alternar os modos.
   - o armazenamento local das preferências.
   - as alterações de estilo dos modos de alto contraste e claro.
   - os atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - o código JavaScript/TypeScript que gerencia a troca de modos.

6. Volte à guia **Conversation** e role para baixo.
7. Verifique se há fluxos aguardando aprovação.
8. Se houver, selecione **Approve and run workflows**.

   ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

9. Aguarde as verificações do aplicativo Astro serem concluídas.
10. **Opcional:** faça checkout da branch no Codespace para testar os recursos. Substitua `<branch-name>` pela branch do Copilot:

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Inicie o aplicativo e teste os controles de alto contraste e modo claro no navegador. Confirme se as preferências continuam valendo após atualizar a página.

O agente personalizado de acessibilidade orientou a implementação de acordo com as boas práticas. Se encontrar problemas de acessibilidade, marque **@copilot** em um comentário e peça as alterações.

## Exercício opcional — continuar delegando

O agente de nuvem funciona melhor quando você pode delegar itens reais do backlog e trabalhar em outra coisa. Abra algumas issues e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse para apoiadores na página de detalhes do jogo.
- implementar paginação na página de listagem de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados.

## Resumo

Você concluiu o percurso do agente de nuvem. Neste workshop, você:

- **Examinou os arquivos de instruções personalizados do repositório** e observou o efeito deles no agente.
- **Atribuiu issues ao agente de nuvem** e acompanhou a configuração, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** de acessibilidade para implementar modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel central** para acompanhar e orientar uma sessão em andamento.
- **Revisou e iterou sobre pull requests**, marcando `@copilot` para solicitar alterações e aprovando fluxos.

## Revisão e próximos passos

Você concluiu o percurso do agente de nuvem. Os outros percursos complementam o que aprendeu:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com Plan e Autopilot, skills, um agente de QA personalizado, MCP do Playwright, Agent Merge e a opção `/delegate` para voltar ao agente de nuvem.

No seu repositório, experimente estas ideias:

- atribuir ao agente de nuvem uma issue de refatoração ou cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra stack.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Boas práticas para o fluxo de configuração do agente de nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente de nuvem][agent-firewall]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
