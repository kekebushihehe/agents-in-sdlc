---
title: "Exercício 2 - Agente em nuvem do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← Lição anterior: Instruções personalizadas][previous-lesson] |
|:--|

É provável que poucas organizações não enfrentem problemas com dívida técnica. Ela pode incluir problemas de segurança não resolvidos, código legado que precisa de atualizações ou solicitações de recursos que ficaram no backlog por falta de tempo. O agente em nuvem do GitHub Copilot foi desenvolvido para tarefas como atualizar código e adicionar funcionalidades de forma autônoma. Quando termina, ele cria um pull request (PR) de rascunho para acompanhar o progresso, permitindo que uma pessoa desenvolvedora revise o trabalho. Assim, é possível delegar tarefas repetitivas e se concentrar em outras atividades.

Neste exercício, você explorará como:

- personalizar o ambiente de geração de código.
- garantir que as operações sejam executadas com segurança.
- criar issues com escopo claro.
- atribuir issues ao Copilot.

## Cenários

A Tailspin Toys tem dívida técnica que deseja resolver. As equipes terceirizadas inicialmente contratadas para criar o site deixaram a documentação em estado inadequado: ela está totalmente ausente. Como primeiro passo, a equipe quer adicionar comentários TSDoc às funções exportadas do aplicativo.

Além disso, a equipe de design quer melhorar a descoberta de jogos. A página de detalhes de cada jogo deve mostrar títulos relacionados — outros jogos da mesma categoria — para que as pessoas possam continuar navegando. A equipe ainda não precisa de uma implementação refinada; quer algo que possa usar em testes de aceitação da experiência do usuário (UX). O trabalho está bloqueado, mas há issues de prioridade maior no momento.

Esses são exemplos de tarefas que podem ser adiadas e que são boas candidatas para o agente em nuvem do Copilot. Ele pode trabalhar de forma assíncrona enquanto você se concentra em outras tarefas e, depois, revisar o resultado.

## Apresentação do agente em nuvem do GitHub Copilot

O [agente em nuvem do GitHub Copilot](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) pode executar tarefas em segundo plano de forma semelhante a uma pessoa desenvolvedora. Uma possibilidade é [atribuir uma issue do GitHub ao Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions). Depois da atribuição, o Copilot cria um pull request (PR) de rascunho para acompanhar o progresso, configura um ambiente e começa a trabalhar. Você pode examinar a sessão durante a execução ou depois que ela terminar. Quando a solução estiver pronta para revisão, o Copilot marcará você no PR.

## A importância de instruções com escopo bem definido

O GitHub Copilot não faz mágica: não é possível pular a etapa de pensar. Mesmo tarefas aparentemente simples podem ter complexidades escondidas. Por isso, [defina com cuidado o escopo das tarefas do agente em nuvem][cloud-agent-best-practices]. Trate-o como um parceiro de programação com IA: trabalhe por etapas, aprenda, experimente e adapte o processo. Os fundamentos do desenvolvimento de software não mudam com a inclusão de IA generativa.

## Instruções personalizadas neste repositório

Exercícios anteriores apresentaram instruções personalizadas e como elas orientam o Copilot. Se você já concluiu o exercício prático sobre instruções personalizadas, viu `.github/copilot-instructions.md` em ação; caso contrário, este é um bom momento para examiná-lo. Antes de atribuir trabalho ao agente em nuvem do Copilot, leia os arquivos de instruções existentes neste repositório sem alterá-los. Assim, você poderá observar o efeito deles mais tarde.

Abra os arquivos a seguir na interface Web do GitHub ou em um codespace que já esteja em execução:

- `.github/copilot-instructions.md`: examine a seção **Code standards**, especialmente as convenções de TypeScript e as expectativas para comentários TSDoc.
- `.github/instructions/unit-tests.instructions.md`: observe o frontmatter `applyTo`, que aplica as instruções a arquivos `**/*.test.ts`.

