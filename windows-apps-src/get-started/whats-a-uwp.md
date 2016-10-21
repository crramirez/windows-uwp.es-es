---
author: GrantMeStrength
ms.assetid: C9787269-B54F-4FFA-A884-D4A3BF28F80D
title: "¿Qué es una aplicación para Plataforma universal de Windows (UWP)?"
description: "Obtén información sobre los distintos tipos de aplicaciones a las que llamamos aplicaciones de universales de Windows: aplicaciones de la Tienda Windows, aplicaciones de la Tienda de Windows Phone y aplicaciones de Windows Runtime."
translationtype: Human Translation
ms.sourcegitcommit: 6be8bb0a78b614e160fb40629601bdab2d7fc71a
ms.openlocfilehash: 3fd9f4d539977c4b531e08efe0719328365c757c

---

# ¿Qué es una aplicación para la Plataforma universal de Windows (UWP)?

Si es la primera vez que usas la plataforma de Windows o si estás acostumbrado a usar .NET, Windows Forms o Silverlight, es posible que te estés preguntando qué *es* una aplicación para UWP. 

Como cita un conocido libro: "No se asuste". Dentro de poco tendrá las cosas claras. 

Una aplicación para Plataforma universal de Windows (UWP) es una experiencia de Windows que está integrada en la Plataforma universal de Windows (UWP) y que se presentó en Windows 8 por primera vez como Windows Runtime. En el núcleo de las aplicaciones para UWP, los usuarios quieren que sus *experiencias* sean móviles en TODOS sus dispositivos, y quieren que el uso de cualquier dispositivo sea más cómodo y productivo para la tarea en cuestión.

Con Windows 10, es más fácil desarrollar aplicaciones para UWP con una sola API establecida, un paquete de la aplicación y una tienda para obtener acceso a todos los dispositivos de Windows 10: equipos, tabletas, teléfonos, Xbox, HoloLens, Surface Hub y mucho más. Es más fácil admitir cierto número de tamaños de pantalla y una variedad de modelos de interacción, ya sea táctil, de mouse y teclado, un dispositivo de juego o un lápiz.

Conclusión: puedes dedicar tu tiempo a trabajar con API y lenguajes de programación familiares en un solo proyecto y conseguir que el mismo código se ejecute en la amplia gama de hardware de Windows que existe hoy en día.

![Dispositivos de Windows](images/1894834-hig-device-primer-01-500.png)

##Por lo tanto, ¿qué es *exactamente* una aplicación para UWP?


¿Qué tiene de especial una aplicación para UWP? Estas son algunas de las características que hacen que las aplicaciones para UWP de Windows 10 sean diferentes.

-   Se abordan familias de dispositivos, no un sistema operativo.

    Una familia de dispositivos identifica las API, las características del sistema y los comportamientos que puedes esperar en todos los dispositivos de la familia. También determina el conjunto de dispositivos en los que se puede instalar la aplicación desde la tienda.

-   Las aplicaciones se empaquetan y distribuyen mediante el formato de empaquetado .AppX.

    Todas las aplicaciones para UWP se entregan como un paquete AppX. Esto proporciona un mecanismo de instalación de confianza y garantiza que las aplicaciones se pueden implementar y actualizar sin problemas.

-   Hay una tienda para todos los dispositivos.

    Después de registrarte como desarrollador de aplicaciones, puedes enviar la aplicación a la tienda y hacer que esté disponible en todas las familias de dispositivos, o solo en las que elijas. Puedes enviar y administrar todas tus aplicaciones para los dispositivos de Windows en un solo lugar.

-   Hay una superficie de API común para las familias de dispositivos.

    Las API principales de la Plataforma universal de Windows (UWP) son las mismas para todas las familias de dispositivos de Windows. Si la aplicación solo usa las API principales, se ejecutará en cualquier dispositivo de Windows 10.

-   Los SDK de extensión hacen que tu aplicación destaque en dispositivos especializados.

    Los SDK de extensión agregan API especializadas para cada familia de dispositivos. Si tu aplicación está diseñada para una familia de dispositivos en particular, puedes hacer que destaque usando estas API. Puedes seguir teniendo un paquete de la aplicación que se ejecute en todos los dispositivos comprobando que la familia de dispositivos de tu aplicación está ejecutándose antes de llamar a una API de extensión.

