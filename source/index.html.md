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

Nuestro API usa servicios REST usando formato JSON para el envío y la recepción de información, además de implementar OAuth2.0 como mecanismo de autorización.

 
# Authenticación

> Para estar autorizado debe usar este código: 

```shell
curl "api_endpoint_here"
  -H "Authorization: Bearer [YOUR_PRIVATE_KEY]"
```

> Asegurate de reemplazar `YOUR_PRIVATE_KEY` con tu API Key

Atlas utiliza un API Key para el acceso de nuestros servicios. Si no posee un API Key puede obtener uno siguiendo los pasos para registrarse en nuestro [portal](http://developer.atlas.cl).

Para acceder a cada uno de los servicios de Atlas es requerido que se agregue en el header su API key como se muestra acontinuación:

`Authorization: Bearer [YOUR_PRIVATE_KEY]`

<aside class="notice">
Debes reemplazar <code>YOUR_PRIVATE_KEY</code> con tu API Key
</aside>

# Personas

## Consultar cuentas

```shell
curl "https://api.atlas.com/v1/persons/accounts"
    -H "Authorization: Bearer [YOUR_PRIVATE_KEY]"
    -H "Content-Type: application/json"
    -d '{"username":"11.111.111-1","password":"0000", "bank":"BCI"}'
```

> El comando anterior retorna un resultado como el siguiente:

```json
[
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
```

Este endpoint obtiene todas las productos que posee el usuario, con su saldo disponible, saldo total, el tipo de producto y el tipo de moneda.

### HTTP Request

`POST https://api.atlas.com/v1/persons/accounts`

### Body Parameters

Parametro | Requerido | Descripción
--------- | ------- | -----------
username | true | Es el nombre de usuario que normalmente se usa para hacer login en el portal del banco
password | true | Es la contraseña que normalmente se usa para hacer login en el portal del banco 
bank | true | El nombre del banco del cual se desea extraer la información


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
curl "https://api.atlas.com/v1/persons/statements"
    -H "Authorization: Bearer [YOUR_PRIVATE_KEY]"
    -H "Content-Type: application/json"
    -d '{"username":"11.111.111-1","password":"0000", "bank":"BCI", "account":"2312312312"}'
```
> El comando anterior retorna un resultado como el siguiente:

```json
[
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
```

Este endpoint obtiene todas las transacciones de la cuenta que fueron realizadas durante el mes, la información que se provee es la fecha, el monto, el código de la transacción y el tipo 

### HTTP Request

`POST https://api.atlas.com/v1/persons/statements`

### Body Parameters

Parametro | Requerido | Descripción
--------- | ------- | -----------
username | true | Es el nombre de usuario que normalmente se usa para hacer login en el portal del banco
password | true | Es la contraseña que normalmente se usa para hacer login en el portal del banco 
bank | true | El nombre del banco del cual se desea extraer la información
account | true | El numero de la cuenta que se espera extraer la informacion


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
curl "https://api.atlas.com/v1/persons/contacts"
    -H "Authorization: Bearer [YOUR_PRIVATE_KEY]"
    -H "Content-Type: application/json"
    -d '{"username":"11.111.111-1","password":"0000", "bank":"BCI"}'
```
> El comando anterior retorna un resultado como el siguiente:

```json
[
  {
    "name": "Leonardo",
    "bank": "Banco",
    "nickname": "leo",
    "numberId": "18.001.001-3",
    "email": "prueba@gmail.com",
    "number": "01231231231",
    "accountType": "ahorro"
  },
  {
    "name": "Juan",
    "bank": "Banco",
    "nickname": "JJ",
    "numberId": "18.001.002-1",
    "email": "prueba@gmail.com",
    "number": "01231231231",
    "accountType": "corriente"
  }
]
```

Este endpoint obtiene la información de los contactos registrado en su cuenta bancaria 

### HTTP Request

`POST https://api.atlas.com/v1/persons/contacts`

### Body Parameters

Parametro | Requerido | Descripción
--------- | ------- | -----------
username | true | Es el nombre de usuario que normalmente se usa para hacer login en el portal del banco
password | true | Es la contraseña que normalmente se usa para hacer login en el portal del banco 
bank | true | El nombre del banco del cual se desea extraer la información

### Objeto Contacto

Atributo | Descripción
--------- | -----------
name | Es el nombre completo del contacto
bank | Es el nombre del banco 
nickname | Es el alias del contacto
numberId | Es numero de identificación (RUT) del contacto
email | Es el correo del contacto
number | Es el numero de la cuenta
accountType | Es el tipo de la cuenta


<aside class="success">
Recuerde incluir su API Key en la invocación del servicio
</aside>

