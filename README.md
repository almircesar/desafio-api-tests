(The file `/Users/almircesar/desafio-api-tests/README.md` exists, but is empty)
# 🚀 Desafio de Automação de Testes de API

## 📌 Objetivo

Automatizar os testes da API de usuários utilizando boas práticas de QA, cobrindo operações de CRUD, autenticação e cenários negativos, com execução via CLI e integração contínua (CI).

---

## 🛠️ Tecnologias utilizadas

* Postman
* Newman
* Node.js
* GitHub Actions

---

## 📁 Estrutura do projeto

```
desafio-api-tests/
├── collections/
│   └── users_collection.json
├── environments/
│   └── dev_environment.json
├── reports/
│   └── report.html
├── .github/
│   └── workflows/
│       └── api-tests.yml
└── README.md
```

---

## ⚙️ Pré-requisitos

Antes de executar o projeto, é necessário ter instalado:

* Node.js
* Newman
* Newman Reporter (HTML extra)

Instalação global recomendada:

```bash
npm install -g newman newman-reporter-htmlextra
```

> Observação: também é possível instalar localmente e usar via npm scripts se preferir manter dependências no projeto.

---

## ▶️ Execução dos testes

### Execução simples

```bash
newman run collections/users_collection.json -e environments/dev_environment.json
```

---

### Execução com geração de relatório HTML

```bash
newman run collections/users_collection.json -e environments/dev_environment.json -r cli,htmlextra --reporter-htmlextra-export reports/report.html
```

---

## 📊 Relatório de execução

Após a execução, o relatório será gerado em:

```
reports/report.html
```

Para visualizar (macOS):

```bash
open reports/report.html
```

O relatório apresenta:

* total de testes executados
* testes aprovados/reprovados
* tempo de execução
* detalhes de cada cenário

---

## 🔐 Autenticação

A API utiliza autenticação via token (JWT).

* O token é gerado no login
* É armazenado automaticamente no environment
* É utilizado nas requisições autenticadas

---

## ✅ Cenários cobertos

### 🔹 Autenticação

* Login com sucesso
* Login com senha inválida
* Login sem email
* Login sem password

---

### 🔹 Usuários - Cenários positivos

* Criar usuário
* Buscar usuário por ID
* Atualizar usuário
* Validar usuário atualizado
* Deletar usuário
* Validar usuário excluído
* Listar usuários autenticado

---

### 🔹 Usuários - Cenários negativos

* Criar usuário sem nome
* Criar usuário sem email
* Criar usuário sem password
* Criar usuário sem administrador
* Criar usuário com administrador inválido
* Criar usuário com email duplicado
* Buscar usuário com ID inválido
* Buscar usuário inexistente
* Atualizar usuário com ID inválido
* Deletar usuário inexistente

---

## 🔄 Integração Contínua (CI)

O projeto utiliza GitHub Actions para execução automática dos testes.

A pipeline:

* é executada a cada push
* roda os testes com Newman
* gera relatório HTML
* disponibiliza o relatório como artefato

---

## 📦 Relatório na pipeline

Após a execução da pipeline:

1. Acesse a aba **Actions** no GitHub
2. Abra a execução do workflow
3. Baixe o artefato:

```
api-test-report
```

Dentro dele estará o arquivo `report.html`.

---

## 🎯 Considerações finais

* Os testes foram organizados em cenários positivos e negativos
* Foram utilizadas variáveis para tornar os testes reutilizáveis
* A automação foi estruturada para execução local e em CI
* O projeto segue boas práticas de organização e legibilidade

---

## � Observações e recomendações

Para melhorar confiabilidade e usabilidade do projeto, seguem algumas recomendações e notas operacionais:

- Versão do Node: o workflow usa Node.js 20. Se for rodar localmente com npm scripts, use v16+ ou v20 para compatibilidade.
- Execução local (instalação local de dependências):

```bash
# instalar como dependências de desenvolvimento no projeto (recomendado)
npm install --save-dev newman newman-reporter-htmlextra

# exemplo de script npm (adicionar ao package.json)
{
	"scripts": {
		"test:newman": "newman run collections/users_collection.json -e environments/dev_environment.json",
		"test:report": "newman run collections/users_collection.json -e environments/dev_environment.json -r cli,htmlextra --reporter-htmlextra-export reports/report.html"
	}
}
```

- Rede/endpoint: os testes usam `https://serverest.dev` (definido em `environments/dev_environment.json`). Verifique se o endpoint está acessível na sua rede antes de rodar os testes.
- Dados duplicados: a coleção possui um caso de email fixo (`duplicado.teste.fixo@email.com`) para validar cenário de email já cadastrado — isso pode gerar flakiness
	se o recurso já existir no ambiente; recomenda-se usar emails gerados dinamicamente ou limpar o estado entre execuções.
- Dependências de ordem: a coleção contém fluxos dependentes (criar usuário → obter/atualizar → deletar). Ao executar apenas testes individuais, assegure-se de fornecer as variáveis necessárias no environment.
- Logs e troubleshooting:
	- Verifique retorno HTTP e mensagens de erro nos testes para identificar causas (ex.: validações do servidor em PT-BR).
	- Falhas comuns: timeout, endpoint inacessível, parâmetros ausentes no environment, limites de taxa.

---

---

## �👨‍💻 Autor

Desenvolvido por Almir Cesar