-   Controles adaptables y entrada

    Los elementos de interfaz de usuario usan *píxeles eficaces* (consulta [Diseño con capacidad de respuesta 101 para aplicaciones para UWP](https://msdn.microsoft.com/library/windows/apps/Dn958435)), de modo que se adapten automáticamente según el número de píxeles de pantalla disponibles en el dispositivo. Y funcionan bien con varios tipos de entrada, como el teclado, el mouse, la funcionalidad táctil, el lápiz y los controladores de Xbox One. Si necesitas personalizar aún más la interfaz de usuario a un tamaño de pantalla o un dispositivo específicos, los nuevos paneles de diseño y las herramientas te ayudarán a adaptar tu interfaz de usuario en la que se ejecute la aplicación.

Para información más detallada sobre UWP, consulta [Guía de aplicaciones de la Plataforma universal de Windows](universal-application-platform-guide.md).

## Usar un lenguaje que ya conoces


Puedes crear aplicaciones para UWP con los lenguajes de programación con los que estés más familiarizado, como C# o Visual Basic con XAML, JavaScript con HTML o C++ con DirectX o lenguaje XAML. Puedes incluso escribir componentes en un lenguaje y usarlos en una aplicación escrita en otro lenguaje.

Las aplicaciones para UWP usan Windows Runtime, una API nativa integrada en el sistema operativo. Esta API está implementada en C++ y es compatible con Visual Basic, C++ y JavaScript de manera que se sienta natural en cada lenguaje.

Microsoft Visual Studio 2015 proporciona una plantilla de aplicación para UWP para cada lenguaje que te permite crear un único proyecto para todos los dispositivos. Cuando hayas terminado, podrás producir un paquete de la aplicación y enviarlo a la Tienda Windows desde Visual Studio para que tu aplicación llegue a los usuarios de cualquier dispositivo de Windows 10.

## Las aplicaciones para UWP cobran vida en Windows


En Windows, tu aplicación puede ofrecerles a los usuarios información relevante y en tiempo real, para garantizar que regresen una y otra vez. En la economía de las aplicaciones modernas, tu aplicación debe ser atractiva para que se convierta en una parte integrante de la vida de los usuarios. Windows te proporciona una gran cantidad de recursos para ayudarte a conseguir que los usuarios vuelvan a usar tu aplicación:

-   Los iconos dinámicos y la pantalla de bloqueo permiten ver de un vistazo información relevante y oportuna según el contexto.
-   Las notificaciones de inserción transmiten alertas importantes y en tiempo real para llamar la atención del usuario cuando sea necesario.

-   El nuevo Centro de actividades te ofrece un espacio para organizar y mostrar las notificaciones y los contenidos que requieren la intervención de los usuarios.

-   La ejecución en segundo plano y los desencadenadores permiten que tu aplicación cobre vida justo cuando el usuario lo necesita.

-   Tu aplicación puede usar la voz y dispositivos Bluetooth de bajo consumo para que los usuarios interactúen con el mundo que los rodea.

Por último, puedes usar datos móviles y la Caja de seguridad de credenciales de Windows para garantizar una experiencia de movilidad coherente en todas las pantallas de Windows cuando los usuarios ejecuten tu aplicación. Los datos de itinerancia te ofrecen una manera sencilla de almacenar las preferencias y la configuración de un usuario en la nube sin necesidad de crear tu propia infraestructura de sincronización. Además, puedes almacenar credenciales de usuario en la Caja de seguridad de credenciales, donde la seguridad y la fiabilidad son la principal prioridad.

##  Rentabiliza tu aplicación a tu manera


En Windows, puedes elegir cómo rentabilizar tu aplicación, ya sea en teléfonos, tabletas, PC u otros dispositivos. Te ofrecemos varias maneras de ganar dinero con tu aplicación y con los servicios ofrecidos en ella. Lo único que necesitas hacer es elegir la que mejor se adapte a ti:

-   Una descarga de pago es la opción más simple. Tú simplemente pones el precio.
-   Las versiones de prueba son perfectas para que el usuario pruebe tu aplicación antes de comprarla, ya que ofrecen capacidades de detección y conversión más sencillas que las opciones "freemium" más tradicionales.
-   Las compras desde la aplicación ofrecen el mayor grado de flexibilidad a la hora de rentabilizar tu aplicación.

## Empecemos


Para información más detallada sobre UWP, lee la [Guía de aplicaciones de la Plataforma universal de Windows](universal-application-platform-guide.md). A continuación, echa un vistazo a [Preparación](get-set-up.md) para descargar las herramientas que necesitas para empezar a crear aplicaciones.

## Temas relacionados


* [Guide to Universal Windows Platform apps (Guía de las aplicaciones para la Plataforma universal de Windows)](universal-application-platform-guide.md)
* [Preparación](get-set-up.md)

## Temas más avanzados

* [.NET Native - What it means for Universal Windows Platform (UWP) developers (.NET Native: qué supone para los desarrolladores de la Plataforma universal de Windows [UWP])](https://blogs.windows.com/buildingapps/2015/08/20/net-native-what-it-means-for-universal-windows-platform-uwp-developers/#TYsD3tJuBJpK3Hc7.97)
* [Universal Windows apps in .NET (Aplicaciones universales de Windows en .NET)](https://blogs.msdn.microsoft.com/dotnet/2015/07/30/universal-windows-apps-in-net)
* [.NET para aplicaciones para UWP](https://msdn.microsoft.com/en-us/library/mt185501.aspx)



<!--HONumber=Sep16_HO3-->


