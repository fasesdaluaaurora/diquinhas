🔹 1. Criar uma Collection
Abra o Postman
Clique em “New” → “Collection”
Dê um nome (ex: Mock Whats API)
🔹 2. Criar um endpoint mockado

Dentro da collection:

Clique em “Add request”
Configure:
Method: GET
URL: /users
Vá em “Save”
🔹 3. Definir a resposta mock

Agora o ponto chave:

Clique na request criada
Vá em “Examples” → “Add Example”

Configure:

Response Body:

[
  { "id": 1, "nome": "Luna" },
  { "id": 2, "nome": "Carlos" }
]

Status:

200 OK

Headers (opcional):

Content-Type: application/json

Salve o example.

🔹 4. Criar o Mock Server
Clique nos 3 pontos da Collection
Escolha “Mock collection”
Configure:
Environment: tanto faz
Name: mock-users
Privacy: Public (se quiser URL externa)

Finalize.

🔹 5. Usar a API mock

O Postman vai gerar algo assim:

https://xxxxx.mock.pstmn.io/users

Agora:

curl https://xxxxx.mock.pstmn.io/users

Resposta:

[
  { "id": 1, "nome": "Luna" },
  { "id": 2, "nome": "Carlos" }
]
🔥 Recursos avançados (vale muito pra integração)
✔ Mock condicional por header

Você pode diferenciar respostas usando:

x-mock-response-name: erro

E criar múltiplos examples:

success
erro
timeout
✔ Simular erro

Exemplo:

{
  "error": "Usuário não encontrado"
}

Status:

404
✔ Simular webhook (seu caso 🔥)

Você pode mockar algo tipo:

POST /webhook

E criar exemplos de payload da Meta (WhatsApp), tipo:

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
                "text": { "body": "Olá" }
              }
            ]
          }
        }
      ]
    }
  ]
}
⚠️ Limitações importantes
Não executa lógica (é estático ou baseado em examples)
Não mantém estado (não salva dados)
Matching é simples (path + método + headers)
✅ Quando usar Postman Mock

Use quando:

Precisa de URL pública rápida
Está testando integração externa
Quer simular payloads específicos (Meta, webhook, etc.)
