---
title: Solicitar una clave de autenticación de mapas
description: La aplicación universal de Windows debe autenticarse para poder usar MapControl y los servicios de mapa en el espacio de nombres Windows.Services.Maps.
ms.assetid: 13B400D7-E13F-4F07-ACC3-9C34087F0F73
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, asignar clave de autenticación, control de mapa
ms.localizationpriority: medium
ms.openlocfilehash: 2f4a76edfe5772665564cb8890ffcdf56205a2f7
ms.sourcegitcommit: d1eba7cf79cd2885b5bf8f5501bc44a569ab9864
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2020
ms.locfileid: "84172597"
---
# <a name="request-a-maps-authentication-key"></a>Solicitar una clave de autenticación de mapas

> [!WARNING]
> Los servicios de mapas en línea pueden no estar disponibles en versiones anteriores de Windows 10. En las siguientes versiones, MapControl ya no puede mostrar que las asignaciones y las API del espacio de nombres Windows. Services. Maps no devuelvan resultados:
> - Windows 10, versión 1607 y versiones anteriores: los servicios de mapa no estarán disponibles en todo el mundo a partir del 2020 de octubre
> - Windows 10, versión 1703 y versiones anteriores: los servicios de mapa no están disponibles en [algunos dispositivos vendidos en China](https://docs.microsoft.com/windows-hardware/customize/desktop/unattend/microsoft-windows-mapcontrol-desktop-chinavariantwin10)

La [aplicación universal de Windows](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide) debe autenticarse para poder usar [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) y los servicios de mapa en el espacio de nombres [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps). Para autenticar la aplicación, debes especificar una clave de autenticación de mapas. En este tema se describe cómo solicitar una clave de autenticación de mapas desde el [Centro para desarrolladores de Mapas de Bing](https://www.bingmapsportal.com/) y agregarla a la aplicación.

**Sugerencia** Para obtener más información sobre el uso de mapas en la aplicación, descarga el ejemplo siguiente del [repositorio de ejemplos de la plataforma universal de Windows](https://github.com/Microsoft/Windows-universal-samples) que encontrarás en GitHub:

-   [Ejemplo de mapa de la Plataforma universal de Windows (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)

## <a name="get-a-key"></a>Obtener una clave


Usa el [Centro para desarrolladores de Mapas de Bing](https://www.bingmapsportal.com/) para crear y administrar claves de autenticación de mapas para tus aplicaciones universales de Windows.

Para crear una nueva clave

1.  En el explorador, vaya al centro para desarrolladores de mapas de Bing ( [https://www.bingmapsportal.com](https://www.bingmapsportal.com/) ).

2.  Si se te pide que inicies sesión, escribe tu cuenta de Microsoft y haz clic en **Iniciar sesión**.

3.  Elige la cuenta para asociarla con tu cuenta de Mapas de Bing. Si quieres usar tu cuenta de Microsoft, haz clic en **Sí**. De lo contrario, haz clic en **Iniciar sesión con otra cuenta**.

4.  Si no tienes una cuenta de Mapas de Bing, crea una nueva. Rellena los campos **Nombre de cuenta**, **Nombre del contacto**, **Nombre de la compañía**, **Dirección de correo electrónico** y **Número de teléfono**. Después de aceptar los términos de uso, haz clic en **Crear**.

5.  En el menú **mi cuenta** , haga clic en **mis claves**.

6.  Si ya ha creado una clave, haga clic en el vínculo para crear una nueva clave. De lo contrario, continúe con el formulario crear clave.

7.  Rellena el formulario **Crear clave** y, después, haz clic en **Crear**.

    -   **Nombre de la aplicación:** el nombre de tu aplicación.
    -   **Dirección URL de la aplicación (opcional):** dirección URL de tu aplicación.
    -   **Tipo de clave:** selecciona **Básica** o **Empresa**.
    -   **Tipo de aplicación:** Seleccione **aplicación Windows** para su uso en la aplicación universal de Windows.

    A continuación se muestra un ejemplo del aspecto del formulario.

    ![ejemplo del formulario crear clave.](images/createkeydialog.png)

8.  Después de hacer clic en **Crear**, la nueva clave aparece debajo del formulario **Crear clave**. Cópiala en un lugar seguro o agrégala inmediatamente a la aplicación, como se describe en el siguiente paso.

## <a name="add-the-key-to-your-app"></a>Agregar la clave a la aplicación


La clave de autenticación de mapa es necesaria para usar [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) y los servicios de mapa ([**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps)) en la aplicación universal de Windows. Agrégala al control de mapa y asigna los objetos de servicio, según corresponda.

### <a name="to-add-the-key-to-a-map-control"></a>Agregar la clave a un control de mapa

Para autenticar servicios en el espacio de nombres [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl), establece la propiedad [**MapServiceToken**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken) en el valor de la clave de autenticación. Puedes establecer esta propiedad en el código o en el marcado XAML, según tus preferencias. Para obtener más información sobre el uso de **MapControl**, consulta [Mostrar mapas con vistas 2D, 3D y Streetside](display-maps.md).

-   Este ejemplo establece **MapServiceToken** en el valor de la clave de autenticación del código.

    ```cs
    MapControl1.MapServiceToken = "abcdef-abcdefghijklmno";
    ```

-   Este ejemplo establece **MapServiceToken** en el valor de la clave de autenticación del marcado XAML.

    ```xml
    <Maps:MapControl x:Name="MapControl1" MapServiceToken="abcdef-abcdefghijklmno"/>
    ```

### <a name="to-add-the-key-to-map-services"></a>Agregar la clave a servicios de mapa

Para usar servicios en el espacio de nombres [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps), establece la propiedad [**ServiceToken**](https://docs.microsoft.com/uwp/api/windows.services.maps.mapservice.servicetoken) en el valor de clave de autenticación. Para obtener más información sobre el uso de los servicios de mapa, consulta [Mostrar rutas e indicaciones](routes-and-directions.md) y [Realizar geocodificación y geocodificación inversa](geocoding.md).

-   Este ejemplo establece **ServiceToken** en el valor de la clave de autenticación del código.

    ```cs
    MapService.ServiceToken = "abcdef-abcdefghijklmno";
    ```

## <a name="related-topics"></a>Temas relacionados

* [Bing Maps Developer Center](https://www.bingmapsportal.com/)
* [Ejemplo de mapa de UWP](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Directrices de diseño para mapas](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [Vídeo de compilación de 2015: Leveraging Maps and Location Across Phone, Tablet, and PC in Your Windows Apps (Aprovechar los mapas y la ubicación entre teléfonos, tabletas y equipos en tus aplicaciones Windows)](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Ejemplo de aplicación de tráfico para UWP](https://github.com/Microsoft/Windows-appsample-trafficapp)
