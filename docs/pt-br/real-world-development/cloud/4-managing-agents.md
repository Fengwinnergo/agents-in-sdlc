---
title: "Exercício 4 - Monitoramento e gerenciamento de agentes"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Nos últimos exercícios, você pediu ao agente de nuvem do Copilot que executasse três tarefas diferentes para melhorar a experiência do usuário e adicionar funcionalidades. Embora o agente de nuvem tenha sido criado para trabalhar de maneira autônoma e assíncrona, ainda é importante acompanhar essas tarefas.

Há várias ferramentas para gerenciar tarefas atribuídas ao agente de nuvem, incluindo a [página de agentes][agents-page] do GitHub.com. Nesse centro de controle, você pode ver todas as tarefas de agentes que têm pull requests (PRs) abertos. Também pode explorar as operações realizadas e orientar uma sessão em andamento.

Nesta lição, você vai:

- explorar a página de agentes para monitorar as tarefas do agente de nuvem;
- orientar uma sessão em andamento para solicitar uma funcionalidade adicional.

## Cenário

Depois de atribuir ao agente a tarefa de criar um modo de alto contraste, a equipe percebeu que também seria interessante adicionar um modo claro. Como o trabalho já envolve atualizar o estilo do site e adicionar controles de alternância, incluir essa funcionalidade parece fazer sentido. Você orientará o agente para garantir que ele adicione o modo claro junto com o alto contraste.

## Revise as tarefas do agente de nuvem do Copilot

Vamos conferir o estado atual das tarefas atribuídas ao agente de nuvem do Copilot.

1. Acesse a [página de agentes do Copilot](https://github.com/copilot/agents).
2. Observe a lista de tarefas no painel principal e no painel à esquerda. Você deverá ver as tarefas atribuídas ao Copilot, incluindo:
   - atualizar a documentação da base de código;
   - gerar APIs para modificar jogos;
   - adicionar um modo de alto contraste ao site.
3. Selecione uma tarefa em execução e revise o trabalho feito pelo Copilot. As etapas podem incluir:
   - obter uma cópia do código do repositório;
   - criar o ambiente de trabalho do Copilot;
   - configurar servidores MCP;
   - executar várias etapas para concluir a tarefa.

> [!NOTE]
> As etapas exibidas variam conforme o estado do trabalho do Copilot e a abordagem adotada.

4. Observe também o painel de pull requests à direita. Ele permite ver o PR e os arquivos alterados para acompanhar o trabalho.

## Oriente o agente de nuvem

Agora que você viu as tarefas em andamento, vamos pedir ao Copilot que inclua uma opção de modo claro enquanto trabalha no modo de alto contraste.

1. Selecione a sessão relacionada à criação do modo de alto contraste. O título exato varia conforme o nome escolhido pelo Copilot e o estado atual do trabalho.

   ![Sessão de acessibilidade no controle de agentes](../../../_images/ex6-accessibility-session.png)

2. Acompanhe a sessão por alguns minutos, até que a configuração seja concluída e o agente comece a trabalhar. Você verá mensagens semelhantes às exibidas abaixo.
3. Na caixa de diálogo **Steer active session while Copilot is working**, digite este prompt:

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![Captura de tela da tarefa do agente de nuvem na página de agentes, com a caixa de diálogo de orientação da sessão ativa destacada](../../../_images/ex6-steer-cloud-agent-task.png)

4. Pressione <kbd>Enter</kbd> para enviar o prompt.
5. Observe que o Copilot confirma o recebimento do prompt e o incorpora ao fluxo de trabalho.

## Deixe o Copilot trabalhar

Como antes, o Copilot começará a trabalhar na tarefa atualizada. Ele incorporará a nova solicitação depois de concluir a etapa em andamento quando você enviou a mensagem.

Isso levará alguns minutos, então aproveite para fazer uma pausa e refletir sobre o que aprendeu até aqui.

## Resumo e próximos passos

Neste exercício, você explorou a página de agentes do Copilot, o ponto central para monitorar e orientar as tarefas do agente de nuvem do GitHub Copilot. Com esse controle, você acompanha tarefas ativas e concluídas, revisa o trabalho realizado e redireciona sessões em andamento para ajustar o escopo ou fornecer orientações.

Você explorou estes conceitos:

- uso do controle de agentes e da página de agentes para monitorar tarefas;
- redirecionamento de uma sessão em andamento para solicitar funcionalidade adicional.

Enquanto o Copilot conclui o trabalho nos recursos de acessibilidade, vamos [iterar sobre os pull requests criados pelo Copilot][next-lesson].

## Recursos

- [Página de agentes do Copilot][agents-page]
- [Sobre os agentes personalizados][custom-agents]

---

| [← Lição anterior: Agentes personalizados][previous-lesson] | [Próxima lição: Iterar sobre o trabalho do Copilot →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
