---
title: "Exercício 2 - Adicionar funcionalidades com o modo de agente do Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Até mesmo uma atualização simples em um aplicativo costuma envolver vários arquivos e tarefas, como executar testes. Em geral, uma pessoa desenvolvedora localiza os arquivos necessários, faz as alterações, executa os testes, depura problemas, descobre o arquivo que faltou atualizar, faz outra alteração e assim por diante.

É aí que entra o modo de agente do Copilot.

O modo de agente do Copilot trabalha de forma mais autônoma no ambiente de desenvolvimento integrado (IDE). Ele explora a estrutura existente do projeto, faz as atualizações necessárias, executa tarefas como testes e corrige automaticamente os problemas encontrados. Vamos explorar como usar o modo de agente para adicionar funcionalidades ao site.

> [!NOTE]
> Embora os nomes sejam parecidos, o modo de agente e o agente em nuvem oferecem experiências diferentes. O modo de agente trabalha no IDE, com ciclos rápidos de feedback e interação. O agente em nuvem atua como uma pessoa parceira de programação, trabalhando de forma assíncrona e interagindo por meio de issues e pull requests.

Neste exercício, você aprenderá como:

- o modo de agente do Copilot pode explorar o projeto, identificar arquivos relevantes e coordenar alterações.
- o modo de agente do GitHub Copilot pode implementar recursos na interface e na camada de dados.
- revisar alterações e testes gerados pelo modo de agente antes de fazer merge no código.

## Cenário

À medida que a lista de jogos cresce, você quer permitir que as pessoas filtrem por categoria e editora. No exercício anterior, você adicionou um auxiliar de editoras. Agora, concluirá o trabalho restante na camada de dados, na interface e nos testes com o modo de agente do Copilot.

## Executar o site da Tailspin Toys

Antes de fazer alterações, explore o site para entender as funcionalidades atuais.

O site é uma plataforma de financiamento coletivo para jogos de tabuleiro com temática de desenvolvimento. Ele permite listar jogos e exibir detalhes sobre eles. O site é um único aplicativo Astro que gera páginas estáticas em linguagem de marcação de hipertexto (HTML) durante a compilação. As páginas consultam diretamente um banco de dados SQLite local no frontmatter usando o Drizzle, um mapeador objeto-relacional (ORM). Não há uma interface de programação de aplicações (API) de back-end separada nem um framework de interface executado no cliente. Os auxiliares de acesso a dados reutilizáveis ficam em `src/lib/`; a interatividade é implementada com um pequeno elemento `<script>` do Astro, com escopo limitado e APIs padrão do Document Object Model (DOM).

### Iniciar o site

Para facilitar a execução do site, a tarefa `npm run dev` inicia o servidor de desenvolvimento Astro. Execute-a no GitHub Codespace:

1. Volte ao codespace. Você deverá estar na `main` depois do exercício anterior.
2. Abra um terminal no Codespace pressionando <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) ou <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux).
3. Crie e selecione uma branch para o trabalho de filtragem:

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. Inicie o site:

   ```bash
   npm run dev
   ```

   Quando o servidor Astro estiver pronto, você deverá ver um aviso com uma URL semelhante a:

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> Se uma caixa de diálogo solicitar a abertura de um navegador para `http://localhost:4321`, feche-a selecionando o **x**.

5. Abra o site mantendo pressionada a tecla <kbd>Command</kbd> (Mac) ou <kbd>Ctrl</kbd> (Windows/Linux) e selecionando o endereço do servidor Astro `http://localhost:4321` no terminal.

> [!NOTE]
> Em um codespace, selecionar o link `localhost` no terminal redireciona automaticamente para `https://<your-codespace-name>-4321.app.github.dev/`, um túnel privado para o servidor Web em execução no codespace.

### Explorar o site

Com o site em execução, explore suas funcionalidades principais:

- **Página inicial**: exibe uma lista de jogos de tabuleiro com títulos, imagens e descrições.
- **Página de detalhes do jogo**: ao selecionar um jogo, você verá uma página com mais informações, como título, descrição, editora e categoria.

## Explorar o backlog com o Copilot

> [!TIP]
> **Abrir o Copilot Chat**
>
> Antes de começar os exercícios a seguir, volte ao codespace, abra o painel Copilot Chat e selecione **New Chat** para iniciar uma conversa limpa. O modo e o modelo variam conforme o exercício; cada etapa especificará essas opções quando necessário.

A primeira versão do site funciona, mas queremos adicionar recursos. Comece revisando o backlog. A configuração do projeto a partir do modelo criou automaticamente um conjunto de issues. Peça ao GitHub Copilot que as liste.

1. No menu suspenso de agentes da visualização Chat, selecione **Agent**. Esse agente planeja e implementa alterações em vários arquivos, executa comandos no terminal e chama ferramentas.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

2. Na lista de modelos disponíveis, selecione **Claude Sonnet 4.5**.

> [!CAUTION]
> Este workshop não recomenda um modelo em particular. Durante sua criação, usamos Claude Sonnet 4.5 e o incluímos para tentar proporcionar uma experiência consistente. Como os modelos de linguagem de grande porte (LLMs) são probabilísticos, as sugestões podem ser diferentes das descritas aqui. Isso é normal e esperado.

> [!NOTE]
> Devido à natureza probabilística dos LLMs, o Copilot pode usar um comando MCP diferente, mas ainda deverá concluir a tarefa.

