---
title: "Lição 1 - Criar no espaço de trabalho"
description: "Crie o Space Quiz no VS Code, visualize-o no navegador integrado e aprimore um elemento selecionado."
authors:
  - jamesmontemagno
lastUpdated: 2026-09-28
---

Mantenha o editor, a conversa, os arquivos e a prévia juntos. Você criará o quiz, jogará no navegador integrado e, depois, enviará um elemento específico diretamente para a conversa para fazer uma alteração com escopo bem definido.

Nesta lição, você vai:

- criar o quiz em um único `index.html`.
- visualizar o quiz no navegador integrado.
- selecionar um elemento no navegador e aprimorá-lo.

## Criar o quiz

Envie o seguinte prompt no Copilot Chat:

```plaintext
Crie um quiz colorido e acessível sobre exploração espacial, com 10 perguntas em um único `index.html`. Adicione uma barra de progresso, um contador de pontos, feedback animado para respostas certas e erradas e uma tela de resultados. Não use servidor nem dependências. Abra-o no navegador integrado do VS Code.
```

1. Revise o arquivo gerado no editor.
2. Abra o navegador integrado e responda a várias perguntas.
3. Abra **Source Control** a qualquer momento para ver os arquivos alterados e o diff.

## Selecionar um elemento e aprimorá-lo

O navegador integrado pode enviar um elemento específico diretamente para a conversa, para que você nunca precise descrever a qual botão está se referindo.

1. Com o quiz aberto no navegador integrado, inicie uma seleção de elemento pela barra de ferramentas do navegador.
2. Selecione os botões de resposta para anexar esse elemento à próxima mensagem da conversa.
3. Envie o seguinte prompt e observe a prévia recarregar:

   ```plaintext
   Usando o elemento selecionado, deixe os botões de resposta mais táteis: adicione um estado sutil ao pressioná-los, um contorno de foco mais evidente para quem usa o teclado e uma transição mais suave para as cores de resposta certa e errada. Não altere mais nada.
   ```

4. Leia o diff em **Source Control** antes de manter a alteração.
5. Pressione <kbd>Tab</kbd> para percorrer as respostas e confirme que o contorno de foco está visível.

## Resumo e próximos passos

Você criou, visualizou e aprimorou o quiz sem sair do editor. Continue com a [Lição 2: Registrar instruções do projeto][next-lesson].

[next-lesson]: ../2-project-instructions/
