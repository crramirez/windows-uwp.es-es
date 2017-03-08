---
author: payzer
title: "Referencia de API de configuración de desarrollador de Xbox de Device Portal"
description: "Obtén información sobre cómo tener acceso a la configuración de desarrollador de Xbox."
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 6ab12b99-2944-49c9-92d9-f995efc4f6ce
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: a17a489944fdc2d78831549c1afdc2bd87deabdf
ms.lasthandoff: 02/08/2017

---

# <a name="developer-settings-api-reference"></a>Referencia de API de la configuración de desarrollador   
Puede obtener acceso a la configuración de Xbox One que es útil para desarrollar mediante esta API.

## <a name="get-all-developer-settings-at-once"></a>Obtener todas las opciones de configuración del desarrollador al mismo tiempo

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
200 | La solicitud se realizó correctamente.
4XX | Códigos de error
5XX | Códigos de error

## <a name="get-settings-one-at-a-time"></a>Obtener las opciones de configuración de una en una
Las opciones de configuración también se pueden recuperar individualmente.

**Solicitud**

Puedes usar la siguiente solicitud para obtener información acerca de una opción de configuración individual.

Método      | URI de la solicitud
:------     | :-----
GET | /ext/settings/\<setting name\>
<br />
**Parámetros de URI**

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
200 | La solicitud se realizó correctamente.
4XX | Códigos de error
5XX | Códigos de error

## <a name="set-the-value-of-a-setting"></a>Configurar el valor de una opción de configuración
Puedes configurar el valor de una opción de configuración.

**Solicitud**

Puedes usar la siguiente solicitud para configurar el valor para una opción de configuración.

Método      | URI de la solicitud
:------     | :-----
PUT | /ext/settings/\<setting name\>
<br />
**Parámetros de URI**

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
200 | La solicitud se realizó correctamente.
4XX | Códigos de error
5XX | Códigos de error

<br />
**Familias de dispositivos disponibles**

* Windows Xbox


