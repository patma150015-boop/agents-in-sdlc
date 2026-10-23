---
title: "Exercício 6 - Iterar sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← Lição anterior: Monitorar e gerenciar agentes][previous-lesson] |
|:--|

## Revisar o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em várias tarefas para melhorar a experiência do usuário e adicionar funcionalidades. Você usou o modo de agente para adicionar filtragem no cliente e no servidor, o servidor MCP do Playwright para testar manualmente esse trabalho em um navegador e, depois, um agente personalizado para implementar recursos de acessibilidade — controles para alternar os modos de alto contraste e claro — e orientou a sessão em andamento para ampliar o trabalho. Agora, publique esse trabalho local e revise-o como sua equipe faria.

### Cenário

Os fundamentos do design de software e do DevOps não mudam com a chegada da IA generativa. É importante revisar de verdade tudo o que o Copilot produz. Com isso em mente, envie as alterações de acessibilidade do Codespace ao repositório remoto, abra um pull request e revise o diff antes de envolver o restante da equipe.

## Publicar os recursos de acessibilidade

Os controles dos modos de alto contraste e claro implementados com o agente personalizado de acessibilidade nos [Exercícios 4][exercise-4] e [5][exercise-5] estão no Codespace como alterações com commit. Envie-as para uma branch no repositório remoto e abra um pull request para que o restante da equipe possa revisá-las.

1. Volte ao Codespace.
2. Abra a exibição **Source Control** no VS Code.
3. Confirme que as alterações de acessibilidade têm commit. Se houver alterações do Exercício 5 sem commit, prepare-as e faça commit agora com uma mensagem descritiva, como `Add high-contrast and light-mode toggles`.
4. Publique a branch selecionando **Publish Branch** ou use o menu **...** e selecione **Push**.
5. O VS Code oferecerá a opção de abrir a nova branch no github.com. Aceite a solicitação ou acesse manualmente o repositório e selecione **Compare & pull request** no banner da branch.
6. Defina um título claro, por exemplo, `Add high-contrast and light-mode toggles`, e uma descrição curta que resuma o que foi feito e por quê.
7. Selecione **Create pull request**.
8. Depois de abrir o PR, selecione a guia **Files changed** para revisar todas as alterações. Preste atenção especial a:
   - componentes de interface para alternar entre os modos.
   - armazenamento local usado para persistir as preferências.
   - alterações de CSS ou estilos dos modos de alto contraste e claro.
   - atributos de acessibilidade, como rótulos ARIA e navegação por teclado.
   - código JavaScript/TypeScript que controla a alternância de modos.

9. Volte à guia **Conversation**.
10. Se houver workflows aguardando aprovação, selecione **Approve and run workflows**.

   ![Aprovar e executar workflows](../../../_images/shared-approve-workflows.png)

11. Aguarde a conclusão dos workflows. Se tudo correr bem, eles serão aprovados.

> [!TIP]
> Quer uma segunda opinião sobre o trabalho de acessibilidade? Mencione `@copilot` em um comentário do PR e faça um pedido, como "revise este PR em busca de outros problemas relacionados às WCAG" ou "sugira melhorias na navegação por teclado". O Copilot iniciará uma nova sessão para atender ao comentário.

## Exercício opcional - continuar explorando localmente

Trabalhar de forma iterativa com um agente no IDE é uma habilidade que se desenvolve com a prática. Experimente estas ideias em sessões futuras no VS Code:

- adicionar um formulário de interesse para apoiadores na página de detalhes do jogo.
- implementar paginação na página da lista de jogos.
- adicionar validação de entrada e tratamento de erros aos helpers de acesso a dados em `src/lib/`.
- ampliar o escopo do agente de acessibilidade — por exemplo, auditar a ordem de foco do teclado em todo o site.

## Resumo

Parabéns, você concluiu o percurso do VS Code. Neste laboratório, você:

- **Usou o MCP do Playwright para testar manualmente a funcionalidade.** Adicionou o servidor MCP do Playwright e deixou o Copilot controlar um navegador para verificar a filtragem antes de abrir um pull request.
- **Conduziu o modo de agente por alterações coordenadas em toda a pilha.** Adicionou uma funcionalidade de filtragem que envolveu o cliente, o servidor e os testes em uma única sessão.
- **Usou um agente personalizado.** Selecionou o agente personalizado de acessibilidade no seletor de agentes e acompanhou a implementação do modo de alto contraste no repositório.
- **Gerenciou e orientou uma sessão de agente.** Revisou as alterações propostas no editor, aceitou as desejadas e ampliou a sessão com uma solicitação para adicionar o modo claro.
- **Concluiu o fluxo com um pull request.** Publicou o trabalho local e revisou todas as alterações como sua equipe faria.

## Revisão e próximos passos

Este é o fim do percurso obrigatório do VS Code. Você pode encerrar o workshop aqui.

Se quiser conhecer outras capacidades de agente do Copilot, os demais percursos apresentam cenários relacionados em outras interfaces:

- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal com os modos Plan e Autopilot, skills, um agente personalizado de QA, Playwright MCP, Agent Merge e transferências de contexto.
- ☁️ **[Percurso do agente de nuvem](../../cloud/)** — concentre-se em atribuir issues ao agente de nuvem, acompanhar as sessões pela página de agentes e iterar de forma assíncrona nos pull requests.

Você também pode continuar desenvolvendo o que começou aqui. O [awesome-copilot][awesome-copilot] é uma ótima fonte de arquivos de instruções, agentes personalizados e skills que você pode adaptar aos seus projetos.

Como extensão opcional, [Opcional: Incorporar o Foundry][exercise-7] usa o VS Code e o Microsoft Foundry Toolkit para preparar um modelo, implantar um Backer Concierge e conectá-lo ao site.

## Recursos

- [GitHub Copilot][github-copilot]
- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Gerenciar agentes][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
