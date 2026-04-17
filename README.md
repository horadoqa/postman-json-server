# 📦 JSON Server + Postman Tests

![API Tests](https://github.com/horadoqa/postma-json-server/actions/workflows/api-tests.yml/badge.svg)

Este projeto demonstra como utilizar o **JSON Server** como uma API fake e o **Postman** para realizar testes automatizados de endpoints REST.

---

## 🚀 Objetivo

Fornecer um ambiente simples para:

* Simular uma API REST completa
* Testar operações CRUD (Create, Read, Update, Delete)
* Validar respostas HTTP com testes automatizados
* Trabalhar com variáveis de ambiente no Postman

---

## 🛠️ Tecnologias utilizadas

* JSON Server (API fake baseada em arquivo JSON)
* Postman (testes de API)
* Node.js + npm

---

## 📁 Estrutura do projeto

```
.
├── db.json
├── json-server-postman-collection.json
└── README.md
```

---

## ⚙️ Pré-requisitos

Antes de começar, você precisa ter instalado:

* Node.js
* npm ou yarn
* Postman

---

## ▶️ Como executar o projeto

### 1. Instalar o JSON Server

```bash
npm install -g json-server
```

---

### 2. Criar o arquivo `db.json`

Exemplo inicial:

```json
{
  "users": []
}
```

---

### 3. Iniciar o servidor

```bash
json-server --watch db.json
```

A API estará disponível em:

```
http://localhost:3000
```

---

## 📬 Importando a collection no Postman

1. Abra o Postman
2. Clique em **Import**
3. Selecione o arquivo:

   ```
   json-server-postman-collection.json
   ```

---

## 🔧 Configuração de variáveis

A collection utiliza a variável:

| Variável | Valor padrão          |
| -------- | --------------------- |
| base_url | http://localhost:3000 |

Você pode alterar conforme necessário (ex: outra porta ou ambiente).

---

## 🧪 Endpoints testados

A collection cobre o fluxo completo de CRUD:

### ✔️ GET /users

* Lista todos os usuários
* Testa status `200`
* Valida retorno como array

---

### ✔️ POST /users

* Cria um novo usuário
* Testa status `201`
* Salva o `id` automaticamente

---

### ✔️ GET /users/:id

* Busca usuário específico
* Valida se o ID está correto

---

### ✔️ PUT /users/:id

* Atualiza um usuário
* Verifica se os dados foram alterados

---

### ✔️ DELETE /users/:id

* Remove usuário
* Valida status `200` ou `204`

---

## 🔁 Execução automatizada

Você pode rodar todos os testes de uma vez usando:

### ✔️ Collection Runner (Postman)

* Execute na ordem correta (POST → GET → PUT → DELETE)

### ✔️ Newman (CLI)

Instale:

```bash
npm install -g newman
```

Execute:

```bash
newman run json-server-postman-collection.json
```

---

## 💡 Dicas

* Sempre execute primeiro o endpoint **POST**, pois ele gera o `user_id`
* O JSON Server persiste os dados diretamente no arquivo `db.json`
* Você pode editar o arquivo manualmente para simular cenários

---

## ⚠️ Limitações

* Não possui autenticação real
* Não simula regras de negócio complexas
* Persistência simples baseada em arquivo

---

## 📚 Possíveis melhorias

* Adicionar testes de erro (404, 500)
* Simular autenticação com middleware
* Criar múltiplos recursos (posts, comments, etc.)
* Integrar com pipelines de CI/CD

---

## 👨‍💻 Contribuição

Sinta-se à vontade para melhorar este projeto:

* Adicionando novos testes
* Criando cenários mais complexos
* Melhorando a documentação

---

## 📄 Licença

Este projeto é livre para uso em estudos e testes.
