---
title: "Lição 0 - Pré-requisitos"
description: "Prepare-se para as lições do aplicativo GitHub Copilot: instale o Node.js para o projeto Tailspin Toys e crie sua cópia do repositório a partir do modelo."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O aplicativo GitHub Copilot é um aplicativo para desktop que funciona como ponto central para o Copilot e o GitHub. Ele oferece acesso rápido a issues e pull requests e permite desenvolver com o GitHub Copilot. Durante este workshop, você trabalhará localmente no aplicativo Tailspin Toys, criado com Astro, usando o aplicativo GitHub Copilot. Antes de começar, instale o Node.js localmente e, depois, o aplicativo Copilot.

Nesta lição, você vai:

- instalar o Node.js para executar os testes do projeto no seu computador.
- criar sua própria cópia do projeto Tailspin Toys a partir do modelo.

## Instalar o Node.js

Várias lições pedirão a um agente que implemente funcionalidades e execute localmente o conjunto de testes do Tailspin Toys. Para isso, você precisa do **[Node.js][nodejs]**, o único ambiente de execução necessário para o projeto. Instale a versão atual de **suporte de longo prazo (LTS)**.

A opção mais simples em todas as plataformas é o instalador oficial:

1. No seu sistema operacional, abra uma janela de terminal usando o Windows Terminal, o Terminal do macOS ou o aplicativo que você costuma usar.
2. Execute os comandos a seguir para verificar as versões instaladas do Node.js e do npm:

    ```shell
    node --version
    npm --version
    ```

3. Se os dois comandos forem executados com sucesso e a versão do Node.js atender aos requisitos do README e do `package.json` do projeto, pule para a próxima seção.

> [!TIP]
> Conclua estas etapas somente se você não tiver o Node instalado ou precisar atualizá-lo.

4. Abra a [página de download do Node.js][node-download].
5. Baixe a versão **LTS** para o seu sistema operacional.
6. Execute o instalador e aceite as opções padrão. No Windows, mantenha selecionada a opção **Add to PATH**.
7. Depois da instalação, abra uma nova janela de terminal.
8. Confirme a instalação na nova janela executando:

    ```bash
    node --version
    npm --version
    ```

9. Você verá as versões instaladas do Node.js e do npm.

> [!IMPORTANT]
> O agente do aplicativo Copilot também precisa conseguir executar `node --version` e `npm --version`. O fato de os comandos funcionarem no seu terminal habitual não garante que estejam disponíveis para o aplicativo. Feche o aplicativo completamente e abra-o novamente depois de instalar o Node.js ou alterar o PATH. Se algum comando ainda falhar em uma sessão, siga as [etapas de solução de problemas do ambiente da sessão][session-environment] antes de continuar.

> [!IMPORTANT]
> Cada worktree também precisa das dependências do projeto e do Chromium do Playwright para verificações de ponta a ponta (E2E). Siga o README do repositório Tailspin Toys ao preparar uma worktree e revise qualquer solicitação de instalação antes de aprová-la.

## Configurar o repositório do laboratório

Você trabalhará na sua própria cópia do projeto Tailspin Toys. Crie-a agora a partir do [repositório modelo][template-repository]. O novo repositório contém todos os arquivos necessários para o laboratório e será conectado quando você instalar o aplicativo.

1. Em uma nova janela do navegador, acesse o repositório do GitHub deste laboratório: `https://github.com/github-samples/tailspin-toys`.
2. Crie sua própria cópia do repositório selecionando o botão **Use this template** na página do repositório do laboratório. Em seguida, selecione **Create a new repository**.

   ![Botão Use this template com Create a new repository selecionado no menu suspenso](../../../_images/app-0-use-template.png)

3. Se você estiver participando de um workshop em um evento conduzido pelo GitHub ou pela Microsoft, siga as instruções das pessoas mentoras. Caso contrário, crie o novo repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

   ![Formulário Create a new repository com github-samples/tailspin-toys definido como modelo e o nome do repositório preenchido](../../../_images/app-0-create-repository.png)

4. Anote o caminho do repositório criado (**organization-or-user-name/repository-name**), pois você o usará mais adiante no laboratório.

> [!NOTE]
> Quando você cria o repositório a partir do modelo, um backlog de issues do GitHub é criado automaticamente. Você trabalhará com essas issues durante todo o workshop; não é necessário criar nenhuma.

Use uma cópia nova do modelo do workshop. Ela inclui instruções do repositório, código do aplicativo, testes, a skill `quality-checks` e uma extensão de canvas existente. Durante o workshop, você personalizará a skill e criará um agente de QA. Se usar uma cópia antiga, confirme com a pessoa facilitadora se ela contém os arquivos necessários.

## Resumo e próximos passos

Tudo está pronto! Nesta lição, você:

- instalou o Node.js para que o projeto possa ser compilado e testado no seu computador.
- criou sua própria cópia do repositório Tailspin Toys a partir do modelo.

Em seguida, você [instalará o aplicativo GitHub Copilot][next-lesson], conectará o repositório criado e conhecerá o espaço de trabalho.

## Recursos

- [Baixar o Node.js][node-download]
- [Criar um repositório a partir de um modelo][template-repository]
- [Sobre o aplicativo GitHub Copilot][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#comecar-pela-issue
