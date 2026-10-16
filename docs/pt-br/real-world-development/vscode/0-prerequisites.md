---
title: "Exercício 0: Pré-requisitos"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Antes de começar os exercícios do VS Code, prepare tudo. Você criará sua própria cópia do repositório Tailspin Toys, iniciará um [Codespace][codespaces] para trabalhar e confirmará se o GitHub Copilot Chat está funcionando no editor.

## Configure o repositório do laboratório

Para criar uma cópia do repositório para o código que você desenvolverá, use o [template][template-repository]. A nova cópia terá todos os arquivos necessários para o laboratório, e você a usará durante os exercícios.

1. Em uma nova janela do navegador, acesse o [template do Tailspin Toys][tailspin-template].
2. Crie sua própria cópia do repositório selecionando **Use this template** na página do repositório do laboratório e, em seguida, **Create a new repository**.

    ![Botão Use this template](../../../_images/ex0-use-template.png)

3. Se você estiver participando de um evento organizado pelo GitHub ou pela Microsoft, siga as instruções dos mentores. Caso contrário, crie o novo repositório em uma organização na qual você tenha acesso ao GitHub Copilot.

    ![Campos de configuração do template do repositório](../../../_images/ex0-repository-settings.png)

4. Anote o caminho do repositório que criou (**organization-or-user-name/repository-name**), pois você precisará dele durante o laboratório.

> [!NOTE]
> **Seu backlog está pronto**
>
> Ao criar o repositório a partir do template, um backlog de issues do GitHub será criado automaticamente. Você trabalhará com essas issues durante o workshop — não precisa criar nenhuma.

## Crie um Codespace

Em seguida, você usará um Codespace para concluir os exercícios do laboratório.

O [GitHub Codespaces][codespaces] oferece um ambiente de desenvolvimento em nuvem que permite escrever, executar e depurar código diretamente no navegador. Ele oferece um IDE completo com suporte a várias linguagens de programação, extensões e ferramentas.

1. Acesse o repositório que acabou de criar.
2. Selecione o botão verde **Code**.

    ![Selecione o botão Code](../../../_images/ex0-code-button.png)

3. Selecione a guia **Codespaces** e o botão **+** para criar um Codespace.

    ![Criar um Codespace](../../../_images/ex0-create-codespace.png)

A criação do Codespace levará alguns minutos, mas ainda será muito mais rápida do que instalar manualmente todos os serviços. Enquanto isso, você pode explorar outros recursos do GitHub Copilot, que veremos em seguida.

> [!CAUTION]
> Você voltará ao Codespace em um exercício futuro. Por enquanto, deixe-o aberto em uma guia do navegador.

> [!NOTE]
> Este workshop foi preparado para ser executado em um Codespace ou em um [contêiner de desenvolvimento local][dev-containers]. Ambos garantem que o ambiente tenha todos os pré-requisitos necessários para uma experiência tranquila. Se preferir trabalhar localmente, abra o repositório clonado no VS Code e selecione **Reopen in Container** quando solicitado — o VS Code criará o mesmo contêiner de desenvolvimento usado pelo Codespace.

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## Use o GitHub Copilot Chat e o modo de agente

Para acessar o modo de agente do GitHub Copilot Chat, é necessário instalar a extensão GitHub Copilot Chat no IDE. Ela já deve estar instalada se você estiver usando um GitHub Codespace.

> [!TIP]
> Se a extensão GitHub Copilot Chat não estiver instalada, você pode [instalá-la no Visual Studio Code Marketplace][copilot-chat-extension]. Outra opção é abrir a visualização **Extensions** no Visual Studio Code, procurar **GitHub Copilot Chat** e selecionar **Install**.

Depois de instalar a extensão, talvez seja necessário autenticar-se com sua conta do GitHub para habilitá-la.

1. Volte ao Codespace.
2. Se você não vir o Copilot Chat à direita do editor, selecione o ícone **Copilot Chat** na parte superior do Codespace.
3. Digite uma mensagem como "Hello world" na janela do Copilot Chat e pressione <kbd>Enter</kbd>. Isso deve ativar o Copilot Chat.
4. Se você ainda não estiver autenticado, uma solicitação de entrada na conta do GitHub será exibida. Siga as instruções para autenticar-se.

    ![Exemplo de solicitação de autenticação do Copilot Chat](../../../_images/ex1-copilot-authentication.png)

5. Depois da autenticação, a janela do Copilot Chat deverá aparecer.

## Resumo

Parabéns, você criou uma cópia do repositório do laboratório! Você também iniciou a criação do Codespace, que usará ao começar a escrever código.

## Próxima etapa

Vamos começar a trabalhar com o Copilot. Continue para [Exercício 1 - Instruções personalizadas][next-lesson] e ensine ao Copilot as convenções do projeto.

## Recursos

- [Visão geral do GitHub Codespaces][codespaces]
- [Criar um repositório a partir de um template][template-repository]
- [Introdução ao Codespaces][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
