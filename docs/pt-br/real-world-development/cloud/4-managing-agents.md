---
title: "Exercício 4 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos exercícios anteriores, você pediu ao agente em nuvem do Copilot que executasse três tarefas para melhorar a experiência e adicionar funcionalidades. Embora o agente trabalhe de forma assíncrona e autônoma, ainda é importante monitorar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente em nuvem, incluindo a [página de agentes][agents-page] no GitHub.com. Nesse painel central, você pode ver todas as tarefas do agente com pull requests (PRs) abertos, examinar as operações realizadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para monitorar tarefas do agente em nuvem.
- orientar uma sessão em andamento para solicitar funcionalidades adicionais.

## Cenário

Depois de atribuir ao agente a implementação do modo de alto contraste, a equipe percebeu que seria um bom momento para adicionar também um modo claro. Como o trabalho já envolve atualizar o estilo do site e adicionar controles de alternância, faz sentido incluir o novo modo. Você orientará o agente para garantir que ele implemente o modo claro junto com o alto contraste.

## Revisar as tarefas do agente em nuvem do Copilot

Veja o estado atual das tarefas atribuídas ao agente em nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação do código.
   - gerar APIs para modificar jogos.
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em andamento e revise as operações feitas pelo Copilot. Elas podem incluir:
   - obter o código do repositório.
   - criar o ambiente de trabalho do Copilot.
   - configurar servidores MCP.
   - executar etapas para concluir a tarefa.

> [!NOTE]
> As etapas exibidas variam de acordo com o estado do trabalho e a abordagem adotada.

4. Observe também o painel de pull request à direita, que permite acompanhar o PR e os arquivos alterados.

## Orientar o agente em nuvem

Agora que você conhece as tarefas ativas, peça ao Copilot para incluir o modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à implementação do modo de alto contraste. O título exato depende do nome escolhido pelo Copilot e do estado do trabalho.

    ![Sessão de acessibilidade no painel central](../../../_images/ex6-accessibility-session.png)

2. Aguarde alguns minutos até que a sessão indique que a configuração foi concluída e o trabalho começou. Você verá mensagens semelhantes às exibidas no exemplo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, adicione este prompt:

    ```plaintext
    Enquanto trabalhamos no modo de alto contraste, adicione também um modo claro. Deve haver um controle para esse modo, para que as pessoas possam selecionar a opção de exibição desejada.
    ```

    ![Tarefa do agente em nuvem na página de agentes com a caixa de orientação da sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe como o Copilot reconhece a solicitação e a incorpora ao trabalho.

## Aguardar o trabalho do Copilot

Como antes, o Copilot continuará trabalhando na tarefa atualizada. Ele incorporará a nova solicitação depois de concluir a etapa em andamento no momento em que você enviou a mensagem.

Esse processo pode levar alguns minutos. Aproveite o tempo para refletir sobre o que você já aprendeu e explorou.

## Resumo e próximos passos

Esta lição apresentou a página de agentes do Copilot, o ponto central para monitorar e orientar tarefas do agente em nuvem do GitHub Copilot. Nela, você pode acompanhar tarefas ativas e concluídas, revisar o trabalho e redirecionar sessões em andamento.

Você explorou:

- o painel de agentes e a página de agentes para monitorar tarefas do agente em nuvem.
- como redirecionar uma sessão em andamento para solicitar funcionalidades adicionais.

Enquanto o Copilot conclui as funcionalidades de acessibilidade, você pode [iterar sobre os pull requests que ele criou][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
