---
title: "Lição 5 - Personalizar e usar uma skill de verificações de qualidade"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Desenvolver software envolve mais do que escrever código. Você já validou manualmente o funcionamento do código e usou arquivos de instruções para garantir que ele siga os padrões da equipe. Mas e os testes, o lint e as outras partes da integração contínua (CI)?

Para essas tarefas, as **skills de agente** são ideais. Elas ajudam o Copilot a entender como executar operações desse tipo.

Nesta lição, você vai:

- explorar a skill `quality-checks` existente.
- personalizar o formato dos resultados.
- recarregar e executar a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para testes de unidade, lint e verificações de tipos. A equipe quer melhorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As skills de agente reúnem instruções reutilizáveis de tarefas, scripts executáveis e recursos de apoio que o agente carrega quando necessário. No centro de uma skill há uma pasta com o nome da skill e um arquivo Markdown chamado `SKILL.md`. O arquivo inclui frontmatter com nome e descrição, uma visão geral da função e orientações sobre quando a skill deve ser usada. A pasta também pode incluir subpastas com scripts e outros recursos.

> [!NOTE]
> Pastas e arquivos adicionais não são obrigatórios para uma skill. A skill `quality-checks` da Tailspin Toys contém apenas `SKILL.md` porque usa os comandos existentes do projeto.

As skills podem ficar na pasta `.github/skills` do projeto para serem compartilhadas e reutilizadas pela equipe ou na pasta pessoal de skills `~/.copilot/skills`.

## Explorar a skill

Explore a skill `quality-checks` criada pela equipe da Tailspin Toys para executar testes de unidade, lint e verificações de tipos.

1. Volte ao Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia `name` e `description` no início do arquivo. A descrição ajuda o Copilot a identificar quando deve usar a skill.
3. Leia as instruções e observe como elas orientam o Copilot durante os testes e o lint.
4. Observe que a skill ainda não inclui uma seção **Results output formatting**.

## Executar a skill antes de alterá-la

As skills podem ser chamadas diretamente pelo Copilot CLI ou por linguagem natural. Peça ao Copilot que execute as três verificações da skill.

1. Volte à conversa de filtragem no modo Interactive.
2. Use este prompt:

   ```plaintext
   Execute a skill `quality-checks` para testes de unidade, lint e verificações de tipos.
   ```

3. Observe o relatório final.

## Personalizar o relatório

Melhore o relatório para indicar o que foi executado, se cada verificação foi aprovada e o que as ferramentas informaram.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione a seção a seguir ao final do arquivo:

   ```markdown
   ## Formato dos resultados

   Ao concluir, informe cada comando executado e se ele foi aprovado, reprovado ou bloqueado. Inclua contagens de testes, durações, erros, avisos e outras métricas somente quando a ferramenta as informar. Indique a próxima ação para cada falha ou bloqueio e nunca descreva uma verificação ignorada ou incompleta como aprovada.
   ```

3. O arquivo será salvo automaticamente.

## Executar a skill atualizada

O Copilot CLI pode recarregar skills editadas sem reiniciar a conversa. Agora, verifique o resultado.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Use exatamente o mesmo prompt:

   ```plaintext
   Execute a skill `quality-checks` para testes de unidade, lint e verificações de tipos.
   ```

3. Observe o relatório final e compare-o com o primeiro.

## Resumo e próximos passos

Você personalizou e usou uma skill de agente existente. Nesta lição, você:

- explorou a skill `quality-checks` para testes de unidade, lint e verificações de tipos.
- personalizou o formato dos resultados.
- recarregou e executou a skill.

Essa alteração acompanhará a filtragem no PR do recurso. Em seguida, você permitirá que o Copilot interaja diretamente com o site [por meio do servidor MCP do Playwright][next-lesson].

## Mais exemplos de skills

Estes exemplos da comunidade servem como referência, não como tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação incluído: `drawio`][drawio-example]
- [Testes de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
