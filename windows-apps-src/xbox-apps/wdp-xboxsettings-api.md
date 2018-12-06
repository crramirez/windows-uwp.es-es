---
title: Referencia de API de configuración de desarrollador de Xbox de Device Portal
description: Obtén información sobre cómo tener acceso a la configuración de desarrollador de Xbox.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6ab12b99-2944-49c9-92d9-f995efc4f6ce
ms.localizationpriority: medium
ms.openlocfilehash: 402d535bf6ff9ced24bc642c17d13b2d48d79681
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8747032"
---
# <a name="developer-settings-api-reference"></a>Referencia de API de la configuración de desarrollador   
Puede obtener acceso a la configuración de Xbox One que es útil para desarrollar mediante esta API.

## <a name="get-all-developer-settings-at-once"></a>Obtener todas las opciones de configuración del desarrollador al mismo tiempo

**Solicitud**

Puedes usar la siguiente solicitud para obtener todas las opciones de configuración de desarrollador en una única solicitud.

Método      | URI de solicitud
:------     | :-----
GET | /ext/settings
<br />
**Parámetros del URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**   
La respuesta es una matriz JSON de configuración que contiene toda la configuración. Cada objeto de configuración contiene estos campos:

* Nombre: (cadena) el nombre de la opción de configuración.
* Valor: (cadena) el valor de la opción de configuración.
* RequiresReboot: ("Yes" | "No") este campo indica si es necesario un reinicio para que la configuración surta efecto.
* Disabled: ("Yes" | "No") este campo indica si la configuración está deshabilitada y no puede editarse.
* Category: (cadena) la categoría de la opción de configuración.
* Type: ("Text" | "Number" | "Bool" | "Select") este campo indica de qué tipo es un valor: entrada de texto, un valor booleano ("true" o "false"), un número con un mínimo y un máximo, o si se selecciona de una lista específica de valores.

Si el valor es un número:
* Min: (número) este campo indica el valor numérico mínimo de la configuración.
* Max: (número) este campo indica el valor numérico máximo de la configuración.

Si se selecciona la opción de configuración:
* > OptionsVariable - ("Yes" | "No") este campo indica si las opciones de configuración son variables, si las opciones válidas pueden cambiar sin un reinicio.
* Options: matriz JSON que contiene la las opciones válidas de selección como cadenas.

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

Método      | URI de solicitud
:------     | :-----
GET | /ext/settings/\<setting name\>
<br />
**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**   
La respuesta es un objeto JSON con los siguientes campos:

* Nombre: (cadena) el nombre de la opción de configuración.
* Valor: (cadena) el valor de la opción de configuración.
* RequiresReboot: ("Yes" | "No") este campo indica si es necesario un reinicio para que la configuración surta efecto.
* Disabled: ("Yes" | "No") este campo indica si la configuración está deshabilitada y no puede editarse.
* Category: (cadena) la categoría de la opción de configuración.
* Type: ("Text" | "Number" | "Bool" | "Select") este campo indica de qué tipo es un valor: entrada de texto, un valor booleano ("true" o "false"), un número con un mínimo y un máximo, o si se selecciona de una lista específica de valores.

Si el valor es un número:
* Min: (número) este campo indica el valor numérico mínimo de la configuración.
* Max: (número) este campo indica el valor numérico máximo de la configuración.

Si se selecciona la opción de configuración:
* > OptionsVariable - ("Yes" | "No") este campo indica si las opciones de configuración son variables, si las opciones válidas pueden cambiar sin un reinicio.
* Options: matriz JSON que contiene la las opciones válidas de selección como cadenas.

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

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**   
El cuerpo de la solicitud es un objeto JSON que contiene el campo siguiente:   
Valor: (cadena) el nuevo valor de la opción de configuración.

**Respuesta**   

- Ninguno

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