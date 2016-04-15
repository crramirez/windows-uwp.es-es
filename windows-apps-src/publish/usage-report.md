---
Description: El informe de uso del panel del Centro de desarrollo de Windows te permite ver cómo usan la aplicación los clientes.
title: Informe de uso
ms.assetid: 5F0E7F94-D121-4AD3-A6E5-9C0DEC437BD3
---

# Informe de uso


> **Importante**  El informe **Uso** solo proporciona datos si has activado el [SDK Visual Studio Application Insights](http://go.microsoft.com/fwlink/?LinkId=615086) en la aplicación (o habilitarlo activando la casilla "Mostrar telemetría en el centro de desarrollo de Windows" al crear el paquete). También debes enviar la aplicación al panel unificado del Centro de desarrollo antes de que se muestren los datos en el informe, y debes tener la telemetría de uso de la aplicación habilitada en la configuración Cuenta.

El informe de **uso** del panel del Centro de desarrollo de Windows te permite ver cómo usan la aplicación los clientes. Puedes visualizar estos datos en tu panel o [descargar el informe](download-analytic-reports.md) para consultarlo sin conexión.

Este informe proporciona un repaso de alto nivel al uso de la aplicación. Si tienes asociada una suscripción de Azure al SDK de Application Insights de Visual Studio para la aplicación, puedes obtener información más detallada de telemetría de uso de aplicación en el Portal de Azure. Los vínculos a los informes del Portal de Azure de la aplicación aparecerán cerca de la parte superior de este informe.

Ten en cuenta que la cuenta de Microsoft asociada a tu cuenta de desarrollador debe estar asociada a la suscripción de Azure usada para Application Insights. Si no estás usando la misma cuenta de Microsoft en ambos lugares, debes iniciar sesión en el Portal de administración de Azure y después agregar la cuenta de Microsoft asociada a tu cuenta de desarrollador para esta suscripción, con los roles de administrador de servicio, coadministrador o propietario.

## Aplicar filtros


En la parte superior de la página, se puede expandir **Aplicar filtros** para filtrar todos los datos de esta página por intervalo de fechas o grupo de productos (versiones de SO relacionadas).

-   **Fecha**: el filtro predeterminado es **Últimas 72 horas**, pero puedes ampliarlo hasta **Últimos 12 meses**.
-   **Grupos de productos**: el valor predeterminado es **Todos**. Si la aplicación incluye más de un grupo de productos, aquí puedes elegir uno específico.

La información de todos los gráficos que aparecen a continuación reflejará el período de tiempo seleccionado en **Aplicar filtros**. De manera predeterminada, se incluirán datos de todos los grupos de productos compatibles, a menos que hayas usado la sección **Aplicar filtros** para filtrar solo por una.

## Sesiones de usuario totales


El gráfico **Sesiones de usuario totales** muestra el número de sesiones de usuario diarias de la aplicación durante el período de tiempo seleccionado.

Cada sesión de usuario representa a un cliente que inicia y usa la aplicación durante un período de tiempo. Este gráfico no realiza un seguimiento de usuarios únicos (es decir, varias sesiones de usuario podrían ser del mismo cliente).

## Usuarios activos


El gráfico **Usuarios activos** muestra el número de clientes que usaron la aplicación en un día determinado durante el período de tiempo seleccionado.

Cada usuario activo representa a un cliente que usó la aplicación ese día. Este gráfico no realiza un seguimiento de las sesiones de usuario único (es decir, un cliente se representa en este gráfico tanto si usó la aplicación una vez como si lo hizo varias veces ese día).

## Promedio de duración de la sesión de usuario en segundos


El gráfico **Promedio de duración de la sesión de usuario en segundos** muestra el promedio de tiempo que un cliente usó la aplicación en un día determinado durante el período de tiempo seleccionado.

También puedes hacer clic en **Promedio de tiempo entre sesiones en segundos** para que este gráfico muestre el promedio de tiempo que transcurre entre sesiones independientes de uso de la aplicación durante el período de tiempo seleccionado.

## Eventos personalizados en los últimos 30 días


El gráfico **Eventos personalizados en los últimos 30 días** muestra el total de repeticiones de cualquier evento personalizado que has definido para la aplicación. Puede incluir varias repeticiones para el mismo cliente.

## Vistas de página en los últimos 30 días


El gráfico **Vistas de página en los últimos 30 días** muestra el número total de vistas de páginas específicas de la aplicación. Puede incluir varias vistas del mismo cliente.

 

 






<!--HONumber=Mar16_HO1-->


