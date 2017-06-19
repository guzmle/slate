---
title: API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='#'>Obtener un API Key</a>

includes:
  - errors

search: true
---

# Introducción

Bienvenido al Atlas API. Nuestro API le ofrece acceder de manera fácil y segura a cualquiera de sus cuentas que pertenecen a nuestra red de bancos registrados, dandóle la oportunidad de consultar sus saldos, el historial de transacciones y descargar su libreta de contactos.

Nuestro API usa servicios REST usando formato HAL+JSON para el envío y la recepción de información, además de implementar OAuth2.0 como mecanismo de autorización.

Nuestros endpoint trabajan de manera asícrona por lo que requieren que en cada petición se prevea una URL la cual nuestro servidor enviará la informacion recolectada como una peticion POST luego de concluir la operación.


# Authenticación

> Para estar autorizado debe usar este código: 

```shell
curl "api_endpoint_here"
  -H "Authorization: Bearer [YOUR_PRIVATE_KEY]"
```

> Asegurate de reemplazar `YOUR_PRIVATE_KEY` con tu API Key

Atlas utiliza un API Key para el acceso de nuestros servicios. Si no posee un API Key solicítala contactándote con nuestro equipo.

Para acceder a cada uno de los servicios de Atlas es requerido que se agregue en el header su API key como se muestra acontinuación:

`Authorization: Bearer [YOUR_PRIVATE_KEY]`

<aside class="notice">
Debes reemplazar <code>YOUR_PRIVATE_KEY</code> con tu API Key
</aside>

# Personas

## Consultar cuentas

```shell
curl "http://mars.staging.atlasgo.cl/v1/person/accounts"
    -H "Authorization: Bearer [YOUR_PRIVATE_KEY]"
    -H "Content-Type: application/json"
    -d '{"username":"11.111.111-1","password":"0000", "bank":"santander", "callback":"https://mylink"}'
```

> El comando anterior retorna un resultado como el siguiente:

```json
{
  "_links": {
    "self": {
      "href": "/v1/person/accounts"
    }
  },
  "_embedded": {
    "requests": [
      {
        "token": "adefcc2a6a7b2c40efd07c09c9b2b7a40155b236c638c3abb0a5a15c2f21c4ecc1e222e463cba4962307d0765bcd60c90950db9a818112b8e14a71bfbb053505115baca44d6ed01fb839e2a043654f5c3c6fbc"
      }
    ]
  }
}
```

> Luego de procesado la petición el resultado que se enviará al link de callback será el siguiente:

```json
{
  "data": {
    "_links": {
      "self": {
        "href": "/v1/person/accounts"
      }
    },
    "_embedded": {
      "accounts": [
          {
            "name": "Cuenta 1",
            "number": "00001",
            "accounting_balance": 10,
            "balance": 12,
            "total_retentions": 2,
            "type": "checking",
            "currency": "national"
          },
          {
            "name": "Cuenta 2",
            "number": "00002",
            "accounting_balance": 10,
            "balance": 12,
            "total_retentions": 2,
            "type": "checking",
            "currency": "national"
          }
      ]
    }
  }
}
```

Este endpoint envía al callback todos las productos que posee el usuario, con su saldo disponible, saldo total, el tipo de producto y el tipo de moneda.

### HTTP Request

`POST http://mars.staging.atlasgo.cl/v1/person/accounts`


### Body Parameters

Parametro | Requerido | Descripción
--------- | ------- | -----------
username | true | Es el nombre de usuario que normalmente se usa para hacer login en el portal del banco
password | true | Es la contraseña que normalmente se usa para hacer login en el portal del banco 
bank | true | El nombre del banco del cual se desea extraer la información 
callback | true | Url donde se espera que la respuesta sea enviada a través de una peticion POST 


### Banks List

Parametro | Nombre
--------- | -------
santander | Banco Santander
BCI | Banco BCI 


### Objeto Cuenta

Atributo | Descripción
--------- | -----------
name | Nombre de la cuenta
number | Número de la cuenta 
accounting_balance | Saldo disponible
balance | Saldo total de la cuenta
total_retentions | Total de retenciones
type | Tipo de producto
currency | Tipo de moneda


<aside class="success">
Recuerde incluir su API Key en la invocación del servicio
</aside>

## Obtener Estado de Cuenta


```shell
curl "http://mars.staging.atlasgo.cl/v1/person/statements"
    -H "Authorization: Bearer [YOUR_PRIVATE_KEY]"
    -H "Content-Type: application/json"
    -d '{"username":"11.111.111-1","password":"0000", "bank":"BCI", "account":"2312312312", "callback":"http://myurl.com"}'
```
> El comando anterior retorna un resultado como el siguiente:

