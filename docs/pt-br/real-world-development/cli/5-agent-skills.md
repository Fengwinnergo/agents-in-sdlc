---
title: "Lição 5 - Personalize e use uma skill de verificações de qualidade"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Programar envolve mais do que apenas escrever código. Você já validou manualmente se o código funciona e usou arquivos de instruções para garantir que ele siga os padrões da equipe. Mas e os testes, o lint e as outras partes da integração contínua (CI)?

Para esse tipo de tarefa, as **skills de agente** são a melhor opção. Elas ajudam o Copilot a executar operações como essas da maneira adequada.

Nesta lição, você vai:

- explorar a skill `quality-checks` existente;
- personalizar o formato dos resultados;
- recarregar e executar a skill.

## Cenário

O Tailspin Toys usa a skill `quality-checks` para executar testes unitários, lint e verificações de tipos. A equipe quer aprimorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As skills de agente reúnem instruções reutilizáveis para tarefas, scripts executáveis e recursos de apoio que o agente carrega quando necessário. Em geral, uma skill é uma pasta com o nome dela e um arquivo Markdown chamado `SKILL.md`. Esse arquivo contém frontmatter com um nome e uma descrição que definem a skill, além de uma visão geral e orientações sobre quando ela deve ser chamada. A pasta também pode conter subpastas com scripts e outros recursos que a skill usa.

> [!NOTE]
> Pastas e arquivos adicionais não são obrigatórios. A skill `quality-checks` do Tailspin Toys contém somente `SKILL.md` porque usa os comandos já existentes no projeto.

As skills podem ficar na pasta `.github/skills` do projeto, como um recurso do repositório que a equipe pode compartilhar e reutilizar, ou na pasta de skills do usuário, em `~/.copilot/skills`.

## Explore a skill

Vamos explorar a skill `quality-checks` que a equipe do Tailspin Toys criou para executar testes unitários, lint e verificações de tipos.

1. Volte ao seu Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia o `name` e a `description` no início do arquivo. A descrição ajuda o Copilot a identificar quando deve chamar a skill.
3. Leia as instruções e observe como elas orientam o Copilot durante os testes e a execução do lint.
4. Observe que a skill ainda não tem uma seção **Results output formatting**.

## Execute a skill antes de fazer uma alteração

As skills podem ser chamadas diretamente pelo Copilot CLI ou por meio de linguagem natural. Vamos pedir ao Copilot que execute as três verificações da skill.

1. Volte à conversa de filtragem no modo Interactive.
2. Use este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final.

## Personalize o relatório

Vamos melhorar o relatório para que ele informe o que foi executado, se houve sucesso e quais resultados as ferramentas apresentaram. Atualize a skill para gerar esse relatório.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione a seguinte seção ao final do arquivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. O arquivo será salvo automaticamente.

## Execute a skill atualizada

Agora que fizemos a alteração, vamos ver a skill em ação. O Copilot CLI pode recarregar as skills editadas sem reiniciar a conversa.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Use exatamente o mesmo prompt de antes:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final e compare-o com o primeiro.

## Resumo e próximos passos

Você personalizou e usou uma skill de agente existente. Nesta lição, você:

- explorou a skill `quality-checks` para testes unitários, lint e verificações de tipos;
- personalizou o formato dos resultados;
- recarregou e executou a skill.

Essa alteração será incluída no PR do recurso de filtragem. Em seguida, você permitirá que o Copilot interaja diretamente com o site [por meio do servidor MCP do Playwright][next-lesson].

## Mais exemplos de skills

Estes exemplos da comunidade são referências, não tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e um script de exportação integrado: `drawio`][drawio-example]
- [Testes de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
