
# 📦 Logistic Connector API

API para integração de pedidos logísticos com autenticação, criação de pedidos, recebimento e confirmação de eventos.

## 🌐 Base URLs

| Ambiente | URL Base |
|----------|----------|
| Produção | `https://api-conector.deliverylab.com.br` |
| Desenvolvimento | `https://api-conector.deliverylab.dev.br` |

---

## 🔐 Autenticação

### `POST /oauth/token`

Obtém um token de acesso com base nas credenciais do cliente.

#### Corpo da Requisição (form-urlencoded)

- `client_id` (string): ID do cliente  
- `client_secret` (string): Segredo do cliente  
- `grant_type` (string): Tipo de concessão (Atualmente só aceita `client_credentials`)  

#### Exemplo de Resposta

```json
{
  "access_token": "eyJhbGciOi...",
  "token_type": "bearer",
  "expires_in": 3600
}
```

---

## 📝 Criar Pedido

### `POST /merchants/:merchantid/orders`

Cria um novo pedido para um estabelecimento específico.  
**Requer autenticação Bearer Token**

#### Parâmetros de URL

- `merchantid`: ID do estabelecimento

#### Corpo da Requisição

```json
{
  "customer": {
    "name": "Ricardo Lins",
    "phone": {
      "countryCode": "55",
      "areaCode": "11",
      "number": "910101010"
    }
  },
  "delivery": {
    "merchantFee": 3.5,
    "deliveryAddress": {
      "postalCode": "08501-180",
      "streetNumber": "12",
      "streetName": "Emilio Ribas",
      "complement": "",
      "neighborhood": "Sitio Paredao",
      "city": "Ferraz de Vascondelos",
      "state": "SP",
      "country": "BR",
      "reference": "",
      "coordinates": {
        "latitude": -23.53950939810104,
        "longitude": -46.36412313222168
      }
    }
  },
  "items": [
    {
      "id": "500",
      "name": "PIZZA - ALHO E ÓLEO - G",
      "quantity": 1.0,
      "unitPrice": 33.0,
      "price": 33.0,
      "optionsPrice": 0,
      "totalPrice": 33.0,
      "options": []
    }
  ],
  "payments": {
    "methods": [
      {
        "method": "cash",
        "type": "OFFLINE",
        "value": 36.5,
        "cash": {
          "changeFor": 40.0
        }
      }
    ]
  }
}
```

---

## 🔁 Polling de Eventos

### `GET /prod/events:polling`

Consulta eventos pendentes.  
**Requer autenticação Bearer Token**

#### Corpo da Resposta

```json
[
  {
    "id": "0b237331-2b5d-4c48-a5fa-adfc1301c2fa",
    "code": "AAD",
    "fullCode": "ARRIVED_AT_DESTINATION",
    "orderId": "b372dbec-3d98-46cd-bc01-b7a69ecb7668",
    "merchantId": "94ed8e9f-d11f-46f0-a144-fa9a9ad39bab",
    "createdAt": "2023-08-22T14:50:30+00:00",
  }
]
```

---

## ✅ Confirmar Recebimento de Eventos

### `POST /events/acknowledgment`

Confirma o recebimento de eventos já processados.
**Requer autenticação Bearer Token**

#### Parâmetros do corpo da requisição

- `id`: ID do evento consumido

#### Corpo da Requisição

```json
[
  { "id": "ba202aba-448c-4f61-8cd3-233db5561a19" },
  { "id": "97a4d9a6-3c2e-40b0-8196-ec83bee6591e" }
]
```
