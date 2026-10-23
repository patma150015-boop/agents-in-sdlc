---
title: "Lição 1 - Instalar o aplicativo GitHub Copilot"
description: "Instale o aplicativo GitHub Copilot, conecte o repositório criado a partir do modelo, conheça o espaço de trabalho e experimente um chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O **[aplicativo GitHub Copilot][about-copilot-app]** é um aplicativo para desktop voltado ao desenvolvimento orientado por agentes. Ele foi criado com base no GitHub Copilot CLI e tem integração nativa com o GitHub, então seus repositórios, branches e pipelines de integração contínua (CI) funcionam imediatamente. O aplicativo foi desenvolvido para fluxos de trabalho em que você orienta vários agentes em paralelo — cada um em um espaço de trabalho isolado — em vez de fazer todo o trabalho por conta própria, além de automatizar tarefas repetitivas. Com o Node.js instalado e sua cópia do projeto pronta, instale o aplicativo e conecte o repositório.

Nesta lição, você vai:

- instalar o aplicativo GitHub Copilot e fazer login.
- adicionar seu projeto ao aplicativo pelo repositório do GitHub.
- conhecer o espaço de trabalho, incluindo o backlog criado pelo modelo.
- experimentar um chat rápido para saber mais sobre o próprio aplicativo.

## Cenário

Sua equipe está adotando agentes de IA para trabalhar em um backlog crescente. O aplicativo Copilot oferece um lugar para orientar esse trabalho: selecionar issues, executar agentes, revisar alterações e mesclar pull requests. Nesta lição, você instalará e conectará o aplicativo e aprenderá a iniciar uma conversa sobre o projeto.

> [!NOTE]
> É necessário ter um plano elegível do Copilot: Copilot Free, Copilot Student ou qualquer plano pago (Pro, Pro+, Business ou Enterprise). Se você usa o Copilot Business ou Enterprise, o administrador deve manter habilitada a política **GitHub Copilot app**.

## Instalar e configurar o aplicativo GitHub Copilot

Para usar o aplicativo GitHub Copilot, primeiro instale-o. Há versões para Windows, macOS e Linux. Instale o aplicativo, faça a autenticação e adicione o repositório Tailspin Toys.

1. Em um navegador, abra a [página inicial do aplicativo GitHub Copilot][download-app].
2. Baixe o aplicativo para sua plataforma e instale-o seguindo as instruções da página.
3. Abra o aplicativo depois de instalá-lo.
4. Selecione **Sign in to GitHub** e siga as instruções para fazer a autenticação. Se você usa o GitHub Enterprise Cloud com residência de dados, selecione **Use GitHub Enterprise** e informe o nome do host `*.ghe.com` quando solicitado. O GitHub Enterprise Server não é compatível.
5. Se o fluxo de configuração oferecer a seleção de repositórios, selecione o repositório Tailspin Toys criado a partir do modelo. Você também pode pular essa etapa e adicionar o projeto depois.
6. Se solicitado, escolha um tema e conclua a configuração.
7. Procure o repositório em **Sessions**, na barra lateral. Se ele já estiver listado, não é necessário adicioná-lo novamente.
8. Se o repositório ainda não estiver conectado, selecione **+** ao lado de **Sessions** para criar uma nova sessão e, depois, selecione **Add GitHub repository**. Use o nome do proprietário e o nome do repositório anotados na lição de pré-requisitos.
9. Confirme que o repositório aparece na barra lateral. No layout usado aqui, as pastas dos repositórios, como `tailspin-toys`, ficam agrupadas em **Sessions**, com as sessões de cada repositório abaixo.

## Conhecer o espaço de trabalho

Com o projeto conectado, conheça o espaço de trabalho. O aplicativo organiza tudo em algumas áreas na barra lateral:

