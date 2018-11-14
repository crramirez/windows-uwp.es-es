---
author: eliotcowley
title: API de .NET que faltan en Unity y UWP
description: Obtén información sobre las API de .NET que faltan al compilar juegos de UWP en Unity y las soluciones de problemas comunes.
ms.assetid: 28A8B061-5AE8-4CDA-B4AB-2EF0151E57C1
ms.author: elcowle
ms.date: 2/21/2018
ms.topic: article
keywords: windows 10, uwp, juegos, .net, unity
ms.localizationpriority: medium
ms.openlocfilehash: 4b795ed47249eee1f9dc21b195d46f450997019e
ms.sourcegitcommit: bdc40b08cbcd46fc379feeda3c63204290e055af
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "6159632"
---
# <a name="missing-net-apis-in-unity-and-uwp"></a>API de .NET que faltan en Unity y UWP

Al crear un juego para UWP con. NET, es posible que algunas API que podrías usar, en el editor de Unity o para un juego de PC independiente, no estén presentes para UWP. Eso es porque .NET para aplicaciones para UWP incluye un subconjunto de los tipos proporcionados en el conjunto completo de .NET Framework para cada espacio de nombres.

Además, algunos motores de juego usan distintos tipos de .NET que no son totalmente compatibles con .NET para UWP, como Mono de Unity. Por lo tanto, cuando estás escribiendo tu juego, todo podría funcionar bien en el editor, pero cuando vayas a compilar para UWP, es posible que obtengas errores como este: **El tipo o el espacio de nombres 'Formateadores' no existen en el espacio de nombres 'System.Runtime.Serialization' (¿tal vez te falta una referencia de ensamblado?)**

