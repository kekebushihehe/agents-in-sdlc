---
title: "Exercício 3 - Agentes personalizados"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## O que são agentes personalizados?

Os [agentes personalizados][custom-agents-concept] do GitHub Copilot permitem criar assistentes de IA especializados em tarefas ou domínios específicos do fluxo de desenvolvimento. Ao definir agentes em arquivos Markdown na pasta `.github/agents` do repositório, você fornece instruções focadas, práticas recomendadas, padrões de código e conhecimento do domínio para orientar o trabalho deles. As equipes podem registrar sua experiência em agentes reutilizáveis: um agente de acessibilidade que aplica as [Diretrizes de Acessibilidade para Conteúdo Web (WCAG)][wcag], um agente de segurança que segue práticas de codificação segura ou um agente de testes que mantém padrões consistentes.

Os agentes personalizados são definidos em arquivos Markdown na pasta `.github/agents` do projeto ou globalmente em `~/.copilot/agents`. Cada arquivo tem frontmatter YAML com pelo menos `name` e `description`, seguido de um prompt Markdown que define o comportamento, a especialização e as instruções do agente.

### Agentes personalizados e skills de agente

Há alguma sobreposição entre agentes personalizados e [skills de agente][agent-skills-concept]. Ambos são definidos principalmente em arquivos Markdown e orientam uma IA a executar tarefas. A distinção mais simples é: o **agente personalizado** é quem executa o trabalho; as **skills** são ferramentas.

Agentes personalizados têm sua própria janela de contexto e podem orquestrar skills e até outros agentes. Neste laboratório, o agente personalizado de acessibilidade revisa e atualiza o site de acordo com as diretrizes de acessibilidade. Como parte do trabalho, ele poderia usar skills para fluxos de pull request ou execução e gerenciamento de testes.

> [!NOTE]
> Não há uma única maneira correta de criar um agente personalizado. Como em qualquer uso de IA, teste e itere para encontrar o que funciona nos seus ambientes e cenários.

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

Com agentes personalizados, você explorará:

- como definir agentes personalizados.
- como atribuir uma tarefa a um agente personalizado.

## Cenário

A Tailspin Toys quer garantir que todas as pessoas possam usar sua plataforma de financiamento coletivo, independentemente de suas capacidades visuais ou preferências. Comentários recentes indicam que algumas pessoas têm dificuldade para ler o tema escuro atual por causa do contraste insuficiente entre o texto e o fundo. Para resolver esse problema de acessibilidade, a equipe de design solicitou um modo de alto contraste que possa ser ativado e desativado.

Como a acessibilidade é importante, você quer implementar esse recurso rapidamente. Para isso, usará um agente personalizado.

## Revisar o agente personalizado de acessibilidade

Um agente personalizado de acessibilidade já foi criado. Revise o conteúdo para entender como ele orientará o Copilot.

Volte ao codespace e examine o arquivo do agente personalizado:

1. Abra `.github/agents/accessibility.md`.
2. Observe o frontmatter YAML com os campos `name` e `description`.

> [!CAUTION]
> Os campos `name` e `description` no frontmatter são obrigatórios para agentes personalizados.

3. Examine as seções seguintes, que destacam:
   - responsabilidades principais ao gerar código para um site acessível.
   - práticas recomendadas de acessibilidade.
   - exemplos de código para linguagem de marcação de hipertexto (HTML), folhas de estilo em cascata (CSS) e JavaScript.
   - erros comuns a evitar.

## Criar e atribuir uma issue

O painel de agentes é o local central para trabalhar com agentes no seu ambiente. Você pode atribuir tarefas ao agente em nuvem do Copilot, monitorar o trabalho e redirecionar uma sessão em andamento. Vamos atribuir ao Copilot a tarefa de criar o modo de alto contraste.

1. Acesse seu repositório.
2. Selecione a guia de issues.
3. Selecione **New issue** para abrir a caixa de criação de issue.
4. Selecione **Blank issue**.
5. Defina **Title** como `Adicionar modo de alto contraste ao site`.
6. Defina **Description** como:

    ```plaintext
    Precisamos de um modo de alto contraste para o site. Deve haver um controle que a pessoa usuária possa ativar para usar o alto contraste. A preferência deve ser armazenada no armazenamento local do navegador.
    ```

7. Selecione **Create** para criar a issue.
8. À direita, selecione **Assign to Copilot** para abrir a caixa de atribuição.
9. Selecione **Accessibility agent** na lista de agentes personalizados.

    ![Caixa de atribuição ao agente em nuvem com o agente de acessibilidade destacado](../../../_images/ex5-select-custom-agent.png)

10. Selecione **Assign**.
11. O Copilot começará a trabalhar na tarefa em segundo plano.

## Resumo e próximos passos

Esta lição apresentou os [agentes personalizados][custom-agents] do GitHub Copilot, assistentes de IA especializados em tarefas e domínios específicos. Com eles, você pode registrar a experiência e os padrões da equipe em agentes reutilizáveis que orientam o trabalho do Copilot.

Você explorou:

- como definir agentes personalizados.
- como atribuir uma tarefa a um agente personalizado.

Enquanto o Copilot implementa o modo de alto contraste, você pode [monitorar e orientar a sessão do agente][next-lesson] no painel de controle.

## Recursos

- [Sobre agentes personalizados][custom-agents]
- [Preparar o uso de agentes personalizados na organização][org-custom-agents]
- [Preparar o uso de agentes personalizados na empresa][enterprise-custom-agents]

---

| [← Lição anterior: Agente em nuvem do GitHub Copilot][previous-lesson] | [Próxima lição: Monitorar e gerenciar agentes →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
