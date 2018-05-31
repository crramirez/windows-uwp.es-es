---
author: PatrickFarley
title: Comunicarse con un servicio de aplicaciones remoto
description: Puedes intercambiar mensajes con un servicio de aplicaciones que se ejecute en un dispositivo remoto mediante Project Rome.
ms.assetid: a0261e7a-5706-4f9a-b79c-46a3c81b136f
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, Windows 10, uwp, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a40b48df9f9d8fe740795d6af0d71ba70a34c469
ms.sourcegitcommit: d780e3a087ab5240ea643346480a1427bea9e29b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/09/2018
ms.locfileid: "1572772"
---
# <a name="communicate-with-a-remote-app-service"></a>Comunicarse con un servicio de aplicaciones remoto

Además de iniciar una aplicación en un dispositivo remoto mediante un URI, también puedes ejecutarla y comunicarte con los *servicios de aplicaciones* de dispositivos remotos. Cualquier dispositivo basado en Windows se puede usar como dispositivo cliente o host. Esto te proporciona un número casi ilimitado de formas de interactuar con dispositivos conectados sin necesidad de llevar una aplicación al primer plano.

## <a name="set-up-the-app-service-on-the-host-device"></a>Configurar el servicio de aplicaciones en el dispositivo host
Para ejecutar un servicio de aplicaciones en un dispositivo remoto, debes contar ya con un proveedor de dicho servicio de aplicaciones instalado en el dispositivo host. Esta guía usará la versión CSharp del servicio de aplicaciones del [ejemplo del servicio de aplicaciones del generador de números aleatorios](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices), que está disponible en el [repositorio de muestras universales de Windows](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AppServices). Para obtener instrucciones sobre cómo escribir tu propio servicio de aplicaciones, consulta [Crear y consumir un servicio de aplicaciones](how-to-create-and-consume-an-app-service.md).

Si usas un servicio de aplicaciones ya hecho o escribes el tuyo propio, deberás realizar algunas modificaciones para que el servicio sea compatible con sistemas remotos. En Visual Studio, ve al proyecto del proveedor de servicios de aplicaciones (denominado "AppServicesProvider" en el ejemplo) y selecciona su archivo _Package.appxmanifest_. Haz clic con el botón secundario y selecciona **Ver código** para ver todo el contenido del archivo. Busca el elemento **Extension** que define el proyecto como un servicio de aplicaciones y denomina su proyecto principal.

``` xml
...
<Extensions>
    <uap:Extension Category="windows.appService" EntryPoint="RandomNumberService.RandomNumberGeneratorTask">
        <uap3:AppService Name="com.microsoft.randomnumbergenerator"/>
    </uap:Extension>
</Extensions>
...
```

Agrega el atributo **SupportsRemoteSystems** si aún no está presente:

``` xml
...
<uap3:AppService Name="com.microsoft.randomnumbergenerator" SupportsRemoteSystems="true"/>
...
```

Compila el proyecto del proveedor del servicio de aplicaciones e impleméntalo en los dispositivos host.

## <a name="target-the-app-service-from-the-client-device"></a>Dirígete al servicio de aplicaciones desde el dispositivo cliente
El dispositivo desde el que se va a llamar al servicio de aplicaciones remoto necesita una aplicación con la funcionalidad de sistemas remotos. Esto se puede agregar en la misma aplicación que proporciona el servicio de aplicaciones en el dispositivo host (en cuyo caso se instala la misma aplicación en ambos dispositivos) o se puede implementar en una aplicación totalmente distinta.

Las siguientes instrucciones **using** son necesarias para que el código de esta sección se ejecute tal cual:

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetUsings)]


Primero debes crear una instancia de un objeto [**AppServiceConnection**](https://msdn.microsoft.com/library/windows/apps/Windows.ApplicationModel.AppService.AppServiceConnection), como si tuvieras que llamar a un servicio de aplicaciones localmente. Este proceso se describe con más detalle en [Crear y consumir un servicio de aplicaciones](how-to-create-and-consume-an-app-service.md). En este ejemplo, el servicio de aplicaciones de destino es el servicio del generador de números aleatorios.

> [!NOTE]
> Se supone que ya se ha adquirido un objeto [RemoteSystem](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystem) por algún medio dentro del código que llamará al siguiente método. Consulta [Iniciar una aplicación remota](launch-a-remote-app.md) para obtener instrucciones sobre cómo configurar esta función.

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetAppService)]

Luego, se crea un objeto [**RemoteSystemConnectionRequest**](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems.RemoteSystemConnectionRequest) para el dispositivo remoto previsto. A continuación, se usa para abrir **AppServiceConnection** para ese dispositivo. Ten en cuenta que, en el siguiente ejemplo, el control y los informes de errores se simplifican por motivos de brevedad.

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetRemoteConnection)]

En este punto, deberías tener una conexión abierta a un servicio de aplicaciones en un equipo remoto.

## <a name="exchange-service-specific-messages-over-the-remote-connection"></a>Mensajes específicos del servicio de Exchange mediante la conexión remota

Desde aquí, puedes enviar y recibir mensajes al servicio y desde el servicio en el formulario de objetos [**ValueSet**](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset) (para obtener más información, consulta [Crear y consumir un servicio de aplicaciones](how-to-create-and-consume-an-app-service.md)). El servicio de generador de números aleatorios toma dos enteros con las claves `"minvalue"` y `"maxvalue"` como entradas, selecciona aleatoriamente un número entero dentro de su intervalo y lo devuelve al proceso de llamada con la clave `"Result"`.

[!code-cs[Main](./code/RemoteAppService/MainPage.xaml.cs#SnippetSendMessage)]

Ahora que te has conectado a un servicio de aplicaciones en un dispositivo remoto host, ejecuta una operación en dicho dispositivo y los datos recibidos en tu dispositivo cliente como respuesta.

## <a name="related-topics"></a>Temas relacionados

[Introducción a aplicaciones y dispositivos conectados (Project Rome)](connected-apps-and-devices.md)  
[Iniciar una aplicación remota](launch-a-remote-app.md)  
[Crear y usar un servicio de aplicaciones](how-to-create-and-consume-an-app-service.md)  
[Referencia de API de sistemas remotos](https://msdn.microsoft.com/library/windows/apps/Windows.System.RemoteSystems)  
[Muestra de sistemas remotos](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/RemoteSystems)
