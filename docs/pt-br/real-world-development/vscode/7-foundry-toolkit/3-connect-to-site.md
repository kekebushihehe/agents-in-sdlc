---
title: "Conectar o agente ao site"
description: "Conecte o Backer Concierge hospedado por meio de um proxy local e teste um widget de chat acessível."
authors:
  - juliamuiruri4
lastUpdated: 2026-09-16
next: false
---

| [← Módulo anterior: Criar e implantar um agente][previous-lesson] |
|:--|

Este módulo conecta o agente hospedado de [Criar e implantar um agente][previous-lesson] à Tailspin Toys. O Copilot Chat no VS Code cria uma integração local, não um endpoint público de produção.

## Objetivos

- Manter as credenciais e os identificadores de conversa do Foundry protegidos por um proxy local no servidor.
- Adicionar um widget de chat acessível com continuidade de conversa.
- Verificar o backend e a experiência completa antes de limpar os recursos.

## Cenário

Os apoiadores devem poder pedir recomendações ao assistente sem sair do catálogo. Uma conversa precisa manter o contexto, funcionar com teclado e proteger os detalhes privados de conexão. A confiança depende tanto de recomendações honestas quanto de uma experiência segura e acessível.

## Retomar o espaço de trabalho

O agente hospedado existente é o destino da integração. A Tailspin Toys é um site estático totalmente pré-renderizado, portanto o código do navegador não pode armazenar as credenciais do agente com segurança.

1. Abra o mesmo repositório da Tailspin Toys em `foundry-agent-vscode` no VS Code. Confirme que o agente hospedado do marco anterior ainda está **Running** no projeto `tailspin-toys` existente e que o login local no Azure está direcionado à assinatura desse projeto.
2. Abra o Copilot Chat no modo **Agent** comum em vez de **AIAgentExpert**. Anexe **Add a Backer Concierge assistant for catalog questions** selecionando **+**, depois **GitHub Issues**, e escolhendo a issue.

## Criar e verificar o proxy local

Um proxy local do Azure Functions em `/api` armazena os detalhes de conexão e encaminha as solicitações enquanto o site é executado localmente. O Copilot pode usar **Azure skills** para prepará-lo e validá-lo.

> [!IMPORTANT]
> Este proxy do workshop é apenas para desenvolvimento local. Não o implante como um endpoint público anônimo. A produção exige autenticação específica da aplicação e controles contra abuso, incluindo limites de taxa ou cotas, restrições de CORS, monitoramento e controles de custo.

1. Peça ao Copilot para criar o proxy:

   ```text
   Adicione um proxy local do Azure Functions em `api` para que o site estático Astro possa chamar com segurança meu Backer Concierge implantado durante o desenvolvimento. Use meu login local existente do Azure para chamar o agente hospedado, mantenha todas as credenciais fora do navegador, proteja o estado da conversa com identificadores opacos, valide solicitações, sanitize erros, adicione testes específicos e configure o servidor de desenvolvimento do Astro para encaminhar solicitações a `/api` para a Function local. Não crie infraestrutura de implantação pública.
   ```

2. Revise as alterações antes de aceitá-las. Confirme que as credenciais e os identificadores de conversa do Foundry permanecem no servidor, que as configurações locais estão excluídas do controle de versão, que as solicitações têm limites e que os testes específicos passam.
3. Comprove que o backend funciona antes de criar a interface:

   ```text
   Inicie o host local do Functions e teste `/api/concierge` perguntando "Quais jogos custam menos de US$ 30?". Mostre a resposta sanitizada e confirme que nenhuma credencial ou identificador interno de conversa é retornado.
   ```

4. Verifique a resposta no terminal. Espere um JSON válido com uma propriedade `response` contendo a resposta, sem informações de preço inventadas e sem credenciais ou identificadores internos de conversa. Se uma verificação falhar, peça ao Copilot para corrigi-la e repita o teste do backend.
5. Selecione **Keep** para manter as alterações e use **/clear** para começar uma nova conversa para o widget no mesmo repositório e na mesma branch. Mantenha a configuração do proxy local e a conexão com o agente hospedado existente.

## Criar e testar o widget

A interface agora tem um backend verificado. Os testes de ponta a ponta verificam tanto a usabilidade quanto os limites das informações do catálogo.

1. Peça ao Copilot para adicionar o widget:

   ```text
   Adicione ao site Astro um widget acessível de chat do Backer Concierge. Conecte-o a `/api/concierge`, preserve a conversa usando o identificador opaco retornado, siga as diretrizes de design existentes, ofereça suporte ao uso pelo teclado e permita testá-lo.
   ```

   ![Captura de tela mostrando o widget de chat Backer Concierge em ação](../../../../_images/tailspin-toys-backer-concierge-agent.png)

2. Mantenha a função e o site em execução e verifique a experiência completa:

   ```text
   Use o Playwright MCP para testar de ponta a ponta o widget Backer Concierge. Verifique o fluxo principal do chat, a continuidade da conversa, o comportamento com o teclado e a acessibilidade, os limites das fontes usadas para fundamentar as respostas e o uso seguro do proxy local. Informe os resultados e corrija eventuais falhas.
   ```

3. Revise os resultados dos testes e as alterações em relação aos critérios de aceitação da issue: respostas fundamentadas, nenhum número de financiamento inventado, uma pergunta de esclarecimento, interface acessível e cobertura de ponta a ponta. Confirme que as falhas foram corrigidas e que as verificações afetadas foram executadas novamente.

## Marco de conclusão

Você criou um proxy local que protege as credenciais, conectou um widget de chat acessível e verificou todo o fluxo de conversa com o Backer Concierge hospedado. O marco de conclusão deste módulo é uma integração com o site testada localmente que preserva os limites do catálogo e mantém credenciais e identificadores internos do Foundry fora do navegador. Esta não é uma implantação em produção do proxy nem do site.

Quando terminar de experimentar, pare os serviços locais e [limpe os recursos do Azure][cleanup] para evitar custos contínuos. Depois, retorne à [visão geral do VS Code][vscode-overview] do workshop principal.

[previous-lesson]: ../2-build-and-deploy/
[cleanup]: ../#limpar-os-recursos
[vscode-overview]: ../../
