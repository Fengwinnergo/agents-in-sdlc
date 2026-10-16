---
title: "Exercício 6 - Itere sobre o trabalho do GitHub Copilot"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← Lição anterior: Monitoramento e gerenciamento de agentes][previous-lesson] |
| :-- |

## Revise o trabalho

Ao longo deste laboratório, você trabalhou com o GitHub Copilot em tarefas para melhorar a experiência do usuário. Usou o modo de agente para implementar a filtragem no cliente e no servidor, o servidor MCP do Playwright para testar manualmente o recurso no navegador e, depois, um agente personalizado para implementar recursos de acessibilidade — modos de alto contraste e claro. Você também orientou a sessão durante a execução para ampliar o trabalho. Agora é hora de publicar as alterações locais e revisá-las como sua equipe faria.

### Cenário

Os fundamentos de design de software e DevOps não mudam com a IA generativa. É necessário manter um ciclo de revisão real para qualquer alteração do Copilot. Vamos enviar as alterações de acessibilidade do Codespace, abrir um pull request e revisar o diff antes de envolver o restante da equipe.

## Publique os recursos de acessibilidade

Os controles dos modos de alto contraste e claro implementados com o agente personalizado de acessibilidade nos [Exercícios 4][exercise-4] e [5][exercise-5] estão salvos como alterações com commit no Codespace. Vamos enviá-las para uma branch e abrir um pull request para revisão da equipe.

1. Volte ao Codespace.
2. Abra a visualização **Source Control** no VS Code.
3. Confirme que as alterações de acessibilidade foram incluídas em um commit. Se houver alterações sem commit do Exercício 5, prepare-as e faça commit agora com uma mensagem descritiva, como `Add high-contrast and light-mode toggles`.
4. Publique a branch selecionando **Publish Branch** (ou use o menu **...** → **Push**).
5. O VS Code oferecerá abrir a nova branch no github.com. Aceite a solicitação ou acesse o repositório manualmente e selecione **Compare & pull request** no aviso da branch.
6. Defina um título claro, como `Add high-contrast and light-mode toggles`, e uma breve descrição do trabalho e da finalidade.
7. Selecione **Create pull request**.
8. Depois de abrir o PR, selecione a guia **Files changed** para revisar todo o trabalho. Preste atenção especial a:
   - componentes de interface para alternar entre os modos;
   - uso do armazenamento local para persistir as preferências;
   - alterações de CSS ou estilo dos modos de alto contraste e claro;
   - atributos de acessibilidade (rótulos ARIA, navegação por teclado etc.);
   - código JavaScript/TypeScript que controla a alternância dos modos.
9. Volte à guia **Conversation**.
10. Se houver fluxos de trabalho aguardando aprovação, selecione **Approve and run workflows**.

    ![Aprovar e executar fluxos de trabalho](../../../_images/shared-approve-workflows.png)

11. Aguarde a conclusão dos fluxos de trabalho. Se tudo correr bem, as verificações serão aprovadas.

> [!TIP]
> Quer outra opinião sobre a acessibilidade? Mencione `@copilot` em um comentário do PR e solicite, por exemplo, "review this PR for additional WCAG issues" ou "suggest improvements to the keyboard navigation". O Copilot iniciará uma nova sessão para atender ao comentário.

## Exercício opcional — continue explorando localmente

Trabalhar iterativamente com um agente no IDE é uma habilidade que se desenvolve com a prática. Algumas ideias para sessões de acompanhamento no VS Code:

- adicionar um formulário de interesse de apoiadores à página de detalhes do jogo;
- implementar paginação na página da lista de jogos;
- adicionar validação de entrada e tratamento de erros aos auxiliares de acesso a dados em `src/lib/`;
- ampliar o escopo do agente de acessibilidade, por exemplo, para auditar a ordem de foco pelo teclado em todo o site.

## Resumo

Parabéns — você concluiu o percurso do VS Code! Neste laboratório, você:

- **Usou o MCP do Playwright para testar manualmente o recurso.** Adicionou o servidor MCP do Playwright e permitiu que o Copilot controlasse um navegador para verificar a filtragem antes de abrir um pull request.
- **Usou o modo de agente para coordenar alterações na pilha.** Adicionou em uma única sessão um recurso de filtragem que alterou o cliente, o servidor e os testes.
- **Usou um agente personalizado.** Selecionou o agente voltado à acessibilidade e acompanhou sua implementação do modo de alto contraste no repositório.
- **Gerenciou e orientou uma sessão do agente.** Revisou as alterações propostas, aceitou as desejadas e ampliou a sessão com uma solicitação para adicionar o modo claro.
- **Concluiu o processo com um pull request.** Publicou o trabalho local e o revisou por completo, como sua equipe faria.

## Revisão e próximos passos

Você concluiu o percurso obrigatório do VS Code. Pode parar por aqui; o workshop está completo.

Se quiser explorar outros recursos de agente do Copilot, os demais percursos abordam cenários relacionados por outras interfaces:

- 💻 **[Percurso da CLI](../../cli/)** — entregue duas alterações revisadas pelo terminal, usando os modos Plan e Autopilot, skills, um agente personalizado de QA, MCP do Playwright, Agent Merge e transições contextualizadas entre tarefas.
- ☁️ **[Percurso do agente de nuvem](../../cloud/)** — concentre-se em atribuir issues ao agente de nuvem, monitorar sessões pela página de agentes e iterar de forma assíncrona sobre pull requests.

Você também pode continuar o trabalho iniciado. O [awesome-copilot][awesome-copilot] é uma ótima fonte de arquivos de instruções, agentes personalizados e skills que podem ser adaptados ao seu projeto.

Como extensão opcional, [Opcional: Incorpore o Foundry][exercise-7] usa o VS Code e o Microsoft Foundry Toolkit para preparar um modelo, implantar o Backer Concierge e conectá-lo ao site.

## Recursos

- [GitHub Copilot][github-copilot]
- [Copilot Chat no VS Code][copilot-chat-vscode]
- [Usar o modo de agente][agent-mode]

---

| [← Lição anterior: Gerenciamento de agentes][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
