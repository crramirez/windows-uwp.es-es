---
description: Si tiene una aplicación universal 8,1&\# 8212; tanto si tiene como destino Windows 8.1, Windows Phone 8,1 o ambos&\# 8212; a continuación, observará que el código fuente y sus habilidades se portan sin problemas a Windows 10.
title: Portar de Windows Runtime 8.x a UWP
ms.assetid: ac163b57-dee0-43fa-bab9-8c37fbee3913
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5509de1e8b4719055a49142fa92bf1704f725aec
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89171179"
---
# <a name="move-from-windows-runtime-8x-to-uwp"></a>Mover de Windows Runtime 8.x a UWP


Si tienes una aplicación Universal 8.1 (tanto si tiene como destino Windows 8.1, Windows Phone 8.1 o ambos), comprobarás que el código fuente y las habilidades se portarán sin dificultad a Windows 10. Con Windows 10, puedes crear una aplicación para la Plataforma universal de Windows (UWP), que es un único paquete de la aplicación que los clientes pueden instalar en todos los tipos de dispositivos. Para obtener más información sobre Windows 10, las aplicaciones para UWP y los conceptos de código adaptable e interfaz de usuario adaptable que mencionaremos en esta guía de migración, consulta [Guía de aplicaciones para UWP](../get-started/universal-application-platform-guide.md).

Durante la migración, verás que Windows 10 comparte la mayoría de las API con las plataformas anteriores, así como el marcado XAML, el marco de trabajo de la interfaz de usuario y las herramientas, por lo que te resultará muy familiar. Al igual que antes, aún puedes elegir entre C++, C# y Visual Basic para el lenguaje de programación a fin de usarlo junto con el marco de trabajo de la interfaz de usuario de XAML. Los primeros pasos de la planificación de qué hacer exactamente con la aplicación o las aplicaciones actuales dependerá de los tipos de aplicaciones y proyectos que tengas. Esto se explica en las siguientes secciones.

## <a name="if-you-have-a-universal-81-app"></a>Si ya tienes una aplicación Universal 8.1

Una aplicación Universal 8.1 se compila a partir de un proyecto de aplicación Universal 8.1. Supongamos que el nombre del proyecto es AppName \_ 81. Contiene estos subproyectos.

-   AppName \_ 81. Windows. Este es el proyecto que crea el paquete de la aplicación para Windows 8.1.
-   AppName \_ 81. windowsphone. Este es el proyecto que crea el paquete de la aplicación para Windows Phone 8.1.
-   AppName \_ 81. Shared. Este es el proyecto que contiene código fuente, archivos de marcado y otros activos y recursos, que usan los otros dos proyectos.

A menudo, una aplicación Universal 8.1 de Windows ofrece las mismas funciones (y lo hace usando el mismo código y marcado) en las versiones de Windows 8.1 y Windows Phone 8.1. Una aplicación como esta es ideal para la migración de una única aplicación de Windows 10 destinada a la familia de dispositivos universales (y que puedes instalar en la gama más amplia de dispositivos). Básicamente, migrarás el contenido del proyecto compartido y tendrás que usar poco o nada de los otros dos proyectos porque están vacíos o no contienen gran cosa.

Otras veces, la forma Windows 8.1 o Windows Phone 8.1 de la aplicación contiene funciones únicas. O bien contienen las mismas características pero las implementan con diferentes técnicas o distinta tecnología. Con una aplicación de este tipo, puedes optar por migrarla a una sola aplicación que tenga como objetivo la familia de dispositivos universales (en cuyo caso, querrás que la aplicación se adapte a diferentes dispositivos) o puedes optar por migrarla como más de una aplicación, quizás una que tenga como destino la familia de dispositivos de escritorio y otra que tenga como destino la familia de dispositivos móviles. La naturaleza de la aplicación Universal 8.1 determinará cuál de estas opciones es mejor en tu caso.

1.  Migra el contenido del proyecto compartido a una aplicación que tenga como destino la familia de dispositivos universales. Si corresponde, recupera cualquier otro contenido de los proyectos de Windows y Windows Phone y úsalo incondicionalmente en la aplicación o condicionalmente en el dispositivo en el que la aplicación se está ejecutando en ese momento (este último comportamiento se conoce como *adaptable*).
2.  Migra el contenido del proyecto de Windows Phone a una aplicación que tenga como destino la familia de dispositivos universales. Si corresponde, recupera cualquier otro contenido del proyecto de Windows y úsalo de manera incondicional o adaptable.
3.  Migra el contenido del proyecto de Windows a una aplicación que tenga como destino la familia de dispositivos universales. Si corresponde, recupera cualquier otro contenido del proyecto de Windows Phone y úsalo de manera incondicional o adaptable.
4.  Migra el contenido del proyecto de Windows a una aplicación que tenga como destino la familia de dispositivos universales o de escritorio y migra también el contenido del proyecto de Windows Phone a una aplicación que tenga como destino la familia de dispositivos universales o móviles. Puedes crear una solución con un proyecto compartido y seguir compartiendo código fuente, archivos de marcado y otros activos, así como recursos entre los dos proyectos. O bien, puedes crear soluciones diferentes y seguir compartiendo los mismos elementos mediante vínculos.

