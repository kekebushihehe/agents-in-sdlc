---
title: "Exercício 5 - Monitorar e gerenciar agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Agentes personalizados][previous-lesson] |
|:--|

No modo de agente, o GitHub Copilot trabalha de forma autônoma: explora o código, propõe alterações, edita arquivos e executa comandos. Como esse trabalho acontece em tempo real na sua máquina, o Copilot Chat no VS Code mostra cada chamada de ferramenta e edição de arquivo. Você pode revisar diferenças no chat, aceitar ou rejeitar alterações e orientar a conversa com prompts de acompanhamento sem sair do editor.

Neste exercício, você vai:

- monitorar em tempo real o trabalho do agente de acessibilidade.
- revisar e aceitar as edições propostas nos arquivos.
- orientar o agente durante a sessão para adicionar um controle do modo claro junto ao modo de alto contraste.

## Cenário

O agente personalizado de acessibilidade usado no [Exercício 4][previous-lesson] está implementando o modo de alto contraste. Enquanto o trabalho continua, a equipe de design fez outra solicitação: também quer um controle do modo claro para as pessoas que consideram o tema padrão escuro demais. Em vez de começar uma conversa do zero, você orientará a sessão existente.

## Monitorar o progresso do Copilot

A visualização Copilot Chat mostra cada etapa do agente: os arquivos explorados, os comandos executados e as edições propostas. Cada edição aparece no chat com uma diferença e os controles **Keep** e **Undo**.

1. Volte à visualização Copilot Chat do Exercício 4.
2. Enquanto o agente trabalha, percorra a conversa para acompanhar as chamadas de ferramentas, leituras e edições de arquivos e comandos do terminal.
3. Quando o agente propuser uma edição, a diferença aparecerá no chat:
   - Selecione **Keep** para aceitar a alteração.
   - Selecione **Undo** para desfazê-la e permitir que o agente tente novamente.
4. Abra o indicador **Files changed** na parte superior do chat para ver o conjunto completo de arquivos alterados durante a sessão.

> [!NOTE]
> Se uma chamada de ferramenta solicitar permissão, por exemplo, para executar um comando do terminal, o agente pausará e exibirá uma solicitação **Allow**. Aprove apenas comandos que você considere seguros.

5. Aguarde o agente concluir o modo de alto contraste. Ele deverá exibir uma mensagem final que resume as alterações.

## Orientar o agente para adicionar o modo claro

Com o modo de alto contraste pronto, amplie a mesma conversa para incluir um controle do modo claro. Como você continuará na mesma sessão, o agente manterá o contexto do trabalho anterior, incluindo os componentes alterados, a abordagem de estilo e o padrão de armazenamento local.

1. No campo de entrada do Copilot Chat, envie este prompt de acompanhamento:

    ```plaintext
    Ótimo trabalho! Agora, adicione um controle do modo claro ao lado do controle de alto contraste. O modo claro deve seguir o mesmo padrão: persistir no armazenamento local e estar disponível na mesma área da interface. A pessoa usuária deve poder ativar o alto contraste e o modo claro de forma independente.
    ```

2. O agente continuará o trabalho, explorando as alterações existentes e propondo o modo claro.
3. Revise cada nova edição e use **Keep** ou **Undo**.
4. Se o agente seguir uma direção indesejada, por exemplo, tentar refatorar os controles de modo de uma forma que você não quer, interrompa-o com o botão **Stop** ao lado do campo de entrada e envie uma mensagem de esclarecimento.

> [!TIP]
> Orientar uma sessão em andamento é mais rápido do que começar de novo. Peça ajustes ("simplifique as folhas de estilo em cascata (CSS) — use variáveis CSS em vez de duplicar seletores"), corrija a abordagem ("não adicione um terceiro controle; integre o modo claro ao painel de configurações existente") ou amplie o escopo ("adicione atalhos de teclado para os dois controles").

## Revisar o conjunto de alterações

Antes de fazer commit, revise rapidamente tudo o que o agente alterou.

1. Abra a visualização **Source Control** no VS Code.
2. Revise a lista completa de arquivos alterados. Você deverá ver atualizações nos componentes Astro, nos estilos e em testes relacionados.
3. Abra alguns arquivos alterados e examine as diferenças. Confirme se a implementação reflete as práticas de acessibilidade do agente personalizado: atributos de Accessible Rich Internet Applications (ARIA), navegação pelo teclado, marcação semântica em linguagem de marcação de hipertexto (HTML) e persistência com armazenamento local.
4. Quando estiver satisfeito, prepare e faça commit das alterações no painel Source Control. Você as publicará na [próxima lição][next-lesson].

## Resumo e próximos passos

Você usou o modo de agente do Copilot Chat para monitorar e orientar uma sessão em andamento: revisou cada edição, aceitou ou rejeitou alterações e enviou prompts de acompanhamento para ampliar o trabalho sem perder o contexto.

Nesta lição, você:

- monitorou o trabalho do agente em tempo real.
- aceitou e rejeitou edições individuais.
- orientou uma sessão em andamento com prompts de acompanhamento.

Na [próxima lição][next-lesson], você publicará o trabalho de acessibilidade como um pull request e revisará as alterações produzidas pelo agente personalizado local durante o laboratório.

## Recursos

- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
