---
title: Comunicarse con un servicio de aplicaciones remoto
description: Intercambie mensajes con un servicio de aplicaciones que se ejecuta en un dispositivo remoto mediante el proyecto Roma.
ms.assetid: a0261e7a-5706-4f9a-b79c-46a3c81b136f
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, dispositivos conectados, sistemas remotos, Roma, proyecto Roma, tarea en segundo plano, App Service
ms.localizationpriority: medium
ms.openlocfilehash: 779205a47b85cf9f9a0aec9db910b97995dc2cd8
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363918"
---
# <a name="communicate-with-a-remote-app-service"></a>Comunicarse con un servicio de aplicaciones remoto

Además de iniciar una aplicación en un dispositivo remoto mediante un URI, también puedes ejecutarla y comunicarte con los *servicios de aplicaciones* de dispositivos remotos. Cualquier dispositivo basado en Windows se puede usar como dispositivo cliente o host. Esto te proporciona un número casi ilimitado de formas de interactuar con dispositivos conectados sin necesidad de llevar una aplicación al primer plano.

## <a name="set-up-the-app-service-on-the-host-device"></a>Configurar el servicio de aplicaciones en el dispositivo host
Para ejecutar un servicio de aplicaciones en un dispositivo remoto, debes contar ya con un proveedor de dicho servicio de aplicaciones instalado en el dispositivo host. En esta guía se usará la versión de CSharp del [ejemplo de App Service generator de número aleatorio](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices), que está disponible en el [repositorio de muestras universales de Windows](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices). Para obtener instrucciones sobre cómo escribir tu propio servicio de aplicaciones, consulta [Crear y consumir un servicio de aplicaciones](how-to-create-and-consume-an-app-service.md).

Si usas un servicio de aplicaciones ya hecho o escribes el tuyo propio, deberás realizar algunas modificaciones para que el servicio sea compatible con sistemas remotos. En Visual Studio, vaya al proyecto del proveedor de App Service (llamado "AppServicesProvider" en el ejemplo) y seleccione su archivo _Package. appxmanifest_ . Haz clic con el botón secundario y selecciona **Ver código** para ver todo el contenido del archivo. Cree un elemento **extensions** dentro del elemento **Application** principal (o busque si ya existe). A continuación, cree una **extensión** para definir el proyecto como App Service y hacer referencia a su proyecto primario.

``` xml
...
<Extensions>
    <uap:Extension Category="windows.appService" EntryPoint="RandomNumberService.RandomNumberGeneratorTask">
        <uap3:AppService Name="com.microsoft.randomnumbergenerator"/>
    </uap:Extension>
</Extensions>
...
```

Junto al elemento **AppService** , agregue el atributo **SupportsRemoteSystems** :

``` xml
...
<uap3:AppService Name="com.microsoft.randomnumbergenerator" SupportsRemoteSystems="true"/>
...
```

Para usar elementos en este espacio de nombres **uap3** , debe agregar la definición de espacio de nombres en la parte superior del archivo de manifiesto si aún no está allí.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3">
  ...
</Package>
```

Después, compile el proyecto de proveedor de App Service e impleméntelo en los dispositivos host.

## <a name="target-the-app-service-from-the-client-device"></a>Dirígete al servicio de aplicaciones desde el dispositivo cliente
El dispositivo desde el que se va a llamar al servicio de aplicaciones remoto necesita una aplicación con la funcionalidad de sistemas remotos. Esto se puede agregar en la misma aplicación que proporciona el servicio de aplicaciones en el dispositivo host (en cuyo caso se instala la misma aplicación en ambos dispositivos) o se puede implementar en una aplicación totalmente distinta.

Las siguientes instrucciones **using** son necesarias para que el código de esta sección se ejecute tal cual:

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteAppService/cs/MainPage.xaml.cs" id="SnippetUsings":::


Primero debes crear una instancia de un objeto [**AppServiceConnection**](/uwp/api/Windows.ApplicationModel.AppService.AppServiceConnection), como si tuvieras que llamar a un servicio de aplicaciones localmente. Este proceso se describe con más detalle en [Crear y consumir un servicio de aplicaciones](how-to-create-and-consume-an-app-service.md). En este ejemplo, el servicio de aplicaciones de destino es el servicio del generador de números aleatorios.

> [!NOTE]
> Se supone que ya se ha adquirido un objeto [RemoteSystem](/uwp/api/Windows.System.RemoteSystems.RemoteSystem) por algún medio dentro del código que llamará al siguiente método. Consulta [Iniciar una aplicación remota](launch-a-remote-app.md) para obtener instrucciones sobre cómo configurar esta función.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteAppService/cs/MainPage.xaml.cs" id="SnippetAppService":::

Luego, se crea un objeto [**RemoteSystemConnectionRequest**](/uwp/api/Windows.System.RemoteSystems.RemoteSystemConnectionRequest) para el dispositivo remoto previsto. A continuación, se usa para abrir **AppServiceConnection** para ese dispositivo. Ten en cuenta que, en el siguiente ejemplo, el control y los informes de errores se simplifican por motivos de brevedad.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteAppService/cs/MainPage.xaml.cs" id="SnippetRemoteConnection":::

En este punto, deberías tener una conexión abierta a un servicio de aplicaciones en un equipo remoto.

## <a name="exchange-service-specific-messages-over-the-remote-connection"></a>Mensajes específicos del servicio de Exchange mediante la conexión remota

Desde aquí, puedes enviar y recibir mensajes al servicio y desde el servicio en el formulario de objetos [**ValueSet**](/uwp/api/windows.foundation.collections.valueset) (para obtener más información, consulta [Crear y consumir un servicio de aplicaciones](how-to-create-and-consume-an-app-service.md)). El servicio de generador de números aleatorios toma dos enteros con las claves `"minvalue"` y `"maxvalue"` como entradas, selecciona aleatoriamente un número entero dentro de su intervalo y lo devuelve al proceso de llamada con la clave `"Result"`.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/launch-resume/RemoteAppService/cs/MainPage.xaml.cs" id="SnippetSendMessage":::

Ahora que te has conectado a un servicio de aplicaciones en un dispositivo remoto host, ejecuta una operación en dicho dispositivo y los datos recibidos en tu dispositivo cliente como respuesta.

## <a name="related-topics"></a>Temas relacionados

[Información general sobre dispositivos y aplicaciones conectados (proyecto Roma)](connected-apps-and-devices.md)  
[Iniciar una aplicación remota](launch-a-remote-app.md)  
[Crear y usar un servicio de aplicación](how-to-create-and-consume-an-app-service.md)  
[Referencia de API de sistemas remotos](/uwp/api/Windows.System.RemoteSystems)  
[Muestra de sistemas remotos](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)