Quando atribuir a issue `Código sem documentação` ao agente em nuvem, observe os comentários TSDoc, as convenções de TypeScript e os cabeçalhos de comentário no PR resultante. Essas orientações vêm dos arquivos de instruções. Você revisará os PRs novamente em um exercício posterior.

## Configurar o ambiente de desenvolvimento do agente em nuvem

A criação de código normalmente exige um ambiente específico e scripts de configuração para instalar dependências e preparar o projeto. O mesmo vale ao atribuir tarefas ao Copilot, que trabalha de maneira semelhante a uma pessoa engenheira de software.

O agente em nuvem usa o [GitHub Actions][github-actions] para configurar o ambiente de trabalho. Você pode personalizá-lo com um [workflow de configuração especial][setup-workflow], definido em `.github/workflows/copilot-setup-steps.yml`, que é executado antes do início do trabalho. Isso disponibiliza ferramentas e dependências necessárias. O arquivo já foi configurado para este laboratório. Ele prepara o Node.js, as dependências do projeto, os navegadores do Playwright para testes de ponta a ponta e o banco de dados SQLite migrado e preenchido para o aplicativo Astro:

```yaml
name: "Copilot Setup Steps"

# Permite testar as etapas de configuração na guia "Actions" do repositório
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Defina as permissões mínimas necessárias para *suas etapas*. O Copilot receberá um token próprio para as operações dele.
    permissions:
      # Se quiser clonar o repositório como parte da configuração, por exemplo, para instalar dependências, você precisará da permissão `contents: read`. Se não clonar o repositório nessas etapas, o Copilot fará isso automaticamente depois que elas forem concluídas.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Configuração do front-end/aplicativo - Node.js (todo o aplicativo agora usa Astro + Drizzle/libSQL)
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

      # Migra e preenche o banco de dados SQLite local para que builds e testes tenham dados.
      - name: Set up the database
        run: npm run db:setup
```

Esse arquivo se parece com qualquer outro workflow do GitHub, mas tem alguns pontos importantes:

- Ele contém um único trabalho chamado `copilot-setup-steps`, executado no GitHub Actions antes de o Copilot começar a trabalhar no PR.
- O gatilho `workflow_dispatch` permite executar o workflow manualmente na guia **Actions** do repositório. Isso é útil para verificar se ele funciona antes de aguardar a execução pelo Copilot.

## Adicionar documentação

Embora todos reconheçam a importância da documentação, muitos projetos têm informações desatualizadas ou não têm documentação. Essa dívida técnica reduz a produtividade e dificulta a manutenção do código e a integração de novas pessoas à equipe. O Copilot pode ajudar a criar documentação, por isso esta é uma issue adequada para atribuir ao agente em nuvem. Em um exercício futuro, você voltará para revisar o trabalho.

