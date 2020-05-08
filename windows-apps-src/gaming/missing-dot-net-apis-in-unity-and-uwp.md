---
title: Falta de API de .NET en Unity y UWP
description: Obtenga información sobre las API de .NET que faltan al compilar juegos para UWP en Unity y soluciones alternativas para problemas comunes.
ms.assetid: 28A8B061-5AE8-4CDA-B4AB-2EF0151E57C1
ms.date: 02/21/2018
ms.topic: article
keywords: Windows 10, UWP, juegos, .net, Unity
ms.localizationpriority: medium
ms.openlocfilehash: 8c7f906a19ddbabea85b0426aca9e41a62327e36
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2020
ms.locfileid: "82729804"
---
# <a name="missing-net-apis-in-unity-and-uwp"></a>Falta de API de .NET en Unity y UWP

Al compilar un juego para UWP con .NET, es posible que algunas API que puede usar en el editor de Unity o en un juego de equipos independiente no estén presentes para UWP. Esto se debe a que .NET para aplicaciones para UWP incluye un subconjunto de los tipos proporcionados en el .NET Framework completo para cada espacio de nombres.

Además, algunos motores de juegos usan diferentes tipos de .NET que no son totalmente compatibles con .NET para UWP, como mono de Unity. Por lo tanto, cuando se escribe el juego, todo podría funcionar correctamente en el editor, pero al ir a la compilación de UWP, podría obtenerse un error similar al siguiente: **el tipo o el espacio de nombres ' formateadores ' no existe en el espacio de nombres ' System. Runtime. Serialization ' (¿falta una referencia de ensamblado?)**

Afortunadamente, Unity proporciona algunas de estas API que faltan como métodos de extensión y tipos de reemplazo, que se describen en [plataforma universal de Windows: faltan tipos de .net en el back-end de scripting de .net](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html). Sin embargo, si la funcionalidad que necesita no está aquí, la [información general sobre las aplicaciones de .net para Windows 8. x](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140)) describe las formas en que puede convertir el código para usar WinRT o .net para las api de Windows Runtime. (Describe Windows 8, pero también es aplicable a las aplicaciones para UWP de Windows 10).

## <a name="net-standard"></a>.NET Standard

