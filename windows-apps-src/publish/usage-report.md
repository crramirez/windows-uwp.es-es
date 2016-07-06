---
author: jnHs
Description: "El informe de uso del panel del Centro de desarrollo de Windows te permite ver cómo usan la aplicación los clientes."
title: Informe de uso
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
translationtype: Human Translation
ms.sourcegitcommit: 056642044953bab02f78912c7611ddcf5d6d48e6
ms.openlocfilehash: 476e7ee0c9c7ea7dce7f5e3a0389091ede9132c4

---

# Informe de uso


El informe de **uso** del panel Centro de desarrollo de Windows te permite ver cómo los clientes usan la aplicación y obtener información sobre los eventos personalizados que definiste. Puedes visualizar estos datos en tu panel o [descargar el informe](download-analytic-reports.md) para consultarlo sin conexión.

> **Nota**  Anteriormente, el informe de **uso** solo proporcionaba datos si el SDK de Visual Studio Application Insights estaba activado en la aplicación. Con el informe de **uso** actualizado, esto ya no es necesario.

## Aplicar filtros


En la parte superior de la página, se puede expandir **Aplicar filtros** para filtrar todos los datos de esta página por intervalo de fechas o grupo de productos (versiones de SO relacionadas).

-   **Fecha**: el filtro predeterminado es **Últimos 30 días**, pero puedes ampliarlo hasta **Últimos 12 meses**.
-   **Versión del paquete**: el valor predeterminado es **Todas las versiones**. Si la aplicación incluye más de un paquete, puedes elegir uno concreto aquí.
-   **Tipo de dispositivo**: el valor predeterminado es **Todos**, pero puede mostrar los datos de un determinado tipo de dispositivo.

La información de todos los gráficos que aparecen a continuación reflejará el período de tiempo seleccionado en **Aplicar filtros**. De manera predeterminada, se incluirán datos de todos los tipos de dispositivo compatibles y las versiones del paquete, a menos que hayas usado la sección **Aplicar filtros** para filtrar solo por una.

## Sesiones de usuario totales

El gráfico **Sesiones de usuario totales** muestra el número de sesiones de usuario diarias de la aplicación durante el período de tiempo seleccionado.

Cada sesión de usuario representa un distinto período de tiempo cuando un usuario interactuó con la aplicación. Se considera que cada sesión de usuario finalizará después de un período de inactividad, por lo que solo un cliente puede tener varias sesiones de usuario en el mismo día. Ten en cuenta que este gráfico no realiza un seguimiento de usuarios únicos de la aplicación.

## Usuarios activos

El gráfico **Usuarios activos** muestra el número de clientes que usaron la aplicación en un día determinado durante el período de tiempo seleccionado.

Cada usuario activo representa a un cliente que usó la aplicación ese día. Este gráfico no realiza un seguimiento de las sesiones de usuario único (es decir, un cliente se representa en este gráfico tanto si usó la aplicación una vez como si lo hizo varias veces ese día).

## Eventos personalizados

El gráfico **Eventos personalizados** muestra el total de repeticiones de cualquier evento personalizado que has definido para la aplicación. Puede incluir varias repeticiones para el mismo cliente.

Los eventos personalizados se implementan con el método [registro](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomevents.log.aspx) del [SDK de Microsoft Store Engagement and Monetization](../monetize/monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md).



 







<!--HONumber=Jun16_HO4-->


