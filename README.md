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

### Exercício 1 - Pipeline no Jenkins

Arquivo criado:

```text
.jenkins/Jenkinsfile
```

Configurações no Jenkins:

```text
Tipo do job: Multibranch Pipeline
Build Configuration > Script Path: .jenkins/Jenkinsfile
NodeJS Tool: NodeJS 24
Global npm packages: yarn
```

Etapas executadas:

1. Checkout do código-fonte.
2. Validação do ambiente.
3. Instalação das dependências.
4. Execução dos testes unitários.
5. Instalação dos navegadores do Playwright.
6. Execução dos testes E2E.
7. Publicação dos resultados de teste.
8. Arquivamento dos relatórios como artefatos.

Comandos usados na pipeline:

```shell
node --version
npm --version
yarn --version
yarn install --frozen-lockfile
yarn run test
yarn playwright install
yarn run e2e
```

Relatórios e artefatos:

```text
results.xml
playwright-report/**
reports/coverage/**
```

### Exercício 2 - Actions do GitHub Marketplace

Arquivo atualizado:

```text
.github/workflows/01-manual-exec.yaml
```

Actions adicionadas:

```yaml
uses: dorny/test-reporter@v3
```

```yaml
uses: actions/upload-artifact@v7.0.1
```

Actions atualizadas:

```yaml
uses: actions/checkout@v5
uses: actions/setup-node@v5
```

Permissões configuradas:

```yaml
permissions:
  contents: read
  checks: write
```

Relatórios e artefatos:

```text
results.xml
playwright-report/
```

### Exercício 3 - Self-hosted agent no Jenkins

Configuração do agent:

```text
Nome do nó: pgats-agent-local
Tipo: Permanent Agent
Número de executores: 1
Diretório raiz remoto: /Users/fernando.lopes/jenkins-agent
Rótulo: pgats-agent-local
Uso: Deixar o processamento para atividades vinculadas
Método de lançamento: Lançar um agente conectando-o ao controlador
```

Trecho atualizado no `.jenkins/Jenkinsfile`:

```groovy
agent {
    label 'pgats-agent-local'
}
```

Comandos para conexão do agent:

```shell
curl -sO http://localhost:8080/jnlpJars/agent.jar
java -jar agent.jar -url http://localhost:8080/ -secret <secret> -name "pgats-agent-local" -webSocket -workDir "/Users/fernando.lopes/jenkins-agent"
```