Para comprender por qué algunas API pueden no funcionar, es importante comprender los distintos tipos de .NET y cómo implementa UWP .NET. El [.net Standard](https://docs.microsoft.com/dotnet/standard/net-standard) es una especificación formal de las API de .net que está pensada para ser multiplataforma y unificar los distintos tipos de .net. Cada implementación de .NET admite una versión determinada del .NET Standard. Puede ver una tabla de estándares e implementaciones en [compatibilidad con la implementación de .net](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support).

Cada versión del SDK de UWP se ajusta a un nivel diferente de .NET Standard. Por ejemplo, el SDK de 16299 (Fall Creators Update) admite .NET Standard 2,0.

Si desea saber si se admite una determinada API de .NET en la versión de UWP a la que está destinada, puede consultar la referencia de la [API de .net Standard](https://docs.microsoft.com/dotnet/api/index?view=netstandard-2.0) y seleccionar la versión de la .net Standard compatible con esa versión de UWP.

## <a name="scripting-backend-configuration"></a>Scripting de configuración de back-end

Lo primero que debe hacer si tiene problemas para compilar para UWP es comprobar la **configuración del reproductor** (**archivos > configuración de compilación**, seleccionar **plataforma universal de Windows**y, después, **configuración del reproductor**). En **otros valores > configuración**, las tres primeras listas desplegables (**versión del tiempo de ejecución del script**, **back-end de scripting**y **nivel de compatibilidad de API**) son importantes que se deben tener en cuenta.

La **versión de scripting en tiempo de ejecución** es lo que el back-end de scripting de Unity usa, lo que le permite obtener la versión equivalente (aproximadamente) de .NET Framework soporte que elija. Sin embargo, tenga en cuenta que no se admitirán todas las API de esa versión de .NET Framework, solo las de la versión de .NET Standard de destino de UWP.

A menudo con las nuevas versiones de .NET, se agregan más API a .NET Standard que pueden permitirle usar el mismo código en la plataforma independiente y en UWP. Por ejemplo, el espacio de nombres [System. Runtime. Serialization. JSON](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.json) se presentó en .net Standard 2,0. Si establece la **versión del tiempo de ejecución de scripting** en **.net 3,5 equivalente** (que tiene como destino una versión anterior del .net Standard), obtendrá un error al intentar usar la API; Cambie al **equivalente de .net 4,6** (que admite .net Standard 2,0) y la API funcionará.

El **back-end de scripting** puede ser **.net** o **IL2CPP**. En este tema, se supone que ha elegido **.net**, ya que es donde surgen los problemas descritos aquí. Consulte [scripting back-ends](https://docs.unity3d.com/Manual/windowsstore-scriptingbackends.html) para obtener más información.

Por último, debe establecer el **nivel de compatibilidad** de la API en la versión de .net en la que desea que se ejecute el juego. Debe coincidir con la **versión del tiempo de ejecución del script**.

En general, para la **versión del tiempo de ejecución de script** y el nivel de compatibilidad de la **API**, debe seleccionar la última versión disponible para tener más compatibilidad con el .NET Framework y, por tanto, permitirle usar más API de .net.

![Configuración: versión de scripting en tiempo de ejecución; Back-end de scripting; Nivel de compatibilidad de API](images/missing-dot-net-apis-in-unity-1.png)

## <a name="platform-dependent-compilation"></a>Compilación dependiente de la plataforma

Si va a compilar el juego Unity para varias plataformas, incluido UWP, querrá usar la compilación dependiente de la plataforma para asegurarse de que el código destinado a UWP solo se ejecuta cuando el juego se compila como una UWP. De este modo, puede usar el .NET Framework completo para el escritorio independiente y otras plataformas, y las API de WinRT para UWP, sin tener que obtener errores de compilación.

Use las siguientes directivas para compilar solo el código cuando se ejecuta como una aplicación de UWP:

```csharp
#if NETFX_CORE
    // Your UWP code here
#else
    // Your standard code here
#endif
```

> [!NOTE]
> `NETFX_CORE`solo está pensado para comprobar si está compilando código C# en el back-end de scripting de .NET. Si usa un back-end de scripting diferente, como IL2CPP, use [`ENABLE_WINMD_SUPPORT`](https://docs.unity3d.com/Manual/windowsstore-code-snippets.html) en su lugar.

Para obtener la lista completa de las directivas de compilación dependientes de la plataforma, consulte [compilación dependiente](https://docs.unity3d.com/Manual/PlatformDependentCompilation.html)de la plataforma.

## <a name="common-issues-and-workarounds"></a>Problemas comunes y soluciones alternativas

En los siguientes escenarios se describen los problemas comunes que pueden surgir cuando faltan las API de .NET en el subconjunto de UWP y las maneras de desplazarse por ellas.

### <a name="data-serialization-using-binaryformatter"></a>Serialización de datos con BinaryFormatter

Es habitual que los juegos serialicen los datos guardados para que los jugadores no puedan manipularlos fácilmente. Sin embargo, [BinaryFormatter](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.formatters.binary.binaryformatter), que serializa un objeto en binario, no está disponible en versiones anteriores del .net Standard (anterior a 2,0). Considere la posibilidad de usar [XmlSerializer](https://docs.microsoft.com/dotnet/api/system.xml.serialization.xmlserializer) o [DataContractJsonSerializer](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.json.datacontractjsonserializer) en su lugar.

```csharp
private void Save()
{
    SaveData data = new SaveData(); // User-defined object to serialize

    DataContractJsonSerializer serializer = 
      new DataContractJsonSerializer(typeof(SaveData));

    FileStream stream = 
      new FileStream(Application.persistentDataPath, FileMode.CreateNew);

    serializer.WriteObject(stream, data);
    stream.Dispose();
}
```

### <a name="io-operations"></a>operaciones de E/S

Algunos tipos del espacio de nombres [System.IO](https://docs.microsoft.com/dotnet/api/system.io) , como [FileStream](https://docs.microsoft.com/dotnet/api/system.io.filestream), no están disponibles en versiones anteriores de la .net Standard. Sin embargo, Unity proporciona los tipos de [directorio](https://docs.microsoft.com/dotnet/api/system.io.directory), [archivo](https://docs.microsoft.com/dotnet/api/system.io.file)y **FileStream** para poder usarlos en el juego.

Como alternativa, puede usar las API de [Windows. Storage](https://docs.microsoft.com/uwp/api/Windows.Storage) , que solo están disponibles para las aplicaciones de UWP. Sin embargo, estas API restringen la aplicación a escribir en su almacenamiento específico y no proporcionan acceso gratuito a todo el sistema de archivos. Consulte [archivos, carpetas y bibliotecas](https://docs.microsoft.com/windows/uwp/files/) para obtener más información.

Una nota importante es que el método [Close](https://docs.microsoft.com/dotnet/api/system.io.stream.close) solo está disponible en .net Standard 2,0 y versiones posteriores (aunque Unity proporciona un método de extensión). Use [Dispose](https://docs.microsoft.com/dotnet/api/system.io.stream.dispose) en su lugar.

### <a name="threading"></a>Subprocesos

Algunos tipos de los espacios de nombres [System. Threading](https://docs.microsoft.com/dotnet/api/system.threading) , como [ThreadPool](https://docs.microsoft.com/dotnet/api/system.threading.threadpool), no están disponibles en versiones anteriores del .net Standard. En estos casos, puede usar el espacio de nombres [Windows. System. Threading](https://docs.microsoft.com/uwp/api/windows.system.threading) en su lugar.

Aquí se muestra cómo puede controlar los subprocesos en un juego de Unity mediante la compilación dependiente de la plataforma para preparar las plataformas UWP y que no son de UWP:

```csharp
private void UsingThreads()
{
#if NETFX_CORE
    Windows.System.Threading.ThreadPool.RunAsync(workItem => SomeMethod());
#else
    System.Threading.ThreadPool.QueueUserWorkItem(workItem => SomeMethod());
#endif
}
```

### <a name="security"></a>Seguridad

Parte de **System. Security.** * los espacios de nombres, como [System. Security. Cryptography. X509Certificates](https://docs.microsoft.com/dotnet/api/system.security.cryptography.x509certificates?view=netstandard-2.0), no están disponibles al compilar un juego de Unity para UWP. En estos casos, use **Windows. Security.** * API, que cubren gran parte de la misma funcionalidad.

En el siguiente ejemplo se obtienen simplemente los certificados de un almacén de certificados con el nombre dado:

```cs
private async void GetCertificatesAsync(string certStoreName)
    {
#if NETFX_CORE
        IReadOnlyList<Certificate> certs = await CertificateStores.FindAllAsync();
        IEnumerable<Certificate> myCerts = 
            certs.Where((certificate) => certificate.StoreName == certStoreName);
#else
        X509Store store = new X509Store(certStoreName, StoreLocation.CurrentUser);
        store.Open(OpenFlags.OpenExistingOnly);
        X509Certificate2Collection certs = store.Certificates;
#endif
    }
```

Consulte [seguridad](https://docs.microsoft.com/windows/uwp/security/) para obtener más información sobre el uso de las API de seguridad de WinRT.

### <a name="networking"></a>Redes

Parte de la **red&period;del sistema.** * los espacios de nombres, como [System .net. mail](https://docs.microsoft.com/dotnet/api/system.net.mail?view=netstandard-2.0), tampoco están disponibles al compilar un juego Unity para UWP. Para la mayoría de estas API, use el correspondiente **Windows. networking.** * y **Windows. Web.** * API de WinRT para obtener una funcionalidad similar. Vea [redes y servicios web](https://docs.microsoft.com/windows/uwp/networking/) para obtener más información.

En el caso de **System .net. mail**, use el espacio de nombres [Windows. ApplicationModel. email](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email) . Consulte [envío de correo electrónico](https://docs.microsoft.com/windows/uwp/contacts-and-calendar/sending-email) para obtener más información.

## <a name="see-also"></a>Vea también

* [Plataforma universal de Windows: faltan tipos de .NET en el back-end de scripting de .NET](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html)
* [Información general de .NET para aplicaciones para UWP](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140))
* [Guías de migración de Unity para UWP](https://unity3d.com/partners/microsoft/porting-guides)