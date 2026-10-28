---
title: "Lição 2 - Adicionar avaliações por estrelas: uma melhoria rápida"
description: "Use o Copilot CLI para fazer uma pequena alteração nos cards de jogos, revise-a em um navegador encaminhado e integre-a como seu primeiro pull request."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Agora que você instalou o Copilot CLI e experimentou uma conversa, é hora de fazer sua primeira alteração no projeto. Ela será pequena: os jogos já têm uma avaliação por estrelas nos dados, mas os cards da página inicial ainda não a exibem. Você pedirá ao agente que mostre a avaliação, revisará a alteração e fará o merge do seu primeiro pull request.

Nesta lição, você vai:

- iniciar uma conversa focada do Copilot em uma branch de recurso.
- pedir ao agente que faça uma pequena alteração no projeto.
- revisar a alteração com `/diff`.
- executar o aplicativo e confirmar a alteração em um navegador encaminhado.
- abrir e fazer merge do seu primeiro pull request.

## Cenário

Cada jogo da Tailspin Toys pode ter uma avaliação por estrelas, que já aparece na página de detalhes. Os cards da página inicial, porém, mostram apenas o título, a categoria, a editora e a descrição. Como aquecimento, peça ao agente para exibir a avaliação existente em cada card: uma alteração pequena e independente, ideal para sua primeira sessão.

## Como funciona uma conversa

Uma **conversa** é o espaço em que você trabalha com o Copilot CLI em uma tarefa. Diferentemente do aplicativo GitHub Copilot, uma conversa normal da CLI usa o repositório e a branch atualmente selecionados no terminal, em vez de criar uma worktree dedicada. As conversas salvas permitem retomar a mesma discussão mais tarde, enquanto os arquivos e a branch continuam sendo o estado comum do Git no disco.

Em uma conversa, você verá três coisas: seus prompts e as respostas do agente; a atividade de ferramentas do agente enquanto ele explora e edita arquivos; e as alterações que você pode inspecionar com `/diff`.

## Iniciar uma conversa e solicitar a alteração

Inicie uma conversa para começar a implementar o recurso.

1. Retorne ao seu Codespace.
2. Se o terminal não estiver aberto, pressione <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Se o Copilot ainda não estiver em execução, use este comando:

   ```bash
   copilot --yolo
   ```

4. Inicie uma nova sessão com o comando de barra `/new` e pressione <kbd>Enter</kbd>.
5. Use este prompt para solicitar a alteração:

   ```plaintext
   Exiba o `starRating` de cada jogo como uma avaliação de até cinco estrelas nos cards da página de listagem. Se a avaliação for `null`, exiba "No rating yet". Mantenha o layout dos cards, adicione testes e execute as verificações relevantes.
   ```

O Copilot explorará o projeto, localizará os arquivos usados para exibir os detalhes dos jogos e criará o código necessário. Você adicionou um recurso com o Copilot CLI.

## Revisar as diferenças

Toda alteração gerada por IA merece uma revisão antes do merge, mesmo que seja pequena. Vamos examinar as alterações no próprio Copilot CLI.

1. Digite `/diff` e inspecione cada arquivo alterado.
2. Confirme se o card do jogo exibe a avaliação numérica quando ela está disponível e `No rating yet` quando `starRating` é `null`.
3. Confirme se os testes abrangem os dois casos.
4. Revise os resultados das verificações executadas pelo Copilot e peça que ele corrija eventuais falhas.
5. Quando terminar a revisão, pressione <kbd>Esc</kbd> para sair da tela de diferenças.

> [!NOTE]
> Como todas as ferramentas de IA generativa, o Copilot é probabilístico, não determinístico. Seu código pode ser diferente. Revise o comportamento em vez de esperar uma implementação idêntica.

## Verificar as alterações

Não basta ler o código e presumir que funciona. Peça ao Copilot para iniciar o site e examine a interface do usuário (UI) atualizada no navegador encaminhado pelo Codespaces.

1. Peça ao Copilot para iniciar o aplicativo:

   ```plaintext
   Inicie o aplicativo para que eu possa verificar no navegador a alteração das avaliações por estrelas. Informe a URL e mantenha o servidor em execução.
   ```

2. Quando o Codespaces informar que a porta `4321` está disponível, selecione **Open in Browser**.
3. Confirme se os cards dos jogos exibem as avaliações de um total de cinco estrelas.
4. Volte ao Copilot e peça que interrompa o servidor iniciado:

   ```plaintext
   Interrompa o servidor de desenvolvimento que você iniciou.
   ```

## Abrir e fazer merge do primeiro pull request

Agora que o recurso está pronto, crie um pull request (PR) para integrar o novo código ao projeto.

1. Peça ao agente padrão para fazer o commit da alteração:

   ```plaintext
   Faça commit das alterações revisadas nas avaliações por estrelas com uma mensagem de commit adequada.
   ```

2. Digite `/pr create`. Ao criar o PR, o Copilot CLI enviará o commit existente e exibirá a URL do PR.
3. Abra o PR mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando a URL exibida pelo Copilot CLI.
4. Revise os arquivos alterados e as verificações.
5. Quando estiver tudo pronto, selecione **Merge pull request** e confirme o merge.
6. Volte ao Codespace e saia do Copilot CLI com `/exit`.
7. Atualize sua `main` local:

   ```bash
   git checkout main
   git pull
   ```

## Resumo e próximos passos

Parabéns! Você entregou sua primeira alteração usando o GitHub Copilot CLI. Nesta lição, você:

- iniciou uma conversa focada do Copilot em uma branch de recurso.
- orientou o agente a fazer uma pequena alteração nos cards dos jogos.
- revisou a alteração com `/diff`.
- executou o aplicativo e confirmou a avaliação por estrelas em um navegador encaminhado.
- abriu e fez merge do seu primeiro pull request.

Agora, você [começará pela issue de filtragem e usará os modos Plan e Autopilot][next-lesson] para desenvolver um recurso maior.

## Recursos

- [Sobre o GitHub Copilot CLI][about-copilot-cli]
- [Referência de comandos do Copilot CLI][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
