---
title: Guía de desarrollo de juegos para Windows 10
description: Una guía completa sobre los recursos y la información que necesitas para el desarrollo de juegos para la Plataforma universal de Windows (UWP).
ms.assetid: 6061F498-96A8-44EF-9711-68AE5A1218C9
ms.date: 04/16/2018
ms.topic: article
keywords: Windows 10, UWP, juegos, desarrollo de juegos
ms.localizationpriority: medium
ms.openlocfilehash: 24414ba36e2ee1af8f391eec38b04d9e17bb7237
ms.sourcegitcommit: 2e597438dafedde3bde24424ef005bb4c24ba3bf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/16/2020
ms.locfileid: "84800331"
---
# <a name="windows-10-game-development-guide"></a>Guía de desarrollo de juegos para Windows 10

Esta es la guía de desarrollo de juegos para Windows 10.

En esta guía te proporcionamos una colección completa de los recursos y la información que necesitas para desarrollar un juego para la Plataforma universal de Windows (UWP). Una versión en inglés (EE. UU.) de esta guía está disponible en formato [PDF](https://download.microsoft.com/download/9/C/9/9C9D344F-611F-412E-BB01-259E5C76B17F/Windev_Game_Dev_Guide_Oct_2017.pdf) .

## <a name="introduction-to-game-development-for-the-universal-windows-platform-uwp"></a>Introducción al desarrollo de juegos para la Plataforma universal de Windows (UWP)

Cuando crea un juego de Windows 10, tendrá la oportunidad de llegar a millones de jugadores de todo el mundo a través del teléfono, del PC y de Xbox One. Con Xbox en Windows, Xbox Live, las partidas multijugador desde diferentes dispositivos, una comunidad de juegos alucinante y nuevas y potentes características como la Plataforma universal de Windows (UWP) y DirectX 12, los juegos de Windows 10 emocionan a jugadores de todas las edades y géneros. Con la nueva Plataforma universal de Windows (UWP), su juego será compatible en distintos dispositivos Windows 10 con una API común para teléfono, PC y Xbox One, así como herramientas y opciones para adaptar el juego a la experiencia de cada dispositivo.

En esta guía se proporciona una colección completa de información y recursos que te ayudarán a medida que desarrollas tu juego. Las secciones se organizan según las fases de desarrollo de los juegos, por lo que sabrás dónde buscar información cuando la necesites.

Si no está familiarizado con el desarrollo de juegos en Windows o Xbox, la guía de [Introducción](getting-started.md) puede ser donde desea iniciar. La sección de [recursos de desarrollo de juegos](#game-development-resources) también proporciona una encuesta de alto nivel de documentación, programas y otros recursos que resultan útiles al crear un juego. Si desea empezar examinando en su lugar algún código UWP, consulte ejemplos de [juegos](#game-samples).

Esta guía se actualizará a medida que recursos de desarrollo de juegos para Windows 10 adicionales y material se hagan disponibles.

## <a name="game-development-resources"></a>Recursos de desarrollo de juegos

Desde la documentación hasta los programas, foros, blogs y muestras para desarrolladores, hay muchos recursos disponibles que te ayudarán en tu camino de desarrollo de juegos. Este es un resumen de los recursos que deberías conocer antes de comenzar a desarrollar tu juego para Windows 10.

> [!Note]
> Algunas características se administran a través de varios programas. En esta guía se cubre una amplia gama de recursos, por lo que es posible que algunos recursos no sean accesibles según el programa en el que participas o tu rol de desarrollo concreto. Las muestras son vínculos que te dirigen a developer.xboxlive.com, forums.xboxlive.com, xdi.xboxlive.com o el portal Game Developer Network (GDN). Para obtener información sobre la asociación con Microsoft, consulte [programas para desarrolladores](#developer-programs).

### <a name="game-development-documentation"></a>Documentación sobre el desarrollo de juegos

A lo largo de esta guía, encontrarás vínculos profundos a documentación importante, que estarán organizados por tarea, tecnología y fase de desarrollo de los juegos. Para ofrecerte una vista amplia de lo que tienes a tu disposición, estos son los principales portales de documentación para el desarrollo de juegos para Windows 10.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Portal principal del Centro de desarrollo de Windows</td>
        <td><a href="https://developer.microsoft.com/windows">Centro de desarrollo de Windows</a></td>
    </tr>
    <tr>
        <td>Desarrollar aplicaciones de Windows</td>
        <td><a href="https://developer.microsoft.com/windows/apps/develop">Desarrollar aplicaciones de Windows</a></td>
    </tr>
    <tr>
        <td>Desarrollo de aplicaciones de la Plataforma universal de Windows</td>
        <td><a href="https://developer.microsoft.com/windows/apps">Guías de procedimientos para aplicaciones Windows 10</a></td>
    </tr>
    <tr>
        <td>Guías de procedimientos para los juegos para UWP</td>
        <td><a href="index.md">Juegos y DirectX</a> </td>
    </tr>
    <tr>
        <td>Información general y referencias sobre DirectX</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/directx">Gráficos y juegos de DirectX</a></td>
    </tr>
    <tr>
        <td>Azure para juegos</td>
        <td><a href="https://azure.microsoft.com/solutions/gaming/">Compilar y escalar tus juegos con Azure</a></td>
    </tr>
    <tr>
        <td>PlayFab</td>
        <td><a href="https://api.playfab.com/">Solución de back-end completa para juegos en vivo</a></td>
    </tr>
    <tr>
        <td>UWP en Xbox One</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/xbox-apps/index">Compilación de aplicaciones para UWP en Xbox One</a></td>
    </tr>
    <tr>
        <td>UWP en HoloLens</td>
        <td><a href="https://developer.microsoft.com/windows/mixed-reality/development_overview">Compilación de aplicaciones para UWP en HoloLens</a></td>
    </tr>
    <tr>
        <td>Documentación de Xbox Live</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/">Guía del desarrollador de Xbox Live</a></td>
    </tr>
    <tr>
        <td>Documentación de desarrollo de Xbox One (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-home">Desarrollo de Xbox One</a></td>
    </tr>
    <tr>
        <td>Notas del producto de desarrollo de Xbox One (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-whitepapers">Notas del producto</a></td>
    </tr>
    <tr>
        <td>Documentación interactiva de mezclador</td>
        <td><a href="https://dev.mixer.com/reference/interactive/index.html">Agregar interactividad al juego</a></td>
    </tr>        
</table>

### <a name="partner-center"></a>Centro de partners

El [registro de una cuenta de Desarrollador en el centro de Partners](https://developer.microsoft.com/store/register) es el primer paso para publicar su juego de Windows. Una cuenta de desarrollador le permite reservar el nombre del juego y enviar juegos gratuitos o de pago a la Microsoft Store para todos los dispositivos Windows. Usa tu cuenta de desarrollador para administrar tu juego y productos desde el juego, obtener análisis detallados y habilitar servicios que crean fantásticas experiencias para tus jugadores en todo el mundo. 

Microsoft también ofrece varios programas para desarrolladores que le ayudarán a desarrollar y publicar juegos de Windows. Se recomienda ver si hay alguna adecuada antes de registrarse para una cuenta del centro de Partners. Para obtener más información, vaya a [programas para desarrolladores](#developer-programs)

### <a name="developer-programs"></a>Programas para desarrolladores

Microsoft ofrece varios programas para desarrollador que te ayudarán a desarrollar y publicar juegos para Windows. Considere la posibilidad de unirse a un programa para desarrolladores si desea desarrollar juegos para Xbox One e integrar las características de Xbox Live en su juego. Para publicar un juego en el Microsoft Store, también deberá crear una cuenta de Desarrollador en el [centro de Partners](https://partner.microsoft.com/dashboard) .

#### <a name="xbox-live-creators-program"></a>Programa de creadores de Xbox Live

El programa Xbox Live Creators permite a cualquier usuario integrar Xbox Live en su título y publicar en Xbox One y Windows 10. Hay un proceso de certificación simplificado y no se requiere ninguna aprobación de concepto fuera de las [directivas de Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies)estándar.

Puede implementar, diseñar y publicar su juego en el programa de creadores sin un kit de desarrollo dedicado, solo con hardware comercial. Para empezar, descargue la [aplicación de activación de modo de desarrollo](https://docs.microsoft.com/windows/uwp/xbox-apps/devkit-activation) en la Xbox One.

Si desea tener acceso a más funcionalidades de Xbox Live, soporte técnico de marketing y desarrollo dedicado y la posibilidad de que se le presenten en la tienda Xbox One principal, aplique el [ID@Xbox](https://www.xbox.com/Developers/id) programa.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Programa de creadores de Xbox Live</td>
        <td><a href="https://developer.microsoft.com/games/xbox/xboxlive/creator">Más información sobre el Xbox Live Creators Program</a></td>
    </tr>
</table>

#### <a name="idxbox"></a>ID@Xbox

El ID@Xbox programa ayuda a los desarrolladores de juegos cualificados a publicarse automáticamente en Windows y Xbox One. Si quiere desarrollar para Xbox One, o agregar características de Xbox Live, como Gamerscore, logros y marcadores, a su juego de Windows 10, regístrese con ID@Xbox . Conviértase en ID@Xbox desarrollador para obtener las herramientas y el soporte técnico que necesita para dar rienda suelta a su creatividad y maximizar el éxito. Se recomienda que se aplique ID@Xbox primero antes de registrarse para obtener una cuenta de Desarrollador en el centro de Partners.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>ID@XboxPrograma para desarrolladores</td>
        <td><a href="https://www.xbox.com/Developers/id">Programa para desarrolladores independientes para Xbox One</a></td>
    </tr>
    <tr>
        <td>ID@Xboxsitio de consumidor</td>
        <td><a href="https://www.idatxbox.com/">ID@Xbox</a></td>
    </tr>
</table>

#### <a name="xbox-tools-and-middleware"></a>Herramientas y software intermedio para Xbox

El programa Herramientas y software intermedio para Xbox ofrece licencias de kits de desarrollo para Xbox a los desarrolladores profesionales de herramientas y software intermedio de juego. Los desarrolladores que se aceptan en el programa pueden compartir y distribuir sus tecnologías Xbox XDK a otros desarrolladores de Xbox con licencia.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Póngase en contacto con el programa Herramientas y software intermedio</td>
        <td><xboxtlsm@microsoft.com></td>
    </tr>
</table>

### <a name="game-samples"></a>Muestras de juegos

Hay muchas muestras de juegos y aplicaciones para Windows 10 disponibles que te ayudarán a comprender las funciones de juegos de Windows 10 y a empezar a desarrollar juegos rápidamente. Se desarrollan y publican muestras continuamente, por lo que te recomendamos que consultes ocasionalmente los portales de muestras para ver las novedades. También puedes [ver](https://help.github.com/en/articles/watching-and-unwatching-repositories) los repositorios de GitHub para recibir notificaciones de cambios y adiciones.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Muestras de aplicaciones para la Plataforma universal de Windows</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples">Muestras universal Windows</a></td>
    </tr>
    <tr>
        <td>Muestras de elementos gráficos de Direct3D 12</td>
        <td><a href="https://github.com/Microsoft/DirectX-Graphics-Samples">DirectX-Graphics-Samples</a></td>
    </tr>
    <tr>
        <td>Muestras de elementos gráficos de Direct3D 11</td>
        <td><a href="https://github.com/walbourn/directx-sdk-samples">directx-sdk-samples</a></td>
    </tr>
    <tr>
        <td>Muestra de juego en primera persona en Direct3D 11</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Crear un juego para UWP sencillo con DirectX</a></td>
    </tr>
    <tr>
        <td>Muestra de efectos de imagen personalizados en Direct2D</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DCustomEffects">D2DCustomEffects</a></td>
    </tr>
    <tr>
        <td>Muestra de malla de degradado Direct2D</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DGradientMesh">D2DGradientMesh</a></td>
    </tr>
    <tr>
        <td>Muestra de ajuste de fotos en Direct2D</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DPhotoAdjustment">D2DPhotoAdjustment</a></td>
    </tr>
    <tr>
        <td>Muestras públicas de Xbox Advanced Technology Group</td>
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples">Xbox-ATG-Samples</a></td>
    </tr>
    <tr>
        <td>Ejemplos de Xbox Live</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples">Xbox-Live-samples</a></td>
    </tr>
    <tr>
        <td>Ejemplos de juegos de Xbox One (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-samples">Muestras</a></td>
    </tr>
    <tr>
        <td>Ejemplos de juegos de Windows (Galería de código de MSDN)</td>
        <td><a href="https://docs.microsoft.com/samples/browse/?term=games">Ejemplos de Microsoft Store Game</a></td>
    </tr>
    <tr>
        <td>Ejemplo de juego 2D de JavaScript</td>
        <td><a href="../get-started/get-started-tutorial-game-js2d.md">Crear un juego para UWP en JavaScript</a></td>
    </tr>
    <tr>
        <td>Ejemplo de juego 3D de JavaScript</td>
        <td><a href="../get-started/get-started-tutorial-game-js3d.md">Crear un juego en 3D en JavaScript con three.js</a></td>
    </tr>
    <tr>
        <td>Ejemplo de juego Multigame 2D para UWP</td>
        <td><a href="../get-started/get-started-tutorial-game-mg2d.md">Crear un juego para UWP en MonoGame 2D</a></td>
    </tr>      
</table>

### <a name="developer-forums"></a>Foros para desarrolladores

Los foros para desarrolladores son un excelente lugar para preguntar y responder a preguntas sobre el desarrollo de juegos y conectarse a la comunidad de desarrollo de juegos. Además, los foros pueden ser fantásticos recursos para encontrar respuestas existentes a problemas complejos a los que se hayan enfrentado los desarrolladores en el pasado, con sus soluciones correspondientes.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Foros para desarrolladores de aplicaciones y juegos</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en/home?forum=windowsstore%2Cwpsubmit%2Caiaads%2Caiasdk%2Caiamgr">Publicación y anuncios en aplicaciones</a></td>
    </tr>
    <tr>
        <td>Foro para desarrolladores de aplicaciones para UWP</td>
        <td><a href="https://docs.microsoft.com/answers/topics/uwp.html">Desarrollo de aplicaciones de la Plataforma universal de Windows</a></td>
    </tr>
    <tr>
        <td>Foros para desarrolladores de aplicaciones de escritorio</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en/home?forum=windowsgeneraldevelopmentissues">Foros de aplicaciones de escritorio de Windows</a></td>
    </tr>
    <tr>
        <td>DirectX Microsoft Store Games (publicaciones archivadas de foros)</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en/home?forum=wingameswithdirectx">Creación de juegos de Microsoft Store con DirectX (archivado)</a></td>
    </tr>
    <tr>
        <td>Foros para desarrolladores de partner administrados de Windows 10</td>
        <td><a href="https://forums.xboxlive.com/users/login.html">Foros para desarrolladores de Xbox: Windows 10</a></td>
    </tr>
    <tr>
        <td>Foro de Xbox Live</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en/home?forum=xboxlivedev">Foro de desarrollo de Xbox Live</a></td>
    </tr>
    <tr>
        <td>Foros de PlayFab</td>
        <td><a href="https://community.playfab.com/index.html">Foros de PlayFab</a></td>
    </tr>
</table>

### <a name="developer-blogs"></a>Blogs para desarrolladores

Los blogs para desarrolladores son otro excelente recurso para obtener la información más reciente sobre el desarrollo de juegos. Encontrarás publicaciones sobre nuevas funciones, detalles de implementación, procedimientos recomendados, fondo de arquitectura y mucho más.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Blog sobre la compilación de aplicaciones para Windows</td>
        <td><a href="https://blogs.windows.com/buildingapps/">Compilación de aplicaciones para Windows</a></td>
    </tr>
    <tr>
        <td>Windows 10 (entradas de blog)</td>
        <td><a href="https://blogs.windows.com/blog/tag/windows-10/">Publicaciones en Windows 10</a></td>
    </tr>
    <tr>
        <td>Blog del equipo de ingeniería de Visual Studio</td>
        <td><a href="https://devblogs.microsoft.com/visualstudio/">Blog de Visual Studio</a></td>
    </tr>
    <tr>
        <td>Blogs sobre herramientas para desarrolladores de Visual Studio</td>
        <td><a href="https://devblogs.microsoft.com/visualstudio/">Blogs de Herramientas de desarrollo</a></td>
    </tr>
    <tr>
        <td>Blog sobre herramientas para desarrolladores de Somasegar</td>
        <td><a href="https://devblogs.microsoft.com/somasegar/">Blog de Somasegar</a></td>
    </tr>
    <tr>
        <td>Blog para desarrolladores de DirectX</td>
        <td><a href="https://devblogs.microsoft.com/directx/">Blog para desarrolladores de DirectX</a></td>
    </tr>
    <tr>
        <td>Introducción a DirectX 12 (entrada de blog)</td>
        <td><a href="https://devblogs.microsoft.com/directx/directx-12/">DirectX 12</a></td>
    </tr>
    <tr>
        <td>Blog del equipo de herramientas de Visual C++</td>
        <td><a href="https://devblogs.microsoft.com/cppblog/">Blog del equipo de Visual C++</a></td>
    </tr>
    <tr>
        <td>Blog del equipo de PIX</td>
        <td><a href="https://devblogs.microsoft.com/pix/">Optimización del rendimiento y depuración de juegos de DirectX 12 en Windows y Xbox</a></td>
    </tr>
    <tr>
        <td>Blog del equipo de implementación de aplicaciones universales de Windows</td>
        <td><a href="https://blogs.msdn.microsoft.com/appinstaller/">Blog del equipo de compilación e implementación de aplicaciones para UWP</a></td>
    </tr>
</table>

## <a name="concept-and-planning"></a>Concepto y planificación

En la fase de concepto y planificación, decides el aspecto de tu juego y las tecnologías y herramientas que usarás para que cobre vida.

### <a name="overview-of-game-development-technologies"></a>Información general sobre las tecnologías para el desarrollo de juegos

Al empezar a desarrollar un juego para la UWP, tienes a tu disposición varias opciones para los gráficos, entrada, audio, redes, utilidades y bibliotecas.

Si ya tomaste una decisión sobre todas las tecnologías que usarás en tu juego, es excelente. De lo contrario, la guía [Tecnologías de juegos para las aplicaciones para la Plataforma universal de Windows](game-development-platform-guide.md) te ofrece información general de gran utilidad sobre muchas de las tecnologías disponibles, y es bastante recomendable leerla para comprender las opciones que tienes y cómo encajan entre sí.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Encuesta sobre las tecnologías de juego para la UWP</td>
        <td><a href="game-development-platform-guide.md">Tecnologías de juego de aplicaciones para la UWP</a></td>
    </tr>
</table>

Estos tres vídeos sobre GDC 2015 ofrecen información general de gran utilidad sobre el desarrollo de juegos para Windows 10 y la experiencia de juegos para ese sistema operativo.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Información general sobre el desarrollo de juegos para Windows 10 (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Developing-Games-for-Windows-10">Desarrollo de juegos para Windows 10</a></td>
    </tr>
    <tr>
        <td>Experiencia de juegos para Windows 10 (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Gaming-Consumer-Experience-on-Windows-10">Experiencia del consumidor de juegos en Windows 10</a></td>
    </tr>
    <tr>
        <td>Juegos en todo el ecosistema de Microsoft (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/The-Future-of-Gaming-Across-the-Microsoft-Ecosystem">El futuro de los juegos en el ecosistema de Microsoft</a></td>
    </tr>
</table>

### <a name="game-planning"></a>Planificación del juego

Estos son algunos conceptos de alto nivel y aspectos de la planificación que debes tener en cuenta para tu juego.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Hacer que el juego sea accesible</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/accessibility-for-games">Accesibilidad para juegos</a></td>
    </tr>
    <tr>
        <td>Cree juegos con Cloud</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/cloud-for-games">Nube para juegos</a></td>
    </tr>
    <tr>
        <td>Monetiza tu juego</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/monetization-for-games">Monetización para juegos</a></td>
    </tr>
</table>

### <a name="choosing-your-graphics-technology-and-programming-language"></a>Elegir la tecnología de gráficos y lenguaje de programación

Hay varios lenguajes de programación y tecnologías de elementos gráficos que se pueden usar en los juegos de Windows 10. El método que elijas depende del tipo de juego que estés desarrollando, la experiencia, las preferencias de tu estudio de desarrollo y los requisitos de las funciones específicas del juego. ¿Usarás C#, C++ o JavaScript? ¿DirectX, XAML o HTML5?

#### <a name="directx"></a>DirectX

Microsoft DirectX es la opción ideal para los elementos gráficos y multimedia 2D y 3D de mayor rendimiento.

DirectX 12 es más rápido y eficaz que cualquier versión anterior. Direct3D 12 permite escenas más enriquecidas, más objetos, efectos más complejos y uso completo del hardware de GPU moderno en equipos con Windows 10 y Xbox One.

Si prefieres usar la conocida canalización de gráficos de Direct3D 11, también puedes aprovechar las nuevas funciones de optimización y representación agregadas a Direct3D 11.3. Además, si eres un desarrollador de API de Windows de escritorio demostrado con raíces de Win32, también tendrás esa opción en Windows 10.

La gran cantidad de funciones y la profunda integración de la plataforma de DirectX proporcionan la potencia y el rendimiento necesarios para los juegos más populares.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DirectX para el desarrollo de UWP</td>
        <td><a href="directx-programming.md">Programación con DirectX</a></td>
    </tr>
    <tr>
        <td>Tutorial: creación de un juego DirectX de UWP</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Crear un juego para UWP sencillo con DirectX</a></td>
    </tr>
    <tr>
        <td>Información general y referencia sobre DirectX</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/directx">Gráficos y juegos de DirectX</a></td>
    </tr>
    <tr>
        <td>Guía y referencia de programación en Direct3D 12</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/direct3d-12-graphics">Gráficos de Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Vídeos de desarrollo de elementos gráficos y DirectX 12 (canal de YouTube)</td>
        <td><a href="https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA">Educación sobre elementos gráficos y Microsoft DirectX 12</a></td>
    </tr>
</table>

#### <a name="xaml"></a>XAML

XAML es un lenguaje declarativo de interfaz de usuario fácil de usar con prácticas funciones como animaciones, guiones gráficos, enlaces de datos, gráficos vectoriales escalables, cambios de tamaño dinámicos y gráficos de escena. XAML funciona a la perfección para la interfaz de usuario de juegos, los menús, los sprites y los gráficos 2D. Para facilitar la distribución de la interfaz de usuario, XAML es compatible con herramientas de desarrollo y diseño como Expression Blend y Microsoft Visual Studio. Generalmente se usa XAML con C#, pero C++ también es una buena opción si es tu lenguaje preferido o si el juego exige un uso de la CPU muy alto.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Información general sobre la plataforma XAML</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/xaml-platform/index">Plataforma XAML</a></td>
    </tr>
    <tr>
        <td>Interfaz de usuario y controles de XAML</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/design/basics/">Controles, diseños y texto</a></td>
    </tr>
</table>

#### <a name="html-5"></a>HTML 5

El lenguaje de marcado de hipertexto (HTML) es un lenguaje de marcado de interfaz de usuario común usado para clientes enriquecidos, aplicaciones y páginas web. Los juegos de Windows pueden usar HTML5 como una capa de presentación completa con las conocidas funciones de HTML, acceso a la Plataforma universal de Windows y compatibilidad con funciones web modernas como AppCache, Web Workers, Canvas, la función de arrastrar y colocar, programación asincrónica y SVG. En segundo plano, la representación HTML aprovecha la potencia de aceleración de hardware de DirectX, para que puedas obtener los beneficios de rendimiento de DirectX sin necesidad de escribir código adicional. HTML5 es una buena opción si eres un experto en desarrollo web, migras un juego web o deseas usar capas de gráficos y lenguajes que pueden ser más fáciles de enfocar que las otras opciones. HTML5 se usa con JavaScript, pero también puede usar componentes creados con C# o C++/CX.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Información sobre HTML5 y Document Object Model</td>
        <td><a href="https://developer.mozilla.org/docs/Web">Referencia de HTML y DOM</a></td>
    </tr>
    <tr>
        <td>Recomendación HTML5 W3C</td>
        <td><a href="https://www.w3.org/TR/html5/">HTML5</a></td>
    </tr>
</table>
 
####Combinación de tecnologías de presentación

La Infraestructura de gráficos de DirectX (DXGI) de Microsoft proporciona interoperabilidad y compatibilidad con varias tecnologías de gráficos. Para los gráficos de alto rendimiento, puedes combinar XAML y DirectX, mediante XAML para los menús y otra interfaz de usuario simple y DirectX para la representación de escenas 2D y 3D complejas. DXGI también proporciona compatibilidad entre Direct2D, Direct3D, DirectWrite, DirectCompute y Microsoft Media Foundation.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guía de programación y referencia de la Infraestructura de gráficos de DirectX</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3ddxgi/dx-graphics-dxgi">DXGI</a></td>
    </tr>
    <tr>
        <td>Combinar DirectX y XAML</td>
        <td><a href="directx-and-xaml-interop.md">Interoperabilidad de DirectX y XAML</a></td>
    </tr>
</table>
 
#### C++

C++/CX es un lenguaje con poca sobrecarga y de alto rendimiento que proporciona una excelente combinación de velocidad, compatibilidad y plataforma de acceso. C++/CX facilita el uso de todas las excelentes funciones de juegos de Windows 10, incluidos DirectX y Xbox Live. También puedes volver a usar las bibliotecas y el código C++ existente. C++/CX crea código nativo y rápido que no produce la sobrecarga de la colección de elementos y, por lo tanto, tu juego puede ofrecer un gran rendimiento y un bajo consumo de energía, lo que permite aumentar la duración de la batería. Usa C++/CX con DirectX o XAML, o crea un juego que use una combinación de ambos.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Referencia e información general sobre C++/CX</td>
        <td><a href="https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx">Referencia del lenguaje de Visual C++ (C++/CX)</a></td>
    </tr>
    <tr>
        <td>Guía de programación y referencia de Visual C++</td>
        <td><a href="https://docs.microsoft.com/cpp/visual-cpp-in-visual-studio">Visual C++ en Visual Studio 2019</a></td>
    </tr>
</table>
 
#### C#

C# (pronunciado "si sharp") es un lenguaje moderno e innovador, además de sencillo, eficaz, con seguridad de tipos y orientado a objetos. C# permite desarrollar rápidamente mientras conserva la familiaridad y la expresividad de los lenguajes de estilo C. Aunque es fácil de usar, C# tiene muchas funciones avanzadas de lenguaje como polimorfismo, delegados, expresiones lambda, clausuras, métodos iterador, covarianza y expresiones de Language Integrated Query (LINQ). C# es una excelente opción si te interesa XAML, quieres empezar rápidamente a desarrollar tu juego o tienes experiencia previa con C#. C# se usa principalmente con XAML, así que si quieres usar DirectX, elige C++ en su lugar, o escribe parte de tu juego como un componente de C++ que interactúe con DirectX. O bien, ten en cuenta [Win2D](https://github.com/Microsoft/Win2D), una biblioteca de gráficos de modo inmediato de Direct2D para C# y C++.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guía de programación y referencia de C#</td>
        <td><a href="https://docs.microsoft.com/dotnet/articles/csharp/csharp">Referencia del lenguaje C#</a></td>
    </tr>
</table>
 
####Código

JavaScript es un lenguaje de scripting dinámico que se usa mucho para aplicaciones de cliente enriquecido y web modernas.

Las aplicaciones JavaScript de Windows pueden tener acceso a las funciones eficaces de la Plataforma universal de Windows de una manera fácil e intuitiva como métodos y propiedades de clases JavaScript orientados a objetos. JavaScript es una buena opción para tu juego si has trabajado en un entorno de desarrollo web, ya estás familiarizado con JavaScript o quieres usar las bibliotecas de HTML5, CSS, WinJS o JavaScript. Si te interesa XAML o DirectX, elige C# o C++/CX en su lugar.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Referencia de JavaScript y Windows Runtime</td>
        <td><a href="https://docs.microsoft.com/scripting/javascript/javascript-language-reference">Referencia de JavaScript</a></td>
    </tr>
</table>

#### <a name="use-windows-runtime-components-to-combine-languages"></a>Usar componentes de Windows Runtime para combinar idiomas

Con la Plataforma universal de Windows, es fácil combinar componentes escritos en lenguajes diferentes. Cree Windows Runtime componentes en C++, C# o Visual Basic y, a continuación, llame a ellos desde JavaScript, C#, C++ o Visual Basic. De esta manera, puedes programar partes de tu juego en el lenguaje que elijas. Los componentes también te permiten usar bibliotecas externas que solo están disponibles en un lenguaje en particular, así como usar código heredado que ya has escrito.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Cómo crear componentes de Windows Runtime</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/winrt-components/creating-windows-runtime-components-in-cpp">Componentes de Windows Runtime con C++/CX</a></td>
    </tr>
</table>

### <a name="which-version-of-directx-should-your-game-use"></a>¿Qué versión de DirectX se debe usar en tu juego?

Si eliges DirectX para tu juego, tendrás que decidir qué versión usar: Microsoft Direct3D 12 o Microsoft Direct3D 11.

DirectX 12 es más rápido y eficaz que cualquier versión anterior. Direct3D 12 permite escenas más enriquecidas, más objetos, efectos más complejos y uso completo del hardware de GPU moderno en equipos con Windows 10 y Xbox One. Dado que Direct3D 12 funciona a un nivel muy bajo, puede dar a un equipo de expertos en desarrollo de gráficos o a un equipo de desarrollo de DirectX 11 con experiencia todo el control que necesiten para maximizar la optimización de gráficos.

Direct3D 11.3 es una API de gráficos de bajo nivel que usa el ya familiar modelo de programación Direct3D y te ayuda a controlar aún más la complejidad que entraña el procesamiento por GPU. También se admite en Windows 10 y Xbox One. Si tienes un motor existente escrito en Direct3D 11 y no estás del todo listo para dar el salto a Direct3D 12, puedes usar Direct3D 11 en 12 para lograr algunas mejoras de rendimiento. Las versiones 11.3+ contienen las nuevas características de representación y optimización habilitadas también en Direct3D 12.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Elegir Direct3D 12 o Direct3D 11</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/what-is-directx-12-">¿Qué es Direct3D 12?</a></td>
    </tr>
    <tr>
        <td>Información general sobre Direct3D 11</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11">Gráficos de Direct3D 11</a></td>
    </tr>
    <tr>
        <td>Información general sobre Direct3D 11 en 12</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/direct3d-11-on-12">Direct3D 11 en 12</a></td>
    </tr>
</table>

### <a name="bridges-game-engines-and-middleware"></a>Puentes, motores de juegos y software intermedio

Según las necesidades de tu juego, el uso de puentes, motores de juego o software intermedio puede ahorrar tiempo de prueba y recursos de desarrollo. Aquí encontrará información general y recursos para puentes, motores de juegos y middleware.

#### <a name="universal-windows-platform-bridges"></a>Puentes de plataforma universal de Windows

Los puentes de plataforma universal de Windows son tecnologías que llevan tu aplicación o juego existente a la UWP. Los puentes ofrecen una buena manera de comenzar el desarrollo de juegos para UWP.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Puentes de UWP</td>
        <td><a href="https://developer.microsoft.com/windows/bridges">Lleva el código a Windows</a></td>
    </tr>
    <tr>
        <td>Puente de Windows para iOS</td>
        <td><a href="https://developer.microsoft.com/windows/bridges/ios">Migrar aplicaciones para iOS a Windows</a></td>
    </tr>
    <tr>
        <td>Puente de Windows para aplicaciones de escritorio (.NET y Win32)</td>
        <td><a href="https://developer.microsoft.com/windows/bridges/desktop">Convertir una aplicación de escritorio en una aplicación para UWP</a></td>
    </tr>
</table>

#### <a name="playfab"></a>PlayFab

Ahora como parte de la familia de Microsoft, PlayFab es una plataforma back-end completa para juegos en vivo y una manera eficaz para que estudios independientes empiecen a trabajar. Aumenta los ingresos, la participación y la retención: mientras reduces los costes, con los servicios de juego, análisis en tiempo real y LiveOps.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>PlayFab</td>
        <td><a href="https://playfab.com/">Información general de herramientas y servicios</a></td>
    </tr>
    <tr>
        <td>Introducción</td>
        <td><a href="https://api.playfab.com/docs/general-getting-started">Guía de introducción general</a></td>
    </tr>
    <tr>
        <td>Serie de tutoriales de vídeo</td>
        <td><a href="https://www.youtube.com/watch?v=fGNpiqVi5xU&list=PLHCfyL7JpoPbLpA_oh_T5PKrfzPgCpPT5">Series de vídeos de demostración sobre los sistemas principales de PlayFab</a></td>
    </tr>
    <tr>
        <td>Recetas</td>
        <td><a href="https://api.playfab.com/docs/tutorials/recipes-index">Ejemplos de patrones de diseño y mecánicas de juego populares</a></td>
    </tr>
    <tr>
        <td>Plataformas</td>
        <td><a href="https://api.playfab.com/platforms">Documentación específica para varias plataformas y motores de juegos</a></td>
    </tr>
    <tr>
        <td>Repositorio de GitHub</td>
        <td><a href="https://github.com/PlayFab">Obtenga scripts y SDK para varias plataformas, como Android, iOS, Windows, Unity y no real.</a></td>
    </tr>
    <tr>
        <td>Documentación de la API</td>
        <td><a href="https://api.playfab.com/documentation/">Acceso directo al servicio PlayFab mediante API Web de tipo REST</a></td>
    </tr>
    <tr>
        <td>Foros</td>
        <td><a href="https://community.playfab.com/index.html">Foros de PlayFab</a></td>
    </tr>
</table>
 
#### Unity

Unity ofrece una plataforma para crear juegos y aplicaciones de 2D, 3D, VR y AR atractivas e interesantes. Le permite obtener una visión rápida y entregar su contenido prácticamente a cualquier medio o dispositivo.

A partir de Unity 5,4, Unity admite el desarrollo de Direct3D 12.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Motor de juego de Unity</td>
        <td><a href="https://unity.com/">Unity: motor de juego</a></td>
    </tr>
    <tr>
        <td>Obtener Unity</td>
        <td><a href="https://store.unity.com/">Obtener Unity</a></td>
    </tr>
    <tr>
        <td>Documentación de Unity para Windows</td>
        <td><a href="https://docs.unity3d.com/Manual/Windows.html">Manual de Unity/Windows</a></td>
    </tr>
    <tr>
        <td>Agregar LiveOps mediante PlayFab</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unity-getting-started">Introducción: realice su primera llamada de API de PlayFab desde el juego de Unity</a></td>
    </tr>
    <tr>
        <td>Cómo agregar interactividad a su juego mediante mezclador interactiva</td>
        <td><a href="https://github.com/mixer/interactive-unity-plugin/wiki/Getting-started">Guía de introducción</a></td>
    </tr>
    <tr>
        <td>SDK de Mixer para Unity</td>
        <td><a href="https://www.assetstore.unity3d.com/en/#!/content/88585">Complemento de Unity de hormigonera</a></td>
    </tr>
    <tr>
        <td>Documentación de referencia del SDK de Mixer para Unity</td>
        <td><a href="https://dev.mixer.com/reference/interactive/csharp/index.html">Referencia de API para el complemento de Unity de hormigonera</a></td>
    </tr>
    <tr>
        <td>Publique su juego Unity en Microsoft Store</td>
        <td><a href="https://unity3d.com/partners/microsoft/porting-guides">Guía de migración</a></td>
    </tr>
    <tr>
        <td>Solución de problemas de referencias de ensamblado que faltan relacionadas con las API de .NET</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/missing-dot-net-apis-in-unity-and-uwp">Falta de API de .NET en Unity y UWP</a></td>
    </tr>
    <tr>
        <td>Publicar tu juego Unity como aplicación de la Plataforma universal de Windows (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/How-to-publish-your-Unity-game-as-a-UWP-app">Cómo publicar tu juego Unity como aplicación para UWP</a></td>
    </tr>
    <tr>
        <td>Usar Unity para crear aplicaciones y juegos de Windows (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/Making-games-and-apps-with-Unity">Hacer juegos y aplicaciones de Windows con Unity</a></td>
    </tr>
    <tr>
        <td>Desarrollo de juegos de Unity con Visual Studio (serie de vídeos)</td>
        <td><a href="https://www.youtube.com/playlist?list=PLReL099Y5nRfseAg0k1SJOlpqdcsDs8Em">Usar Unity con Visual Studio 2015</a></td>
    </tr>
</table>
 
####Havok

El conjunto modular de herramientas y tecnologías de Havok ayuda a los creadores de juegos llegar a nuevos niveles de interactividad e inmersión. Havok permite una física altamente realista, simulaciones interactivas y secuencias cinematográficas sorprendentes. La versión 2015,1 y versiones posteriores admiten oficialmente UWP en Visual Studio 2015 en x86, 64 bits y ARM.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Sitio web de Havok</td>
        <td><a href="https://www.havok.com/">Havok</a></td>
    </tr>
    <tr>
        <td>Conjunto de herramientas de Havok</td>
        <td><a href="https://www.havok.com/products/">Información general sobre el producto de Havok</a></td>
    </tr>
    <tr>
        <td>Foros de soporte técnico de Havok</td>
        <td><a href="https://www.havok.com/">Havok</a></td>
    </tr>
</table>
 
####MonoGame

MonoGame es un marco de trabajo de código abierto para el desarrollo de juegos multiplataforma originalmente basado en XNA Framework 4.0 de Microsoft. MonoGame es compatible actualmente con Windows, Windows Phone y Xbox, así como con Linux, macOS, iOS, Android y otras plataformas.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>MonoGame</td>
        <td><a href="https://www.monogame.net">Sitio web de MonoGame</a></td>
    </tr>
    <tr>
        <td>Documentación de MonoGame</td>
        <td><a href="https://www.monogame.net/documentation/">Documentación de MonoGame (más reciente)</a></td>
    </tr>
    <tr>
        <td>Descargas de MonoGame</td>
        <td><a href="https://www.monogame.net/downloads/">Descarga versiones, compilaciones de desarrollo y el código fuente</a> desde el sitio web de MonoGame u <a href="https://www.nuget.org/profiles/MonoGame">obtén la versión más reciente a través de NuGet</a>.
    </tr>
    <tr>
        <td>Ejemplo de juego Multigame 2D para UWP</td>
        <td><a href="../get-started/get-started-tutorial-game-mg2d.md">Crear un juego para UWP en MonoGame 2D</a></td>
    </tr>    
</table>

#### <a name="cocos2d"></a>Cocos2d

Cocos2d-x es un conjunto de herramientas y un motor de desarrollo de juegos de código abierto multiplataforma que admite la creación de juegos para UWP. A partir de la versión 3, también se agregan funciones 3D.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Cocos2d-x</td>
        <td><a href="https://www.cocos2d-x.org/">¿Qué es Cocos2d-x?</a></td>
    </tr>
    <tr>
        <td>Guía del programador de Cocos2d-x</td>
        <td><a href="https://www.cocos2d-x.org/programmersguide/">Guía para desarrolladores de Cocos2d-x</a></td>
    </tr>
    <tr>
        <td>Cocos2d-x en Windows 10 (entrada de blog)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/06/15/running-cocos2d-x-on-windows-10/">Ejecutar Cocos2d-x en Windows 10</a></td>
    </tr>
    <tr>
        <td>Agregar LiveOps mediante PlayFab</td>
        <td><a href="https://api.playfab.com/docs/getting-started/cocos2d-x-getting-started-guide">Introducción: creación de la primera llamada de API de PlayFab desde el juego de Cocos2d</a></td>
    </tr>
</table>

#### <a name="unreal-engine"></a>Unreal Engine

Unreal Engine 4 es un conjunto completo de herramientas de desarrollo de juegos para todos los tipos de juegos y desarrolladores. Con Unreal Engine, los desarrolladores de juegos de todo el mundo crean los juegos más exigentes para consola y PC.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Información general sobre Unreal Engine</td>
        <td><a href="https://www.unrealengine.com/what-is-unreal-engine-4">Unreal Engine 4</a></td>
    </tr>
    <tr>
        <td>Agregar LiveOps con PlayFab-C++</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-cpp-getting-started">Introducción: realice su primera llamada de API de PlayFab desde su juego inreal</a></td>
    </tr>
    <tr>
        <td>Agregar LiveOps con PlayFab-Blueprints</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-blueprints-getting-started">Introducción: realice su primera llamada de API de PlayFab desde su juego inreal</a></td>
    </tr>
</table>

#### <a name="babylonjs"></a>BabylonJS

BabylonJS es un marco de JavaScript completo para compilar juegos 3D con HTML5, WebGL, WebVR y Web audio.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>BabylonJS</td>
        <td><a href="https://www.babylonjs.com/">BabylonJS</a></td>
    </tr>
    <tr>
        <td>WebGL 3D con HTML5 y BabylonJS (serie de vídeos)</td>
        <td><a href="https://channel9.msdn.com/Series/Introduction-to-WebGL-3D-with-HTML5-and-Babylonjs/01">Aprender WebGL 3D y BabylonJS</a></td>
    </tr>
    <tr>
        <td>Compilar un juego WebGL multiplataforma con BabylonJS</td>
        <td><a href="https://www.smashingmagazine.com/2016/07/babylon-js-building-sponza-a-cross-platform-webgl-game/">Usar BabylonJS para desarrollar un juego multiplataforma</a></td>
    </tr>    
</table>

### <a name="porting-your-game"></a>Migrar juegos

Si tienes un juego existente, hay muchos recursos y guías disponibles que te ayudarán a llevar tu juego rápidamente a la UWP. Para empezar rápidamente las tareas de migración, también tienes la opción de usar un [Puente de plataforma universal de Windows](#universal-windows-platform-bridges).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Migrar una aplicación para Windows 8 a una aplicación para la Plataforma universal de Windows</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/porting/w8x-to-uwp-root">Mover de Windows Runtime 8.x a UWP</a></td>
    </tr>
    <tr>
        <td>Portar una aplicación para Windows 8 a una aplicación de la Plataforma universal de Windows (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/21">Migrar aplicaciones de 8.1 a Windows 10</a></td>
    </tr>
    <tr>
        <td>Portar una aplicación de iOS a una aplicación de la Plataforma universal de Windows</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/porting/ios-to-uwp-root">Migrar de iOS a UWP</a></td>
    </tr>
    <tr>
        <td>Portar una aplicación Silverlight a una aplicación de la Plataforma universal de Windows</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/porting/wpsl-to-uwp-root">Migrar de Windows Phone Silverlight a UWP</a></td>
    </tr>
    <tr>
        <td>Portar de XAML o Silverlight a una aplicación de la Plataforma universal de Windows (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2015/3-741">Migrar una aplicación de Silverlight o XAML a Windows 10</a></td>
    </tr>
    <tr>
        <td>Portar un juego de Xbox a una aplicación de la Plataforma universal de Windows</td>
        <td><a href="https://developer.xboxlive.com/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx">Migrar de Xbox One a UWP de Windows 10</a></td>
    </tr>
    <tr>
        <td>Migrar de DirectX 9 a DirectX 11</td>
        <td><a href="porting-your-directx-9-game-to-windows-store.md">Migrar de DirectX 9 a la Plataforma universal de Windows (UWP)</a></td>
    </tr>
    <tr>
        <td>Portar de Direct3D 11 a Direct3D 12</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/porting-from-direct3d-11-to-direct3d-12">Portabilidad de Direct3D 11 a Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Portar de OpenGL ES a Direct3D 11</td>
        <td><a href="port-from-opengl-es-2-0-to-directx-11-1.md">Migrar de OpenGL ES 2.0 a Direct3D 11</a></td>
    </tr>
    <tr>
        <td>De OpenGL ES a Direct3D 11 mediante ANGLE</td>
        <td><a href="https://github.com/microsoft/angle/wiki">Angulo</a></td>
    </tr>
    <tr>
        <td>Equivalentes de API clásica de Windows en la UWP</td>
        <td><a href="https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps">Alternativas a las API de Windows en las aplicaciones de la Plataforma universal de Windows (UWP)</a></td>
    </tr>
</table>

## <a name="prototype-and-design"></a>Prototipo y diseño

Ahora que decidiste el tipo de juego que quieres crear y las herramientas y tecnología de gráficos que usarás para hacerlo, estás listo para empezar a trabajar en el diseño y prototipo. En esencia, tu juego es una aplicación de la Plataforma universal de Windows, por lo que comenzarás por allí.

### <a name="introduction-to-the-universal-windows-platform-uwp"></a>Introducción a la Plataforma universal de Windows (UWP)

Windows 10 presenta la Plataforma universal de Windows (UWP), que proporciona una plataforma común de API entre todos los dispositivos Windows 10. UWP evoluciona y expande el modelo de Windows Runtime y lo perfecciona para obtener una base cohesiva y unificada. Los juegos destinados a la UWP pueden llamar a las API de WinRT que son comunes para todos los dispositivos. Dado que UWP proporciona capas de API garantizadas, puede elegir crear un paquete de aplicación único que se instalará en los dispositivos de Windows 10. Además, si quieres, tu juego aún puede llamar a las API (por ejemplo, algunas API clásicas de Windows de Win32 y. NET) que son específicas de los dispositivos en los que se ejecuta tu juego.

Esta es una lista de guías de gran calidad en las que se describen detalladamente las aplicaciones de la Plataforma universal de Windows. Se recomienda que las leas para poder comprender la plataforma.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Introducción a las aplicaciones de la Plataforma universal de Windows</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/get-started/whats-a-uwp">¿Qué es una aplicación para Plataforma universal de Windows?</a></td>
    </tr>
    <tr>
        <td>Información general sobre la UWP</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide">Guía de aplicaciones para UWP</a></td>
    </tr>
</table>
 
###Introducción al desarrollo de UWP

La preparación para desarrollar una aplicación de la Plataforma universal de Windows es rápida y sencilla. En las guías siguientes se proporcionan los procedimientos paso a paso.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Introducción al desarrollo para UWP</td>
        <td><a href="https://developer.microsoft.com/windows/apps/getstarted">Introducción a las aplicaciones de Windows</a></td>
    </tr>
    <tr>
        <td>Preparación para el desarrollo en la UWP</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/get-started/get-set-up">Prepárate</a></td>
    </tr>
</table>

Si empiezas a dar tus primeros pasos en la programación para la UWP y estás considerando usar XAML en tu juego (consulta [Elegir la tecnología de gráficos y lenguaje de programación](#choosing-your-graphics-technology-and-programming-language)), la serie de vídeos [Windows 10 development for absolute beginners (Desarrollo de Windows 10 para principiantes)](https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners) es un buen punto de partida.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Desarrollo de Windows 10 con XAML para principiantes (serie de vídeo)</td>
        <td><a href="https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners">Desarrollo de Windows 10 para principiantes</a></td>
    </tr>
    <tr>
        <td>Presentación de la serie de Windows 10 con XAML para principiantes (entrada de blog)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/09/30/windows-10-development-for-absolute-beginners/">Desarrollo de Windows 10 para principiantes</a></td>
    </tr>
</table>

### <a name="uwp-development-concepts"></a>Conceptos de desarrollo para UWP

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Información general sobre el desarrollo de aplicaciones de la Plataforma universal de Windows</td>
        <td><a href="https://developer.microsoft.com/windows/apps/develop">Desarrollar aplicaciones de Windows</a></td>
    </tr>
    <tr>
        <td>Información general sobre la programación de red en la UWP</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/networking/index">Servicios web y redes</a></td>
    </tr>
    <tr>
        <td>Usar Windows.Web.HTTP y Windows.Networking.Sockets en los juegos</td>
        <td><a href="work-with-networking-in-your-directx-game.md">Conexión en red de juegos</a></td>
    </tr>
    <tr>
        <td>Conceptos de programación asincrónica en la UWP</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps">Programación asincrónica</a></td>
    </tr>
</table>

### <a name="windows-desktop-apisto-uwp"></a>API de escritorio de Windows para UWP

Estos son algunos vínculos que le ayudarán a migrar el juego de escritorio de Windows a UWP.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Usar código C++ existente para el desarrollo de juegos para UWP</td>
        <td><a href="https://docs.microsoft.com/cpp/porting/how-to-use-existing-cpp-code-in-a-universal-windows-platform-app">Cómo: usar código C++ existente en una aplicación de UWP</a></td>
    </tr>
    <tr>
        <td>API de Windows Runtime para las API de Win32 y COM</td>
        <td><a href="https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps">API de Win32 y COM para las aplicaciones para UWP</a></td>
    </tr>
    <tr>
        <td>Funciones de CRT no admitidas en UWP</td>
        <td><a href="https://docs.microsoft.com/cpp/cppcx/crt-functions-not-supported-in-universal-windows-platform-apps">Funciones de CRT no admitidas en aplicaciones de la Plataforma universal de Windows</a></td>
    </tr>
    <tr>
        <td>Alternativas para las API de Windows</td>
        <td><a href="https://docs.microsoft.com/uwp/win32-and-com/alternatives-to-windows-apis-uwp">Alternativas a las API de Windows en las aplicaciones de la Plataforma universal de Windows (UWP)</a></td>
    </tr>
</table>
 
###Administración de la duración del proceso

En la Administración del ciclo de vida de los procesos, o ciclo de vida de la aplicación, se describen los diferentes estados de activación por los que puede pasar una aplicación de la Plataforma universal de Windows. El juego se puede activar, suspender, reanudar o finalizar y puede pasar por esos estados en una variedad de formas.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Controlar las transiciones de ciclo de vida de la aplicación</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/launch-resume/app-lifecycle">Ciclo de vida de la aplicación</a></td>
    </tr>
    <tr>
        <td>Usar Microsoft Visual Studio para desencadenar las transiciones de la aplicación</td>
        <td><a href="https://docs.microsoft.com/visualstudio/debugger/how-to-trigger-suspend-resume-and-background-events-for-windows-store-apps-in-visual-studio?view=vs-2015">Cómo desencadenar eventos de suspensión, reanudación y en segundo plano para aplicaciones UWP en Visual Studio</a></td>
    </tr>
</table>
 
###Diseñar experiencia de usuario de juegos

El origen de un gran juego es un diseño inspirado.

Los juegos comparten algunos principios de diseño y elementos de interfaz de usuario comunes con las aplicaciones, pero a menudo cuentan con un aspecto, sensación y objetivo de diseño únicos en la experiencia de usuario. Los juegos tienen éxito cuando se aplica un diseño detallista en dos aspectos, por lo que debes preguntarte: ¿cuándo debería usar el juego una experiencia de usuario probada y cuándo debería ser esta diferente e innovadora? La tecnología de presentación que elijas para tu juego (DirectX, XAML, HTML5, o alguna combinación de las tres), influirá en los detalles de implementación, pero los principios de diseño que apliques serán totalmente independientes de esa opción.

Aparte del diseño de la experiencia de usuario, el diseño mismo del juego (como el diseño del nivel, el ritmo, el diseño del mundo y otros aspectos) ya es por sí solo una forma de arte; una que depende exclusivamente de ti y tu equipo y que no se tratará en esta guía de desarrollo.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Conceptos básicos y directrices del diseño para UWP</td>
        <td><a href="https://developer.microsoft.com/windows/apps/design">Diseñar aplicaciones para UWP</a></td>
    </tr>
    <tr>
        <td>Diseñar para los estados de ciclo de vida de la aplicación</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/launch-resume/index">Directrices sobre la experiencia del usuario para inicio, suspensión y reanudación</a></td>
    </tr>
    <tr>
        <td>Diseño de la aplicación para UWP para Xbox One y pantallas de televisión</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv">Diseño para Xbox y televisión</a></td>
    </tr>
    <tr>
        <td>Dirigirse a varios factores de forma de dispositivo (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Designing-Games-for-a-Windows-Core-World">Diseño de juegos para un mundo de Windows Core</a></td>
    </tr>   
</table>
 
####Paleta y directriz de color

El uso de una pauta de colores coherente en tu juego mejora la estética, acelera la navegación y, además, es una herramienta eficaz para informar al jugador del menú y las funciones de la pantalla de visualización frontal. El hecho de colorear de forma coherente los elementos del juego como, por ejemplo, las advertencias, los daños, los puntos de experiencia y los logros, puede dar lugar a una interfaz de usuario más limpia y reducir la necesidad de usar etiquetas explícitas.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guía de colores</td>
        <td><a href="https://assets.windowsphone.com/499cd2be-64ed-4b05-a4f5-cd0c9ad3f6a3/101_BestPractices_Color_InvariantCulture_Default.zip">Procedimientos recomendados: colores</a></td>
    </tr>
</table>

#### <a name="typography"></a>Tipografía

El uso adecuado de la tipografía mejora muchos aspectos de tu juego, incluida la distribución de la interfaz de usuario, la navegación, la legibilidad, la atmósfera, la marca y la inmersión del jugador.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guía de tipografía</td>
        <td><a href="https://cmsresources.windowsphone.com/devcenter/common/resources/content/101_BestPractices_Typography.pdf">Procedimientos recomendados: tipografía</a></td>
    </tr>
</table>

#### <a name="ui-map"></a>Mapa de la interfaz de usuario

Un mapa de la interfaz de usuario es una distribución de los menús y la navegación del juego expresada como un diagrama de flujo. El mapa de la interfaz de usuario ayuda a todas las partes interesadas implicadas a comprender la interfaz del juego y las rutas de navegación, y puede exponer obstáculos potenciales y puntos muertos al principio del ciclo de desarrollo.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guía del mapa de la interfaz de usuario</td>
        <td><a href="https://cmsresources.windowsphone.com/devcenter/common/resources/content/101_BestPractices_UI_Map.pdf">Procedimientos recomendados: mapa de la interfaz de usuario</a></td>
    </tr>
</table>

### <a name="game-audio"></a>Audio de juegos

Guías y referencias para la implementación de audio en juegos con XAudio2, XAPO y Windows Sonic. XAudio2 es una API de audio de bajo nivel que proporciona procesamiento de señal y combinación de base para desarrollar motores de audio de alto rendimiento. La API de XAPO permite la creación de objetos de procesamiento de audio (XAPO) multiplataforma para su uso en XAudio2 en Windows y Xbox. La compatibilidad con audio de Windows Sonic le permite agregar Dolby Atmos para Home Theater, Dolby Atmos para auriculares y soporte técnico de Windows HRTF a su aplicación multimedia de juegos o streaming.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>API de XAudio2</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-apis-portal">Guía de programación y referencia de API para XAudio2</a></td>
    </tr>
    <tr>
        <td>Crear objetos de procesamiento de audio multiplataforma</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/xaudio2/xapo-overview">Información general de XAPO</a></td>
    </tr>
    <tr>
        <td>Introducción a los conceptos de audio</td>
        <td><a href="working-with-audio-in-your-directx-game.md">Audio para juegos</a></td>
    </tr>
    <tr>
        <td>Información general de Sonic de Windows</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/CoreAudio/spatial-sound">Sonido espacial</a></td>
    </tr>
    <tr>
        <td>Ejemplos de sonido espacial de Windows Sonic</td>
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/Audio">Ejemplos de audio de grupo de tecnología avanzada de Xbox</a></td>
    </tr>
    <tr>
        <td>Aprenda a integrar Windows Sonic en sus juegos (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-002">Introducción a las funcionalidades de audio espacial para Xbox y Windows</a></td>
    </tr>
</table>

### <a name="directx-development"></a>Desarrollo de DirectX

Guías y referencias para desarrollar un juego con DirectX.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>DirectX para el desarrollo de UWP</td>
        <td><a href="directx-programming.md">Programación con DirectX</a></td>
    </tr>
    <tr>
        <td>Tutorial: creación de un juego DirectX de UWP</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Crear un juego para UWP sencillo con DirectX</a></td>
    </tr>
    <tr>
        <td>Interacción de DirectX con el modelo de aplicaciones para UWP</td>
        <td><a href="about-the-uwp-user-interface-and-directx.md">Objeto de aplicación y DirectX</a></td>
    </tr>
    <tr>
        <td>Vídeos de desarrollo de elementos gráficos y DirectX 12 (canal de YouTube)</td>
        <td><a href="https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA">Educación sobre elementos gráficos y Microsoft DirectX 12</a></td>
    </tr>
    <tr>
        <td>Información general y referencia sobre DirectX</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/directx">Gráficos y juegos de DirectX</a></td>
    </tr>
    <tr>
        <td>Guía y referencia de programación en Direct3D 12</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/direct3d-12-graphics">Gráficos de Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Conceptos básicos de DirectX 12 (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Better-Power-Better-Performance-Your-Game-on-DirectX12">Mejor alimentación, mejor rendimiento: tu juego en DirectX 12</a></td>
    </tr>
</table>

#### <a name="learning-direct3d-12"></a>Aprendizaje de Direct3D 12

Obtén información sobre qué ha cambiado en Direct3D 12 y cómo empezar a programar con Direct3D 12. 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Configurar el entorno de programación</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/directx-12-programming-environment-set-up">Configuración del entorno de programación de Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Cómo crear un componente básico</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/creating-a-basic-direct3d-12-component">Crear un componente básico de Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Cambios en Direct3D 12</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/important-changes-from-directx-11-to-directx-12">Cambios importantes al migrar de Direct3D 11 a Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Cómo portar de Direct3D 11 a Direct3D 12</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/porting-from-direct3d-11-to-direct3d-12">Portabilidad de Direct3D 11 a Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Conceptos de enlace de recursos (descriptor de cobertura, tabla de descriptor, montón de descriptor y firma raíz) </td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/resource-binding">Enlace de recursos en Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Administración de memoria</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/memory-management">Administración de la memoria en Direct3D 12</a></td>
    </tr>
</table>
 
####Kit de herramientas y bibliotecas de DirectX

El kit de herramientas de DirectX, la biblioteca de procesamiento de texturas de DirectX, la biblioteca de procesamiento de geometría DirectXMesh, la biblioteca de UVAtlas y la biblioteca de DirectXMath proporcionan texturas, mallas, sprite y otras clases auxiliares y funciones de utilidad para el desarrollo en DirectX. Estas bibliotecas pueden ayudarte a ahorrar tiempo y esfuerzo en el desarrollo.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Obtén el kit de herramientas de DirectX 11.</td>
        <td><a href="https://github.com/Microsoft/DirectXTK">DirectXTK</a></td>
    </tr>
    <tr>
        <td>Obtén el kit de herramientas de DirectX 12.</td>
        <td><a href="https://github.com/Microsoft/DirectXTK12">DirectXTK 12</a></td>
    </tr>
    <tr>
        <td>Obtén la biblioteca de procesamiento de texturas de DirectX.</td>
        <td><a href="https://github.com/Microsoft/DirectXTex">DirectXTex</a></td>
    </tr>
    <tr>
        <td>Obtén la biblioteca de procesamiento de geometría de DirectXMesh.</td>
        <td><a href="https://github.com/Microsoft/DirectXMesh">DirectXMesh</a></td>
    </tr>
    <tr>
        <td>Obtén UVAtlas para crear y empaquetar atlas de textura de isochart</td>
        <td><a href="https://github.com/Microsoft/UVAtlas">UVAtlas</a></td>
    </tr>
    <tr>
        <td>Obtén la biblioteca de DirectXMath</td>
        <td><a href="https://github.com/Microsoft/DirectXMath">DirectXMath</a></td>
    </tr>
    <tr>
        <td>Compatibilidad con Direct3D 12 en DirectXTK (entrada de blog)</td>
        <td><a href="https://github.com/Microsoft/DirectXTK/issues/2">Compatibilidad con DirectX 12</a></td>
    </tr>
</table>

#### <a name="directx-resources-from-partners"></a>Recursos de DirectX de partners

A continuación se presenta documentación adicional de DirectX creada por partners externos.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Nvidia: DX12 Do's and Don'ts (Nvidia: Qué hacer y qué no hacer en DX12) (entrada de blog) </td>
        <td><a href="https://developer.nvidia.com/dx12-dos-and-donts-updated">DirectX 12 en las GPU Nvidia</a></td>
    </tr>
    <tr>
        <td>Intel: representación eficaz con DirectX 12</td>
        <td><a href="https://software.intel.com/sites/default/files/managed/4a/38/Efficient-Rendering-with-DirectX-12-on-Intel-Graphics.pdf">Representación de DirectX 12 en gráficos de Intel</a></td>
    </tr>
    <tr>
        <td>Intel: compatibilidad con varios adaptadores en DirectX 12</td>
        <td><a href="https://software.intel.com/articles/multi-adapter-support-in-directx-12">Cómo implementar una aplicación de varios adaptadores explícita mediante DirectX 12</a></td>
    </tr>
    <tr>
        <td>Intel: tutorial de DirectX 12</td>
        <td><a href="https://software.intel.com/articles/tutorial-migrating-your-apps-to-directx-12-part-1">Notas del producto de colaboración de Intel, Suzhou Snail y Microsoft</a></td>
    </tr>
</table>

## <a name="production"></a>Producción

Tu estudio ahora está totalmente comprometido y pasa al ciclo de producción, con trabajo distribuido entre todos los miembros de tu equipo. Estás puliendo, refactorizando y ampliando el prototipo para modelarlo en un juego completo.

### <a name="notifications-and-live-tiles"></a>Notificaciones e iconos dinámicos

Un icono es una representación de tu juego en el menú Inicio. Los iconos y las notificaciones pueden impulsar el interés de los jugadores, incluso si no están usando tu juego.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Desarrollar iconos y distintivos</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-badges-notifications">Iconos, distintivos y notificaciones</a></td>
    </tr>
    <tr>
        <td>Muestra que ilustra iconos dinámicos y notificaciones</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications">Muestra de notificaciones</a></td>
    </tr>
    <tr>
        <td>Plantillas de icono adaptables (entrada de blog)</td>
        <td><a href="https://blogs.msdn.microsoft.com/tiles_and_toasts/2015/06/30/adaptive-tile-templates-schema-and-documentation/">Plantillas de icono adaptables: esquema y documentación</a></td>
    </tr>
    <tr>
        <td>Diseñar iconos y distintivos</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-creating-tiles">Directrices sobre iconos y distintivos</a></td>
    </tr>
    <tr>
        <td>Aplicación de Windows 10 para desarrollar interactivamente plantillas de icono dinámico</td>
        <td><a href="https://www.microsoft.com/store/apps/9nblggh5xsl1">Notifications Visualizer</a></td>
    </tr>
    <tr>
        <td>Extensión del generador de iconos de UWP para Visual Studio</td>
        <td><a href="https://marketplace.visualstudio.com/items?itemName=shenchauhan.UWPTileGenerator">Herramienta para crear todos los iconos con única imagen</a></td>
    </tr>
    <tr>
        <td>Extensión del generador de iconos para UWP para Visual Studio (entrada de blog)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2016/02/15/uwp-tile-generator-extension-for-visual-studio/">Sugerencias sobre el uso de la herramienta de generador de iconos para UWP</a></td>
    </tr>
</table>

### <a name="enable-in-app-product-add-on-purchases"></a>Habilitar compras de productos en la aplicación (complemento)

Un complemento (producto en aplicación) es un elemento complementario que los jugadores pueden comprar en el juego. Los complementos pueden ser niveles de juego, elementos o cualquier otro elemento que los jugadores puedan disfrutar. Si se usa correctamente, los complementos pueden proporcionar ingresos a la vez que mejoran la experiencia del juego. Puede definir y publicar los complementos del juego a través del centro de Partners y habilitar las compras desde la aplicación en el código del juego.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Complementos duraderos</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/monetize/enable-in-app-product-purchases">Habilitar compras de productos desde la aplicación</a></td>
    </tr>
    <tr>
        <td>Complementos consumibles</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/monetize/enable-consumable-in-app-product-purchases">Habilitar compras de productos consumibles desde la aplicación</a></td>
    </tr>
    <tr>
        <td>Detalles del complemento y envío</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/iap-submissions">Envíos de complementos</a></td>
    </tr>
    <tr>
        <td>Supervise las ventas y los datos demográficos de los complementos para su juego</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/iap-acquisitions-report">Informe de adquisiciones de complementos</a></td>
    </tr>
</table>
 
###Depuración, optimización del rendimiento y supervisión

Para optimizar el rendimiento, aproveche el modo de juego de Windows 10 para proporcionar a los jugadores la mejor experiencia de juego posible mediante el uso completo de la capacidad de su hardware actual.

Windows Performance Toolkit (WPT) se compone de herramientas de supervisión del rendimiento que producen perfiles detallados del rendimiento de aplicaciones y sistemas operativos Windows. Esto es sumamente útil para supervisar el uso de memoria y mejorar el rendimiento de juegos. Windows Performance Toolkit se incluye en el SDK de Windows 10 y Windows ADK. Este kit de herramientas consta de dos herramientas independientes: Windows Performance Recorder (WPR) y Windows Performance Analyzer (WPA). ProcDump, que forma parte de [Windows Sysinternals](https://technet.microsoft.com/sysinternals/default), es una utilidad de línea de comandos que supervisa los picos de CPU y genera archivos de volcado durante los bloqueos del juego. 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Probar el rendimiento del código</td>
        <td><a href="https://azure.microsoft.com/services/devops/test-plans/">Pruebas de carga basadas en la nube</a></td>
    </tr>
    <tr>
        <td>Obtiene el tipo de consola Xbox mediante la información del dispositivo de juego</td>
        <td><a href="https://docs.microsoft.com/previous-versions/windows/desktop/gamingdvcinfo/gaming-device-information-portal">Información del dispositivo de juegos</a></td>
    </tr>
    <tr>
        <td>Mejorar el rendimiento al obtener acceso exclusivo o prioritario a los recursos de hardware mediante las API del modo de juego</td>
        <td><a href="https://docs.microsoft.com/previous-versions/windows/desktop/gamemode/game-mode-portal">Modo Juego</a></td>
    </tr>
    <tr>
        <td>Obtener Windows Performance Toolkit (WPT) del SDK de Windows 10</td>
        <td><a href="https://developer.microsoft.com/windows/downloads/windows-10-sdk">SDK de Windows 10</a></td>
    </tr>
    <tr>
        <td>Obtener Windows Performance Toolkit (WPT) de Windows ADK</td>
        <td><a href="https://msdn.microsoft.com/windows/hardware/dn913721.aspx">Windows ADK</a></td>
    </tr>
    <tr>
        <td>Solucionar problemas de una interfaz de usuario que no responde con Windows Performance Analyzer (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-156-Critical-Path-Analysis-with-Windows-Performance-Analyzer">Análisis de la ruta crítica con WPA</a></td>
    </tr>
    <tr>
        <td>Diagnosticar el uso de memoria y pérdidas mediante Windows Performance Recorder (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-154-Memory-Footprint-and-Leaks">Pérdidas y superficie de memoria</a></td>
    </tr>
    <tr>
        <td>Obtener ProcDump</td>
        <td><a href="https://technet.microsoft.com/sysinternals/dd996900">ProcDump</a></td>
    </tr>
    <tr>
        <td>Aprender a usar ProcDump (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-131-Windows-10-SDK">Configurar ProcDump para crear archivos de volcado de memoria</a></td>
    </tr>
</table>

### <a name="advanced-directx-techniques-and-concepts"></a>Conceptos y técnicas avanzadas de DirectX

Algunas partes del desarrollo en DirectX pueden ser complejas y matizadas. Cuando estés en la fase de producción y llegues al punto donde necesites profundizar en los detalles de tu motor de DirectX o debas depurar problemas relacionados con el rendimiento especialmente difíciles, los recursos y la información de esta sección podrían ayudarte.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>PIX en Windows</td>
        <td><a href="https://devblogs.microsoft.com/pix/introducing-pix-on-windows-beta/">Herramienta de optimización y depuración del rendimiento para DirectX 12 en Windows</a></td>
    </tr>
    <tr>
        <td>Herramientas de depuración y validación para el desarrollo de D3D12 (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-003">Optimización del rendimiento y depuración de D3D12 con validación de la GPU y PIX</a></td>
    </tr>
    <tr>
        <td>Optimizar el rendimiento y los elementos gráficos (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Advanced-DirectX12-Graphics-and-Performance">Rendimiento y elementos gráficos avanzados de DirectX 12</a></td>
    </tr>
    <tr>
        <td>Depuración de elementos gráficos en DirectX (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Solve-the-Tough-Graphics-Problems-with-your-Game-Using-DirectX-Tools">Solucionar problemas difíciles relacionados con los elementos gráficos de tu juego mediante herramientas de DirectX</a></td>
    </tr>
    <tr>
        <td>Herramientas de Visual Studio 2015 para depurar DirectX 12 (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Series/ConnectOn-Demand/212">Herramientas de DirectX para Windows 10 en Visual Studio 2015</a></td>
    </tr>
    <tr>
        <td>Guía de programación de Direct3D 12</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/direct3d12/direct3d-12-graphics">Guía de programación de Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Combinar DirectX y XAML</td>
        <td><a href="directx-and-xaml-interop.md">Interoperabilidad de DirectX y XAML</a></td>
    </tr>
</table>

### <a name="high-dynamic-range-hdr-content-development"></a>Desarrollo de contenido de intervalo dinámico alto (HDR)

Compilar contenido de juego que usa las capacidades de color completo de HDR.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Introducción a los conceptos de HDR y color (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2017/P4061">Iluminación de color avanzado y HDR en DirectX</a></td>
    </tr>
    <tr>
        <td>Obtenga información sobre cómo representar el contenido HDR y detectar si la pantalla actual lo admite</td>
        <td><a href="https://github.com/Microsoft/DirectX-Graphics-Samples/tree/master/Samples/UWP/D3D12HDR">Ejemplo HDR</a></td>
    </tr>
    <tr>
        <td>Crear y configurar un color avanzado con DirectX</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DAdvancedColorImages">Ejemplo de representación de imagen de color avanzado de Direct2D</a></td>
    </tr>   
</table>

### <a name="globalization-and-localization"></a>Globalización y localización

Desarrolla juegos internacionales para la plataforma de Windows y obtén información sobre las características internacionales integradas en los principales productos de Microsoft.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Preparar tu juego para el mercado global</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/globalizing/globalizing-portal">Directrices de desarrollo para un público global</a></td>
    </tr>
    <tr>
        <td>Puente entre idiomas, culturas y tecnologías</td>
        <td><a href="https://www.microsoft.com/Language/Default.aspx">Recurso en línea para las convenciones de idioma y la terminología estándar de Microsoft</a></td>
    </tr>
</table>

## <a name="submitting-and-publishing-your-game"></a>Enviar y publicar juegos

Las siguientes guías e información ayudan a agilizar lo más posible el proceso de publicación y envío.

### <a name="publishing"></a>Publicación

Usará el [centro de Partners](https://partner.microsoft.com/dashboard) para publicar y administrar los paquetes de juegos.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Publicación de aplicaciones del centro de Partners</td>
        <td><a href="https://developer.microsoft.com/store/publish-apps">Publicar aplicaciones de Windows</a></td>
    </tr>
    <tr>
        <td>Publicación avanzada del centro de Partners (GDN)</td>
        <td><a href="https://developer.xboxlive.com/windows/documentation/Pages/home.aspx">Guía de publicación avanzada del centro de Partners</a></td>
    </tr>
    <tr>
        <td>Usar Azure Active Directory (AAD) para agregar usuarios a la cuenta del centro de Partners</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/manage-account-users">Administrar usuarios de la cuenta</a></td>
    </tr>   
    <tr>
        <td>Clasificación del juego (entrada de blog)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2016/01/06/now-available-single-age-rating-system-to-simplify-app-submissions/">Flujo de trabajo único para asignar la clasificación por edades mediante el sistema IARC</a></td>
    </tr>
</table>

#### <a name="packaging-and-uploading"></a>Empaquetar y cargar

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Aprenda a usar la instalación de streaming y paquetes opcionales (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2017/B8093">Distribución de aplicaciones para UWP de NextGen: creación de aplicaciones en componentes extensibles y con capacidad de streaming</a></td>
    </tr>
    <tr>
        <td>División y agrupación de contenido para habilitar la instalación de streaming</td>
        <td><a href="https://docs.microsoft.com/windows/msix/package/streaming-install">Instalación de streaming de aplicaciones para UWP</a></td>
    </tr>
    <tr>
        <td>Crear paquetes opcionales como el contenido del juego DLC</td>
        <td><a href="https://docs.microsoft.com/windows/msix/package/optional-packages">Creación de paquetes opcionales y conjuntos relacionados</a></td>
    </tr>
    <tr>
        <td>Empaquetar el juego de UWP</td>
        <td><a href="../packaging/index.md">Empaquetado de aplicaciones</a></td>
    </tr>
    <tr>
        <td>Empaquetar el juego DirectX de UWP</td>
        <td><a href="package-your-windows-store-directx-game.md">Empaquetar el juego DirectX de UWP</a></td>
    </tr>
    <tr>
        <td>Empaquetar el juego como un desarrollador de terceros (entrada de blog)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/">Crear paquetes que se puedan cargar sin acceso a la cuenta de la Tienda del publicador</a></td>
    </tr>
    <tr>
        <td>Creación paquetes de aplicaciones y lotes de paquetes de aplicaciones con MakeAppx</td>
        <td><a href="https://docs.microsoft.com/windows/msix/package/create-app-package-with-makeappx-tool">Crear paquetes mediante la herramienta de empaquetador de aplicaciones MakeAppx.exe</a></td>
    </tr>
    <tr>
        <td>Firma digital de los archivos con SignTool</td>
        <td><a href="https://docs.microsoft.com/windows/desktop/SecCrypto/signtool">Firma archivos y comprueba las firmas en los archivos con SignTool.</a></td>
    </tr>    
    <tr>
        <td>Carga y control de versiones del juego</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/upload-app-packages">Cargar paquetes de la aplicación</a></td>
    </tr>
</table>

### <a name="policies-and-certification"></a>Directivas y certificación

No dejes que los problemas de certificación retrasen el lanzamiento de tu juego. Estas son las directivas y los problemas de certificación comunes que deberías tener en cuenta.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Acuerdo de desarrollador de aplicaciones Microsoft Store</td>
        <td><a href="https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement">Acuerdo para desarrolladores de aplicaciones</a></td>
    </tr>
    <tr>
        <td>Directivas para publicar aplicaciones en el Microsoft Store</td>
        <td><a href="https://docs.microsoft.com/legal/windows/agreements/store-policies">Directivas de Microsoft Store</a></td>
    </tr>
    <tr>
        <td>Cómo evitar algunos problemas de certificación comunes en las aplicaciones</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/avoid-common-certification-failures">Evitar errores de certificación comunes</a></td>
    </tr>
</table>

### <a name="store-manifest-storemanifestxml"></a>Manifiesto de la tienda (StoreManifest.xml)

El manifiesto de la tienda (StoreManifest.xml) es un archivo de configuración opcional que se puede incluir en tu paquete de la aplicación. En él se proporcionan funciones adicionales que no forman parte del archivo AppxManifest.xml. Por ejemplo, puedes usar el manifiesto de la tienda para bloquear la instalación de tu juego si un dispositivo de destino no tiene el nivel mínimo especificado de funciones de DirectX o la memoria mínima especificada del sistema.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Esquema del manifiesto de la tienda</td>
        <td><a href="https://docs.microsoft.com/uwp/schemas/storemanifest/storemanifestschema2015/schema-root">Esquema StoreManifest (Windows 10)</a></td>
    </tr>
</table>

## <a name="game-lifecycle-management"></a>Administración del ciclo de vida del juego

Cuando hayas terminado el desarrollo y enviado tu juego, todavía no habrás terminado. Quizás hayas terminado el desarrollo de la versión uno, pero el camino de tu juego en el mercado tan solo acaba de comenzar. Es recomendable supervisar el uso y los informe de errores, responder a los comentarios de los usuarios y publicar actualizaciones para tu juego.

### <a name="partner-center-analytics-and-promotion"></a>Promoción y análisis del centro de Partners

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Análisis del centro de Partners</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/analytics">Analizar el rendimiento de las aplicaciones</a></td>
    </tr>
    <tr>
        <td>Obtenga información sobre cómo los clientes intervienen con las características de Xbox del juego</td>
        <td><a href="../publish/xbox-analytics-report.md">Informe de análisis de Xbox</a></td>
    </tr>
    <tr>
        <td>Responder a las críticas de los clientes</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/respond-to-customer-reviews">Responde a las opiniones de los clientes</a></td>
    </tr>
    <tr>
        <td>Formas para promover tu juego</td>
        <td><a href="https://developer.microsoft.com/store/promote-your-apps">Promociona tus aplicaciones</a></td>
    </tr>
</table>
 
###Application Insights de Visual Studio

Visual Studio Application Insights proporciona análisis de rendimiento, telemetría y uso para tu juego publicado. Application Insights te ayuda a detectar y solucionar problemas después del lanzamiento de tu juego, supervisar y mejorar el uso continuamente, así como comprender la forma en que los jugadores interactúan con el juego. Aplicación Insights funciona agregando un SDK a la aplicación, que envía la telemetría al [Portal de Azure](https://portal.azure.com/).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Análisis de rendimiento y uso de la aplicación</td>
        <td><a href="https://azure.microsoft.com/documentation/articles/app-insights-get-started/">Application Insights de Visual Studio</a></td>
    </tr>
    <tr>
        <td>Habilitar Application Insights en aplicaciones de Windows</td>
        <td><a href="https://azure.microsoft.com/documentation/articles/app-insights-windows-get-started/">Application Insights para aplicaciones de Windows Phone y la Tienda</a></td>
    </tr>
</table>

### <a name="third-party-solutions-for-analytics-and-promotion"></a>Soluciones de terceros para análisis y promoción

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Descripción del comportamiento del reproductor mediante GameAnalytics</td>
        <td><a href="https://gameanalytics.com/">GameAnalytics</a></td>
    </tr>
    <tr>
        <td>Conexión del juego de UWP a Google Analytics</td>
        <td><a href="https://github.com/dotnet/windows-sdk-for-google-analytics">Obtener Windows SDK para Google Analytics</a></td>
    </tr>
    <tr>
        <td>Aprenda a usar Windows SDK para Google Analytics (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-the-Windows-SDK-for-Google-Analytics">Introducción a Windows SDK para Google Analytics</a></td>
    </tr>    
    <tr>
        <td>Usar la aplicación de Facebook instala anuncios para promocionar el juego a los usuarios de Facebook</td>
        <td><a href="https://github.com/Microsoft/winsdkfb">Obtener Windows SDK para Facebook</a></td>
    </tr>
    <tr>
        <td>Más información sobre cómo usar las instalaciones de aplicaciones de Facebook anuncios (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-Facebook-App-Install-Ads">Introducción a Windows SDK para Facebook</a></td>
    </tr>
    <tr>
        <td>Use Vungle para agregar anuncios de vídeo en sus juegos</td>
        <td><a href="https://publisher.vungle.com/sdk/">Obtener Windows SDK para Vungle</a></td>
    </tr>
</table>

### <a name="creating-and-managing-content-updates"></a>Crear y administrar actualizaciones de contenido

Para actualizar tu juego publicado, envía un nuevo paquete de la aplicación con un número de versión superior. Después de que el paquete pase por el proceso de envío y certificación, estará disponible automáticamente para los clientes como una actualización.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Actualizar y controlar las versiones de tu juego</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/package-version-numbering">Numeración de la versión del paquete</a></td>
    </tr>
    <tr>
        <td>Instrucciones para administrar paquetes de juego</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/package-version-numbering">Orientación para administrar paquetes de la aplicación</a></td>
    </tr>
</table>

## <a name="adding-xbox-live-to-your-game"></a>Agregar Xbox Live a tu juego

Xbox Live es una red de juegos líder que conecta a millones de jugadores de todo el mundo. Los desarrolladores obtienen acceso a las características de Xbox Live que pueden aumentar la audiencia de sus juegos, como la presencia de Xbox Live, los marcadores, el ahorro en la nube, los centros de juegos, los clubs, el chat de la fiesta, el juego DVR, etc.

> [!Note]
> Si desea desarrollar títulos habilitados para Xbox Live, hay varias opciones disponibles. Para obtener información sobre los distintos programas, consulte [información general del programa para desarrolladores](https://docs.microsoft.com/gaming/xbox-live/developer-program-overview).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Información general de Xbox Live</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/index.md">Guía del desarrollador de Xbox Live</a></td>
    </tr>
    <tr>
        <td>Comprender qué características están disponibles en función del programa</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/developer-program-overview.md#feature-table">Información general del programa para desarrolladores: tabla de características</a></td>
    </tr>
    <tr>
        <td>Vínculos a recursos útiles para desarrollar juegos de Xbox Live</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/xbox-live-resources.md">Recursos de Xbox Live</a></td>
    </tr>
    <tr>
        <td>Obtenga información acerca de cómo obtener información de los servicios de Xbox Live</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/introduction-to-xbox-live-apis.md">Introducción a las API de Xbox Live</a></td>
    </tr>
</table>

### <a name="for-developers-in-the-xbox-live-creators-program"></a>Para desarrolladores en el programa de creadores de Xbox Live

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Información general</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md">Introducción al programa de creadores de Xbox Live</a></td>
    </tr>
    <tr>
        <td>Agregar Xbox Live al juego</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/creators-step-by-step-guide.md">Guía paso a paso para integrar el programa Xbox Live Creators</a></td>
    </tr>
    <tr>
        <td>Agregar Xbox Live al juego UWP creado con Unity</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/develop-creators-title-with-unity.md">Introducción al desarrollo de un título de programa de Xbox Live Creators con el motor de juegos de Unity</a></td>
    </tr>
    <tr>
        <td>Configurar el espacio aislado de desarrollo</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/xbox-live-sandboxes-creators.md">Introducción a los espacios aislados de Xbox Live</a></td>
    </tr>
    <tr>
        <td>Configurar cuentas para pruebas</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/authorize-xbox-live-accounts.md">Autorizar cuentas de Xbox Live en el entorno de prueba</a></td>
    </tr>
    <tr>
        <td>Ejemplos del programa Xbox Live Creators</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/CreatorsSDK">Ejemplos de código para desarrolladores de programas de Creators</a></td>
    </tr>
    <tr>
        <td>Obtenga información sobre cómo integrar experiencias de Xbox Live multiplataforma en juegos para UWP (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-005">Programa de creadores de Xbox Live</a></td>
    </tr>  
</table>

### <a name="for-managed-partners-and-developers-in-the-idxbox-program"></a>Para socios y desarrolladores administrados en el ID@Xbox programa

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Información general</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-partner/get-started-with-xbox-live-partner.md">Introducción a Xbox Live como un asociado administrado o un desarrollador de ID.</a></td>
    </tr>
    <tr>
        <td>Agregar Xbox Live al juego</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-partner/partners-step-by-step-guide.md">Guía paso a paso para integrar Xbox Live para asociados administrados y miembros de identificador</a></td>
    </tr>
    <tr>
        <td>Agregar Xbox Live al juego UWP creado con Unity</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-partner/partner-unity-uwp-il2cpp.md">Agregar compatibilidad con Xbox Live a Unity para UWP con el back-end de scripting de IL2CPP para IDENTIFICADOres y asociados administrados</a></td>
    </tr>
    <tr>
        <td>Configurar el espacio aislado de desarrollo</td>
        <td><a href="https://docs.microsoft.com/gaming/xbox-live/get-started-with-partner/advanced-xbox-live-sandboxes.md">Espacio aislado de Xbox Live avanzado</a></td>
    </tr>
    <tr>
        <td>Requisitos para juegos que usan Xbox Live (GDN)</td>
        <td><a href="https://edadfs.partners.extranet.microsoft.com/adfs/ls/?wa=wsignin1.0&wtrealm=https%3a%2f%2fdeveloper.xboxlive.com&wctx=rm%3d0%26id%3dpassive%26ru%3d%252fen-us%252flive%252fcertification%252frequirements%252fPages%252fTCR.aspx&wct=2019-11-20T19%3a55%3a26Z">Requisitos de Xbox para Xbox Live en Windows 10</a></td>
    </tr>
    <tr>
        <td>Ejemplos</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK">Ejemplos de código para ID@Xbox desarrolladores</a></td>
    </tr>  
    <tr>
        <td>Información general sobre el desarrollo de juegos para Xbox Live (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Developing-with-Xbox-Live-for-Windows-10">Desarrollar con Xbox Live para Windows 10</a></td>
    </tr>
    <tr>
        <td>Asociación entre plataformas (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Xbox-Live-Multiplayer-Introducing-services-for-cross-platform-matchmaking-and-gameplay">Xbox Live para varios jugadores: introducción a los servicios de asociación y juego entre plataformas</a></td>
    </tr>
    <tr>
        <td>Juegos entre dispositivos en Fable Legends (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Fable-Legends-Cross-device-Gameplay-with-Xbox-Live">Fable Legends: juegos entre dispositivos con Xbox Live</a></td>
    </tr>
    <tr>
        <td>Estadísticas y logros de Xbox Live (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Best-Practices-for-Leveraging-Cloud-Based-User-Stats-and-Achievements-in-Xbox-Live">Procedimientos recomendados para aprovechar las estadísticas y los logros del usuario basados en la nube en Xbox Live</a></td>
    </tr>
</table>

## <a name="additional-resources"></a>Recursos adicionales

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Vídeos sobre el desarrollo de juegos</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/game-development-videos">Vídeos de conferencias principales como GDC y//Build</a></td>
    </tr>
    <tr>
        <td>Desarrollo de juegos independientes (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/New-Opportunities-for-Independent-Developers">Nuevas oportunidades para los desarrolladores independientes</a></td>
    </tr>
    <tr>
        <td>Consideraciones para dispositivos móviles de varios núcleos (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/Sustained-gaming-performance-in-multi-core-mobile-devices">Rendimiento mantenido de juegos en dispositivos móviles de varios núcleos</a></td>
    </tr>
    <tr>
        <td>Desarrollar juegos de escritorio para Windows 10 (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2015/PC-Games-for-Windows-10">Juegos de PC para Windows 10</a></td>
    </tr>
</table>
