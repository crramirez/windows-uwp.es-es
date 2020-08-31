---
title: Referencia de API de configuración de desarrollador de Xbox de Device Portal
description: Obtenga información sobre cómo obtener acceso a la configuración de Xbox One que resulta útil para el desarrollo mediante la API de REST del portal de dispositivos Xbox.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 6ab12b99-2944-49c9-92d9-f995efc4f6ce
ms.localizationpriority: medium
ms.openlocfilehash: 0aceb7afdce9cc76eab3ee330f0018fdc7ccd1bb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168989"
---
# <a name="developer-settings-api-reference"></a>Referencia de API de la configuración de desarrollador

Puede obtener acceso a la configuración de Xbox One que es útil para desarrollar mediante esta API.

## <a name="get-all-developer-settings-at-once"></a>Obtener todas las opciones de configuración del desarrollador al mismo tiempo

**Solicitud**

Puedes usar la siguiente solicitud para obtener todas las opciones de configuración de desarrollador en una única solicitud.

Método      | URI de solicitud
:------     | :-----
GET | /ext/settings

**Parámetros de URI**

- None

**Encabezados de solicitud**

- None

**Cuerpo de la solicitud**

- None

**Ante**   
La respuesta es una matriz JSON de configuración que contiene toda la configuración. Cada objeto de configuración contiene estos campos:

* Nombre: (cadena) el nombre de la opción de configuración.
* Valor: (cadena) el valor de la opción de configuración.
* RequiresReboot - ("Sí" | "No") Este campo indica si es necesario un reinicio para que la configuración surta efecto.
* Deshabilitado: ("sí" | "No") este campo indica si la configuración está deshabilitada y no se puede editar.
* Category: (cadena) la categoría de la configuración.
* Type-("Text" | "Número" | "Bool" | "Seleccionar") este campo indica el tipo de valor: entrada de texto, un valor booleano ("true" o "false"), un número con un valor de min y Max o Select con una lista específica de valores.

Si el valor es un número:

* Min: (número) este campo indica el valor numérico mínimo de la configuración.
* Max-(número) este campo indica el valor numérico máximo de la configuración.

Si el valor es Select:

* OptionsVariable-("sí" | "No") este campo indica si las opciones de configuración son variables, si las opciones válidas pueden cambiar sin necesidad de reiniciar.
* Options: matriz JSON que contiene las opciones Select válidas como cadenas.

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

Método      | URI de solicitud
:------     | :-----
GET | /ext/settings/\<setting name\>

**Parámetros de URI**

- None

**Encabezados de solicitud**

- None

**Cuerpo de la solicitud**

- None

**Ante**   
La respuesta es un objeto JSON con los siguientes campos:

* Nombre: (cadena) el nombre de la opción de configuración.
* Valor: (cadena) el valor de la opción de configuración.
* RequiresReboot - ("Sí" | "No") Este campo indica si es necesario un reinicio para que la configuración surta efecto.
* Deshabilitado: ("sí" | "No") este campo indica si la configuración está deshabilitada y no se puede editar.
* Category: (cadena) la categoría de la configuración.
* Type-("Text" | "Número" | "Bool" | "Seleccionar") este campo indica el tipo de valor: entrada de texto, un valor booleano ("true" o "false"), un número con un valor de min y Max o Select con una lista específica de valores.

Si el valor es un número:

* Min: (número) este campo indica el valor numérico mínimo de la configuración.
* Max-(número) este campo indica el valor numérico máximo de la configuración.

Si el valor es Select:

* OptionsVariable-("sí" | "No") este campo indica si las opciones de configuración son variables, si las opciones válidas pueden cambiar sin necesidad de reiniciar.
* Options: matriz JSON que contiene las opciones Select válidas como cadenas.

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

Método      | URI de solicitud
:------     | :-----
PUT | /ext/settings/\<setting name\>

**Parámetros de URI**

- None

**Encabezados de solicitud**

- None

**Cuerpo de la solicitud**   
El cuerpo de la solicitud es un objeto JSON que contiene el campo siguiente:   
Valor: (cadena) el nuevo valor de la opción de configuración.

**Respuesta**   

- None

**Código de estado**

Esta API tiene los siguientes códigos de estado previstos.

Código de estado HTTP      | Descripción
:------     | :-----
200 | La solicitud se realizó correctamente.
4XX | Códigos de error
5XX | Códigos de error

**Familias de dispositivos disponibles**

* Windows Xbox