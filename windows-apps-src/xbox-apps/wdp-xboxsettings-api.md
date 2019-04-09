---
title: Referencia de API de configuración de desarrollador de Xbox de Device Portal
description: Obtén información sobre cómo tener acceso a la configuración de desarrollador de Xbox.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6ab12b99-2944-49c9-92d9-f995efc4f6ce
ms.localizationpriority: medium
ms.openlocfilehash: 54a15be26adf0da97105f15f3a44f26ee7bfc96d
ms.sourcegitcommit: 681c1e3836d2a51cd3b31d824ece344281932bcd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59240043"
---
# <a name="developer-settings-api-reference"></a>Referencia de API de la configuración de desarrollador

Puede obtener acceso a la configuración de Xbox One que es útil para desarrollar mediante esta API.

## <a name="get-all-developer-settings-at-once"></a>Obtener todas las opciones de configuración del desarrollador al mismo tiempo

**Solicitud**

Puedes usar la siguiente solicitud para obtener todas las opciones de configuración de desarrollador en una única solicitud.

Método      | URI de la solicitud
:------     | :-----
GET | /ext/settings

**Parámetros de URI**

- Ninguno

**Encabezados de solicitud**

- Ninguno

**Cuerpo de la solicitud**

- Ninguno

**Respuesta**   
La respuesta es una matriz JSON de configuración que contiene toda la configuración. Cada objeto de configuración contiene estos campos:

* Nombre: (cadena) el nombre de la opción de configuración.
* Valor: (cadena) el valor de la opción de configuración.
* RequiresReboot - ("Sí" | "No") Este campo indica si es necesario un reinicio para que la configuración surta efecto.
* Disabled: ("Yes" | "No") este campo indica si la configuración está deshabilitada y no puede editarse.
* Category: (cadena) la categoría de la opción de configuración.
* Type: ("Text" | "Number" | "Bool" | "Select") este campo indica de qué tipo es un valor: entrada de texto, un valor booleano ("true" o "false"), un número con un mínimo y un máximo, o si se selecciona de una lista específica de valores.

Si el valor es un número:

* Min - (número) de este campo indica el valor numérico mínimo de la configuración.
* Max - (número) de este campo indica el valor numérico máximo de la configuración.

Si se selecciona la configuración:

* OptionsVariable - ("Sí" | "No") en este campo indica si las opciones de configuración son variables, si las opciones válidas se pueden cambiar sin tener que reiniciar.
* Options: matriz JSON que contiene la las opciones válidas de selección como cadenas.

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

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
GET | /ext/Settings/\<nombre de la configuración\>

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
* RequiresReboot - ("Sí" | "No") Este campo indica si es necesario un reinicio para que la configuración surta efecto.
* Disabled: ("Yes" | "No") este campo indica si la configuración está deshabilitada y no puede editarse.
* Category: (cadena) la categoría de la opción de configuración.
* Type: ("Text" | "Number" | "Bool" | "Select") este campo indica de qué tipo es un valor: entrada de texto, un valor booleano ("true" o "false"), un número con un mínimo y un máximo, o si se selecciona de una lista específica de valores.

Si el valor es un número:

* Min - (número) de este campo indica el valor numérico mínimo de la configuración.
* Max - (número) de este campo indica el valor numérico máximo de la configuración.

Si se selecciona la configuración:

* OptionsVariable - ("Sí" | "No") en este campo indica si las opciones de configuración son variables, si las opciones válidas se pueden cambiar sin tener que reiniciar.
* Options: matriz JSON que contiene la las opciones válidas de selección como cadenas.

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

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
PUT | /ext/Settings/\<nombre de la configuración\>

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

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
200 | La solicitud se realizó correctamente.
4XX | Códigos de error
5XX | Códigos de error

**Familias de dispositivos disponibles**

* Windows Xbox