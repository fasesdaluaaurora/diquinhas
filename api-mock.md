# 📡 Mock API com Postman

Este guia mostra como criar uma **API mockada usando o Postman**, ideal para testar integrações (ex: webhooks, APIs externas como WhatsApp/Meta) sem precisar de backend real.

---

## 🚀 Objetivo

Criar uma API fake com:

* Endpoints HTTP (GET, POST, etc.)
* Respostas JSON customizadas
* URL pública para testes

---

## 🔹 1. Criar uma Collection

1. Abra o Postman
2. Clique em **"New" → "Collection"**
3. Defina um nome (ex: `mock-api`)

---

## 🔹 2. Criar um endpoint

Dentro da collection:

1. Clique em **"Add request"**
2. Configure:

```
Method: GET
URL: /users
```

3. Clique em **Save**

---

## 🔹 3. Criar resposta mock (Example)

1. Abra a request criada
2. Vá em **"Examples" → "Add Example"**

Configure:

### Response Body

```json
[
  { "id": 1, "nome": "Luna" },
  { "id": 2, "nome": "Carlos" }
]
```

### Status

```
200 OK
```

### Headers (opcional)

```
Content-Type: application/json
```

Salve o Example.

---

## 🔹 4. Criar o Mock Server

1. Clique nos **3 pontos da Collection**
2. Selecione **"Mock collection"**
3. Configure:

* **Name:** mock-api
* **Environment:** default
* **Privacy:** Public (para gerar URL externa)

Finalize a criação.

---

## 🔹 5. Consumir a API

O Postman irá gerar uma URL como:

```
https://xxxxx.mock.pstmn.io/users
```

### Teste com curl:

```bash
curl https://xxxxx.mock.pstmn.io/users
```

### Resposta esperada:

```json
[
  { "id": 1, "nome": "Luna" },
  { "id": 2, "nome": "Carlos" }
]
```

---

## 🔥 Recursos avançados

### ✔ Múltiplas respostas (mock dinâmico)

Você pode criar vários Examples:

* `success`
* `error`
* `timeout`

E escolher via header:

```
x-mock-response-name: error
```

---

### ✔ Simular erro

**Response Body:**

```json
{
  "error": "Usuário não encontrado"
}
```

**Status:**

```
404
```

---

### ✔ Simular webhook (ex: WhatsApp/Meta)

Crie um endpoint:

```
POST /webhook
```

**Example de payload:**

```json
{
  "object": "whatsapp_business_account",
  "entry": [
    {
      "changes": [
        {
          "value": {
            "messages": [
              {
                "from": "554899999999",
                "text": {
                  "body": "Olá"
                }
              }
            ]
          }
        }
      ]
    }
  ]
}
```

---

## ⚠️ Limitações

* Não executa lógica (somente respostas estáticas)
* Não persiste dados
* Matching simples (rota + método + headers)

---

## ✅ Quando usar

Use Mock Server do Postman quando:

* Precisa de **URL pública rápida**
* Está testando **integração com terceiros**
* Quer simular **payloads reais sem backend**

---

## 📌 Dica

Para testes mais robustos (com lógica, validação e estado), considere complementar com:

* Mock em Node.js ou Go
* Ferramentas como JSON Server ou Prism

---

## 🧪 Conclusão

O Mock Server do Postman é a forma mais rápida de expor uma API fake funcional para testes de integração, especialmente útil em cenários com webhooks e serviços externos.

---
