---
slug: pt-br/real-world-development/cli
title: "CLI do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

O **[GitHub Copilot CLI](https://docs.github.com/copilot/concepts/agents/about-copilot-cli)** coloca o GitHub Copilot no terminal como um assistente de programação baseado em agentes. Ele explora bases de código, gera código, executa comandos e se conecta a ferramentas externas — tudo pela linha de comando, para que você mantenha o foco sem precisar alternar para um editor gráfico.

Ao longo destas lições, você instalará e autenticará o Copilot CLI, adicionará uma pequena melhoria aos cards de jogos e, em seguida, usará os modos Plan e Autopilot para implementar a filtragem. Depois, fornecerá contexto do projeto com instruções personalizadas, personalizará uma skill de agente e conectará o servidor MCP do Playwright para testar o recurso em um navegador real. Você também criará um agente de garantia de qualidade (QA), abrirá um pull request e usará o Agent Merge. Por fim, explorará comandos de barra para gerenciar contexto, modelos e compartilhamento, além da delegação opcional para o agente em nuvem. Você também pode seguir uma [série opcional de três módulos com GitHub Copilot CLI e Foundry][foundry] para preparar um modelo, criar e implantar um agente hospedado e integrá-lo ao site.

## Lições

| Lição | Tópico | Descrição |
|----------|-------|-------------|
| [0. Pré-requisitos][ex0] | Configuração | Crie seu repositório e codespace |
| [1. Instalar o Copilot CLI][ex1] | Instalação | Instale e autentique o Copilot CLI |
| [2. Adicionar avaliações por estrelas][ex2] | Primeiro recurso | Exiba as avaliações nos cards e abra seu primeiro pull request |
| [3. Modos de agente][ex3] | Plan e Autopilot | Planeje e implemente a filtragem a partir de uma issue |
| [4. Instruções personalizadas][ex4] | Contexto | Atualize as instruções do repositório e aplique-as ao recurso |
| [5. Skills de agente][ex5] | Skills | Personalize e use a skill de verificações de qualidade |
| [6. Validar com Playwright MCP][ex6] | Ferramentas externas | Use o Playwright MCP para explorar a filtragem em um navegador |
| [7. Criar e usar um agente de QA][ex7] | Agentes personalizados | Crie um agente de QA e revise os resultados |
| [8. Criar e integrar o PR do recurso][ex8] | Pull requests | Revise a alteração e use o Agent Merge |
| [9. Comandos de barra][ex9] | Recursos da CLI | Explore contexto, modelos, compartilhamento e delegação opcional |
| [10. Encerramento e próximos passos][ex10] | Resumo | Revise o fluxo de trabalho e os próximos passos |
| [Opcional: Incorpore o Foundry][foundry] | Agentes hospedados | Prepare um modelo, crie e implante um Backer Concierge baseado no catálogo e conecte-o ao site |

## Pré-requisitos

Antes de participar deste workshop, verifique se você tem:

- [ ] Uma conta do GitHub com um plano ativo **Copilot Student, Pro, Pro+, Business ou Enterprise**
- [ ] Familiaridade básica com operações de terminal e linha de comando
- [ ] O Git instalado e configurado

> [!TIP]
> Não tem um plano pago? Estudantes verificados podem obter o GitHub Copilot gratuitamente por meio do [GitHub Education][callout-student-plan-education]. O plano **Copilot Student** inclui os recursos de agente, MCP, revisão de código e Copilot CLI usados neste workshop. Portanto, você pode concluir todos os percursos com esse plano.

> [!NOTE]
> Se você usa o Copilot Business ou o Copilot Enterprise, verifique se o administrador habilitou o Copilot CLI para uso.

## Começar

[**Comece pela Lição 0: Pré-requisitos →**][ex0]

[ex0]: 0-prerequisites/
[ex1]: 1-install-copilot-cli/
[ex2]: 2-add-star-rating/
[ex3]: 3-agent-modes/
[ex4]: 4-custom-instructions/
[ex5]: 5-agent-skills/
[ex6]: 6-mcp-playwright/
[ex7]: 7-qa-agent/
[ex8]: 8-create-pull-request/
[ex9]: 9-cli-power-tools/
[foundry]: 8-foundry-agent/
[ex10]: 10-review/
[callout-student-plan-education]: https://github.com/education/students
