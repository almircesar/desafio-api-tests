# Desafio API Tests - Suite de Testes Automatizados para API de Usuários

> Automação completa de testes de API utilizando Postman e Newman com foco em qualidade, cobertura de cenários e geração de relatórios detalhados

## 📋 Descrição do Projeto

Este projeto implementa uma suite robusta de testes automatizados para validação da API de Usuários do [Serverest](https://serverest.dev). A solução abrange toda a jornada do usuário, desde autenticação até operações CRUD (Create, Read, Update, Delete), com testes estruturados para cenários de sucesso e tratamento de erros. 

A automação é construída com foco em **manutenibilidade**, **escalabilidade** e **geração de relatórios profissionais**, permitindo integração contínua com pipelines de CI/CD.

## 🎯 Objetivo

Garantir a qualidade, confiabilidade e consistência da API de Usuários através de testes automatizados abrangentes, validando funcionalidades críticas, tratamento de erros e conformidade com especificações de contrato de API.

## � Tecnologias Utilizadas

| Tecnologia | Versão | Propósito |
|-----------|---------|----------|
| **Postman** | 8.0+ | Ferramenta de teste e automação de API |
| **Newman** | CLI | Executor de coleções Postman via linha de comando |
| **JavaScript** | ES6+ | Scripts de pré-requisição, testes e manipulação de dados |
| **HTMLExtra Reporter** | Latest | Geração de relatórios visuais e detalhados |
| **Serverest API** | v2 | API de teste para testes de usuários |
| **Node.js** | 14+ | Runtime para execução de Newman e reporters |

## 🧪 Estratégia de Testes

### Abordagem de Cobertura

A suite de testes foi estruturada utilizando as seguintes estratégias:

#### **Fluxos Positivos**
- ✅ Validação de operações bem-sucedidas (caminho feliz)
- ✅ Testes de autenticação com credenciais válidas
- ✅ Operações CRUD completas com dados válidos
- ✅ Encadeamento de requests com persistência de estado

#### **Fluxos Negativos**
- ❌ Validação de campos obrigatórios (nome, email, password, administrador)
- ❌ Validação de formato de dados (emails inválidos, IDs malformados)
- ❌ Validação de regras de negócio (emails duplicados, recursos inexistentes)
- ❌ Testes de autenticação com credenciais inválidas

### Técnicas Implementadas

**Persistência de Variáveis de Ambiente**
- Armazenamento dinâmico de token de autenticação após login
- Armazenamento de IDs de usuários para utilização em testes subsequentes
- Geração de dados únicos com timestamps para evitar conflitos de duplicação

**Encadeamento de Requests**
- Fluxo integrado: Login → Criar Usuário → Buscar → Atualizar → Deletar
- Validação de estado entre requests
- Reutilização de dados gerados em requests anteriores

**Validações Implementadas**
- 🔹 **Status HTTP**: Validação de códigos de resposta esperados (200, 201, 400, 401)
- 🔹 **Payload de Resposta**: Verificação de estrutura e presença de campos obrigatórios
- 🔹 **Integridade de Dados**: Confirmação de valores antes/depois de operações
- 🔹 **Mensagens de Erro**: Validação de mensagens de erro específicas por cenário
- 🔹 **Tipo de Dados**: Garantia de tipos corretos na resposta (string, boolean, etc)

**Geração Dinâmica de Massa de Testes**
```javascript
// Exemplo: Geração de email único com timestamp
const unique = Date.now();
pm.environment.set("email", "teste." + unique + "@email.com");
```
- Cada execução gera novos dados para evitar dependências entre testes
- Timestamps garantem unicidade de emails e elimina necessidade de cleanup manual

## �📁 Estrutura do Projeto

```
desafio-api-tests/
├── collections/              # Coleções Postman
│   └── users_collection.json # Coleção com testes de usuários
├── environments/             # Ambientes configuráveis
│   └── dev_environment.json  # Variáveis para o ambiente de desenvolvimento
├── reports/                  # Relatórios de execução
│   └── report.html          # Relatório HTML dos testes
└── README.md                 # Este arquivo
```

## 🚀 Como Usar

### Pré-requisitos

- [Postman](https://www.postman.com/downloads/) instalado (versão 8.0 ou superior)
- Git (opcional, para clonar o repositório)

### Importação da Coleção

1. Abra o Postman
2. Clique em **Import** (botão superior esquerdo)
3. Selecione **Upload Files**
4. Navegue até o diretório do projeto e selecione o arquivo `collections/users_collection.json`
5. Repita o processo para importar o ambiente: `environments/dev_environment.json`

### Configuração do Ambiente

Após importar os arquivos na interface do Postman:

1. Acesse o ícone de engrenagem (⚙️) no canto superior direito
2. Selecione **Manage Environments**
3. Escolha o ambiente **DEV** importado
4. Revise as variáveis de ambiente (serão preenchidas automaticamente durante a execução dos testes):

| Variável | Descrição | Preenchimento |
|----------|-----------|--------------|
| `baseUrl` | URL base da API | Manual (padrão: `https://serverest.dev`) |
| `token` | Token JWT de autenticação | Automático (após execução do teste de Login) |
| `userId` | ID do usuário criado | Automático (após execução do teste de Criar Usuário) |
| `email` | Email do usuário | Automático (gerado dinamicamente com timestamp) |
| `nome` | Nome do usuário | Automático (gerado dinamicamente com timestamp) |
| `password` | Senha do usuário | Automático (definido nos scripts de pré-requisição) |
| `administrador` | Flag de administrador | Automático (definido nos scripts de pré-requisição) |

**Nota sobre o Token**: O token é obtido automaticamente no primeiro teste (Login) e armazenado na variável de ambiente `token`. Este token é utilizado em requisições subsequentes que requerem autenticação. A persistência automática garante que testes encadeados funcionem sem intervenção manual.

### Executar os Testes

#### 🖥️ Via Interface do Postman (GUI)

Para executar os testes através da interface gráfica do Postman:

1. Selecione a coleção **Desafio API Users** no painel esquerdo
2. Clique no botão **Run** (ícone de play)
3. Configure as opções de execução:
   - Escolha o ambiente **DEV**
   - Verifique o delay entre requisições
4. Clique em **Run Desafio API Users** para iniciar
5. Acompanhe o progresso e resultados em tempo real

#### 💻 Via Linha de Comando (Newman)

Newman permite executar testes de forma automatizada, ideal para CI/CD. Abaixo estão as instruções e exemplos.

##### 1. Instalar Newman e Reporters

O Newman é uma ferramenta CLI que permite executar coleções do Postman via linha de comando. Para instalar:

```bash
# Instalar Newman globalmente
npm install -g newman

# Instalar o reporter HTMLExtra (para gerar relatórios em HTML com mais detalhes)
npm install -g newman-reporter-htmlextra
```

**Nota**: O reporter `htmlextra` oferece um relatório HTML mais visual e detalhado do que o reporter padrão. Se você usar o flag `-r htmlextra`, o Newman tentará usar este reporter. Se não estiver instalado, retornará um erro indicando que é necessário instalar.

##### 2. Executar os Testes

```bash
# Comando básico (apenas saída no terminal)
newman run collections/users_collection.json \
  -e environments/dev_environment.json

# Com geração de relatório HTMLExtra
newman run collections/users_collection.json \
  -e environments/dev_environment.json \
  -r cli,htmlextra \
  --reporter-htmlextra-export reports/report.html

# Com relatório detalhado (terminal + HTML)
newman run collections/users_collection.json \
  -e environments/dev_environment.json \
  -r cli,htmlextra \
  --reporter-htmlextra-export reports/report.html
```

##### 3. Entendendo os Reporters

Newman suporta diferentes tipos de reporters. Recomendamos usar **htmlextra** para melhor visualização:

| Reporter | Descrição | Comando |
|----------|-----------|---------|
| **cli** | Saída em terminal (padrão) | `-r cli` |
| **htmlextra** | Relatório HTML avançado e visual | `-r htmlextra --reporter-htmlextra-export reports/report.html` |
| **json** | Saída em formato JSON | `-r json --reporter-json-export reports/report.json` |
| **junit** | Formato JUnit (CI/CD) | `-r junit --reporter-junit-export reports/report.xml` |

##### 4. Exemplos Práticos

```bash
# Apenas saída no terminal
newman run collections/users_collection.json \
  -e environments/dev_environment.json \
  -r cli

# Gerar apenas relatório HTMLExtra (RECOMENDADO)
newman run collections/users_collection.json \
  -e environments/dev_environment.json \
  -r htmlextra \
  --reporter-htmlextra-export reports/report.html

# Com terminal + relatório HTMLExtra
newman run collections/users_collection.json \
  -e environments/dev_environment.json \
  -r cli,htmlextra \
  --reporter-htmlextra-export reports/report.html

# Gerar múltiplos relatórios (HTML + JSON)
newman run collections/users_collection.json \
  -e environments/dev_environment.json \
  -r htmlextra --reporter-htmlextra-export reports/report.html \
  -r json --reporter-json-export reports/report.json

# Com opções adicionais (timeout, iterações)
newman run collections/users_collection.json \
  -e environments/dev_environment.json \
  --request-timeout 5000 \
  -r cli,htmlextra \
  --reporter-htmlextra-export reports/report.html
```

##### 5. Troubleshooting

Se receber erro "Reporter not found":

```bash
# Verifique se o reporter está instalado
npm list -g newman-reporter-htmlextra

# Se não estiver, instale:
npm install -g newman-reporter-htmlextra

# Ou instale localmente no projeto
npm install newman-reporter-htmlextra --save-dev

# Se receber erro ao exportar, certifique-se que a pasta reports existe
mkdir -p reports
```

## 📝 Testes Inclusos

### 1. **Autenticação (Auth)**

#### ✅ Fluxos Positivos

- **Login com Sucesso**
  - Método: `POST /login`
  - Credenciais: `fulano@qa.com` / `teste`
  - Validação: Status 200, recebimento de token
  - Resultado: Token armazenado automaticamente na variável `token`

### 2. **Usuários - Fluxos Positivos**

#### ✅ Casos de Sucesso (Users - Positive)

- **Criar Usuário**
  - Método: `POST /usuarios`
  - Validações: Status 201, retorna ID e mensagem de sucesso
  - Dados: Gerados dinamicamente com timestamp para evitar duplicação
  - Armazena o `userId` para próximos testes

- **Buscar Usuário por ID**
  - Método: `GET /usuarios/{{userId}}`
  - Validações: Status 200, retorna dados corretos do usuário
  - Verifica: ID, email e nome correspondem aos valores cadastrados

- **Atualizar Usuário**
  - Método: `PUT /usuarios/{{userId}}`
  - Validações: Status 200, retorna mensagem de sucesso
  - Altera: Nome do usuário (com novo timestamp)
  - Mantém: Email, password e administrador inalterados

- **Validar Usuário Atualizado**
  - Método: `GET /usuarios/{{userId}}`
  - Validações: Status 200
  - Verifica: Nome foi atualizado corretamente
  - Verifica: Email permanece inalterado

- **Deletar Usuário**
  - Método: `DELETE /usuarios/{{userId}}`
  - Validações: Status 200, retorna mensagem de exclusão
  - Resultado: Usuário removido do banco de dados

- **Validar Usuário Excluído**
  - Método: `GET /usuarios/{{userId}}`
  - Validações: Status 400
  - Verifica: Mensagem "Usuário não encontrado"
  - Confirma: Exclusão foi bem-sucedida

### 3. **Usuários - Fluxos Negativos**

#### ❌ Casos de Erro (Users - Negative)

**Validação de Campos Obrigatórios:**

- **Criar Usuário sem Nome**
  - Método: `POST /usuarios`
  - Validações: Status 400, mensagem "nome não pode ficar em branco"
  - Objetivo: Garantir que o campo nome é obrigatório

- **Criar Usuário sem Email**
  - Método: `POST /usuarios`
  - Validações: Status 400, retorna erro de email
  - Objetivo: Garantir que o campo email é obrigatório

- **Criar Usuário sem Administrador**
  - Método: `POST /usuarios`
  - Validações: Status 400, retorna erro de administrador
  - Objetivo: Garantir que o campo administrador é obrigatório

- **Criar Usuário sem Password**
  - Método: `POST /usuarios`
  - Validações: Status 400, retorna erro de password
  - Objetivo: Garantir que o campo password é obrigatório

**Validação de Formato:**

- **Criar Usuário com Email Inválido**
  - Método: `POST /usuarios`
  - Validações: Status 400, retorna erro de formato de email
  - Objetivo: Validar formato de email (ex: "email-invalido")

- **Buscar Usuário com ID Inválido**
  - Método: `GET /usuarios/id-inexistente`
  - Validações: Status 400, mensagem "id deve ter exatamente 16 caracteres alfanuméricos"
  - Objetivo: Validar formato do ID

**Validação de Regras de Negócio:**

- **Criar Usuário com Email Duplicado (Preparo)**
  - Método: `POST /usuarios`
  - Validações: Status 201 ou 400 (aceita ambos)
  - Objetivo: Cria usuário base para teste de duplicação
  - Email: `duplicado.teste.fixo@email.com` (fixo para reutilização)

- **Criar Usuário com Email Duplicado**
  - Método: `POST /usuarios`
  - Validações: Status 400, mensagem "Este email já está sendo usado"
  - Objetivo: Garantir unicidade de email

- **Deletar Usuário Inexistente**
  - Método: `DELETE /usuarios/1234567890abcdef`
  - Validações: Status 200, mensagem "Nenhum registro excluído"
  - Objetivo: Validar comportamento ao deletar ID que não existe

**Autenticação (Negative):**

- **Login com Senha Inválida**
  - Método: `POST /login`
  - Validações: Status 401, mensagem "Email e/ou senha inválidos"
  - Objetivo: Rejeitar credenciais incorretas

- **Login sem Email**
  - Método: `POST /login`
  - Validações: Status 400, retorna erro de email
  - Objetivo: Garantir que email é obrigatório no login

- **Login sem Password**
  - Método: `POST /login`
  - Validações: Status 400, retorna erro de password
  - Objetivo: Garantir que password é obrigatório no login

##  Relatórios

Os relatórios de execução dos testes são gerados automaticamente em `reports/report.html` usando o reporter **HTMLExtra**, que oferece uma visualização mais completa e interativa.

Para gerar um novo relatório:

```bash
newman run collections/users_collection.json \
  -e environments/dev_environment.json \
  -r cli,htmlextra \
  --reporter-htmlextra-export reports/report.html
```

**Visualizar o relatório**: Abra o arquivo `reports/report.html` em um navegador web para ver:
- Resumo geral de testes (passados, falhados, total de requisições)
- Detalhes de cada teste e validação
- Tempo de resposta de cada requisição
- Status codes e corpos de resposta
- Gráficos e estatísticas

## 🔄 Fluxo de Execução dos Testes

A suite segue um fluxo integrado que simula cenários reais de uso da API:

```
┌─────────────────────────────────────────────────────────────┐
│ 1. LOGIN (POST /login)                                      │
│    • Autentica com credenciais                              │
│    • Obtém token JWT                                        │
│    • Armazena em variável: token                            │
└──────────────────────┬──────────────────────────────────────┘
                       ↓
┌─────────────────────────────────────────────────────────────┐
│ 2. CRIAR USUÁRIO (POST /usuarios)                           │
│    • Gera dados únicos com timestamp                        │
│    • Envia requisição com payload completo                  │
│    • Armazena em variável: userId                           │
└──────────────────────┬──────────────────────────────────────┘
                       ↓
┌─────────────────────────────────────────────────────────────┐
│ 3. BUSCAR USUÁRIO (GET /usuarios/{userId})                  │
│    • Valida dados retornados                                │
│    • Confirma integração com banco                          │
└──────────────────────┬──────────────────────────────────────┘
                       ↓
┌─────────────────────────────────────────────────────────────┐
│ 4. ATUALIZAR USUÁRIO (PUT /usuarios/{userId})               │
│    • Modifica dados (nome com novo timestamp)               │
│    • Valida resposta de sucesso                             │
└──────────────────────┬──────────────────────────────────────┘
                       ↓
┌─────────────────────────────────────────────────────────────┐
│ 5. VALIDAR ATUALIZAÇÃO (GET /usuarios/{userId})             │
│    • Confirma dados foram persistidos                       │
│    • Verifica integridade das alterações                    │
└──────────────────────┬──────────────────────────────────────┘
                       ↓
┌─────────────────────────────────────────────────────────────┐
│ 6. DELETAR USUÁRIO (DELETE /usuarios/{userId})              │
│    • Remove usuário do banco                                │
│    • Valida mensagem de sucesso                             │
└──────────────────────┬──────────────────────────────────────┘
                       ↓
┌─────────────────────────────────────────────────────────────┐
│ 7. VALIDAR EXCLUSÃO (GET /usuarios/{userId})                │
│    • Confirma que usuário foi removido                      │
│    • Valida erro 400 "Usuário não encontrado"               │
└─────────────────────────────────────────────────────────────┘
```

**Testes de Validação de Erros** (Negative Tests) são executados independentemente, simulando cenários de uso indevido da API.

## 💡 Boas Práticas e Implementação

### Padrões Aplicados na Suite

- **Scripts de Pré-Requisição**: Utilizam JavaScript para gerar dados dinâmicos com `Date.now()`, garantindo unicidade de emails e eliminando dependência de limpeza manual entre execuções

- **Testes Encadeados**: Cada teste aproveita dados gerados por testes anteriores através de variáveis de ambiente, criando fluxo realista de uso da API

- **Validações Robustas**: 
  - Status HTTP codes esperados (200, 201, 400, 401)
  - Estrutura de payload e presença de campos obrigatórios
  - Integridade de dados antes/depois de operações
  - Tipos de dados corretos na resposta

- **Persistência de Estado**: Token JWT é capturado após login e automaticamente reutilizado em requisições que requerem autenticação

- **Isolamento de Testes**: Cada execução gera novos dados, permitindo múltiplas execuções sem conflitos de dados duplicados

### Integração com CI/CD

Para integrar esta suite em pipelines automatizados:

```bash
# Exemplo para GitHub Actions ou similar
newman run collections/users_collection.json \
  -e environments/dev_environment.json \
  -r cli,htmlextra \
  --reporter-htmlextra-export reports/report-$(date +%Y%m%d-%H%M%S).html \
  --bail
```

O flag `--bail` interrompe a execução no primeiro erro, útil para pipeline CI/CD.

## 📚 Recursos Úteis

- [Documentação Serverest](https://serverest.dev/)
- [Postman Documentation - Getting Started](https://learning.postman.com/docs/getting-started/overview/)
- [Postman Scripts - Test Scripts](https://learning.postman.com/docs/writing-scripts/test-scripts/)
- [Newman CLI Reference](https://github.com/postmanlabs/newman)

## 📄 Licença

Este projeto é fornecido como está para fins educacionais e de testes.

## ✍️ Autor

Desenvolvido como desafio de automação de testes de API

---

**Última atualização**: 10 de maio de 2026