- **New**: inicie uma nova sessão de chat com o Copilot.
- **Pull requests**: acesse seus pull requests pela integração nativa com o GitHub. Nessa área, você pode procurar e filtrar pull requests, conferir o estado da CI e revisá-los sem sair do aplicativo.
- **Issues**: acesse suas issues da mesma maneira. Nessa área, você pode procurar e filtrar issues e iniciar uma sessão a partir de uma issue.
- **Automations**: tarefas de agente salvas que são executadas em uma agenda ou sob demanda. Elas são úteis para gerenciar listas de tarefas, fazer a manutenção regular do projeto ou delegar outras atividades repetitivas. A revisão final indica essas automações como próximo passo, não como outro exercício do workshop.
- **Customize**: adicione recursos ao aplicativo Copilot na forma de servidores do Model Context Protocol (MCP), plugins, skills e outros componentes. Você usará essa área para configurar o MCP do Playwright.
- **Chats**: conversas rápidas para perguntas e brainstorming que não precisam de uma branch ou espaço de trabalho próprio. Esta seção aparece depois que você inicia seu primeiro chat. Se ela não estiver visível, selecione **+** ao lado de **Sessions** para iniciar um chat.
- **Sessions**: grupos de pastas de repositório e sessões de agente. Selecione o nome de um repositório, como `tailspin-toys`, para abrir a visualização do repositório ou selecione uma sessão abaixo dele para retomar a conversa. Cada sessão é executada em seu próprio espaço de trabalho isolado, então você pode executar várias ao mesmo tempo sem que as alterações entrem em conflito. Você iniciará sua primeira sessão ao adicionar avaliações por estrelas.

Ao longo do workshop, você explorará o espaço de trabalho.

> [!TIP]
> Na dúvida, pergunte ao Copilot. Se você não souber como fazer algo ou quiser saber se é possível, pergunte ao Copilot; ele ajudará a orientar você.

### Encontrar o backlog criado pelo modelo

É provável que todo projeto tenha um backlog, e a Tailspin Toys não é diferente. Explore o backlog criado quando você criou sua cópia a partir do modelo.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys, não uma sessão específica abaixo dela. Depois, selecione **Issues** na navegação superior do repositório.
2. Procure estas issues pelo título, sem presumir os números:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecione uma issue para ler os detalhes. Cada issue também pode iniciar uma sessão de agente. Você começará pela issue de filtragem depois de fazer uma primeira alteração rápida.

> [!NOTE]
> As listas em **Issues** e **Pull requests** são filtradas automaticamente para mostrar somente itens dos repositórios adicionados ao aplicativo Copilot. Para ver itens de outros repositórios, adicione-os ao aplicativo.

## Experimentar um chat rápido

Uma ótima maneira de se familiarizar com o aplicativo é usá-lo para saber mais sobre o *próprio aplicativo*. Um **chat rápido** é a ferramenta certa para isso. Com chats rápidos, você pode fazer perguntas ou trocar ideias sem criar uma branch ou worktree. Eles são ideais para perguntas rápidas e descartáveis, sem precisar iniciar uma sessão.

1. Na barra lateral, selecione **+** ao lado de **Chats** para abrir um chat. Se **Chats** ainda não estiver visível, selecione **+** ao lado de **Sessions** para iniciar seu primeiro chat; **Chats** aparecerá depois.
2. Pergunte ao aplicativo como funcionam as sessões:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Leia a resposta na exibição da conversa. Você verá que cada sessão é executada em sua própria worktree isolada do Git, o que permite executar vários agentes em paralelo sem que as alterações entrem em conflito. Você pode continuar a conversa ou iniciar outro chat a qualquer momento.

## Resumo e próximos passos

Parabéns! Você instalou o aplicativo GitHub Copilot, conectou o projeto e explorou o espaço de trabalho. Você aprendeu a:

- instalar o aplicativo e fazer login no GitHub.
- adicionar um projeto pelo repositório do GitHub.
- conhecer o espaço de trabalho e encontrar o backlog criado pelo modelo em **Issues**.
- usar um chat rápido para fazer uma pergunta curta e descartável.

Em seguida, você [iniciará sua primeira sessão de agente][next-lesson] e a usará para exibir avaliações por estrelas nos cartões dos jogos.

## Recursos

- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Introdução ao aplicativo GitHub Copilot][getting-started]
- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
