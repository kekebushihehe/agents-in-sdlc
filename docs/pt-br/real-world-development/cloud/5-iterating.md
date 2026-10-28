---
title: "Exercício 5: Iterar o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você pediu ao Copilot que documentasse o código, criasse o recurso de jogos relacionados para a equipe de design testar e implementasse recursos de acessibilidade, incluindo alternâncias para os modos de alto contraste e claro. Agora, examine as alterações e, se necessário, dê feedback ao Copilot para aprimorar o trabalho.

### Cenário

Os fundamentos de design de software e DevOps não mudam com a inclusão de IA generativa. Sempre revise o código gerado e siga o processo normal de DevOps. Com isso em mente, revise as sugestões do GitHub Copilot para a documentação, o recurso de jogos relacionados e os recursos de acessibilidade antes de habilitar a revisão para o restante da equipe.

## Segurança e agente em nuvem do GitHub Copilot

Como o agente em nuvem do Copilot executa tarefas de forma assíncrona e sem supervisão direta, há restrições de segurança para proteger o ambiente. Entre elas:

- O Copilot tem acesso de leitura ao repositório e acesso de gravação **somente** à branch usada para o código.
- O agente em nuvem é executado no GitHub Actions, em um ambiente isolado e temporário.
- Uma pessoa precisa aprovar a execução de qualquer workflow do GitHub Actions.
- [O acesso a recursos externos é limitado por padrão](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall), incluindo servidores MCP.

## Revisar a documentação gerada

Comece examinando o primeiro pull request (PR) criado pelo agente em nuvem do GitHub Copilot: o PR que adiciona documentação ao código. Use a interface padrão de PR no GitHub.com.

> [!NOTE]
> Ao abrir o PR, você poderá ver um aviso de que o GitHub Copilot foi bloqueado por um firewall. Isso é **esperado**, pois o acesso a recursos externos, incluindo servidores MCP, é limitado por padrão. Se quiser, você pode [personalizar ou desabilitar o firewall do agente em nuvem][agent-firewall].

1. Acesse seu repositório no github.com.
2. Selecione **Pull Requests** para abrir a lista de PRs.
3. Abra o PR com um título semelhante a **Add missing documentation**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho. Isso pode levar alguns minutos.

4. Quando o PR estiver pronto, selecione a guia **Files changed** e revise as alterações.

    ![Guia Files changed](../../../_images/shared-pr-files-changed.png)

5. Examine o código atualizado, que inclui comentários TSDoc e outra documentação. As alterações exatas podem variar. Procure comentários TSDoc, convenções de TypeScript e cabeçalhos de comentário. Essas orientações vêm dos arquivos de instruções personalizadas examinados no Exercício 2.
6. Depois da revisão, volte à guia **Conversation** e role para baixo.
7. Procure um indicador de que alguns workflows aguardam aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão ser adicionados à seção de verificações do PR. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.

## Solicitar alterações ao GitHub Copilot

O trabalho com o Copilot em um pull request não é unilateral. Você pode mencioná-lo em comentários do PR ou em comentários de linha no código. O Copilot verá esses comentários e iniciará outra sessão para atendê-los. Como os resultados são não determinísticos, não há um texto único a recomendar. Algumas ideias de solicitações:

- adicionar cabeçalhos de comentário no início de cada arquivo de código para explicar sua finalidade.
- adicionar comentários TSDoc a arquivos TypeScript e Astro.
- criar um README que descreva a estrutura do aplicativo Astro.

1. Adicione um comentário solicitando uma alteração à documentação gerada e mencione **@copilot**, como faria com outra pessoa. Use uma das ideias acima ou peça outra melhoria de documentação para o código.
2. Selecione **View Session** para acompanhar o trabalho do Copilot. Observe que ele inicia uma nova sessão para fazer as atualizações.
3. Selecione **Back to pull request** para voltar ao PR.

    ![Voltar ao pull request](../../../_images/ex7-back-to-pr.png)

4. Quando o Copilot concluir as alterações, deverá aparecer um novo commit no PR.
5. Selecione **Files changed** para revisar as alterações.

Você pode continuar iterando até ficar satisfeito. Depois, marque o PR como pronto para revisão e faça o merge na branch principal.

![Marcar o PR como pronto para revisão](../../../_images/ex7-ready-for-review.png)

## Revisar o recurso de jogos relacionados

Agora, volte ao PR criado pelo Copilot para resolver a issue de jogos relacionados na página de detalhes.

1. Acesse seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Exibir jogos relacionados na página de detalhes do jogo**.
4. Selecione a guia **Files changed** para revisar o código gerado.
5. Depois da revisão, volte à guia **Conversation** e role para baixo.
6. Procure um indicador de que alguns workflows aguardam aprovação.
7. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

