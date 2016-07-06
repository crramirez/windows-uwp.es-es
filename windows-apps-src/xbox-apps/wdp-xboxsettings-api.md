---
author: payzer
title: "Referencia de API de configuración de desarrollador de Xbox de Device Portal"
description: "Obtén información sobre cómo tener acceso a la configuración de desarrollador de Xbox."
ms.sourcegitcommit: c392e6a2356d4180943c17d37512ef8ea35f5719
ms.openlocfilehash: 52fb6e76bd7ad35127eeebeced8f6f8cc076fb7f

---

# Referencia de API de la configuración de desarrollador   
Puede obtener acceso a la configuración de Xbox One que es útil para desarrollar mediante esta API.

## Obtener todas las opciones de configuración del desarrollador al mismo tiempo

**Solicitud**

Puedes usar la siguiente solicitud para obtener todas las opciones de configuración de desarrollador en una única solicitud.

Método      | URI de la solicitud
:------     | :-----
GET | /ext/settings
<br />
**Parámetros del URI**

- Ninguno

**Encabezados de la solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**   
La respuesta es una matriz JSON de configuración que contiene toda la configuración. Cada objeto de configuración contiene estos campos:   

Nombre: (cadena) el nombre de la opción de configuración.   
Valor: (cadena) el valor de la opción de configuración.   
RequiresReboot - ("Sí" | "No") Este campo indica si es necesario un reinicio para que la configuración surta efecto.
Categoría: (cadena) la categoría de la opción de configuración

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Se concedió la solicitud para acceder a las credenciales para el recurso compartido de archivos.
4XX | Códigos de error
5XX | Códigos de error

## Obtener las opciones de configuración de una en una
Las opciones de configuración también se pueden recuperar individualmente.

**Solicitud**

Puedes usar la siguiente solicitud para obtener información acerca de una opción de configuración individual.

Método      | URI de la solicitud
:------     | :-----
GET | /ext/settings/<setting name>
<br />
**Parámetros del URI**

- Ninguno

**Encabezados de la solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**   
La respuesta es un objeto JSON con los siguientes campos:   

Nombre: (cadena) el nombre de la opción de configuración.   
Valor: (cadena) el valor de la opción de configuración.   
RequiresReboot - ("Sí" | "No") Este campo indica si es necesario un reinicio para que la configuración surta efecto.
Categoría: (cadena) la categoría de la opción de configuración

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Se concedió la solicitud para acceder a las credenciales para el recurso compartido de archivos.
4XX | Códigos de error
5XX | Códigos de error

## Configurar el valor de una opción de configuración
Puedes configurar el valor de una opción de configuración.

**Solicitud**

Puedes usar la siguiente solicitud para configurar el valor para una opción de configuración.

Método      | URI de la solicitud
:------     | :-----
PUT | /ext/settings/<setting name>
<br />
**Parámetros del URI**

- Ninguno

**Encabezados de la solicitud**

- Ninguno

**Cuerpo de la solicitud**   
El cuerpo de la solicitud es un objeto JSON que contiene el campo siguiente:   
Valor: (cadena) el nuevo valor de la opción de configuración.

**Respuesta**   

- Ninguna

**Código de estado**

Esta API tiene los siguientes códigos de estado esperado.

Código de estado HTTP      | Descripción
:------     | :-----
200 | Se concedió la solicitud para acceder a las credenciales para el recurso compartido de archivos.
4XX | Códigos de error
5XX | Códigos de error

<br />
**Familias de dispositivos disponibles**

* Windows Xbox




<!--HONumber=Jun16_HO4-->

