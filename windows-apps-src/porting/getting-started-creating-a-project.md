---
ms.assetid: 08C8F359-E8B6-4A45-8F4B-8A1962F0CE38
description: Microsoft Visual Studio es a Windows lo que Xcode a iOS y Mac OS. En este tutorial, te ayudamos a que te sientas cómodo usando Visual Studio.
title: Creación de un proyecto en Visual Studio
---

# Introducción: creación de un proyecto

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## Creación de un proyecto

Microsoft Visual Studio es a Windows lo que Xcode a iOS y Mac OS. En este tutorial, te ayudamos a que te sientas cómodo usando Visual Studio. Muestra los fundamentos que necesitarás para comenzar. Cada vez que crees una aplicación, seguirás unos pasos similares a estos.

En el siguiente vídeo se comparan Xcode y Visual Studio.

<iframe src="https://hubs-video.ssl.catalog.video.msn.com/embed/5b7bd91f-6a2f-40b6-9b19-eb2994931d0a/IA?csid=ux-en-us&MsnPlayerLeadsWith=html&PlaybackMode=Inline&MsnPlayerDisplayShareBar=false&MsnPlayerDisplayInfoButton=false&iframe=true&QualityOverride=HD" width="720" height="405" allowFullScreen="true" frameBorder="0" scrolling="no">Un minuto para el desarrollador: comparar Xcode con Visual Studio</iframe>

La creación de una aplicación para Windows 10 (denominada más formalmente como aplicación para la plataforma universal de Windows [UWP]) es como crear una aplicación de iOS con guión gráfico. Las aplicaciones de Windows 10 se suelen construir en varias páginas y cada una de ellas contiene una parte distinta de la interfaz de usuario, como un sitio web. Cada página tiene dos archivos de origen asociados: uno para almacenar la interfaz de usuario definida visualmente o mediante programación, almacenada en formato de [Introducción a XAML](https://msdn.microsoft.com/library/windows/apps/mt185595), y otro que contiene el código fuente. A medida que el usuario interactúe con la aplicación, irá pasando por estas páginas. En este tutorial vas a crear una aplicación con dos páginas.

**Nota:** Una característica importante de las aplicaciones de Windows 10 es el hecho de que el mismo código fuente y el mismo conjunto de API están disponibles independientemente de la plataforma. Como sabes, cuando escribes una aplicación de iOS universal para iPhone y iPad, puedes determinar en el tiempo de ejecución la plataforma en que se ejecuta la aplicación y realizar la acción apropiada. De manera similar, las aplicaciones de Windows 10 saben reconocer en tiempo de ejecución el dispositivo en que se ejecutan. Con una aplicación para UWP, no es necesario usar elementos \#ifdef en el código fuente para crear compilaciones de teléfono frente a compilaciones de escritorio. Para mayor comodidad, las aplicaciones de Windows 10 también usan sus controles de interfaz de usuario de forma inteligente en función del dispositivo: por ejemplo, la aplicación puede hacer referencia a un control de selector de fecha, y el control adoptará automáticamente el aspecto y funcionamiento dependiendo de si la pantalla es de escritorio o de teléfono. Sin embargo, el código fuente no se modifica.

Veamos cómo crear una aplicación de Windows 10. En primer lugar, ejecuta Visual Studio. La primera vez que lo hagas, Visual Studio te pedirá que obtengas una licencia de desarrollador. Una licencia de desarrollador te permite instalar y probar aplicaciones de la Tienda Windows en el equipo local antes de enviarlas a la Tienda Windows. Para obtener una licencia, sigue las instrucciones en pantalla para iniciar sesión con una cuenta Microsoft. Si no tienes una, haz clic en el vínculo **Regístrese** del cuadro de diálogo **Licencia de desarrollador** y sigue las instrucciones en pantalla.

Si lo comparamos con Xcode, lo primero que ves cuando inicias Xcode es la pantalla de **bienvenida a Xcode**, parecida a la ilustración siguiente.

![pantalla de bienvenida de Xcode](images/ios-to-uwp/ios-to-uwp-xcode-welcome.png)

Visual Studio es muy similar. Verás la **página de inicio** que se muestra en la figura siguiente.

![pantalla inicio de visual studio](images/ios-to-uwp/ios-to-uwp-vs-welcome.png)

Para crear una aplicación nueva, empieza por crear un proyecto siguiendo uno de estos pasos:

-   En el área **Inicio**, pulsa **Nuevo proyecto**.
-   Pulsa el menú **Archivo** y, a continuación, pulsa **Nuevo proyecto**.

Como comparativa, cuando crees un proyecto en Xcode, verás una lista de plantillas de proyecto como las que se muestran en la ilustración siguiente.

![cuadro de diálogo de nuevo proyecto de xcode](images/ios-to-uwp/ios-to-uwp-xcode-choose-template.png)

En Visual Studio, también puedes elegir entre distintas plantillas de proyecto, como se muestra en la ilustración siguiente.

![cuadro de diálogo nuevo proyecto de visual studio](images/ios-to-uwp/ios-to-uwp-vs-choose-template.png)
Para iniciar este tutorial, pulsa **Visual C#** y después **Windows**, **Windows Universal** y **Aplicación vacía (Windows Universal)**. En el cuadro **Nombre**, escribe "MyApp" y después pulsa **Aceptar**. Visual Studio crea el primer proyecto y después lo muestra. Ya puedes empezar a diseñar la aplicación y a agregarle código.

## Paso siguiente

[Introducción: elección de un lenguaje de programación](getting-started-choosing-a-programming-language.md)


<!--HONumber=Mar16_HO1-->


