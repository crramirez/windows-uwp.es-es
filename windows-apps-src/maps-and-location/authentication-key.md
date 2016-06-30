---
author: msatranjr
title: "Solicitar una clave de autenticación de mapas"
description: "La aplicación universal de Windows debe autenticarse para poder usar MapControl y los servicios de mapa en el espacio de nombres Windows.Services.Maps."
ms.assetid: 13B400D7-E13F-4F07-ACC3-9C34087F0F73
ms.sourcegitcommit: 92285ce32548bd6035c105e35c2b152432f8575a
ms.openlocfilehash: b7c981e071f70ab0a76d73333a94580b3c497b0e

---

# Solicitar una clave de autenticación de mapas


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


La [aplicación universal de Windows](https://msdn.microsoft.com/library/windows/apps/dn894631) debe autenticarse para poder usar [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) y los servicios de mapa en el espacio de nombres [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979). Para autenticar la aplicación, debes especificar una clave de autenticación de mapas. En este tema se describe cómo solicitar una clave de autenticación de mapas desde el [Centro para desarrolladores de Mapas de Bing](https://www.bingmapsportal.com/) y agregarla a la aplicación.

**Sugerencia** Para obtener más información sobre el uso de mapas en la aplicación, descarga el ejemplo siguiente del [repositorio de ejemplos de la plataforma universal de Windows](http://go.microsoft.com/fwlink/p/?LinkId=619979) que encontrarás en GitHub:

-   [Muestra de mapa en la Plataforma universal de Windows (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## Obtener una clave


Usa el [Centro para desarrolladores de Mapas de Bing](https://www.bingmapsportal.com/) para crear y administrar claves de autenticación de mapas para tus aplicaciones universales de Windows.

Para crear una nueva clave

1.  En el explorador, ve al Centro para desarrolladores de Mapas de Bing ([https://www.bingmapsportal.com](https://www.bingmapsportal.com/)).

2.  Si se te pide que inicies sesión, escribe tu cuenta de Microsoft y haz clic en **Iniciar sesión**.

3.  Elige la cuenta para asociarla con tu cuenta de Mapas de Bing. Si quieres usar tu cuenta de Microsoft, haz clic en **Sí**. De lo contrario, haz clic en **Iniciar sesión con otra cuenta**.

4.  Si no tienes una cuenta de Mapas de Bing, crea una nueva. Rellena los campos **Nombre de cuenta**, **Nombre del contacto**, **Nombre de la compañía**, **Dirección de correo electrónico** y **Número de teléfono**. Después de aceptar los términos de uso, haz clic en **Crear**.

5.  En el menú **Mi cuenta**, haz clic en **Crear o ver claves**.

6.  Haz clic en el vínculo **para crear una clave nueva**.

7.  Rellena el formulario **Crear clave** y, después, haz clic en **Crear**.

    -   **Nombre de la aplicación:** el nombre de tu aplicación.
    -   **Dirección URL de la aplicación (opcional):** dirección URL de tu aplicación.
    -   **Tipo de clave:** selecciona **Básica** o **Empresa**.
    -   **Tipo de aplicación:** selecciona **Aplicación universal de Windows** para usarla en tu aplicación universal de Windows.

    A continuación se muestra un ejemplo del aspecto del formulario.

    ![ejemplo del formulario crear clave.](images/createkeydialog.png)

8.  Después de hacer clic en **Crear**, la nueva clave aparece debajo del formulario **Crear clave**. Cópiala en un lugar seguro o agrégala inmediatamente a la aplicación, como se describe en el siguiente paso.

## Agregar la clave a la aplicación


La clave de autenticación de mapa es necesaria para usar [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) y los servicios de mapa ([**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979)) en la aplicación universal de Windows. Agrégala al control de mapa y asigna los objetos de servicio, según corresponda.

### Agregar la clave a un control de mapa

Para autenticar servicios en el espacio de nombres [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004), establece la propiedad [**MapServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn637036) en el valor de la clave de autenticación. Puedes establecer esta propiedad en el código o en el marcado XAML, según tus preferencias. Para obtener más información sobre el uso de **MapControl**, consulta [Mostrar mapas con vistas 2D, 3D y Streetside](display-maps.md).

-   Este ejemplo establece **MapServiceToken** en el valor de la clave de autenticación del código.

    ```cs
    MapControl1.MapServiceToken = "abcdef-abcdefghijklmno";
    ```

-   Este ejemplo establece **MapServiceToken** en el valor de la clave de autenticación del marcado XAML.

    ```xml
    <Maps:MapControl x:Name="MapControl1" MapServiceToken="abcdef-abcdefghijklmno"/>
    ```

### Agregar la clave a servicios de mapa

Para usar servicios en el espacio de nombres [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979), establece la propiedad [**ServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn636977) en el valor de clave de autenticación. Para obtener más información sobre el uso de los servicios de mapa, consulta [Mostrar rutas e indicaciones](routes-and-directions.md) y [Realizar geocodificación y geocodificación inversa](geocoding.md).

-   Este ejemplo establece **ServiceToken** en el valor de la clave de autenticación del código.

    ```cs
    MapService.ServiceToken = "abcdef-abcdefghijklmno";
    ```

## Temas relacionados

* [Centro para desarrolladores de Mapas de Bing](https://www.bingmapsportal.com/)
* [Muestra de mapa de UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Directrices de diseño para mapas](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vídeo de compilación de 2015: Aprovechamiento de mapas y ubicación entre teléfonos, tabletas y equipos en tus aplicaciones de Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Ejemplo de aplicación de tráfico de UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)





<!--HONumber=Jun16_HO4-->