3. Peça ao Copilot que mostre o backlog:

   ```plaintext
   Mostre o backlog de itens do meu repositório do GitHub e ajude-me a priorizá-los com base no que será mais útil para as pessoas usuárias.
   ```

4. Selecione **Continue** para permitir que o comando liste todas as issues.
5. Revise a lista gerada.

Observe como o Copilot priorizou os itens de acordo com o que considera mais útil para as pessoas usuárias.

## Revisar os arquivos de instruções

Antes de pedir ao agente que gere código, revise o arquivo de instruções que fornecerá contexto. Você usará o arquivo de [interface do usuário (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md), que explica como adicionar funcionalidades ao site.

1. No codespace, abra `.github/instructions/ui.instructions.md`.
2. Examine as orientações gerais para adicionar funcionalidades, incluindo:
   - uma visão geral da arquitetura.
   - princípios de design de componentes, capacidade de teste e acessibilidade.
   - links para instruções específicas de tipos de arquivo, como Astro, Tailwind CSS e Drizzle.

> [!TIP]
> Os arquivos de instruções podem fazer referência a outros arquivos de instruções e a arquivos do projeto. Os caminhos são relativos ao local do arquivo de instruções. Assim, é possível reutilizar orientações, dividir conjuntos grandes em arquivos menores e incluir exemplos e modelos.

## Implementar a filtragem

Para concluir a filtragem, serão necessárias pelo menos três alterações no aplicativo:

- adicionar ou ajustar a lógica de filtragem na camada de dados (`src/lib/`).
- adicionar ou atualizar testes do comportamento de filtragem.
- atualizar a página de listagem de jogos para incluir a interface de filtragem.

Os testes também precisam ser executados e aprovados antes do merge. O modo de agente do Copilot pode executar essas tarefas.

1. Continue a conversa atual com o Copilot ou selecione **New Chat** para iniciar outra.
2. Selecione **Add Context**, **Instructions** e **ui** para adicionar o arquivo de instruções.

   ![Exemplo de seleção do arquivo de instruções de UI](../../../_images/ex3-select-instructions-file.png)

3. No menu suspenso de agentes da visualização Chat, confirme que **Agent** continua selecionado.

   ![Seletor de agente na visualização Chat](../../../_images/shared-chat-mode-selector.png)

4. Confirme que **Claude Sonnet 4.5** continua selecionado como modelo.
5. Peça ao Copilot que implemente a funcionalidade conforme a issue relacionada no backlog:

   ```plaintext
   Atualize o site para incluir filtros por editora e categoria de acordo com os requisitos da issue relacionada no backlog. Já existe um auxiliar de editoras do exercício anterior; preserve e refine esse trabalho conforme necessário enquanto conclui as alterações restantes na camada de dados, na interface e nos testes. Confirme que todos os testes foram aprovados antes de concluir. O servidor já está em execução, então não é necessário iniciá-lo.
   ```

6. Acompanhe o Copilot enquanto ele explora o projeto, localiza os arquivos relevantes — camada de dados, interface e testes —, faz alterações e executa os testes.

   ![Copilot explorando os arquivos do projeto](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> O Copilot executará várias tarefas, como explorar o projeto, alterar arquivos e rodar testes. Isso pode levar alguns minutos, dependendo da tarefa e do código. Enquanto ele trabalha, talvez apareçam os controles **Keep** e **Undo** no editor. Aguarde até que ele termine para aceitar ou desfazer as alterações.

7. Quando solicitado pelo Copilot, selecione **Continue** para executar os testes.

   ![Caixa de diálogo do Copilot Chat solicitando autorização para executar testes](../../../_images/ex3-agent-mode-run-tests.png)

8. Alguns testes podem falhar durante o processo. O Copilot alterna entre geração de código e testes até concluir a tarefa e não detectar erros.

   ![Sessão concluída do modo de agente no Copilot Chat](../../../_images/ex3-agent-mode-proposed-changes.png)

9. Examine o código gerado para identificar possíveis problemas.

> [!CAUTION]
> Sempre revise o código gerado pelo Copilot ou por outras ferramentas de IA.

10. Volte ao navegador em que o site está em execução e explore o recurso novo.
11. Depois de confirmar que tudo funciona e revisar o código, selecione **Keep** na janela do Copilot Chat.

## Resumo e próximos passos

Você usou o modo de agente do GitHub Copilot para adicionar funcionalidades ao site da Tailspin Toys. Nesta lição, você:

- viu como o modo de agente do GitHub Copilot pode implementar recursos na interface e na camada de dados.
- explorou como o modo de agente pode localizar arquivos relevantes e coordenar alterações.
- revisou as alterações e os testes gerados antes do merge.

Agora, vamos [testar o recurso com o servidor MCP do Playwright][next-lesson] e abrir um pull request.

### Exercício extra de exploração — implementar paginação

À medida que a lista de jogos cresce, será necessário implementar paginação. Use as habilidades deste exercício e peça ao Copilot que atualize o site. Considere:

- seguir as práticas existentes, inclusive o uso dos arquivos de instruções.
- decidir como implementar a paginação: permitir a escolha do tamanho da página ou defini-lo no código.
- incluir no prompt as orientações necessárias para obter a implementação desejada.
- iterar com o GitHub Copilot, pedindo alterações e fornecendo contexto. Esse é o fluxo normal de trabalho com o Copilot.

## Recursos

- [Modos ask, edit e agent do Copilot: o que fazem e quando usar cada um][choose-mode]
- [Modo de agente no VS Code][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
