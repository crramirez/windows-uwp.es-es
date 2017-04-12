---
author: jnHs
Description: "El informe de uso del panel del Centro de desarrollo de Windows te permite ver cómo usan la aplicación los clientes."
title: Informe de uso
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: f15b72a814c713a389a1f6126e2261959a16ee6c
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="usage-report"></a>Informe de uso


El informe **Uso** del panel del Centro de desarrollo de Windows te permite ver cómo los clientes de Windows10 usan la aplicación y obtener información sobre los eventos personalizados que hayas definido. Puedes visualizar estos datos en tu panel o [descargar el informe](download-analytic-reports.md) para consultarlo sin conexión.

> **Nota**  Anteriormente, el informe de **uso** solo proporcionaba datos si el SDK de Visual Studio Application Insights estaba activado en la aplicación. Con el informe de **uso** actualizado, esto ya no es necesario.

## <a name="apply-filters"></a>Aplicar filtros


En la parte superior de la página, se puede expandir **Aplicar filtros** para filtrar todos los datos de esta página por intervalo de fechas o grupo de productos (versiones de SO relacionadas).

-   **Fecha**: el filtro predeterminado es **Últimos 30 días**, pero puedes ampliarlo hasta **Últimos 3 meses**.
-   **Versión del paquete**: el valor predeterminado es **Todas**. Si la aplicación incluye más de un paquete, puedes elegir uno concreto aquí.
-   **Tipo de dispositivo**: el valor predeterminado es **Todos**, pero también puedes mostrar los datos de un determinado tipo de dispositivo.

La información de todos los gráficos que aparecen a continuación reflejará el período de tiempo seleccionado en **Aplicar filtros**. De manera predeterminada, se incluirán datos de todos los tipos de dispositivo compatibles y las versiones del paquete, a menos que hayas usado la sección **Aplicar filtros** para filtrar solo por una.

> **Nota** En este informe solo se incluyen los datos de uso de los clientes de Windows 10.

## <a name="total-user-sessions"></a>Sesiones de usuario totales

El gráfico **Sesiones de usuario totales** muestra el número de sesiones de usuario diarias de la aplicación durante el período de tiempo seleccionado.

Cada sesión de usuario representa un distinto período de tiempo cuando un usuario interactuó con la aplicación. Se considera que cada sesión de usuario finalizará después de un período de inactividad, por lo que solo un cliente puede tener varias sesiones de usuario en el mismo día. Ten en cuenta que este gráfico no realiza un seguimiento de usuarios únicos de la aplicación.

## <a name="active-users"></a>Usuarios activos

El gráfico **Usuarios activos** muestra el número de clientes que usaron la aplicación en un día determinado durante el período de tiempo seleccionado.

Cada usuario activo representa a un cliente que usó la aplicación ese día. Este gráfico no realiza un seguimiento de las sesiones de usuario único (es decir, un cliente se representa en este gráfico tanto si usó la aplicación una vez como si lo hizo varias veces ese día).

## <a name="custom-events"></a>Eventos personalizados

El gráfico **Eventos personalizados** muestra el total de repeticiones de cualquier evento personalizado que has definido para la aplicación. Puede incluir varias repeticiones para el mismo cliente.

Los eventos personalizados se implementan mediante el método [StoreServicesCustomEventLogger.Log](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) de [Microsoft Store Services SDK](../monetize/microsoft-store-services-sdk.md).



 
