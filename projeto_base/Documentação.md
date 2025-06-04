# Umbrella
## Frontend
## Pacotes
* Shadcn-ui (Biblioteca de componentes para Next.js)
* Zod (Validação de formulários)
* React-Hook-Form (Gerenciamento de formulários)
* Axios (Gerenciamento de Requisições)
* Lucide React (Ícones)
* Nookies (Gerenciamento de Cookies no Next.js)
* Zustand (Gerenciamento de Estados Entre Páginas)

## Backend
### Autenticação
**Tipo de Token:** OAT (opaque access token);\
**Tempo de Expiração de Token:**  Indeterminado;\
**Dados para Autenticação:** Email e Senha;

### Middlewares
Middlewares são úteis para fazer validações em suas requisições antes mesmo que elas
cheguem aos seus respectivos handlers. Na aplicação estão implementados os seguintes
middlewares:
* **auth:** Verifica se na requisição está presente um **OAT** token válido, entaão prossegue com a requisição ou recusa a mesma.
* **PermissionGroupMiddleware:** Detecta se o usuário logado pertence a determinado group.

## Macros e Handlers
### Sucesso
Usamos um macro para respostas bem sucedidas neste modelo:\

```json
{
      status:number,
      timeStamp: new Date().toISOString(),
      success: true,
      request_id: request.id(),
      result:data
    }
```
para usa-los basta ao chamar o response do HttpContext do adonis, basta chamar o método *sendSuccess*, o qual aceita 3 parametros o dado a ser retornado, a request que está regerando a response e o código que é opcional e por padrão é 200.
Exempo:
`response.sendSuccess(accessToken,request,201);`

### handler
Para handle usamos como previsto na documentação do adonis e o padrão é o seguinte:

```json
{
        status,
        timestamp: timestamp,
        success: false,
        request_id: requestId,
        error: {
          type,
          code: status.toString(),
          message,
          ...(validationErrors.length > 0 && { validation_errors: validationErrors }),
        },
      }
```


## Rotas

### **POST** /register
* **Descrição:** Recebe um usuário a registrar, valida os dados e cadastra retornando um token aot de acesso.

#### Entrada
| Campo            | Valor          |
|------------------|----------------|
| fullName         | string         |
| email            | string         |
| password         | string         |
| phone            | string         |
| dob              | string (data)  |
| registration     | string         |
| status           | boolean        |
| address_id       | integer        |
| module_id        | integer        |
| gender           | string         |
| cpf              | string         |
| rg               | string         |
| pis_pasep        | string         |
| salary           | float          |
| ctps             | string         |
| marital_status   | string         |
| employer_id      | integer        |

#### Retorno


| Campo             | Valor                                                        |
|-------------------|--------------------------------------------------------------|
| status            | 200                                                          |
| timeStamp         | string (ISO 8601 datetime)                                   |
| success           | boolean                                                      |
| request_id        | string                                                       |
| result.type       | string                                                       |
| result.name       | string \| null                                                |
| result.token      | string                                                       |
| result.abilities | array de strings                                              |
| result.lastUsedAt | string \| null                                                |
| result.expiresAt  | string \| null                                                |

### Exemplo de resposta:

```json
{
  "status": 200,
  "timeStamp": "2025-06-04T22:16:33.135Z",
  "success": true,
  "request_id": "iqgkt4nprez4a03qt39ynz37",
  "result": {
    "type": "bearer",
    "name": null,
    "token": "oat_Nw.NU1XQnUxYVlXTnl6RjNuQUM5amZaVzBLZHhINzBid0o5NzFCVUxRbTM5NDcyNDU2OTU",
    "abilities": [
      "*"
    ],
    "lastUsedAt": null,
    "expiresAt": null
  }
}
```
