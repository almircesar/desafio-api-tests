# Desafio API Tests - Testes de API de Usuários

> Coleção de testes automatizados para a API de Usuários utilizando Postman

## 📋 Descrição do Projeto

Este projeto contém uma suite completa de testes automatizados para a API de Usuários do [Serverest](https://serverest.dev). Os testes cobrem fluxos de autenticação e operações CRUD (Create, Read, Update, Delete) de usuários, incluindo validações de casos positivos e negativos.

## 🎯 Objetivo

Validar a funcionalidade, integridade e comportamento esperado da API de Usuários através de testes automatizados implementados no Postman.

## 📁 Estrutura do Projeto

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

Após importar os arquivos:

1. No Postman, acesse o ícone de engrenagem (⚙️) no canto superior direito
2. Selecione **Manage Environments**
3. Escolha o ambiente **DEV** importado
4. Configure as variáveis conforme necessário:
   - `baseUrl`: URL base da API (padrão: `https://serverest.dev`)
   - `token`: Token de autenticação (preenchido automaticamente após login)
   - `userId`: ID do usuário (preenchido durante testes)

### Executar os Testes

#### Via Postman (GUI)

1. Selecione a coleção **Desafio API Users** no painel esquerdo
2. Clique em **Run** (ícone de play)
3. Escolha o ambiente **DEV**
4. Clique em **Run Desafio API Users**

#### Via Postman CLI (Newman)

```bash
# Instalar Newman (se não estiver instalado)
npm install -g newman

# Executar a coleção
newman run collections/users_collection.json \
  -e environments/dev_environment.json \
  -r html \
  -o reports/report.html
```

## 📝 Testes Inclusos

### 1. **Autenticação (Auth)**

- **Login**: Valida acesso com credenciais válidas
  - Método: `POST /login`
  - Credenciais padrão: `fulano@qa.com` / `teste`
  - Validações: Status 200, recebimento de token

### 2. **Usuários - Casos Positivos (Users - Positive)**

- **Criar Usuário**: Testa criação bem-sucedida de novo usuário
  - Método: `POST /usuarios`
  - Validações: Status 201, resposta com ID e mensagem de sucesso
  - Dados gerados dinamicamente com timestamp para evitar conflitos

## 🔧 Variáveis de Ambiente

| Variável | Descrição | Exemplo |
|----------|-----------|---------|
| `baseUrl` | URL base da API | `https://serverest.dev` |
| `token` | Token JWT para autenticação | `eyJhbGciOi...` |
| `userId` | ID do usuário autenticado | `507f1f77...` |
| `email` | Email do usuário | `teste@email.com` |
| `nome` | Nome do usuário | `Teste Usuario` |
| `password` | Senha do usuário | `123456` |
| `administrador` | Flag de administrador | `true` / `false` |

## 📊 Relatórios

Os relatórios de execução dos testes são gerados automaticamente em `reports/report.html`. 

Para gerar um novo relatório:

```bash
newman run collections/users_collection.json \
  -e environments/dev_environment.json \
  --reporters html \
  --reporter-html-export reports/report.html
```

## 🔄 Fluxo de Execução

```
1. Login → Obtém token de autenticação
   ↓
2. Usar token → Configurado automaticamente nas requisições
   ↓
3. Criar Usuário → Testa criação com dados válidos
   ↓
4. Validações → Verifica status, resposta e dados retornados
```

## 💡 Dicas

- Os testes usam **scripts de pre-request** para gerar dados dinâmicos (timestamps) e evitar conflitos
- Os **testes automatizados** validam status HTTP, estrutura de resposta e dados
- O **token** é armazenado em variável de ambiente após login bem-sucedido
- Cada execução cria um novo usuário com email único

## 📚 Recursos Úteis

- [Documentação Serverest](https://serverest.dev/)
- [Documentação Postman](https://learning.postman.com/docs/getting-started/overview/)
- [Postman API Testing](https://learning.postman.com/docs/writing-scripts/test-scripts/)

## 📄 Licença

Este projeto é fornecido como está para fins educacionais e de testes.

## ✍️ Autor

Criado para o desafio de testes de API

---

**Última atualização**: 10 de maio de 2026
