---
title: "Lição 4 - Oriente o Copilot com instruções personalizadas"
description: "Explore as instruções do repositório, adicione um padrão de documentação e aplique-o ao código de filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

O contexto é fundamental ao trabalhar com IA generativa. Se uma tarefa precisa ser executada de uma forma específica, disponibilize essa orientação ao Copilot. Os [arquivos de instruções][instruction-files] descrevem não apenas *o que* você quer que o código faça, mas também *como* ele deve ser estruturado. Agora que você desenvolveu a filtragem, explorará as instruções usadas pelo Copilot, adicionará um padrão de documentação e o aplicará ao código.

Nesta lição, você vai:

- explorar como as instruções do repositório e os arquivos de instruções aplicáveis a caminhos específicos chegam ao agente;
- atualizar o arquivo de instruções para garantir que os padrões de código sejam seguidos;
- observar o efeito dos arquivos de instruções no código.

## Cenário

Como qualquer boa empresa de desenvolvimento, a Tailspin Toys tem diretrizes e requisitos para as práticas de desenvolvimento. Entre eles:

- Os comentários devem explicar a intenção e as decisões não óbvias, em vez de apenas repetir o código.
- As funções exportadas em `db/` e `src/lib/` devem documentar a finalidade, os parâmetros e os valores retornados com TSDoc/JSDoc, incluindo um argumento `db` injetável, quando houver.
- Os componentes reutilizáveis do Astro devem documentar os contratos de `Props`, e os comentários precisam ser atualizados quando o código relacionado mudar.
- As orientações existentes de formatação e lint devem ser preservadas.

Com os arquivos de instruções, você garantirá que o Copilot tenha o contexto necessário para executar as tarefas de acordo com essas práticas.

## Arquivos de instruções

As instruções personalizadas fornecem contexto e preferências ao Copilot para que ele compreenda melhor seu estilo de programação e seus requisitos. Esse recurso pode orientar o Copilot a gerar sugestões e trechos de código mais relevantes. Você pode especificar convenções de programação, bibliotecas e até os tipos de comentários que prefere. É possível criar instruções para todo o repositório ou para tipos específicos de arquivo.

Há dois tipos de arquivos de instruções:

- `.github/copilot-instructions.md`: um único arquivo enviado ao Copilot em **todas** as solicitações referentes ao repositório. Ele deve conter informações relevantes para a maioria das solicitações enviadas ao Copilot.
- `.github/instructions/*.instructions.md`: arquivos que fornecem orientações para linguagens, tipos de arquivo ou tarefas específicas.

> [!NOTE]
> O suporte a outros formatos de instrução varia conforme o ambiente. Consulte a [referência de suporte a instruções personalizadas][custom-instructions-support] antes de depender de um formato específico.

## Explore os arquivos de instruções personalizadas do projeto

O projeto inicial já inclui vários arquivos de instruções. Vamos explorá-los antes de fazer uma alteração para observar o efeito.

1. Volte ao Codespace.
2. No editor do Codespaces, e não no terminal, abra `.github/copilot-instructions.md`.
3. Explore o arquivo, observando a breve descrição do projeto e as orientações de código. Essas instruções se aplicam a todas as interações com o Copilot neste repositório.
4. Abra a pasta `.github/instructions` e explore os arquivos. Observe as instruções para arquivos Astro, a camada de dados Drizzle, testes e outros assuntos.
5. Abra `.github/instructions/unit-tests.instructions.md`. Observe o campo `applyTo` no início: ele define um glob que determina quais arquivos receberão as instruções.
6. Abra `.github/instructions/drizzle.instructions.md` e observe os links para outros arquivos de instruções e para arquivos do projeto. Isso permite dividir instruções maiores em arquivos reutilizáveis menores e apontar o Copilot para exemplos.

## Atualize as instruções conforme as orientações da equipe

Os arquivos existentes são um bom começo, mas ainda falta algo. Vamos modificar o arquivo principal `copilot-instructions.md` para que novos arquivos TypeScript incluam comentários TSDoc.

1. Em `.github/copilot-instructions.md`, localize a seção **Code formatting guidance**, que deve estar próxima à linha 35.
2. Adicione o item a seguir como o último marcador da seção:

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

O arquivo será salvo automaticamente.

## Use a orientação atualizada

O Copilot CLI carrega as instruções do repositório quando uma conversa começa. Depois de salvar as instruções atualizadas, saia da conversa e retome a conversa de filtragem para que o Copilot inicie com a nova orientação sem perder o contexto do recurso.

1. Se a conversa de filtragem ainda estiver aberta, digite `/exit` para fechá-la. O Copilot CLI salva as conversas automaticamente.
2. Na raiz do repositório, na branch `game-filters-cli`, retome a conversa salva:

   ```bash
   copilot --resume
   ```

   Se solicitado, selecione a conversa de filtragem e confirme que a branch selecionada é `game-filters-cli`.
3. Peça ao Copilot que aplique a orientação atualizada:

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. Digite `/diff` e revise os arquivos TypeScript alterados. Observe os comentários TSDoc adicionados e confirme que eles descrevem o código com precisão.

## Resumo e próximos passos

Você explorou como o Copilot obtém contexto dos arquivos de instruções e aplicou um novo padrão ao recurso. Nesta lição, você:

- explorou como as instruções do repositório e os arquivos aplicáveis a caminhos específicos chegam ao agente;
- atualizou o arquivo de instruções para garantir que os padrões de código sejam seguidos;
- observou o efeito dos arquivos de instruções no código.

Em seguida, você [personalizará e executará a skill reutilizável `quality-checks`][next-lesson] para garantir que o lint e os testes sejam executados de maneira consistente.

## Recursos

- [Adicionar instruções personalizadas ao Copilot CLI][instruction-files]
- [Suporte a instruções personalizadas][custom-instructions-support]
- [Práticas recomendadas para criar instruções personalizadas][instructions-best-practices]
- [Awesome Copilot — coleção de arquivos de instruções e outros recursos][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
