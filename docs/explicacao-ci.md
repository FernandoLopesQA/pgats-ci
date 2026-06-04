# Explicação das Implementações de CI

## Exercício 1 - Jenkins

Foi implementada uma pipeline de Integração Contínua no Jenkins utilizando uma Multibranch Pipeline. Essa abordagem permite que o Jenkins identifique branches do repositório que contenham o arquivo `.jenkins/Jenkinsfile` e execute a pipeline correspondente.

A pipeline executa as etapas principais do projeto: checkout do código, configuração do ambiente Node.js, instalação de dependências, testes unitários e testes end-to-end com Playwright. Ao final, o Jenkins publica o arquivo `results.xml` como relatório de testes e arquiva os relatórios HTML do Playwright e de cobertura como artefatos.

## Exercício 2 - GitHub Marketplace

Foram adicionadas actions do GitHub Marketplace ao workflow do GitHub Actions para melhorar a visibilidade dos resultados.

A action `dorny/test-reporter@v3` lê o arquivo `results.xml`, gerado pelo Playwright no formato JUnit, e publica um resumo visual dos testes na execução do workflow.

A action `actions/upload-artifact@v7.0.1` salva o relatório HTML do Playwright como artefato, permitindo baixar e consultar o relatório completo após a finalização da pipeline.

Também foram atualizadas as actions `actions/checkout` e `actions/setup-node` para versões compatíveis com runtime Node.js 24, removendo o aviso de depreciação relacionado ao Node.js 20.

## Exercício 3 - Self-hosted Agent

Foi configurado um self-hosted agent no Jenkins chamado `pgats-agent-local`. Esse agent foi conectado ao controller Jenkins por meio do `agent.jar` e configurado com o rótulo `pgats-agent-local`.

O `.jenkins/Jenkinsfile` foi ajustado para executar a pipeline especificamente nesse agent, usando o label configurado.

Self-hosted agents fazem sentido quando a pipeline precisa de maior controle sobre o ambiente de execução, acesso a recursos internos, ferramentas específicas instaladas na máquina, dependências locais, hardware dedicado ou integração com redes privadas.

Outras plataformas oferecem recursos similares:

1. GitHub Actions oferece self-hosted runners.
2. Azure DevOps oferece self-hosted agents.
3. GitLab CI oferece GitLab Runner.
4. CircleCI oferece self-hosted runners.
5. Jenkins oferece agents/nodes conectados ao controller.
