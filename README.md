[![Code coverage badge](https://img.shields.io/badge/coverage-100%25-brightgreen)](https://stryker-mutator.io/robo-coasters-example/reports/coverage/lcov-report/index.html)
[![Mutation testing badge](https://img.shields.io/endpoint?style=flat&url=https%3A%2F%2Fbadge-api.stryker-mutator.io%2Fgithub.com%2Fstryker-mutator%2Frobo-coasters-example%2Fmaster)](https://dashboard.stryker-mutator.io/reports/github.com/stryker-mutator/robo-coasters-example/master)

# PGATS - CI

## Pré-requisitos

1. Instale o [git](https://git-scm.com)
2. Instale o [nodejs](https://nodejs.org/)
3. Instale o Yarn - `npm install -g yarn`
4. Faça um _Fork_ do projeto
5. Clone o repositório para sua máquina (seu fork)
6. Instale as dependências
   ```shell
   cd pgats-ci
   yarn
   ```
7. Execute os testes de unidade - isso vai gerar um relatório
   ```shell
   yarn run test
   ```
8. Abra o relatório de cobertura de código em `reports/coverage/lcov-report`
9. Execute os testes de mutação com o Stryker
   ```shell
   yarn run test:mutation
   ```
10. Abra o relatório de mutação em `reports/mutation`
11. Instale os navegadores do Playwright
    ```shell
    yarn playwright install
    ```
12. Execute os testes end-to-end com o Playwright
    ```shell
    yarn run e2e
    ```
13. Execute a aplicação com `yarn start`
14. Acesse a aplicação publicada [neste link](https://pgats-ci-example.netlify.app)

---

💜⚡️
# pgats-ci

---

## Implementações de Integração Contínua

Esta seção registra as implementações realizadas para os exercícios de Integração Contínua, mantendo o conteúdo original do projeto sem alterações.

### Exercício 1 - Pipeline no Jenkins

Foi implementada uma pipeline de CI no Jenkins utilizando uma Multibranch Pipeline.

A configuração da pipeline foi criada no arquivo `.jenkins/Jenkinsfile`, permitindo que o Jenkins identifique branches do repositório que contenham esse arquivo e execute o fluxo automaticamente.

Etapas configuradas na pipeline Jenkins:

1. Checkout do código-fonte.
2. Configuração do ambiente com Node.js 24.
3. Validação das versões de Node.js, npm e Yarn.
4. Instalação das dependências com `yarn install --frozen-lockfile`.
5. Execução dos testes unitários com `yarn run test`.
6. Instalação dos navegadores do Playwright com `yarn playwright install`.
7. Execução dos testes end-to-end com `yarn run e2e`.
8. Publicação dos resultados de teste no Jenkins a partir do arquivo `results.xml`.
9. Arquivamento dos relatórios `playwright-report` e `reports/coverage` como artefatos da build.

Com isso, o projeto passou a ter uma pipeline equivalente em outra ferramenta de Integração Contínua, além do GitHub Actions.

### Exercício 2 - Actions do GitHub Marketplace

Foram adicionadas actions do GitHub Marketplace ao workflow `.github/workflows/01-manual-exec.yaml` para melhorar a visibilidade dos resultados da pipeline.

Actions implementadas:

1. `dorny/test-reporter@v3`

   Utilizada para ler o arquivo `results.xml`, gerado pelo Playwright, e publicar um resumo visual dos testes no GitHub Actions.

2. `actions/upload-artifact@v7.0.1`

   Utilizada para salvar o relatório HTML do Playwright como artefato da execução, permitindo baixar e consultar o relatório completo após a finalização da pipeline.

Também foram atualizadas as actions oficiais `actions/checkout` e `actions/setup-node` para versões compatíveis com runtime Node.js 24, removendo o aviso de depreciação relacionado ao Node.js 20.
