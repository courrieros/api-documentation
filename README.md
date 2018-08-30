# Ecolivery Courrieros - Documentação API

Documentação para uso dos endpoints da API.

Endereço de acesso: [https://api.courrieros.com.br](https://api.courrieros.com.br)

## Autenticação

Todas as requisições precisam informar o token de acesso para o processo de autenticação. Informe a chave com `x-api-key`
juntamente com o valor do token de acesso no cabeçalho HTTP de cada requisição.

Considerando o cenário de exemplo onde a chave de acesso possui o valor `key-abc-123`, segue a chamada para a cURL: 

```
culr -X GET https://api.courrieros.com.br/  -H 'x-api-key: key-abc-123'
```

## Envio de status

* **URL**

`/api/orders/{id}/status`

* **Method**

`POST`

### Status recebido pela transportadora

* **Parâmetros via body**

    |Atributo    | Tipo de dado | Descrição                    | Obrigatório | Valor padrão      | Exemplo                      |
    |------------|--------------|------------------------------|-------------|-------------------|------------------------------|
    | status     | String       | Status atual da entrega      | sim         | Received          | Received                     | 
    | updateTime | String       | Data da ocorrencia do status | sim         | 2018-08-22 08:59Z | 2018-08-22 08:59Z            | 
    
* **Json**
	
    ```
    {
    	"status":"Received",
        "eventDate":"2018-08-22 08:59Z"
    }
	```

* **Retorno**

    **Status code:** 201

-----
    
### Status em rota


* **Parâmetros via body**

    |Atributo    | Tipo de dado | Descrição                    | Obrigatório | Valor padrão      | Exemplo                      |
    |------------|--------------|------------------------------|-------------|-------------------|------------------------------|
    | status     | String       | Status atual da entrega      | sim         | EnRoute           | EnRoute                      | 
    | updateTime | String       | Data da ocorrencia do status | sim         | 2018-08-22 08:59Z | 2018-08-22 08:59Z            |
    
* **Json**
	
    ```
    {
    	"status":"EnRoute",
        "eventDate":"2018-08-22 08:59Z"
    }
	```

* **Retorno**
    
   **Status code:** 201 

-----

### Status Entregue

* **Parâmetros via body**
    
     |Atributo          | Tipo de dado | Descrição                    | Obrigatório | Valor padrão      | Exemplo                      |
     |------------------|--------------|------------------------------|-------------|-------------------|------------------------------|
     | status           | String       | Status atual da entrega      | sim         | Delivered         | Delivered                    |
     | receiverName     | String       | Nome do recebedor            | sim         | -                 | João da Silva                |
     | receiverDocument | String       | Documento do recebedor       | sim         | -                 | 123456789-00                 |
     | relationship     | String       | Cargo do recebedor           | sim         | -                 | porteiro                     |        
     | updateTime       | String       | Data da ocorrencia do status | sim         | 2018-08-22 08:59Z | 2018-08-22 08:59Z            |
   
* **Json**
	
    ```
    {
    	"status":"Delivered",
        "deliveredInfo":
        {
        	"receiverName":"João da Silva",
            "receiverDocument":"123456789-00",
            "relationship":"porteiro",
        },
        "eventDate":"2018-08-22 08:59Z"
    }
	```

*  **Retorno**
    
    **Status code:** 201
    
-----
    
### Status Não Entregue

* **Razões para não entregue**
    
     |Atributo         | Tipo de dado | Descrição                      |
     |-----------------|--------------|--------------------------------|
     | Absent          | String       | Cliente Ausente                |
     | AddressNotFound | String       | Endereço não encontrado        |       
     | GoodsExchange   | String       | Mercadoria trocada             |
     | CustomerMoved   | String       | Cliente mudou                  |
     | OutBusiness     | String       | Fora do horário comercial      |
     | Others          | String       | Outros                         |  

* **Parâmetros via body**
     
     |Atributo         | Tipo de dado | Descrição                      | Obrigatório | Valor padrão       | Exemplo                      |
     |-----------------|--------------|--------------------------------|-------------|--------------------|------------------------------|
     | status          | String       | Status atual da entrega        | sim         | NotDelivered       | NotDelivered                    |
     | reason          | String       | razão da entrega não realizada | sim         | NotDeliveredReason | AddressNotFound              |       
     | updateTime      | String       | Data da ocorrencia do status   | sim         | 2018-08-22 08:59Z  | 2018-08-22 08:59Z            |

* **Json**
	
    ```
    {
    	"status":"NotDelivered",
        "reason":"AddressNotFound",
        "eventDate":"2018-08-22 08:59Z"
    }
	```
    
* **Retorno**
    
    **Status code:** 201
    
### Status Cancelado     

* **Parâmetros via body** 

     |Atributo    | Tipo de dado | Descrição                    | Obrigatório | Valor padrão      | Exemplo                      |
     |------------|--------------|------------------------------|-------------|-------------------|------------------------------|
     | status     | String       | Status atual da entrega      | sim         | Canceled          | Canceled                     | 
     | updateTime | String       | Data da ocorrencia do status | sim         | 2018-08-22 08:59Z | 2018-08-22 08:59Z            | 

*  **Json**
	
    ```
    {
    	"status":"Canceled",
        "eventDate":"2018-08-22 08:59Z"
    }
	```
  
* **Retorno**
    
    **Status code:** 201

### Status Retornado ao remetente

* **Parâmetros via body**

     |Atributo    | Tipo de dado | Descrição                    | Obrigatório | Valor padrão      | Exemplo                      |
     |------------|--------------|------------------------------|-------------|-------------------|------------------------------|
     | status     | String       | Status atual da entrega      | sim         | Returned          | Returned                     | 
     | updateTime | String       | Data da ocorrencia do status | sim         | 2018-08-22 08:59Z | 2018-08-22 08:59Z            |

* **Json**
	
    ```
    {
    	"status":"Returned",
        "eventDate":"2018-08-22 08:59Z"
    }
	```
  
* **Retorno**
    
    **Status code:** 201
    
## Máquina de estado

### Pedidos

![State machine](https://s3.amazonaws.com/static-courrieros/State+Machine.png)

O status canceled tem conexão com todos status.

## Suporte da API

Caso tenha dúvidas ou esteja com problemas na integração, mande um e-mail para *devs@courrierossp.com*