8. Os workflows deverão ser adicionados à seção de verificações do PR. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
9. **Opcional:** alterne para esta branch no Codespace para testar manualmente o recurso de jogos relacionados. Acesse o Codespace, abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot, por exemplo, `copilot/fix-8`:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

O Copilot criou o recurso de jogos relacionados. Como antes, você pode iterar e solicitar alterações, como ajustar a quantidade de jogos relacionados ou adicionar cabeçalhos de comentário e comentários TSDoc. Lembre-se: essa issue foi atribuída **antes** da atualização das instruções personalizadas. Para pedir alterações, adicione um comentário na guia **Conversation**; o Copilot o verá e iniciará outra sessão.

## Revisar os recursos de acessibilidade

Por fim, revise os recursos de acessibilidade implementados pelo agente personalizado. Este PR deve incluir o modo de alto contraste solicitado no Exercício 3 e o modo claro solicitado no painel central no Exercício 4.

1. Acesse seu repositório no GitHub.com.
2. Selecione a guia **Pull Requests**.
3. Abra o PR com um título semelhante a **Adicionar modo de alto contraste ao site**.

> [!NOTE]
> Se o Copilot ainda estiver trabalhando, o PR terá a indicação **[WIP]**. Nesse caso, aguarde a conclusão do trabalho, que pode levar alguns minutos.

4. Selecione **Files changed** para revisar o código gerado.
5. Revise a implementação, com atenção especial a:
   - controles da interface para alternar entre os modos.
   - uso do armazenamento local para persistir as preferências.
   - alterações de CSS ou estilo para os modos de alto contraste e claro.
   - atributos de Accessible Rich Internet Applications (ARIA), navegação pelo teclado e outros recursos de acessibilidade.
   - código JavaScript/TypeScript que gerencia a alternância entre modos.
6. Depois da revisão, volte à guia **Conversation** e role para baixo.
7. Procure um indicador de que alguns workflows aguardam aprovação.
8. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

9. Os workflows deverão ser adicionados à seção de verificações do PR. Se tudo correr bem, as verificações do projeto para o aplicativo Astro serão aprovadas. Isso pode levar alguns minutos.
10. **Opcional:** alterne para esta branch no Codespace para testar manualmente os recursos de acessibilidade. Acesse o Codespace, abra o terminal e execute os comandos a seguir, substituindo `<branch-name>` pelo nome da branch criada pelo Copilot:

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    Em seguida, inicie o aplicativo e teste os controles dos modos de alto contraste e claro no navegador. Confirme se eles funcionam como esperado e se as preferências continuam ativas depois de recarregar a página.

Observe como o agente personalizado de acessibilidade orientou a implementação de acordo com as práticas recomendadas. Se encontrar problemas ou oportunidades de melhoria de acessibilidade, mencione **@copilot** em um comentário para solicitar alterações, como nos PRs anteriores.

## Exercício opcional — continuar delegando

O agente em nuvem é útil para tarefas reais do backlog que você pode delegar enquanto trabalha em outra atividade. Para praticar, crie algumas issues no repositório e atribua-as ao Copilot. Algumas ideias:

- criar um formulário de interesse de apoiadores na página de detalhes do jogo.
- implementar paginação na página de listagem de jogos.
- adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados.

## Resumo

Você concluiu o percurso do agente em nuvem. Nestas lições, você:

- **Inspecionou os arquivos de instruções personalizados incluídos no repositório** para observar mais tarde o efeito deles no trabalho do agente em nuvem.
- **Atribuiu issues ao agente em nuvem do Copilot** e acompanhou a configuração do ambiente, o planejamento e a execução assíncrona.
- **Criou e usou um agente personalizado** para acessibilidade, adicionando alternâncias para os modos de alto contraste e claro.
- **Usou a página de agentes do Copilot como painel central** para monitorar e orientar a sessão de acessibilidade.
- **Revisou e iterou os pull requests do agente em nuvem**, mencionando `@copilot` para solicitar alterações e aprovando workflows.

## Revisão e próximos passos

Você concluiu o percurso do agente em nuvem. Para continuar explorando, os outros percursos complementam o que você praticou:

- 🖥️ **[Percurso do VS Code](../../vscode/)** — explore o modo de agente do Copilot Chat e a integração com MCP diretamente no IDE.
- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills de agente, um agente de QA personalizado, Playwright MCP, Agent Merge e uma integração opcional de `/delegate` com o agente em nuvem.

No seu repositório, experimente estas ideias:

- atribuir ao agente em nuvem uma issue de refatoração ou cobertura de testes.
- criar um agente personalizado para outro domínio.
- configurar `copilot-setup-steps.yml` para outra pilha tecnológica.

## Recursos

- [GitHub Copilot][github-copilot]
- [Sobre agentes do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente em nuvem][cloud-agent-best-practices]
- [Configurar o firewall do agente em nuvem do Copilot][agent-firewall]

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
