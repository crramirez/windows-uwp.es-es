---
title: Solicitar una clave de autenticación de mapas
description: La aplicación universal de Windows debe autenticarse para poder usar MapControl y los servicios de mapa en el espacio de nombres Windows.Services.Maps.
ms.assetid: 13B400D7-E13F-4F07-ACC3-9C34087F0F73
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, clave de autenticación de mapa, control de mapa
ms.localizationpriority: medium
ms.openlocfilehash: e986880ccedfdb4648b1554c35c23a8a841fe820
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/28/2018
ms.locfileid: "7841525"
---
# <a name="request-a-maps-authentication-key"></a>Solicitar una clave de autenticación de mapas




La [aplicación universal de Windows](https://msdn.microsoft.com/library/windows/apps/dn894631) debe autenticarse para poder usar [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) y los servicios de mapa en el espacio de nombres [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979). Para autenticar la aplicación, debes especificar una clave de autenticación de mapas. En este tema se describe cómo solicitar una clave de autenticación de mapas desde el [Centro para desarrolladores de Mapas de Bing](https://www.bingmapsportal.com/) y agregarla a la aplicación.

**Sugerencia** Para obtener más información sobre el uso de mapas en la aplicación, descarga la muestra siguiente del [repositorio Windows-universal-samples](http://go.microsoft.com/fwlink/p/?LinkId=619979) que encontrarás en GitHub:

-   [Muestra de mapa en la Plataforma universal de Windows (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## <a name="get-a-key"></a>Obtener una clave


Usa el [Centro para desarrolladores de Mapas de Bing](https://www.bingmapsportal.com/) para crear y administrar claves de autenticación de mapas para tus aplicaciones universales de Windows.

Para crear una nueva clave

1.  En el explorador, ve al centro de desarrollo de mapas de Bing ([https://www.bingmapsportal.com](https://www.bingmapsportal.com/)).

2.  Si se te pide que inicies sesión, escribe tu cuenta de Microsoft y haz clic en **Iniciar sesión**.

3.  Elige la cuenta para asociarla con tu cuenta de Mapas de Bing. Si quieres usar tu cuenta de Microsoft, haz clic en **Sí**. De lo contrario, haz clic en **Iniciar sesión con otra cuenta**.

4.  Si no tienes una cuenta de Mapas de Bing, crea una nueva. Rellena los campos **Nombre de cuenta**, **Nombre del contacto**, **Nombre de la compañía**, **Dirección de correo electrónico** y **Número de teléfono**. Después de aceptar los términos de uso, haz clic en **Crear**.

5.  En el menú **Mi cuenta**, haz clic en **Mis claves**.

6.  Si creaste una clave anteriormente, haz clic en el vínculo para crear una nueva clave. De lo contrario, continúa con el formulario Crear clave.

7.  Rellena el formulario **Crear clave** y, después, haz clic en **Crear**.

    -   **Nombre de la aplicación:** El nombre de tu aplicación.
    -   **Dirección URL de la aplicación (opcional):** La dirección URL de tu aplicación.
    -   **Tipo de clave:** Selecciona **Básica** o **Empresa**.
    -   **Tipo de aplicación:** Selecciona **Aplicación universal de Windows** para usarla en tu aplicación universal de Windows.

    A continuación se muestra un ejemplo del aspecto del formulario.

    ![ejemplo del formulario crear clave.](images/createkeydialog.png)

8.  Después de hacer clic en **Crear**, la nueva clave aparece debajo del formulario **Crear clave**. Cópiala en un lugar seguro o agrégala inmediatamente a la aplicación, como se describe en el siguiente paso.

## <a name="add-the-key-to-your-app"></a>Agregar la clave a la aplicación


La clave de autenticación de mapa es necesaria para usar [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) y los servicios de mapa ([**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979)) en la aplicación universal de Windows. Agrégala al control de mapa y asigna los objetos de servicio, según corresponda.

### <a name="to-add-the-key-to-a-map-control"></a>Agregar la clave a un control de mapa

Para autenticar servicios en el espacio de nombres [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004), establece la propiedad [**MapServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn637036) en el valor de la clave de autenticación. Puedes establecer esta propiedad en el código o en el marcado XAML, según tus preferencias. Para obtener más información sobre el uso de **MapControl**, consulta [Mostrar mapas con vistas 2D, 3D y Streetside](display-maps.md).

-   Este ejemplo establece **MapServiceToken** en el valor de la clave de autenticación del código.

    ```cs
    MapControl1.MapServiceToken = "abcdef-abcdefghijklmno";
    ```

-   Este ejemplo establece **MapServiceToken** en el valor de la clave de autenticación del marcado XAML.

    ```xml
    <Maps:MapControl x:Name="MapControl1" MapServiceToken="abcdef-abcdefghijklmno"/>
    ```

### <a name="to-add-the-key-to-map-services"></a>Agregar la clave a servicios de mapa

Para usar servicios en el espacio de nombres [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979), establece la propiedad [**ServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn636977) en el valor de clave de autenticación. Para obtener más información sobre el uso de los servicios de mapa, consulta [Mostrar rutas e indicaciones](routes-and-directions.md) y [Realizar geocodificación y geocodificación inversa](geocoding.md).

-   Este ejemplo establece **ServiceToken** en el valor de la clave de autenticación del código.

    ```cs
    MapService.ServiceToken = "abcdef-abcdefghijklmno";
    ```

## <a name="related-topics"></a>Temas relacionados

* [Centro para desarrolladores de Mapas de Bing](https://www.bingmapsportal.com/)
* [Muestra de mapa de UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Directrices de diseño para mapas](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vídeo de compilación de 2015: Leveraging Maps and Location Across Phone, Tablet, and PC in Your Windows Apps (Aprovechamiento de mapas y ubicación entre teléfonos, tabletas y equipos en tus aplicaciones de Windows)](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Ejemplo de aplicación de tráfico de UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)
