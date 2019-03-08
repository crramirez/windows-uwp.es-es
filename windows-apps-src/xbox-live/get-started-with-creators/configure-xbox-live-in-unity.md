---
title: Configurar Xbox Live en Unity
description: Obtenga información sobre cómo usar el complemento de Xbox Live Unity para configurar Xbox Live en tu juego Unity.
ms.assetid: 55147c41-cc49-47f3-829b-fa7e1a46b2dd
ms.date: 01/25/2018
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox, Unity, configurar
localizationpriority: medium
ms.openlocfilehash: d464fc54d322db9da91870bd3ca7cbc29957b379
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596730"
---
# <a name="configure-xbox-live-in-unity"></a>Configurar Xbox Live en Unity

> [!NOTE]
> El complemento de Xbox Live Unity solo se recomienda para [programa de creadores de Xbox Live](../developer-program-overview.md) miembros, porque actualmente no hay compatibilidad para varios jugadores o de logros.

Con el [complemento de Xbox Live Unity](https://github.com/Microsoft/xbox-live-unity-plugin), agregar compatibilidad con Xbox Live a un juego de Unity es fácil, lo que le ofrece más tiempo para centrarse en el uso de Xbox Live en formas que mejor se adapten a su título.

En este tema se pasará a través del proceso de configurar el complemento de Xbox Live en Unity.

## <a name="prerequisites"></a>Requisitos previos

Para poder usar Xbox Live en Unity, necesitará lo siguiente:

1. Un  **[cuenta de Xbox Live](https://support.xbox.com/browse/my-account/manage-account/Create%20account)**.
1. Inscripción en el  **[programa para desarrolladores de centro de partners](https://developer.microsoft.com/store/register)**.
2. **[Windows 10 Anniversary Update](https://microsoft.com/windows)**  o posterior
3. **[Unity](https://store.unity.com/)**  versiones **5.5.4p5** (o posterior), **2017.1p5** (o posterior), o **2017.2.0f3** (o posterior) con **[Microsoft Visual Studio Tools para Unity](https://marketplace.visualstudio.com/items?itemName=SebastienLebreton.VisualStudio2015ToolsforUnity)** y **Scripting back-end de .NET de Windows Store**.
4. **[Visual Studio 2015](https://www.visualstudio.com/)**  o **[Visual Studio 2017 15.3.3](https://www.visualstudio.com/)** (o posterior) con el **herramientas de desarrollo de aplicaciones universales de Windows**.
5. **[Xbox Live SDK de extensiones de la plataforma](https://aka.ms/xblextsdk)**.


> [!NOTE]
> Si desea usar el back-end de scripting IL2CPP con Xbox Live, necesita Unity 2017.2.0p2 o versiones más recientes y la versión del complemento de Xbox Live Unity "versión de vista previa de 1802" o superior.


## <a name="import-the-unity-plugin"></a>Importar el complemento de Unity

Para importar el complemento en su proyecto Unity nuevo o existente, siga estos pasos:

1. Vaya a la pestaña de la versión de complemento de Unity de Xbox Live en [ https://github.com/Microsoft/xbox-live-unity-plugin/releases ](https://github.com/Microsoft/xbox-live-unity-plugin/releases).
2. Descargar **XboxLive.unitypackage**.
3. En Unity, haga clic en **activos** > **Importar paquete** > **paquete personalizado** y vaya a **XboxLive.unitypackage**.

![importación correcta](../images/unity/get-started-with-creators/importXBL_Small.gif)

### <a name="optional-configure-the-plugin-to-work-in-the-unity-editor-net-46-or-il2cpp-only"></a>(Opcional) Configurar el complemento para que funcione en el Editor de Unity (.NET 4.6 o IL2CPP solo)

> [!NOTE]
> Soporte técnico para cambiar la versión de tiempo de ejecución de secuencias de comandos en Unity requiere la versión del complemento de Xbox Live Unity "versión 1711" o superior para la versión "versión de vista previa de 1802" y .NET 4.6 o superior para IL2CPP.

Hay tres opciones que se pueden configurar en Unity para definir cómo se compila el código:

1. El **scripting back-end** es el compilador que se usa. Unity es compatible con dos diferentes secuencias de comandos servidores back-end para la plataforma Universal de Windows: .NET y IL2CPP.
2. El **Scripting Runtime Version** es la versión del tiempo de ejecución de secuencias de comandos que se ejecuta el Editor de Unity.
3. El **nivel de compatibilidad de API** es va a crear tu juego en la superficie de API.

En la tabla siguiente se muestra la matriz de compatibilidad de scripting actual para el complemento de Unity de Xbox Live:

| Scripting de back-end     | Versión en tiempo de ejecución de secuencias de comandos | Se admite     | Versión de Unity mínima requerida |
|-------------------    |-------------------        |-----------    |------------------------------- |
| IL2CPP                | .NET 3.5 equivalente       | No            | N/D                            |
| Il2CPP                | .NET 4.6 equivalente       | Sí           | 2017.2.0p2                     |
| .NET                  | .NET 3.5 equivalente       | Sí           | Igual que los requisitos previos          |
| .NET                  | .NET 4.6 equivalente       | Sí           | Igual que los requisitos previos          |

Hemos agregado compatibilidad en tiempo de ejecución de secuencias de comandos adicional a la Xbox Live Unity complemento, comenzando con la versión "versión 1711". De forma predeterminada, el complemento está configurado para ejecutarse en el editor de Unity con el back-end de secuencias de comandos y scripting de la versión del runtime de .NET 3.5 de .NET. Si su proyecto usa la versión en secuencias en tiempo de ejecución de .NET 4.6, deberá configurar el complemento funcione correctamente en el editor:

1. En el Explorador de proyectos de Unity, navegue a **Xbox Live\Libs\UnityEditor\NET46** y seleccione todos los archivos DLL en la carpeta.
2. En la ventana del Inspector, compruebe **Editor** en **plataformas incluyen**.
3. En el Explorador de proyectos de Unity, navegue a **Xbox Live\Libs\UnityEditor\NET35** y seleccione todos los archivos DLL en la carpeta.
4. En la ventana del Inspector, desactive la opción **Editor** en **plataformas incluyen**.

![cambiar el tiempo de ejecución de secuencias de comandos](../images/unity/get-started-with-creators/changeScriptingRuntime.gif)

> [!IMPORTANT]
> Estos pasos deberá revertirse si cambia la versión de tiempo de ejecución de secuencias de comandos en el proyecto a 3.5.

## <a name="set-visual-studio-as-the-ide-in-unity"></a>Conjunto de Visual Studio como el IDE en Unity

Se requiere Visual Studio para compilar un [plataforma Universal de Windows (UWP)](https://docs.microsoft.com/windows/uwp/get-started/whats-a-uwp) juegos. Puede establecer el IDE en Unity a Visual Studio, vaya a **editar** > **preferencias** > **herramientas externas** y establecer el **External Script Editor** a Visual Studio.

![Establezca la herramienta externa de VS](../images/unity/get-started-with-creators/setVSExternalTool_Small.gif)

## <a name="unity-plugin-file-structure"></a>Estructura de archivos de complemento de Unity

Estructura del archivo del complemento de Unity se divide en las siguientes partes:

* __Xbox Live__ contiene los activos de complemento real que se incluyen en el publicado **.unitypackage**.
    * __Editor__ contiene secuencias de comandos que proporcionan la UI de configuración básica de Unity y procesa los proyectos durante la compilación.
    * __Ejemplos__ contiene un conjunto de archivos de escena simple que muestran cómo utilizar los diversos prefabricados y conectarlos todos entre sí.
    * __Imágenes__ es un conjunto pequeño de imágenes que usan el prefabricados.
    * __Bibliotecas__ es donde se almacenan las bibliotecas de Xbox Live.
    * __Prefabricados__ contiene diversos [prefabricado Unity](https://docs.unity3d.com/Manual/Prefabs.html) objetos que implementan la funcionalidad de Xbox Live.
    * __Las secuencias de comandos__ contiene todos los archivos de código que llame a las API de Xbox Live desde el prefabricados. Esto es un excelente lugar para buscar para ver ejemplos sobre cómo llamar correctamente a las API de Xbox Live.
    * __Tools\AssociationWizard__ contiene el Xbox Live asociación asistente, usado para desplegar la configuración de la aplicación de [centro de partners](https://developer.microsoft.com/windows) para su uso dentro de Unity.

## <a name="enable-xbox-live"></a>Habilitar Xbox Live

En el título interactuar con Xbox Live, deberá configurar la configuración inicial de Xbox Live. Puede hacerlo fácilmente y desde Unity mediante el Asistente para asociación de Xbox Live:

1. En el **Xbox Live** menú, seleccione **configuración**.
2. En el **Xbox Live** ventana, seleccione **ejecutar Xbox Live Asistente para la asociación**.
3. En el **asociar el título con el Store Windows** cuadro de diálogo, haga clic en **siguiente**y, a continuación, inicie sesión con su cuenta de centro de partners.
4. Seleccione la aplicación que desea asociar a este proyecto y, a continuación, haga clic en **seleccione**. Si no ve existe, intente hacer clic en **actualizar**. Como alternativa, puede crear una nueva aplicación reservar un nombre y haciendo clic en **reserva**.
5. Se le pedirá habilitar Xbox Live si aún no lo hizo. Haga clic en **habilitar** habilitar Xbox Live en su título.
6. Haga clic en **finalizar** para guardar la configuración.

Para llamar a servicios de Xbox Live, el escritorio debe estar en modo de programador y establecer en el mismo espacio aislado como su título aparece en la configuración de Xbox Live. Puede comprobar ambos examinando el **Xbox Live configuración** ventana en Unity:

1. **Configuración del modo de desarrollador** debe indicar **habilitado**. Si dice **deshabilitado**, haga clic en **cambiar al modo de programador**.
2. **Configuración del título** > **Sandbox** debe tener el mismo identificador que **configuración del modo de desarrollador** > **modo de programador**.

![XBL habilitado](../images/unity/unity-xbl-enabled.png)

Consulte [espacios aislados de Xbox Live](../xbox-live-sandboxes.md) para obtener información acerca de los espacios aislados.

## <a name="build-and-test-the-project"></a>Compilar y probar el proyecto

Cuando se ejecuta el título en el editor, verá datos falsos cuando intenta usar la funcionalidad de Xbox Live. Por ejemplo, si se [agregar inicio de sesión en las capacidades de](unity-prefabs-and-sign-in.md) a la escena y vuelva a iniciar sesión, verá **usuario imitar** aparecen como el nombre de perfil con un icono de marcador de posición. Para iniciar sesión con un perfil real y pruebe la funcionalidad de Xbox Live en su título, deberá crear una solución UWP y ejecútelo en Visual Studio.  Puede compilar el proyecto UWP en Unity siguiendo estos pasos:

1. Abra el **configuración de compilación** ventana seleccionando **archivo** > **configuración de compilación**.
2. Agregue todas las escenas que se van a incluir en la compilación en el **escenas en compilación** sección.
3. Cambie a la **Universal Windows Platform** seleccionando **plataforma Universal de Windows** en **plataforma** y haga clic en **Cambiar plataforma**.
4. Establecer **SDK** a **10.0.15063.0** o superior.
5. Para habilitar la comprobación de depuración de script **Unity C# proyectos**.
6. Haga clic en **compilar** y especifique la ubicación del proyecto.

![configuración de compilación](../images/unity/build_settings.JPG)

Una vez finalizada la compilación, Unity habrá genera un nuevo archivo de solución UWP que necesitará para ejecutar en Visual Studio:

1. En la carpeta que especificó, abra  **&lt;ProjectName&gt;.sln** en Visual Studio.
2. En la barra de herramientas en la parte superior, seleccione **x64** e implementar en el **máquina Local**.

Si habilitó **depuración de scripts** cuando compile la solución para UWP desde Unity, los scripts se encontrarán en el **ensamblado-CSharp (Windows Universal)** proyecto.

![Usuario ficticio: 123456789](../images/unity/get-started-with-creators/visualStudio.PNG)

> [!NOTE]
> Antes de usar la compilación de Visual Studio para probar el juego con datos reales, siga [esta lista de comprobación](test-visual-studio-build.md) para ayudar a garantizar su título podrán tener acceso al servicio de Xbox Live.

> [!IMPORTANT]
> Como de puede 2018 ahora es necesario realizar una actualización en el archivo package.appxmanifest.xml con el fin de probar el título de la UWP correctamente en Visual Studio. Para ello:
>
> 1. Buscar en el Explorador de soluciones para el archivo package.appxmanifest.xml
> 2. A la derecha, haga clic en el archivo y elija Ver código.  
    Si no está disponible la opción de ver código o el archivo package.appxmanifest no tiene una extensión. Deberá abrir el archivo como un documento xml y continúe con los pasos restantes.
> 3. En el `<Properties></Properties>` sección, agregue la siguiente línea: `<uap:SupportedUsers>multiple</uap:SupportedUsers>`.
> 4. Implementar el juego en Xbox iniciando una compilación de depuración remota desde Visual Studio. Puede encontrar instrucciones para configurar el título de una consola Xbox en el [configurar su UWP en el entorno de desarrollo de Xbox](../../xbox-apps/development-environment-setup.md) artículo.
>
> El fragmento de configuración ha cambiado puede parecer que se está habilitando a multijugador pero sigue siendo necesario ejecutar el juego en escenarios de un solo jugador.

## <a name="try-out-the-examples"></a>Probar los ejemplos

Ya está listo para empezar a usar Xbox Live en el proyecto de Unity. Pruebe a abrir en segundo plano en el **Xbox Live/ejemplos** carpeta para ver el complemento en acción y para obtener ejemplos de cómo usar la funcionalidad usted mismo. Ejecutar los ejemplos en el editor le proporcionará datos falsos, pero si compila el proyecto en Visual Studio y [asociar su cuenta de Xbox Live con el espacio aislado](authorize-xbox-live-accounts.md), puede iniciar sesión con tu nombre de jugador.

Pruebe el **SignInAndProfile** escena para iniciar sesión en su Account de Microsoft, el **marcador** escena para crear un marcador y el **UpdateStat** escena para Mostrar y actualizar las estadísticas.

## <a name="see-also"></a>Consulte también

* [Inicie sesión en Xbox Live en Unity](unity-prefabs-and-sign-in.md)
* [Autorizar a las cuentas de Xbox Live](authorize-xbox-live-accounts.md)