```json
{
  "_links": {
    "self": {
      "href": "/v1/person/statements"
    }
  },
  "_embedded": {
    "requests": [
      {
        "token": "adefcc2a6a7b2c40efd07c09c9b2b7a40155b236c638c3abb0a5a15c2f21c4ecc1e222e463cba4962307d0765bcd60c90950db9a818112b8e14a71bfbb053505115baca44d6ed01fb839e2a043654f5c3c6fbc"
      }
    ]
  }
}
```

> Luego de procesado la petición el resultado que se enviará al link de callback será el siguiente:

```json
{
  "data": {
    "_links": {
      "self": {
        "href": "/v1/person/statements"
      }
    },
    "_embedded": {
      "accounts": [
          {
            "date": "12/03/1999",
            "amount": 10,
            "serial": "000001",
            "description": "Depósito",
            "type": "deposit"
          },
          {
            "date": "12/03/1999",
            "amount": -10,
            "serial": "000002",
            "description": "Transferencia entre cuentas del mismo banco",
            "type": "credit"
          }
      ]
    }
  }
}
```

Este endpoint obtiene todas las transacciones de la cuenta que fueron realizadas durante el mes, la información que se provee es la fecha, el monto, el código de la transacción y el tipo 

### HTTP Request

`POST http://mars.staging.atlasgo.cl/v1/person/statements`

### Body Parameters

Parametro | Requerido | Descripción
--------- | ------- | -----------
username | true | Es el nombre de usuario que normalmente se usa para hacer login en el portal del banco
password | true | Es la contraseña que normalmente se usa para hacer login en el portal del banco 
bank | true | El nombre del banco del cual se desea extraer la información
account | true | El numero de la cuenta que se espera extraer la informacion
callback | true | Url donde se espera que la respuesta sea enviada a través de una peticion POST 


### Objeto Transacción

Atributo | Descripción
--------- | -----------
date | Es la fecha de la transacción
amount | Cantidad de la transacción 
serial | Código de referencia de la transacción
description | Descripción de la transacción
type | Tipo de la transacción

<aside class="success">
Recuerde incluir su API Key en la invocación del servicio
</aside>


## Obtener Contactos


```shell
curl "http://mars.staging.atlasgo.cl/v1/person/payees"
    -H "Authorization: Bearer [YOUR_PRIVATE_KEY]"
    -H "Content-Type: application/json"
    -d '{"username":"11.111.111-1","password":"0000", "bank":"BCI", "callback":"http://myurl.com"}'
```
> El comando anterior retorna un resultado como el siguiente:


```json
{
  "_links": {
    "self": {
      "href": "/v1/person/payees"
    }
  },
  "_embedded": {
    "requests": [
      {
        "token": "adefcc2a6a7b2c40efd07c09c9b2b7a40155b236c638c3abb0a5a15c2f21c4ecc1e222e463cba4962307d0765bcd60c90950db9a818112b8e14a71bfbb053505115baca44d6ed01fb839e2a043654f5c3c6fbc"
      }
    ]
  }
}
```

> Luego de procesado la petición el resultado que se enviará al link de callback será el siguiente:

```json
{
  "data": {
    "_links": {
      "self": {
        "href": "/v1/person/payees"
      }
    },
    "_embedded": {
      "accounts": [
        {
          "name": "NOMBRE 1",
          "idNumber": "43242343",
          "bank": "Corpbanca",
          "accountNumber": "312312312312",
          "email": "dasd@dasda.cl",
          "nickname": "",
          "accountType": "Cuenta Corriente"
        },
        {
          "name": "NOMBRE 2",
          "idNumber": "42342342342",
          "bank": "Banco Santander",
          "accountNumber": "31232312312",
          "email": "ewqw@dasda.com",
          "nickname": "",
          "accountType": "Cuenta Corriente"
        }
      
      ]
    }
  }
}
```

Este endpoint obtiene la información de los contactos registrado en su cuenta bancaria 

### HTTP Request

`POST http://mars.staging.atlasgo.cl/v1/person/payees`

### Body Parameters

Parametro | Requerido | Descripción
--------- | ------- | -----------
username | true | Es el nombre de usuario que normalmente se usa para hacer login en el portal del banco
password | true | Es la contraseña que normalmente se usa para hacer login en el portal del banco 
bank | true | El nombre del banco del cual se desea extraer la información
callback | true | Url donde se espera que la respuesta sea enviada a través de una peticion POST 

### Objeto Contacto

Atributo | Descripción
--------- | -----------
name | Es el nombre completo del contacto
bank | Es el nombre del banco 
nickname | Es el alias del contacto
idNumber | Es numero de identificación (RUT) del contacto
email | Es el correo del contacto
number | Es el numero de la cuenta
accountType | Es el tipo de la cuenta


<aside class="success">
Recuerde incluir su API Key en la invocación del servicio
</aside>