## <a name="if-you-have-a-windows-81-app"></a>Si ya tienes una aplicación de Windows 8.1

Migra el proyecto a una aplicación que tenga como destino la familia de dispositivos universales o de escritorio. Si eliges la familia de dispositivos universales y la aplicación llama a las API implementadas solamente en la familia de dispositivos de escritorio, puedes proteger esas llamadas con código adaptable.

## <a name="if-you-have-a-windows-phone-81-app"></a>Si tienes una aplicación de Windows Phone 8.1

Migra el proyecto a una aplicación que tenga como destino la familia de dispositivos universales o móviles. Si eliges la familia de dispositivos universales y la aplicación llama a las API implementadas solamente en la familia de dispositivos móviles, puedes proteger esas llamadas con código adaptable.

## <a name="adapting-your-app-to-multiple-form-factors"></a>Adaptar la aplicación a distintos factores de forma

La opción que elijas de las secciones anteriores determinará la gama de dispositivos en que se ejecutarán tus aplicaciones, la cual puede ser muy amplia. Incluso limitando la aplicación a la familia de dispositivos móviles, dispones de una amplia variedad de tamaños de pantalla compatibles. Por lo tanto, si la aplicación se va a ejecutar en factores de forma que antes no se admitían, prueba la interfaz de usuario en esos factores de forma y realiza todos los cambios necesarios para que la interfaz de usuario se adapte correctamente a cada uno. Piensa que se trata de una tarea posterior a la migración o un objetivo de ampliación de migración. Existen algunos ejemplos de esta en práctica en los casos prácticos [Bookstore2](w8x-to-uwp-case-study-bookstore2.md) y [QuizGame](w8x-to-uwp-case-study-quizgame.md).

## <a name="approaching-porting-layer-by-layer"></a>Enfoque de la migración capa a capa

Al migrar una aplicación Universal 8.1 al modelo de las aplicaciones para UWP, prácticamente todos tus conocimientos y experiencia se transferirán, así como la mayoría del código fuente, el marcado y los patrones de software que usas.