Afortunadamente, Unity proporciona algunas de estas API que faltan como métodos de extensión y tipos de sustitución, que se describen en [Plataforma Universal de Windows: tipos de .NET que faltan en el back-end de scripting de . NET](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html). Sin embargo, si la funcionalidad que necesitas no está aquí, [Introducción a .NET para aplicaciones de Windows 8.x](https://msdn.microsoft.com/library/windows/apps/br230302) describe formas de convertir el código para usar WinRT o .NET para API de UWP. (Trata de Windows 8, pero también es aplicable a las aplicaciones para UWP de Windows 10).

## <a name="net-standard"></a>.NET estándar

Para comprender por qué algunas API podrían no funcionar, es importante comprender los distintos tipos de .NET y cómo UWP implementa .NET. La [.NET estándar](https://docs.microsoft.com/dotnet/standard/net-standard) es una especificación formal de las API de .NET que está diseñada para ser multiplataforma y unificar los diferentes tipos .NET. Cada implementación de .NET admite una versión determinada de .NET estándar. Puedes ver una tabla de los estándares y las implementaciones en [Soporte técnico de implementación de .NET](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support).

Cada versión del SDK de UWP se ajusta a un nivel diferente de .NET estándar. Por ejemplo, el SDK 16299 (la Fall Creators Update) admite .NET 2.0 estándar.

Si quieres saber si una API de .NET determinada se admite en la versión UWP que te interesa, puedes comprobar la [Referencia de API de .NET estándar](https://docs.microsoft.com/dotnet/api/index?view=netstandard-2.0) y seleccionar la versión de .NET estándar que es compatible con esa versión de UWP.

## <a name="scripting-backend-configuration"></a>Configuración de back-end de scripting

Lo primero que debes hacer si tienes problemas al compilar para UWP es comprobar la **Configuración del reproductor** (**Archivo > Configuración de compilación**, selecciona **Plataforma Universal de Windows** y después **Configuración del reproductor**). En **Otras opciones de configuración > Configuración**, los tres primeros puntos de la lista desplegable (**Versión de tiempo de ejecución de scripting**, **Back-end de scripting** y **Nivel de compatibilidad de Api**) son opciones de configuración importantes a considerar.

La **Versión de tiempo de ejecución de scripting** es lo que usa el back-end de scripting de Unity que te permite obtener la versión equivalente (aproximada) de soporte técnico de .NET Framework que elijas. Sin embargo, ten en cuenta que no todas las API de dicha versión de .NET Framework serán compatibles, solo aquellas de la versión de .NET estándar a la que está orientada la UWP.

A menudo, con las nuevas versiones. NET, se agregan más API a .NET estándar, que te podrían permitir usar el mismo código en modos independiente y UWP. Por ejemplo, el espacio de nombres [System.Runtime.Serialization.Json](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.json) se introdujo en .NET 2.0 estándar. Si estableces la **Versión de tiempo de ejecución de scripting** en **Equivalentes de .NET 3.5** (que se orienta a una versión anterior de . NET estándar), obtendrás un error al intentar usar la API; cambia a **Equivalente a .NET 4.6** (que es compatible con .NET 2.0 estándar) y la API funcionará.

El **back-end de scripting** puede ser **.NET** o **IL2CPP**. En este tema, supondremos que has elegido **.NET**, ya que es donde se producen los problemas que se tratan aquí. Para más información, consulta [Back-end de scripting](https://docs.unity3d.com/Manual/windowsstore-scriptingbackends.html).

Por último, debes establecer el **Nivel de compatibilidad de api** a la versión de .NET en la que quieras que tu juego pueda ejecutarse. Este debe coincidir con la **Versión de tiempo de ejecución de scripting**.

En general, para **Versión de tiempo de ejecución de scripting** y **Nivel de compatibilidad de api**, debes seleccionar la versión más reciente disponible con el fin de tener mayor compatibilidad con .NET Framework y, por lo tanto, tener la posibilidad de usar más API de .NET.

![Configuración: Versión de tiempo de ejecución de scripting; Back-end de scripting; Nivel de compatibilidad de api](images/missing-dot-net-apis-in-unity-1.png)

## <a name="platform-dependent-compilation"></a>Compilación dependiente de la plataforma

Si vas a crear tu juego de Unity para varias plataformas, incluyendo la UWP, puede que quieras usar una compilación dependiente de la plataforma para asegurarte de que solo se ejecute código destinado a UWP cuando se compile el juego como una UWP. De esta forma, puedes usar el conjunto completo de .NET Framework para escritorios independientes y otras plataformas, y las API WinRT para UWP, sin obtener errores de compilación.

Usa las siguientes directivas para compilar solo el código solo cuando se ejecute como una aplicación para UWP:

```csharp
#if NETFX_CORE
    // Your UWP code here
#else
    // Your standard code here
#endif
```

> [!NOTE]
> `NETFX_CORE` solamente están destinadas a comprobar si estás compilando código C# con el back-end de scripting .NET. Si estás usando un back-end de scripting diferente, como IL2CPP, usa en su lugar `UNITY_WSA_10_0`.

Para obtener una lista completa de directivas de compilación dependientes de la plataforma, consulta [Compilación dependiente de la plataforma](https://docs.unity3d.com/Manual/PlatformDependentCompilation.html).

## <a name="common-issues-and-workarounds"></a>Problemas y soluciones habituales

Los escenarios siguientes describen problemas comunes que pueden surgir donde faltan las API de .NET en el subconjunto UWP y las formas intentar resolverlos.

### <a name="data-serialization-using-binaryformatter"></a>Serialización de datos mediante BinaryFormatter

Es habitual que los juegos serialicen datos de guardado, para que los jugadores puedan manipularlos fácilmente. Sin embargo, [BinaryFormatter](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.formatters.binary.binaryformatter), que serializa un objeto en binario, no está disponible en versiones anteriores de .NET estándar (anteriores a 2.0). Considera el uso de [XmlSerializer](https://docs.microsoft.com/dotnet/api/system.xml.serialization.xmlserializer) o [DataContractJsonSerializer](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.json.datacontractjsonserializer) en su lugar.

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

### <a name="io-operations"></a>Operaciones de E/S

Algunos tipos del espacio de nombres [System.IO](https://docs.microsoft.com/dotnet/api/system.io), como [FileStream](https://docs.microsoft.com/dotnet/api/system.io.filestream), no están disponibles en versiones anteriores de .NET estándar. Sin embargo, Unity proporciona los tipos de [Directorio](https://docs.microsoft.com/dotnet/api/system.io.directory), [Archivo](https://docs.microsoft.com/dotnet/api/system.io.file) y **FileStream** para que puedas usarlos en tu juego.

Como alternativa, puedes usar las API de [Windows.Storage](https://docs.microsoft.com/uwp/api/Windows.Storage), que solo están disponibles para aplicaciones para UWP. Sin embargo, estas API restringen la aplicación a escribir en su almacenamiento específico y no le conceden libre acceso al sistema de archivos completo. Para obtener más información, consulta [Archivos, carpetas y bibliotecas](https://docs.microsoft.com/windows/uwp/files/).

Un aspecto importante es que el método [Cerrar](https://docs.microsoft.com/dotnet/api/system.io.stream.close) solo está disponible en .NET 2.0 estándar y posteriores (aunque Unity proporciona un método de extensión). En su lugar, usa [Disponer](https://docs.microsoft.com/dotnet/api/system.io.stream.dispose).

### <a name="threading"></a>Subprocesos

Algunos tipos de los espacios de nombres [System.Threading](https://docs.microsoft.com/dotnet/api/system.threading), como [ThreadPool](https://docs.microsoft.com/dotnet/api/system.threading.threadpool), no están disponibles en versiones anteriores de .NET estándar. En estos casos, puedes usar en su lugar el espacio de nombres [Windows.System.Threading](https://docs.microsoft.com/uwp/api/windows.system.threading).

Aquí te mostramos cómo puedes controlar los subprocesos en un juego de Unity, usando la compilación dependiente de la plataforma para prepararte para plataformas UWP y no UWP:

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

Algunos de los espacios de nombres **System.Security.***, como [System.Security.Cryptography.X509Certificates](https://docs.microsoft.com/dotnet/api/system.security.cryptography.x509certificates?view=netstandard-2.0), no están disponibles al compilar un juego Unity para UWP. En estos casos, usa la API **Windows.Security.***, que abarca gran parte de la misma funcionalidad.

El siguiente ejemplo simplemente obtiene los certificados de un almacén de certificados con el nombre proporcionado:

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

Para obtener más información acerca de las API de seguridad WinRT, consulta [Seguridad](https://docs.microsoft.com/windows/uwp/security/).

### <a name="networking"></a>Redes

Algunos de los espacios de nombres **System&period;Net.***, como [System.Net.Mail](https://docs.microsoft.com/dotnet/api/system.net.mail?view=netstandard-2.0), tampoco están disponibles al compilar un juego Unity para UWP. Para la mayoría de estas API, usa las correspondientes API **Windows.Networking.*** y **Windows.Web.*** WinRT para obtener una funcionalidad similar. Consulta [Servicios web y redes](https://docs.microsoft.com/windows/uwp/networking/) para obtener más información.

En el caso de **System.Net.Mail**, usa el espacio de nombres [Windows.ApplicationModel.Email](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email). Consulta [Enviar correo electrónico](https://docs.microsoft.com/windows/uwp/contacts-and-calendar/sending-email) para obtener más información.

## <a name="see-also"></a>Ver también

* [Plataforma universal de Windows: Tipos de .NET que faltan en el back-end de scripting de .NET](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html)
* [.NET para introducción de aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/br230302)
* [Guías para portar juegos de Unity UWP](https://unity3d.com/partners/microsoft/porting-guides)