1. Em uma nova guia do navegador, acesse seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de criação de issue.
4. Selecione **Blank issue**.
5. Defina **Title** como `Código sem documentação`.
6. Defina **Description** como:

    ```plaintext
    Nossa organização exige que funções e métodos tenham comentários TSDoc quando eles forem úteis. Infelizmente, as atualizações recentes não seguiram esse padrão. Precisamos atualizar o código existente para incluir comentários de documentação quando eles esclarecerem o comportamento.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

    ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.
10. Selecione a guia **Pull Requests**.
11. Abra o PR recém-criado, com um título semelhante a `[WIP]: Código sem documentação`. Se ele ainda não aparecer, aguarde um ou dois minutos e atualize a página.
12. Depois de alguns minutos, o Copilot deverá criar uma lista de tarefas.

> [!NOTE]
> A lista de tarefas pode levar alguns minutos para aparecer no PR. O Copilot está configurando o ambiente com o workflow descrito anteriormente, analisando o projeto e definindo uma abordagem.

13. Revise a lista e as tarefas que serão executadas.
14. Role a linha do tempo do PR para baixo e procure uma atualização indicando que o Copilot começou a trabalhar na issue.
15. Selecione o botão **View session**.

    ![Visualização da sessão do Copilot](../../../_images/ex4-view-session.png)

> [!CAUTION]
> Talvez seja necessário atualizar a janela para ver o indicador atualizado.

16. Observe a sessão em andamento e acompanhe como o Copilot resolve o problema: ele explora o código, entende o estado atual, planeja a abordagem e cria código.

O processo pode levar alguns minutos. Um dos objetivos do agente em nuvem é executar tarefas de forma assíncrona, liberando você para trabalhar em outras atividades. Aproveite esse recurso atribuindo outra tarefa ao agente e, em seguida, desenvolvendo uma funcionalidade no aplicativo.

## Adicionar uma seção de jogos relacionados à página de detalhes

Uma das vantagens do agente em nuvem do GitHub Copilot é dividir o trabalho: você pode se concentrar em uma tarefa enquanto o agente cuida de outra. Mesmo que a seção de jogos relacionados não demore muito, delegá-la libera tempo para outras atividades. Vamos atribuí-la ao agente em nuvem.

1. Volte ao seu repositório no github.com.
2. Selecione a guia **Issues**.
3. Selecione **New issue** para abrir a caixa de criação de issue.
4. Selecione **Blank issue**.
5. Defina **Title** como `Exibir jogos relacionados na página de detalhes do jogo`.
6. Defina **Description** como:

    ```markdown
    Queremos ajudar as pessoas a descobrir mais jogos exibindo jogos relacionados na página de detalhes de cada jogo. A equipe de design quer explorar a experiência do usuário (UX) e fazer alguns testes de aceitação. Estes são os requisitos:

   - Adicione em `src/lib/` um auxiliar de acesso a dados que retorne outros jogos da mesma categoria de um jogo, sem incluir o próprio jogo.
   - Exiba uma seção "Jogos relacionados" na página de detalhes do jogo usando o auxiliar.
   - Trate o caso em que um jogo não tenha outros jogos relacionados.
   - Crie testes de unidade para o novo auxiliar.
   - Antes de criar o PR, confirme que todos os testes foram aprovados.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.

    ![Atribuição de uma issue ao Copilot](../../../_images/shared-assign-copilot.png)

9. Selecione **Assign**.

Pouco depois, você deverá ver o emoji 👀 no primeiro comentário da issue, indicando que o Copilot começou a trabalhar nela.

![O Copilot usa o emoji de olhos para indicar que está trabalhando na issue](../../../_images/ex4-issue-eyes-emoji.png)

O Copilot começou a trabalhar na segunda solicitação. Como o agente em nuvem trabalha de maneira semelhante a uma pessoa engenheira de software, não é necessário monitorá-lo continuamente; revise o resultado quando estiver pronto. Agora, vamos criar e usar agentes personalizados.

## Resumo e próximos passos

Esta lição apresentou o [agente em nuvem do GitHub Copilot][copilot-agents]. Com ele, você pode atribuir issues ao Copilot para execução assíncrona. O Copilot pode ajudar a resolver dívida técnica, criar recursos ou migrar código entre frameworks.

Você explorou:

- a personalização do ambiente de geração de código.
- como garantir que as operações sejam executadas com segurança.
- a importância de issues com escopo claro.
- como atribuir issues ao Copilot.

Enquanto o agente em nuvem trabalha em segundo plano, você pode criar e usar agentes personalizados. O [agente em nuvem do Copilot também pode usar servidores do Model Context Protocol (MCP)][cloud-agent-mcp] e instruções personalizadas, apresentadas em exercícios anteriores.

## Recursos

- [Sobre o agente em nuvem do Copilot][copilot-agents]
- [Atribuir issues do GitHub ao Copilot][assign-issue]
- [Práticas recomendadas para o workflow de configuração do agente em nuvem][cloud-agent-best-practices]

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