-   **Ver**. La vista (junto con el modelo de vista) conforma la interfaz de usuario de la aplicación. Lo ideal es que la vista conste de marcado enlazado a propiedades observables de un modelo de vista. Otro patrón (común y conveniente, pero solo a corto plazo) es para el código imperativo en un archivo de código subyacente para manipular directamente elementos de la interfaz de usuario. En cualquier caso, el marcado y diseño de la interfaz de usuario (incluso el código imperativo que manipula los elementos de la interfaz de usuario) se podrán portar fácilmente.
-   .**Modelos de vistas y modelos de datos** Aunque no adoptes formalmente patrones de separación de cuestiones (por ejemplo, MVVM), inevitablemente hay código presente en la aplicación que realiza la función de modelo de vista y modelo de datos. El código del modelo de vista usa tipos en los espacios de nombres del marco de trabajo de la interfaz de usuario. Tanto el modelo de vista como el modelo de datos usan además un sistema operativo no visual y las API de .NET Framework (incluidas las API para el acceso a datos). Y estas API también están [disponibles para aplicaciones para UWP, también](/previous-versions/windows/br211369(v=win.10)), de modo que la mayoría del código, si no todo, se migrará sin cambios.
-   **Servicios en la nube**. Es probable que parte de la aplicación (quizás una gran parte) se ejecute en la nube en forma de servicios. La parte de la aplicación que se ejecuta en el dispositivo cliente se conecta a ellos. Esta es la parte de una aplicación distribuida que probablemente no se modificará al migrar la parte del cliente. Si aún no tienes uno, una buena opción de servicios en la nube para tu aplicación para UWP es [Servicios móviles de Microsoft Azure](https://azure.microsoft.com/services/mobile-services/), que proporciona componentes back-end eficaces a los que puede llamar tu aplicación para servicios que van desde sencillas notificaciones de actualizaciones de iconos dinámicos hasta la clase de escalabilidad de tareas difíciles que puede proporcionar una granja de servidores.

Antes o durante la migración, considera la posibilidad de si la aplicación podría mejorarse mediante la refactorización de modo que el código con un propósito similar se agrupe en capas y no se disperse arbitrariamente. La factorización de la aplicación en capas, como las descritas anteriormente, facilita corregir la aplicación, probarla y posteriormente leerla y mantenerla. Puedes hacer que la funcionalidad sea más reutilizable siguiendo el patrón Model-View-ViewModel ([MVVM](/archive/msdn-magazine/2009/february/patterns-wpf-apps-with-the-model-view-viewmodel-design-pattern)). Este patrón mantiene separadas entre sí las partes de la aplicación correspondientes a los datos, al negocio y a la interfaz de usuario. Incluso dentro de la interfaz de usuario, puede mantener separados el estado y el comportamiento, además de poderse probar por separado, desde los elementos visuales. Con MVVM, puedes escribir una vez la lógica de datos y de negocio, y usarla en todos los dispositivos, independientemente de la interfaz de usuario. Es probable que también puedas volver a usar la mayor parte del modelo de vista y los elementos de vista entre dispositivos.

| Tema | Descripción |
|-------|-------------|
| [Migración del proyecto](w8x-to-uwp-porting-to-a-uwp-project.md) | Tienes dos opciones cuando empieza el proceso de migración. Una es editar una copia de los archivos de proyecto existentes, entre los que se incluye el manifiesto del paquete de la aplicación (para esta opción, consulta la información sobre cómo actualizar los archivos de proyecto en [Migrar aplicaciones a la Plataforma universal de Windows (UWP)](/visualstudio/misc/migrate-apps-to-the-universal-windows-platform-uwp?view=vs-2015)). La otra opción es crear un nuevo proyecto de Windows 10 en Visual Studio y copiar los archivos en él. |
| [Solución de problemas](w8x-to-uwp-troubleshooting.md) | Se recomienda leer hasta el final esta guía de migración, aunque somos conscientes de que estás deseando seguir avanzando y llegar a la fase de compilación y ejecución de tu proyecto. Con esto en mente, puedes avanzar temporalmente si comentas o quitas cualquier código que no sea esencial y más tarde regresas allí para restaurar lo que has quitado. La tabla de solución de problemas de síntomas y soluciones de este tema puede resultarte útil en esta etapa, aunque no sustituye la lectura de los siguientes temas. Siempre puedes consultar la tabla a medida que avances a través de los temas posteriores. |
| [Migración de XAML y la interfaz de usuario](w8x-to-uwp-porting-xaml-and-ui.md) | La práctica de definir la interfaz de usuario en forma de marcado XAML declarativo se traduce muy bien desde las aplicaciones Universal 8.1 a las aplicaciones de UWP. Verás que la mayor parte del código de marcado es compatible, aunque es posible que necesites realizar algunos ajustes en las claves de recurso del sistema o plantillas personalizadas que estás usando. |
| [Migración de modelo de E/S, dispositivos y aplicaciones](w8x-to-uwp-input-and-sensors.md) | El código que se integra con el dispositivo y sus sensores implica la entrada del usuario y la salida de este. También puede implicar el procesamiento de datos. No obstante, este código no se considera generalmente como la capa de interfaz de usuario o la capa de datos. Este código incluye la integración con el controlador de vibración, el acelerómetro, el giroscopio, el micrófono y los altavoces (que se relacionan con el reconocimiento y la síntesis de voz), la localización (geográfica) y las modalidades de entrada, como, por ejemplo, entrada táctil, mouse, teclado y lápiz. |
| [Caso práctico: Bookstore1](w8x-to-uwp-case-study-bookstore1.md) | En este tema se presenta un caso práctico de migración de una aplicación Universal 8.1 a una aplicación para UWP de Windows 10. Una aplicación Universal 8.1 es aquella que crea un paquete de la aplicación para Windows 8.1 y otro paquete de la aplicación para Windows Phone 8.1. Con Windows 10, puedes crear un paquete de la aplicación único que los clientes pueden instalar en una amplia variedad de dispositivos, como se verá en este caso práctico. Consulta [Guía de aplicaciones para UWP](../get-started/universal-application-platform-guide.md). |
| [Caso práctico: Bookstore2](w8x-to-uwp-case-study-bookstore2.md) | Este caso práctico, que se basa en la información proporcionada en el control [SemanticZoom](/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom). En el modelo de vista, cada instancia de la clase Author representa el grupo de los libros que ha escrito ese autor y en SemanticZoom podemos ver la lista de libros agrupados por autor, o bien podemos alejar la vista para ver una lista de accesos directos a autores. |
| [Caso práctico: QuizGame](w8x-to-uwp-case-study-quizgame.md) | En este tema se presenta un caso práctico de migración de una aplicación de muestra de WinRT 8.1 de un juego de preguntas de punto a punto funcional a una aplicación para UWP de Windows 10. |

## <a name="related-topics"></a>Temas relacionados

**Documentación**
* [Referencia de Windows Runtime](/uwp/api/)
* [Compilar aplicaciones universales de Windows para todos los dispositivos Windows](../get-started/universal-application-platform-guide.md)
* [Diseño de la experiencia de usuario para aplicaciones](/previous-versions/windows/hh767284(v=win.10))