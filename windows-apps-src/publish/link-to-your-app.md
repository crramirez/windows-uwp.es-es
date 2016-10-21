---
author: jnHs
Description: "Puedes ayudar a los clientes a descubrir tu aplicación mediante un vínculo a su descripción en la Tienda."
title: "Vincular a tu aplicación"
ms.assetid: 5420B65C-7ECE-4364-8959-D1683684E146
translationtype: Human Translation
ms.sourcegitcommit: d36f14604bd8c2ce0d5778a67f5b5b9460d9fbf3
ms.openlocfilehash: 8e4756fa7cb7b808e543453e69b5bb3f09ffd5ce

---

# Vincular a tu aplicación


Puedes ayudar a los clientes a descubrir tu aplicación mediante un vínculo a su descripción en la Tienda.

## Obtener el vínculo a la descripción de la aplicación en la Tienda


Puedes encontrar el vínculo a la descripción en la Tienda en la página [Identidad de la aplicación](view-app-identity-details.md), en la sección **Administración de aplicaciones** de cada aplicación de tu panel.

Este vínculo está en formato **`https://www.microsoft.com/store/apps/<your app's Store ID>`**

Cuando un cliente hace clic en este vínculo, se abrirá la página de descripción basada en web de la aplicación. Si la aplicación está disponible para el dispositivo del cliente, la aplicación de la Tienda también se inicia y muestra la sinopsis de la aplicación.

> **Nota** Según las versiones de los sistemas operativos que te interesen, puede que veas más de un vínculo. Todas las aplicaciones mostrarán la dirección URL para Windows 10, que funciona en cualquier sistema operativo. Puedes ver vínculos adicionales para Windows 8.1 y versiones anteriores o para Windows Phone 8.1 y versiones anteriores, aunque solo funcionarán para las versiones de sistema operativo especificadas.

 

## Crear un vínculo a la descripción de la aplicación en la Tienda con el distintivo de la Tienda Windows


Puedes crear un vínculo directo a la sinopsis de tu aplicación mediante un distintivo personalizado que permita a los clientes saber que la aplicación se encuentra en la Tienda Windows.

Para crear el distintivo, visita la página [Distintivos de la Tienda Windows](http://go.microsoft.com/fwlink/p/?LinkID=534236). Debes tener el id. de la Tienda de la aplicación para usar este formulario para generar el distintivo y un vínculo. Este identificador consiste en los 12 últimos caracteres de la **dirección URL para Windows 10** que se muestra en la página [Identidad de la aplicación](view-app-identity-details.md), en la sección **Administración de aplicaciones**.

> **Nota** Consulta [Directrices para el marketing de aplicaciones](app-marketing-guidelines.md) para obtener más información sobre el uso del distintivo de la Tienda Windows.

 

## Vincular directamente a tu aplicación en la Tienda Windows


Puedes crear un vínculo que inicie la Tienda Windows y lleve directamente a la página de descripción de la aplicación sin tener que abrir un explorador mediante el uso del esquema de URI **ms-windows-store:**.

Estos vínculos son útiles cuando sabes que los usuarios usan un dispositivo Windows y quieres que lleguen directamente a la página de descripción de la Tienda; por ejemplo, quizás quieras aplicar este protocolo después de comprobar las cadenas de agente de usuario en un explorador para confirmar el sistema operativo del usuario o cuando ya te estás comunicando a través de una aplicación para UWP.

Para usar el protocolo de la Tienda Windows para vincular directamente a la lista de la Tienda de la aplicación, anexa el id. de la Tienda de la aplicación a este vínculo:

`ms-windows-store://pdp/?ProductId=`

Para obtener más información sobre cómo usar el protocolo de la Tienda Windows, consulta [Iniciar la aplicación de la Tienda Windows](../launch-resume/launch-store-app.md).

 

 







<!--HONumber=Aug16_HO3-->


