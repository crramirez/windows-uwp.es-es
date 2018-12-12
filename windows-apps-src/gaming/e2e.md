---
title: Guía de desarrollo de juegos para Windows 10
description: Una guía completa sobre los recursos y la información que necesitas para el desarrollo de juegos para la Plataforma universal de Windows (UWP).
ms.assetid: 6061F498-96A8-44EF-9711-68AE5A1218C9
ms.date: 04/16/2018
ms.topic: article
keywords: Windows 10, uwp, juegos, games, desarrollo de juegos, game development
ms.localizationpriority: medium
ms.openlocfilehash: 58044fba24450c397ee58b1034429f2af8d23ed6
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/11/2018
ms.locfileid: "8940046"
---
# <a name="windows-10-game-development-guide"></a>Guía de desarrollo de juegos para Windows 10


Esta es la guía de desarrollo de juegos para Windows 10.

En esta guía te proporcionamos una colección completa de los recursos y la información que necesitas para desarrollar un juego para la Plataforma universal de Windows (UWP). Una versión en inglés (Estados Unidos) de esta guía está disponible en formato [PDF](http://download.microsoft.com/download/9/C/9/9C9D344F-611F-412E-BB01-259E5C76B17F/Windev_Game_Dev_Guide_Oct_2017.pdf).

## <a name="introduction-to-game-development-for-the-universal-windows-platform-uwp"></a>Introducción al desarrollo de juegos para la Plataforma universal de Windows (UWP)


Al crear un juego para Windows 10, tienes la oportunidad de llegar a millones de jugadores en todo el mundo a través del teléfono, PC y Xbox One. Gracias a cosas como Xbox en Windows, Xbox Live, el modo multijugador para varios dispositivos, una increíble comunidad de aficionados a los juegos y las nuevas y eficaces funciones como la Plataforma universal de Windows (UWP) y DirectX 12, los juegos para Windows 10 emocionan a jugadores de todas las edades y géneros. La nueva Plataforma universal de Windows (UWP) ofrece compatibilidad para tu juego en todos los dispositivos con Windows 10, con una API común para teléfonos, PC y consolas Xbox One, junto con las herramientas y opciones que necesitas para adaptar tu juego a la experiencia de cada dispositivo.

En esta guía se proporciona una colección completa de información y recursos que te ayudarán a medida que desarrollas tu juego. Las secciones se organizan según las fases de desarrollo de los juegos, por lo que sabrás dónde buscar información cuando la necesites.

Si no estás familiarizado con el desarrollo de juegos en Windows o Xbox, puedes empezar por la guía de [Tareas iniciales](getting-started.md). La sección [Recursos de desarrollo de juegos](#game-development-resources) también proporciona una encuesta de alto nivel sobre la documentación, los programas y otros recursos que son de utilidad a la hora de crear un juego. Si, en lugar de eso, quieres empezar con algún código UWP, consulta [Muestras de juegos](#game-samples).

Esta guía se actualizará a medida que se hagan disponibles materiales y recursos de desarrollo de juegos para Windows 10 adicionales.

## <a name="game-development-resources"></a>Recursos de desarrollo de juegos

Desde la documentación hasta los programas, foros, blogs y muestras para desarrolladores, hay muchos recursos disponibles que te ayudarán en tu camino de desarrollo de juegos. Este es un resumen de los recursos que deberías conocer antes de comenzar a desarrollar tu juego para Windows 10.

> [!Note]
> Algunas funciones se administran a través de diversos programas. En esta guía se cubre una amplia gama de recursos, por lo que es posible que algunos recursos no sean accesibles según el programa en el que participas o tu rol de desarrollo concreto. Las muestras son vínculos que te dirigen a developer.xboxlive.com, forums.xboxlive.com, xdi.xboxlive.com o el portal Game Developer Network (GDN). Para información sobre cómo asociarte con Microsoft, consulta [Programas de desarrolladores](#developer-programs).


### <a name="game-development-documentation"></a>Documentación sobre el desarrollo de juegos

A lo largo de esta guía, encontrarás vínculos profundos a documentación importante, que estarán organizados por tarea, tecnología y fase de desarrollo de los juegos. Para ofrecerte una vista amplia de lo que tienes a tu disposición, estos son los principales portales de documentación para el desarrollo de juegos para Windows 10.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Portal principal del Centro de desarrollo de Windows</td>
        <td><a href="https://dev.windows.com">Centro de desarrollo de Windows</a></td>
    </tr>
    <tr>
        <td>Desarrollar aplicaciones de Windows</td>
        <td><a href="https://dev.windows.com/develop">Desarrollar aplicaciones de Windows</a></td>
    </tr>
    <tr>
        <td>Desarrollo de aplicaciones de la Plataforma universal de Windows</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt244352">Guías de procedimientos para aplicaciones Windows 10</a></td>
    </tr>
    <tr>
        <td>Guías de procedimientos para los juegos para UWP</td>
        <td><a href="index.md">Juegos y DirectX</a> </td>
    </tr>
    <tr>
        <td>Información general y referencias sobre DirectX</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee663274">Juegos y gráficos de DirectX</a></td>
    </tr>
    <tr>
        <td>Azure para juegos</td>
        <td><a href="https://azure.microsoft.com/solutions/gaming/">Compilar y escalar tus juegos con Azure</a></td>
    </tr>
    <tr>
        <td>PlayFab</td>
        <td><a href="https://api.playfab.com/">Soluciones backend completas para juegos dinámicos</a></td>
    </tr>
    <tr>
        <td>UWP en Xbox One</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/xbox-apps/index">Compilación de aplicaciones para UWP en Xbox One</a></td>
    </tr>
    <tr>
        <td>UWP en HoloLens</td>
        <td><a href="https://developer.microsoft.com/windows/mixed-reality/development_overview">Compilación de aplicaciones para UWP en HoloLens</a></td>
    </tr>
    <tr>
        <td>Documentación de Xbox Live</td>
        <td><a href="../xbox-live/index.md">Guía del desarrollador de Xbox Live</a></td>
    </tr>
    <tr>
        <td>Documentación para desarrollo de Xbox One (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-home">Desarrollo de Xbox One</a></td>
    </tr>
    <tr>
        <td>Notas del producto para desarrollo de Xbox One (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-whitepapers">Notas del producto</a></td>
    </tr>
    <tr>
        <td>Documentación interactiva de mezclador</td>
        <td><a href="https://dev.mixer.com/reference/interactive/index.html">Agrega interactividad a tu juego.</a></td>
    </tr>        
</table>

### <a name="partner-center"></a>Centro de partners

[Registrar una cuenta de desarrollador en el centro de partners](https://developer.microsoft.com/store/register) es el primer paso para publicar tu juego de Windows. Una cuenta de desarrollador te permite reservar el nombre de tu juego y enviar juegos gratuitos o de pago a Microsoft Store para todos los dispositivos Windows. Usa tu cuenta de desarrollador para administrar tu juego y productos desde el juego, obtener análisis detallados y habilitar servicios que crean fantásticas experiencias para tus jugadores en todo el mundo. 

Microsoft ofrece también varios programas para desarrolladores que te ayudarán a desarrollar y publicar juegos para Windows. Te recomendamos ver si alguno es adecuado para TI antes de registrarte para una cuenta del centro de partners. Para obtener más información, consulta [Programas de desarrolladores](#developer-programs)


### <a name="developer-programs"></a>Programas para desarrolladores

Microsoft ofrece varios programas para desarrollador es que te ayudarán a desarrollar y publicar juegos para Windows. Considera la posibilidad de unirte a un programa para desarrolladores, si quieres desarrollar juegos para Xbox One e integrar funciones de Xbox Live en tu juego. Para publicar un juego en Microsoft Store, también tendrás que crear una cuenta de desarrollador en [El centro de partners](https://partner.microsoft.com/dashboard) .

#### <a name="xbox-live-creators-program"></a>Programa de creadores de XboxLive

El Programa de creadores de XboxLive permite a cualquier persona integrar XboxLive en su juego y publicarlo en XboxOne y Windows10. Hay un proceso de certificación simplificado y no se requiere ninguna aprobación de concepto aparte de las [Directivas de Microsoft Store](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx) estándar.

Puedes implementar, diseñar y publicar tu juego en el Programa de creadores sin un kit de desarrollo dedicado, solo con retail hardware. Para comenzar, descarga la [aplicación Activación del modo de desarrollo](https://docs.microsoft.com/windows/uwp/xbox-apps/devkit-activation) en tu Xbox One.

Si quieres obtener acceso a todavía más funcionalidades de Xbox Live, marketing dedicado y soporte técnico de desarrollo, así como la posibilidad de aparecer destacado en la Tienda principal de Xbox One, presenta la solicitud para el programa [ID@Xbox](http://www.xbox.com/Developers/id).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Programa de creadores de XboxLive</td>
        <td><a href="https://developer.microsoft.com/games/xbox/xboxlive/creator">Más información sobre el Programa de creadores de Xbox Live</a></td>
    </tr>
</table>

#### <a name="idxbox"></a>ID@Xbox

El programa ID@Xbox ayuda a los desarrolladores de juegos cualificados a publicar por si mismos en Windows y Xbox One. Si quieres desarrollar para Xbox One o agregar funciones de Xbox Live, como puntuaciones de jugador, logros y marcadores, a tu juego para Windows 10, regístrate ahora en ID@Xbox. Conviértete en un desarrollador de ID@Xbox para obtener las herramientas y el soporte técnico necesarios para desarrollar tu creatividad y maximizar tu éxito. Te recomendamos que apliques a ID@Xbox primera antes de registrarte para una cuenta de desarrollador en el centro de partners.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Programa para desarrolladores ID@Xbox</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkID=526271">Programa para desarrolladores independientes para Xbox One</a></td>
    </tr>
    <tr>
        <td>sitio del consumidor ID@Xbox</td>
        <td><a href="http://www.idatxbox.com/">ID@Xbox</a></td>
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

Hay muchas muestras de juegos y aplicaciones para Windows 10 disponibles que te ayudarán a comprender las funciones de juegos de Windows10 y a empezar a desarrollar juegos rápidamente. Se desarrollan y publican muestras continuamente, por lo que te recomendamos que consultes ocasionalmente los portales de muestras para ver las novedades. También puedes [ver](https://help.github.com/articles/watching-repositories/) los repositorios de GitHub para recibir notificaciones de cambios y adiciones.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Muestras de aplicaciones para la Plataforma universal de Windows</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples">Muestras universales de Windows</a></td>
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
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkId=620531">D2DCustomEffects</a></td>
    </tr>
    <tr>
        <td>Muestra de malla de degradado Direct2D</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkId=620532">D2DGradientMesh</a></td>
    </tr>
    <tr>
        <td>Muestra de ajuste de fotos en Direct2D</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkId=620533">D2DPhotoAdjustment</a></td>
    </tr>
    <tr>
        <td>Muestras públicas de Xbox Advanced Technology Group</td>
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples">Xbox-ATG-Samples</a></td>
    </tr>
    <tr>
        <td>Muestras de Xbox Live.</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples">muestras de Xbox Live.</a></td>
    </tr>
    <tr>
        <td>Muestras de juego para Xbox One (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-samples">Muestras</a></td>
    </tr>
    <tr>
        <td>Muestras de juegos para Windows (galería de códigos de MSDN)</td>
        <td><a href="https://code.msdn.microsoft.com/windowsapps/site/search?f%5B0%5D.Type=SearchText&f%5B0%5D.Value=game&f%5B1%5D.Type=Contributors&f%5B1%5D.Value=Microsoft&f%5B1%5D.Text=Microsoft">Muestras de juegos de Microsoft Store</a></td>
    </tr>
    <tr>
        <td>Muestra de juego 2D en JavaScript</td>
        <td><a href="../get-started/get-started-tutorial-game-js2d.md">Crear un juego para UWP en JavaScript</a></td>
    </tr>
    <tr>
        <td>Muestra de juego 3D en JavaScript</td>
        <td><a href="../get-started/get-started-tutorial-game-js3d.md">Crear un juego en 3D en JavaScript con three.js</a></td>
    </tr>
    <tr>
        <td>Muestra de juego de UWP 2D MonoGame</td>
        <td><a href="../get-started/get-started-tutorial-game-mg2d.md">Crear un juego para UWP en MonoGame 2D</a></td>
    </tr>      
</table>


### <a name="developer-forums"></a>Foros para desarrolladores

Los foros para desarrolladores son un buen lugar para hacer y responder preguntas sobre el desarrollo de juegos y conectarse con la comunidad de desarrollo de juegos. Además, los foros pueden ser fantásticos recursos para encontrar respuestas existentes a problemas complejos a los que se hayan enfrentado los desarrolladores en el pasado, con sus soluciones correspondientes.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Foros para desarrolladores de juegos y aplicaciones de publicación</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?category=windowsapps">Publicación y anuncios en aplicaciones</a></td>
    </tr>
    <tr>
        <td>Foro para desarrolladores de aplicaciones para UWP</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?forum=wpdevelop">Desarrollo de aplicaciones de la Plataforma universal de Windows</a></td>
    </tr>
    <tr>
        <td>Foros para desarrolladores de aplicaciones de escritorio</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?category=windowsdesktopdev">Foros de aplicaciones de escritorio de Windows</a></td>
    </tr>
    <tr>
        <td>Juegos de Microsoft Store DirectX (entradas de foro archivadas)</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/vstudio/home?forum=wingameswithdirectx">Compilar juegos de Microsoft Store con DirectX (archivado)</a></td>
    </tr>
    <tr>
        <td>Foros para desarrolladores de partner administrados de Windows 10</td>
        <td><a href="http://aka.ms/win10devforums">Foros para desarrolladores de Xbox: Windows 10</a></td>
    </tr>
    <tr>
        <td>Foros de DirectX</td>
        <td><a href="http://forums.directxtech.com/index.php">Foro de DirectX 12</a></td>
    </tr>
    <tr>
        <td>Foros de plataforma de Azure</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?category=windowsazureplatform">Foro de Azure</a></td>
    </tr>
    <tr>
        <td>Foro de Xbox Live</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=xboxlivedev">Foro de desarrollo de Xbox Live</a></td>
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
        <td><a href="http://blogs.windows.com/buildingapps/">Compilación de aplicaciones para Windows</a></td>
    </tr>
    <tr>
        <td>Windows 10 (entradas de blog)</td>
        <td><a href="http://blogs.windows.com/blog/tag/windows-10/">Publicaciones en Windows 10</a></td>
    </tr>
    <tr>
        <td>Blog del equipo de ingeniería de Visual Studio</td>
        <td><a href="http://blogs.msdn.com/b/visualstudio/">El blog de Visual Studio</a></td>
    </tr>
    <tr>
        <td>Blogs sobre herramientas para desarrolladores de Visual Studio</td>
        <td><a href="http://blogs.msdn.com/b/developer-tools/">Blogs sobre herramientas para desarrolladores</a></td>
    </tr>
    <tr>
        <td>Blog sobre herramientas para desarrolladores de Somasegar</td>
        <td><a href="http://blogs.msdn.com/b/somasegar/">Blog de Somasegar</a></td>
    </tr>
    <tr>
        <td>Blog para desarrolladores de DirectX</td>
        <td><a href="http://blogs.msdn.com/b/directx">Blog para desarrolladores de DirectX</a></td>
    </tr>
    <tr>
        <td>Introducción a DirectX 12 (entrada de blog)</td>
        <td><a href="http://blogs.msdn.com/b/directx/archive/2014/03/20/directx-12.aspx">DirectX 12</a></td>
    </tr>
    <tr>
        <td>Blog del equipo de herramientas de Visual C++</td>
        <td><a href="http://blogs.msdn.com/b/vcblog/">Blog del equipo de Visual C++</a></td>
    </tr>
    <tr>
        <td>Blog del equipo PIX</td>
        <td><a href="https://blogs.msdn.microsoft.com/pix/">Ajuste del rendimiento y depuración para juegos de DirectX 12 en Windows y Xbox</a></td>
    </tr>
    <tr>
        <td>Blog del equipo de implementación de aplicaciones universales de Windows</td>
        <td><a href="https://blogs.msdn.microsoft.com/appinstaller/">Crear e implementar un blog del equipo de aplicaciones para UWP</a></td>
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
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-Games-for-Windows-10">Desarrollo de juegos para Windows 10</a></td>
    </tr>
    <tr>
        <td>Experiencia de juegos para Windows 10 (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Gaming-Consumer-Experience-on-Windows-10">Experiencia de juegos para consumidor en Windows10</a></td>
    </tr>
    <tr>
        <td>Juegos en todo el ecosistema de Microsoft (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/The-Future-of-Gaming-Across-the-Microsoft-Ecosystem">El futuro de los juegos en el ecosistema de Microsoft</a></td>
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
        <td>Haz que tu juego sea accesible</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/gaming/accessibility-for-games">Accesibilidad para juegos</a></td>
    </tr>
    <tr>
        <td>Compila juegos con la nube</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/gaming/cloud-for-games">Nube para juegos</a></td>
    </tr>
    <tr>
        <td>Rentabiliza tu juego</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/gaming/monetization-for-games">Monetización para juegos</a></td>
    </tr>
</table>



### <a name="choosing-your-graphics-technology-and-programming-language"></a>Elegir la tecnología de gráficos y lenguaje de programación

Hay varios lenguajes de programación y tecnologías de gráficos disponibles que se pueden usar en los juegos para Windows 10. El método que elijas depende del tipo de juego que estés desarrollando, la experiencia, las preferencias de tu estudio de desarrollo y los requisitos de las funciones específicas del juego. ¿Usarás C#, C++ o JavaScript? ¿DirectX, XAML o HTML5?

#### <a name="directx"></a>DirectX

Microsoft DirectX es la opción ideal para los elementos gráficos y multimedia 2D y 3D de mayor rendimiento.

DirectX 12 es más rápida y eficiente que ninguna otra versión anterior. Direct3D12 permite crear escenas más vivaces, más objetos, efectos más complejos, así como aprovechar al máximo el hardware de GPU moderno en los PC Windows 10 y en Xbox One.

Si quieres usar la conocida canalización de gráficos de Direct3D 11, también podrás beneficiarte de las nuevas funciones de optimización y representación agregadas en Direct3D 11.3. Además, si eres un desarrollador de API de Windows de escritorio demostrado con raíces de Win32, también tendrás esa opción en Windows 10.

La gran cantidad de funciones y la profunda integración de la plataforma de DirectX proporcionan la eficacia y el rendimiento necesarios para los juegos más populares.

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
        <td>Tutorial: Cómo crear un juego DirectX de UWP</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Crear un juego sencillo para UWP con DirectX</a></td>
    </tr>
    <tr>
        <td>Información general y referencia sobre DirectX</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee663274">Juegos y gráficos de DirectX</a></td>
    </tr>
    <tr>
        <td>Guía y referencia de programación en Direct3D 12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn903821">Gráficos de Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Vídeos de desarrollo de elementos gráficos y DirectX 12 (canal de YouTube)</td>
        <td><a href="https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA">Educación sobre elementos gráficos y Microsoft DirectX 12</a></td>
    </tr>
</table>
 

#### <a name="xaml"></a>XAML

XAML es un lenguaje declarativo de interfaz de usuario fácil de usar con prácticas funciones como animaciones, guiones gráficos, enlaces de datos, gráficos vectoriales escalables, cambios de tamaño dinámicos y gráficos de escena. XAML funciona a la perfección para la interfaz de usuario de juegos, los menús, los sprites y los gráficos 2D. Para facilitar la distribución de la interfaz de usuario, XAML es compatible con herramientas de desarrollo y diseño como Expression Blend y Microsoft Visual Studio. Generalmente, se usa XAML con C#, pero C++ también es una buena opción si es tu lenguaje preferido o si el juego exige un uso muy elevado de la CPU.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Información general sobre la plataforma XAML</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt228259">Plataforma XAML</a></td>
    </tr>
    <tr>
        <td>Interfaz de usuario y controles de XAML</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt228348">Controles, diseños y texto</a></td>
    </tr>
</table>
 

#### <a name="html-5"></a>HTML 5

El lenguaje de marcado de hipertexto (HTML) es un lenguaje de marcado de interfaz de usuario común que se usa para clientes enriquecidos, aplicaciones y páginas web. Los juegos de Windows pueden usar HTML5 como una capa de presentación completa con las conocidas funciones de HTML, acceso a la Plataforma universal de Windows y compatibilidad con funciones web modernas como AppCache, Web Workers, Canvas, la función de arrastrar y colocar, programación asincrónica y SVG. En segundo plano, la representación HTML aprovecha la potencia de aceleración de hardware de DirectX, para que puedas obtener los beneficios de rendimiento de DirectX sin necesidad de escribir código adicional. HTML5 es una buena opción si eres un experto en desarrollo web, migras un juego web o deseas usar capas de gráficos y lenguajes que pueden ser más fáciles de enfocar que las otras opciones. HTML5 se usa con JavaScript, pero también puede llamar a los componentes creados con C# o C++/CX.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Información sobre HTML5 y Document Object Model</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/br212882.aspx">Referencia de HTML y DOM</a></td>
    </tr>
    <tr>
        <td>Recomendación HTML5 W3C</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?linkid=221374">HTML5</a></td>
    </tr>
</table>
 

#### <a name="combining-presentation-technologies"></a>Combinar tecnologías de presentación

La Infraestructura de gráficos de DirectX (DXGI) de Microsoft proporciona interoperabilidad y compatibilidad con varias tecnologías de gráficos. Para los gráficos de alto rendimiento, puedes combinar XAML y DirectX, mediante XAML para los menús y otra interfaz de usuario simple y DirectX para la representación de escenas 2D y 3D complejas. DXGI también proporciona compatibilidad entre Direct2D, Direct3D, DirectWrite, DirectCompute y Microsoft Media Foundation.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guía de programación y referencia de la Infraestructura de gráficos de DirectX</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/hh404534">DXGI</a></td>
    </tr>
    <tr>
        <td>Combinar DirectX y XAML</td>
        <td><a href="directx-and-xaml-interop.md">Interoperabilidad de DirectX y XAML</a></td>
    </tr>
</table>
 

#### <a name="c"></a>C++

C++/CX es un lenguaje con poca sobrecarga y de alto rendimiento que proporciona una excelente combinación de velocidad, compatibilidad y plataforma de acceso. C++/CX facilita el uso de todas las excelentes funciones de juegos de Windows 10, incluidos DirectX y Xbox Live. También puedes volver a usar las bibliotecas y el código C++ existente. C++/CX crea código nativo y rápido que no produce la sobrecarga de la colección de elementos y, por lo tanto, tu juego puede ofrecer un gran rendimiento y un bajo consumo de energía, lo que permite aumentar la duración de la batería. Usa C++/CX con DirectX o XAML, o crea un juego que use una combinación de ambos.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Referencia e información general sobre C++/CX</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh699871.aspx">Referencia del lenguaje de VisualC++ (C++/CX)</a></td>
    </tr>
    <tr>
        <td>Guía de programación y referencia de Visual C++</td>
        <td><a href="https://docs.microsoft.com/cpp/visual-cpp-in-visual-studio">Visual C++ en Visual Studio 2017</a></td>
    </tr>
</table>
 

#### <a name="c"></a>C#

C# (pronunciado "si sharp") es un lenguaje moderno e innovador, además de sencillo, eficaz, con seguridad de tipos y orientado a objetos. C# permite desarrollar rápidamente mientras conserva la familiaridad y la expresividad de los lenguajes de estilo C. Aunque es fácil de usar, C# tiene muchas funciones avanzadas de lenguaje como polimorfismo, delegados, expresiones lambda, clausuras, métodos iterador, covarianza y expresiones de Language Integrated Query (LINQ). C# es una excelente opción si te interesa XAML, quieres empezar rápidamente a desarrollar tu juego o tienes experiencia previa con C#. C# se usa principalmente con XAML, así que si quieres usar DirectX, elige C++ en su lugar o escribe parte de tu juego como un componente de C++ que interactúe con DirectX. O bien, ten en cuenta [Win2D](https://github.com/Microsoft/Win2D), una biblioteca de gráficos de modo inmediato de Direct2D para C# y C++.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guía de programación y referencia de C#</td>
        <td><a href="https://msdn.microsoft.com/library/kx37x362.aspx">Referencia de lenguaje C#</a></td>
    </tr>
</table>
 

#### <a name="javascript"></a>JavaScript

JavaScript es un lenguaje de scripting dinámico que se usa mucho para aplicaciones de cliente enriquecido y web modernas.

Las aplicaciones JavaScript de Windows pueden tener acceso a las funciones eficaces de la Plataforma universal de Windows de una manera fácil e intuitiva como métodos y propiedades de clases JavaScript orientados a objetos. JavaScript es una buena opción para tu juego si has trabajado en un entorno de desarrollo web, ya estás familiarizado con JavaScript o quieres usar HTML5, CSS, WinJS o bibliotecas de JavaScript. Si te interesa XAML o DirectX, elige C# o C++/CX en su lugar.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Referencia de JavaScript y Windows Runtime</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/jj613794">Referencia de JavaScript</a></td>
    </tr>
</table>


#### <a name="use-windows-runtime-components-to-combine-languages"></a>Usar componentes de Windows Runtime para combinar lenguajes

Con la Plataforma universal de Windows, es fácil combinar componentes escritos en lenguajes diferentes. Crea componentes de Windows Runtime en C++, C# o Visual Basic y, a continuación, llámalos desde JavaScript, C#, C++ o Visual Basic. De esta manera, puedes programar partes de tu juego en el lenguaje que elijas. Los componentes también te permiten usar bibliotecas externas que solo están disponibles en un lenguaje en particular, así como usar código heredado que ya has escrito.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Cómo crear componentes de Windows Runtime</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/winrt-components/creating-windows-runtime-components-in-cpp">Crear componentes de Windows Runtime</a></td>
    </tr>
</table>


### <a name="which-version-of-directx-should-your-game-use"></a>¿Qué versión de DirectX se debe usar en tu juego?

Si eliges DirectX para tu juego, tendrás que decidir qué versión usar: Microsoft Direct3D12 o Microsoft Direct3D11.

DirectX 12 es más rápida y eficiente que ninguna otra versión anterior. Direct3D12 permite crear escenas más vivaces, más objetos, efectos más complejos, así como aprovechar al máximo el hardware de GPU moderno en los PC Windows 10 y en Xbox One. Dado que Direct3D 12 funciona a un nivel muy bajo, puede dar a un equipo de expertos en desarrollo de gráficos o a un equipo de desarrollo de DirectX 11 con experiencia todo el control que necesiten para maximizar la optimización de gráficos.

Direct3D 11.3 es una API de gráficos de bajo nivel que usa el ya familiar modelo de programación Direct3D y te ayuda a controlar aún más la complejidad que entraña el procesamiento por GPU. También se admite en Windows 10 y Xbox One. Si tienes un motor existente escrito en Direct3D 11 y no estás del todo listo para dar el salto a Direct3D 12, puedes usar Direct3D 11 en 12 para lograr algunas mejoras de rendimiento. Las versiones 11.3+ contienen las nuevas características de representación y optimización habilitadas también en Direct3D 12.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Elegir Direct3D12 o Direct3D11</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899228">¿Qué es Direct3D12?</a></td>
    </tr>
    <tr>
        <td>Introducción a Direct3D11</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ff476080">Gráficos de Direct3D 11</a></td>
    </tr>
    <tr>
        <td>Información general sobre Direct3D 11 en 12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn913195">Direct3D 11 en 12</a></td>
    </tr>
</table>


### <a name="bridges-game-engines-and-middleware"></a>Puentes, motores de juegos y software intermedio

Según las necesidades de tu juego, el uso de puentes, motores de juego o software intermedio puede ahorrar tiempo de prueba y recursos de desarrollo. Aquí te ofrecemos información general y recursos sobre puentes, motores de juego y software intermedio.

#### <a name="universal-windows-platform-bridges"></a>Puentes de plataforma universal de Windows

Los puentes de plataforma universal de Windows son tecnologías que llevan tu aplicación o juego existente a la UWP. Los puentes ofrecen una buena manera de comenzar el desarrollo de juegos para UWP.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Puentes de UWP</td>
        <td><a href="https://dev.windows.com/bridges/">Lleva el código a Windows</a></td>
    </tr>
    <tr>
        <td>Puente de Windows para iOS</td>
        <td><a href="https://dev.windows.com/bridges/ios">Migrar aplicaciones para iOS a Windows</a></td>
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
        <td><a href="https://playfab.com/">Introducción a las herramientas y los servicios</a></td>
    </tr>
    <tr>
        <td>Tareas iniciales</td>
        <td><a href="https://api.playfab.com/docs/general-getting-started">Guía de introducción general</a></td>
    </tr>
    <tr>
        <td>Serie de tutoriales de vídeo</td>
        <td><a href="https://www.youtube.com/watch?v=fGNpiqVi5xU&list=PLHCfyL7JpoPbLpA_oh_T5PKrfzPgCpPT5">Serie de vídeos de demostración sobre los sistemas de núcleos PlayFab</a></td>
    </tr>
    <tr>
        <td>Recetas</td>
        <td><a href="https://api.playfab.com/docs/tutorials/recipes-index">Mecánicas de juego y muestras de tramas de diseño populares</a></td>
    </tr>
    <tr>
        <td>Plataformas</td>
        <td><a href="https://api.playfab.com/platforms">Documentación específica para varias plataformas y motores de juegos</a></td>
    </tr>
    <tr>
        <td>Repositorio de GitHub</td>
        <td><a href="https://github.com/PlayFab">Obtener SDK y scripts para varias plataformas como Android, iOS, Windows, Unity y Unreal.</a></td>
    </tr>
    <tr>
        <td>Documentación de la API</td>
        <td><a href="https://api.playfab.com/documentation/">Acceder a servicio de PlayFab directamente a través de las API Web como REST</a></td>
    </tr>
    <tr>
        <td>Foros</td>
        <td><a href="https://community.playfab.com/index.html">Foros de PlayFab</a></td>
    </tr>
</table>
 

#### <a name="unity"></a>Unity

Unity ofrece una plataforma para crear juegos y aplicaciones 2D, 3D, VR y AR atractivos y atrayentes. Te permite hacer realidad tu visión creativa rápidamente y ofrecer tu contenido prácticamente a cualquier medio o dispositivo.

A partir de Unity 5.4, Unity es compatible con el desarrollo de Direct3D 12.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Motor de juego de Unity</td>
        <td><a href="http://unity3d.com/">Unity: motor de juego</a></td>
    </tr>
    <tr>
        <td>Obtener Unity</td>
        <td><a href="http://unity3d.com/get-unity">Obtener Unity</a></td>
    </tr>
    <tr>
        <td>Documentación de Unity para Windows</td>
        <td><a href="http://docs.unity3d.com/Manual/Windows.html">Manual de Unity/Windows</a></td>
    </tr>
    <tr>
        <td>Agregar LiveOps con PlayFab</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unity-getting-started">Tareas iniciales: crear tu primera llamada API PlayFab desde tu juego Unity</a></td>
    </tr>
    <tr>
        <td>Cómo agregar interactividad a tu juego mediante Mixer Interactive</td>
        <td><a href="https://github.com/mixer/interactive-unity-plugin/wiki/Getting-started">Guía de introducción</a></td>
    </tr>
    <tr>
        <td>SDK de Mixer para Unity</td>
        <td><a href="https://www.assetstore.unity3d.com/en/#!/content/88585">Complemento de Mixer para Unity</a></td>
    </tr>
    <tr>
        <td>SDK de Mixer para documentación de referencia de Unity</td>
        <td><a href="https://dev.mixer.com/reference/interactive/csharp/index.html">Referencia de API para complemento de Mixer Unity</a></td>
    </tr>
    <tr>
        <td>Publicar un juego de Unity en Microsoft Store</td>
        <td><a href="https://unity3d.com/partners/microsoft/porting-guides">Guía de migración</a></td>
    </tr>
    <tr>
        <td>Solución de problemas cuando faltan referencias de ensamblado relacionadas con API .NET</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/missing-dot-net-apis-in-unity-and-uwp">API de .NET que faltan en Unity y UWP</a></td>
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
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=722359">Usar Unity con Visual Studio 2015</a></td>
    </tr>
</table>
 

#### <a name="havok"></a>Havok

El conjunto modular de herramientas y tecnologías de Havok ayuda a los creadores de juegos llegar a nuevos niveles de interactividad e inmersión. Havok permite una física altamente realista, simulaciones interactivas y secuencias cinematográficas sorprendentes. La versión 2015.1 y posteriores admiten oficialmente UWP en Visual Studio 2015 en x86, 64 bits y ARM.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Sitio web de Havok</td>
        <td><a href="http://www.havok.com/">Havok</a></td>
    </tr>
    <tr>
        <td>Conjunto de herramientas de Havok</td>
        <td><a href="http://www.havok.com/products/">Información general sobre el producto de Havok</a></td>
    </tr>
    <tr>
        <td>Foros de soporte técnico de Havok</td>
        <td><a href="http://support.havok.com">Havok</a></td>
    </tr>
</table>
 

#### <a name="monogame"></a>MonoGame

MonoGame es un marco de trabajo de código abierto para el desarrollo de juegos multiplataforma originalmente basado en XNA Framework 4.0 de Microsoft. MonoGame es compatible actualmente con Windows, Windows Phone y Xbox, así como con Linux, macOS, iOS, Android y otras plataformas.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>MonoGame</td>
        <td><a href="http://www.monogame.net">Sitio web de MonoGame</a></td>
    </tr>
    <tr>
        <td>Documentación de MonoGame</td>
        <td><a href="http://www.monogame.net/documentation/">Documentación de MonoGame (más reciente)</a></td>
    </tr>
    <tr>
        <td>Descargas de MonoGame</td>
        <td><a href="http://www.monogame.net/downloads/">Descarga versiones, compilaciones de desarrollo y el código fuente</a> desde el sitio web de MonoGame u <a href="https://www.nuget.org/profiles/MonoGame">obtén la versión más reciente a través de NuGet</a>.
    </tr>
    <tr>
        <td>Muestra de juego de UWP 2D MonoGame</td>
        <td><a href="../get-started/get-started-tutorial-game-mg2d.md">Crear un juego para UWP en MonoGame 2D</a></td>
    </tr>    
</table>


#### <a name="cocos2d"></a>Cocos2d

Cocos2d-x es un motor y conjunto de herramientas de desarrollo de juegos de código abierto y multiplataforma que admite la creación de juegos para UWP. A partir de la versión 3, también se agregan funciones 3D.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Cocos2d-x</td>
        <td><a href="http://www.cocos2d-x.org/">¿Qué es Cocos2d-x?</a></td>
    </tr>
    <tr>
        <td>Guía del programador de Cocos2d-x</td>
        <td><a href="http://www.cocos2d-x.org/programmersguide/">Guía del programador de Cocos2d-x</a></td>
    </tr>
    <tr>
        <td>Cocos2d-x en Windows10 (entrada de blog)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/06/15/running-cocos2d-x-on-windows-10/">Ejecutar Cocos2d-x en Windows10</a></td>
    </tr>
    <tr>
        <td>Agregar LiveOps con PlayFab</td>
        <td><a href="https://api.playfab.com/docs/getting-started/cocos2d-x-getting-started-guide">Tareas iniciales: crear tu primera llamada API PlayFab desde tu juego Cocos2d</a></td>
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
        <td>Agregar LiveOps con PlayFab - C++</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-cpp-getting-started">Tareas iniciales: crear tu primera llamada API PlayFab desde tu juego Unreal</a></td>
    </tr>
    <tr>
        <td>Agregar LiveOps con PlayFab - Blueprints</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-blueprints-getting-started">Tareas iniciales: crear tu primera llamada API PlayFab desde tu juego Unreal</a></td>
    </tr>
</table>

#### <a name="babylonjs"></a>BabylonJS

BabylonJS es un marco de JavaScript completo para compilar juegos en 3D con HTML5, WebGL, WebVR y Web Audio.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>BabylonJS</td>
        <td><a href="http://www.babylonjs.com/">BabylonJS</a></td>
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
        <td>Migrar una aplicación para Windows8 a una aplicación para la Plataforma universal de Windows</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt238322">Mover de Windows Runtime8.x a UWP</a></td>
    </tr>
    <tr>
        <td>Portar una aplicación para Windows8 a una aplicación de la Plataforma universal de Windows (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/21">Migrar aplicaciones de 8.1 a Windows10</a></td>
    </tr>
    <tr>
        <td>Portar una aplicación de iOS a una aplicación de la Plataforma universal de Windows</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt238320">Migrar de iOS a UWP</a></td>
    </tr>
    <tr>
        <td>Portar una aplicación Silverlight a una aplicación de la Plataforma universal de Windows</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt238323">Migrar de WindowsPhone Silverlight a UWP</a></td>
    </tr>
    <tr>
        <td>Portar de XAML o Silverlight a una aplicación de la Plataforma universal de Windows (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2015/3-741">Migrar una aplicación de Silverlight o XAML a Windows10</a></td>
    </tr>
    <tr>
        <td>Portar un juego de Xbox a una aplicación de la Plataforma universal de Windows</td>
        <td><a href="https://developer.xboxlive.com/en-us/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx">Migrar de Xbox One a UWP de Windows 10</a></td>
    </tr>
    <tr>
        <td>Migrar de DirectX 9 a DirectX 11</td>
        <td><a href="porting-your-directx-9-game-to-windows-store.md">Migrar de DirectX9 a la Plataforma universal de Windows (UWP)</a></td>
    </tr>
    <tr>
        <td>Migrar de Direct3D 11 a Direct3D 12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt431709">Migrar de Direct3D 11 a Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Migrar de OpenGL ES a Direct3D 11</td>
        <td><a href="port-from-opengl-es-2-0-to-directx-11-1.md">Migrar de OpenGL ES 2.0 a Direct3D 11</a></td>
    </tr>
    <tr>
        <td>De OpenGL ES a Direct3D 11 mediante ANGLE</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?linkid=618387">ANGLE</a></td>
    </tr>
    <tr>
        <td>Equivalentes de API clásica de Windows en la UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh464945">Alternativas a las API de Windows en las aplicaciones de la Plataforma universal de Windows (UWP)</a></td>
    </tr>
</table>


## <a name="prototype-and-design"></a>Prototipo y diseño


Ahora que decidiste el tipo de juego que quieres crear y las herramientas y tecnología de gráficos que usarás para hacerlo, estás listo para empezar a trabajar en el diseño y prototipo. En esencia, tu juego es una aplicación de la Plataforma universal de Windows, por lo que comenzarás por allí.

### <a name="introduction-to-the-universal-windows-platform-uwp"></a>Introducción a la Plataforma universal de Windows (UWP)

Windows 10 presenta la Plataforma universal de Windows (UWP), que proporciona una plataforma común de API entre todos los dispositivos Windows10. UWP evoluciona y expande el modelo de Windows Runtime y lo perfecciona para obtener una base cohesiva y unificada. Los juegos destinados a la UWP pueden llamar a las API de WinRT que son comunes para todos los dispositivos. Debido a que la UWP proporciona capas API garantizadas, tienes la opción de crear un único paquete de la aplicación que se instalará en todos los dispositivos Windows10. Además, si quieres, tu juego aún puede llamar a las API (por ejemplo, algunas API clásicas de Windows de Win32 y. NET) que son específicas de los dispositivos en los que se ejecuta tu juego.

Esta es una lista de guías de gran calidad en las que se describen detalladamente las aplicaciones de la Plataforma universal de Windows. Se recomienda que las leas para poder comprender la plataforma.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Introducción a las aplicaciones de la Plataforma universal de Windows</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn726767">¿Qué es una aplicación para Plataforma universal de Windows?</a></td>
    </tr>
    <tr>
        <td>Información general sobre la UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn894631">Guía de aplicaciones para UWP</a></td>
    </tr>
</table>
 

### <a name="getting-started-with-uwp-development"></a>Introducción al desarrollo para UWP

La preparación para desarrollar una aplicación de la Plataforma universal de Windows es rápida y sencilla. En las guías siguientes se proporcionan los procedimientos paso a paso.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Introducción al desarrollo para UWP</td>
        <td><a href="https://dev.windows.com/getstarted">Introducción a las aplicaciones de Windows</a></td>
    </tr>
    <tr>
        <td>Preparación para el desarrollo en la UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn726766">Preparación</a></td>
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
        <td><a href="http://blogs.windows.com/buildingapps/2015/09/30/windows-10-development-for-absolute-beginners/">Desarrollo de Windows 10 para principiantes</a></td>
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
        <td><a href="https://dev.windows.com/develop">Desarrollar aplicaciones de Windows</a></td>
    </tr>
    <tr>
        <td>Información general sobre la programación de red en la UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt280378">Redes y servicios web</a></td>
    </tr>
    <tr>
        <td>Usar Windows.Web.HTTP y Windows.Networking.Sockets en los juegos</td>
        <td><a href="work-with-networking-in-your-directx-game.md">Conexión en red de juegos</a></td>
    </tr>
    <tr>
        <td>Conceptos de programación asincrónica en la UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt187335">Programación asincrónica</a></td>
    </tr>
</table>

### <a name="windows-desktop-apisto-uwp"></a>APIsto de escritorio de Windows UWP

Estos son algunos vínculos que te ayudarán a pasar tu juego de escritorio de Windows a UWP.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Usar código C++ existente para el desarrollo de juegos de UWP</td>
        <td><a href="https://docs.microsoft.com/cpp/porting/how-to-use-existing-cpp-code-in-a-universal-windows-platform-app">Procedimiento: usar código C++ existente en una aplicación para UWP</a></td>
    </tr>
    <tr>
        <td>API de UWP para las API de Win32 y COM</td>
        <td><a href="https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps">API de Win32 y COM para las aplicaciones para UWP</a></td>
    </tr>
    <tr>
        <td>Funciones de CRT no admitidas en UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/jj606124.aspx">Funciones de CRT no admitidas en las aplicaciones de la Plataforma universal de Windows</a></td>
    </tr>
    <tr>
        <td>Alternativas a las API de Windows</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt592894.aspx">Alternativas a las API de Windows en las aplicaciones de la Plataforma universal de Windows (UWP)</a></td>
    </tr>
</table>
 

### <a name="process-lifetime-management"></a>Administración del ciclo de vida de los procesos

En la Administración del ciclo de vida de los procesos, o ciclo de vida de la aplicación, se describen los diferentes estados de activación por los que puede pasar una aplicación de la Plataforma universal de Windows. El juego se puede activar, suspender, reanudar o finalizar y puede pasar por esos estados en una variedad de formas.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Controlar las transiciones de ciclo de vida de la aplicación</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt243287">Ciclo de vida de la aplicación</a></td>
    </tr>
    <tr>
        <td>Usar Microsoft Visual Studio para desencadenar las transiciones de la aplicación</td>
        <td><a href="https://msdn.microsoft.com/library/hh974425.aspx">Cómo desencadenar la suspensión, reanudación y los eventos en segundo plano para aplicaciones para UWP en Visual Studio</a></td>
    </tr>
</table>
 

### <a name="designing-game-ux"></a>Diseñar la experiencia de usuario del juego

El rigen de un gran juego es un diseño inspirado.

Los juegos comparten algunos principios de diseño y elementos de interfaz de usuario comunes con las aplicaciones, pero a menudo cuentan con un aspecto, sensación y objetivo de diseño únicos en la experiencia de usuario. Los juegos tienen éxito cuando se aplica un diseño detallista en dos aspectos, por lo que debes preguntarte: ¿cuándo debería usar el juego una experiencia de usuario probada y cuándo debería ser esta diferente e innovadora? La tecnología de presentación que elijas para tu juego (DirectX, XAML, HTML5, o alguna combinación de las tres), influirá en los detalles de implementación, pero los principios de diseño que apliques serán totalmente independientes de esa opción.

Aparte del diseño de la experiencia de usuario, el diseño mismo del juego (como el diseño del nivel, el ritmo, el diseño del mundo y otros aspectos) ya es por sí solo una forma de arte; una que depende exclusivamente de ti y tu equipo y que no se tratará en esta guía de desarrollo.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Conceptos básicos y directrices del diseño para UWP</td>
        <td><a href="https://dev.windows.com/design">Diseñar aplicaciones para UWP</a></td>
    </tr>
    <tr>
        <td>Diseñar para los estados de ciclo de vida de la aplicación</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn611862">Directrices de experiencia del usuario para iniciar, suspender y reanudar</a></td>
    </tr>
    <tr>
        <td>Diseñar tu aplicación para UWP para Xbox One y pantallas de televisión</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv">Diseño para Xbox y televisión</a></td>
    </tr>
    <tr>
        <td>Dirigirse a múltiples factores de forma de dispositivos (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Designing-Games-for-a-Windows-Core-World">Diseñar juegos para un mundo de Windows Core</a></td>
    </tr>   
</table>
 

#### <a name="color-guideline-and-palette"></a>Paleta y pauta de colores

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
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=535007">Procedimientos recomendados: tipografía</a></td>
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
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=535008">Procedimientos recomendados: mapa de la interfaz de usuario</a></td>
    </tr>
</table>

### <a name="game-audio"></a>Audio de juegos

Guías y referencias para la implementación de audio en los juegos con XAudio2, XAPO y Windows Sonic. XAudio2 es una API de audio de bajo nivel que proporciona procesamiento de señal y base de mezclado para desarrollar motores de audio de alto rendimiento. API de XAPO permite la creación de objetos de procesamiento de audio multiplataforma (XAPO) para su uso en XAudio2, tanto en Windows como en Xbox. La compatibilidad de audio de Windows Sonic te permite agregar Dolby Atmos for Home Theater, Dolby Atmos for Headphones y soporte Windows HRTF a tu juego o aplicación multimedia en transmisión por secuencias.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>API de XAudio2</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/hh405049.aspx">Guía de programación y referencia de API para XAudio2</a></td>
    </tr>
    <tr>
        <td>Crear objetos de procesamiento de audio multiplataforma</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee415735.aspx">Información general de XAPO</a></td>
    </tr>
    <tr>
        <td>Introducción a los conceptos de audio</td>
        <td><a href="working-with-audio-in-your-directx-game.md">Audio para juegos</a></td>
    </tr>
    <tr>
        <td>Introducción a Windows Sonic</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt807491.aspx">Sonido espacial</a></td>
    </tr>
    <tr>
        <td>Muestras de sonido espacial de Windows Sonic</td>
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/Audio">Muestras de audio del Xbox Advanced Technology Group</a></td>
    </tr>
    <tr>
        <td>Obtén información sobre cómo integrar Windows Sonic en tus juegos (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-002">Introducción a las funciones de Audio espacial para Xbox de red</a></td>
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
        <td>Tutorial: Cómo crear un juego DirectX de UWP</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Crear un juego sencillo para UWP con DirectX</a></td>
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
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee663274">Juegos y gráficos de DirectX</a></td>
    </tr>
    <tr>
        <td>Guía y referencia de programación en Direct3D 12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn903821">Gráficos de Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Conceptos básicos de DirectX 12 (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Better-Power-Better-Performance-Your-Game-on-DirectX12">Mejor alimentación, mejor rendimiento: tu juego en DirectX 12</a></td>
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
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899120.aspx">Configuración del entorno de programación de Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Cómo crear un componente básico</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn859356.aspx">Crear un componente básico de Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Cambios en Direct3D 12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899194.aspx">Cambios importantes al migrar de Direct3D 11 a Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Cómo portar de Direct3D 11 a Direct3D 12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt431709.aspx">Portar de Direct3D 11 a Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Conceptos de enlace de recursos (descriptor de cobertura, tabla de descriptor, montón de descriptor y firma raíz) </td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899206.aspx">Enlace de recursos en Direct3D12</a></td>
    </tr>
    <tr>
        <td>Administración de memoria</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899198.aspx">Administración de la memoria en Direct3D12</a></td>
    </tr>
</table>
 

#### <a name="directx-tool-kit-and-libraries"></a>Kit de herramientas de DirectX y bibliotecas

El kit de herramientas de DirectX, la biblioteca de procesamiento de texturas de DirectX, la biblioteca de procesamiento de geometría DirectXMesh, la biblioteca de UVAtlas y la biblioteca de DirectXMath proporcionan texturas, mallas, sprite y otras clases auxiliares y funciones de utilidad para el desarrollo en DirectX. Estas bibliotecas pueden ayudarte a ahorrar tiempo y esfuerzo en el desarrollo.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Obtén el kit de herramientas de DirectX 11.</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=248929">DirectXTK</a></td>
    </tr>
    <tr>
        <td>Obtén el kit de herramientas de DirectX 12.</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=615561">DirectXTK 12</a></td>
    </tr>
    <tr>
        <td>Obtén la biblioteca de procesamiento de texturas de DirectX.</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=248926">DirectXTex</a></td>
    </tr>
    <tr>
        <td>Obtén la biblioteca de procesamiento de geometría de DirectXMesh.</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=324981">DirectXMesh</a></td>
    </tr>
    <tr>
        <td>Obtén UVAtlas para crear y empaquetar atlas de textura de isochart</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=512686">UVAtlas</a></td>
    </tr>
    <tr>
        <td>Obtén la biblioteca de DirectXMath</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=615560">DirectXMath</a></td>
    </tr>
    <tr>
        <td>Soporte técnico de Direct3D12 en DirectXTK (entrada de blog)</td>
        <td><a href="https://github.com/Microsoft/DirectXTK/issues/2">Compatibilidad con DirectX12</a></td>
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
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt185606">Iconos, distintivos y notificaciones</a></td>
    </tr>
    <tr>
        <td>Muestra que ilustra iconos dinámicos y notificaciones</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications">Muestra de notificaciones</a></td>
    </tr>
    <tr>
        <td>Plantillas de icono adaptables (entrada de blog)</td>
        <td><a href="http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/06/30/adaptive-tile-templates-schema-and-documentation.aspx">Plantillas de icono adaptables: esquema y documentación</a></td>
    </tr>
    <tr>
        <td>Diseñar iconos y distintivos</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh465403">Directrices sobre iconos y distintivos</a></td>
    </tr>
    <tr>
        <td>Aplicación de Windows10 para desarrollar interactivamente plantillas de icono dinámico</td>
        <td><a href="https://www.microsoft.com/store/apps/9nblggh5xsl1">Visualizador de notificaciones</a></td>
    </tr>
    <tr>
        <td>Extensión del generador de iconos de UWP para Visual Studio</td>
        <td><a href="https://visualstudiogallery.msdn.microsoft.com/09611e90-f3e8-44b7-9c83-18dba8275bb2">Herramienta para crear todos los iconos con única imagen</a></td>
    </tr>
    <tr>
        <td>Extensión del generador de iconos para UWP para Visual Studio (entrada de blog)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2016/02/15/uwp-tile-generator-extension-for-visual-studio/">Sugerencias sobre el uso de la herramienta de generador de iconos para UWP</a></td>
    </tr>
</table>
 

### <a name="enable-in-app-product-add-on-purchases"></a>Habilitar compras de productos de la aplicación (complemento)

Un complemento (producto de la aplicación) es un elemento complementario que los jugadores pueden comprar en el juego. Complementos puede niveles del juego, artículos o cualquier otra cosa que los jugadores podrían disfrutar. Cuando se usan correctamente, complementos pueden proporcionar ingresos mientras mejora la experiencia de juego. Definir y publicar los complementos de tu juego a través del centro de partners y habilitar las compras desde la aplicación en el código del juego.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Complementos duraderos</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt219684">Habilitar compras de productos desde la aplicación</a></td>
    </tr>
    <tr>
        <td>Complementos consumibles</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt219683">Habilitar compras de productos consumibles desde la aplicación</a></td>
    </tr>
    <tr>
        <td>Envío y detalles de complementos</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148551">Envíos de complementos</a></td>
    </tr>
    <tr>
        <td>Supervisar las ventas de complementos y los datos demográficos para tu juego</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148538">Informe de adquisiciones de complementos</a></td>
    </tr>
</table>
 

### <a name="debugging-performance-optimization-and-monitoring"></a>Depuración, optimización de rendimiento y monitorización

Para optimizar el rendimiento, aprovecha el modo de juego en Windows 10 para proporcionar a los jugadores la mejor experiencia de juego posible mediante un uso completo la capacidad de su hardware actual.

Windows Performance Toolkit (WPT) se compone de herramientas de supervisión del rendimiento que producen perfiles detallados del rendimiento de aplicaciones y sistemas operativos Windows. Esto es sumamente útil para supervisar el uso de memoria y mejorar el rendimiento de juegos. Windows Performance Toolkit se incluye en el SDK de Windows 10 y Windows ADK. Este kit de herramientas consta de dos herramientas independientes: Windows Performance Recorder (WPR) y Windows Performance Analyzer (WPA). ProcDump, que forma parte de [Windows Sysinternals](https://technet.microsoft.com/sysinternals/default), es una utilidad de línea de comandos que supervisa los picos de la CPU y genera archivos de volcado de memoria durante los bloqueos de juegos. 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Prueba de rendimiento de código</td>
        <td><a href="https://www.visualstudio.com/team-services/cloud-load-testing/">Prueba de carga en la nube</a></td>
    </tr>
    <tr>
        <td>Obtener el tipo de consola Xbox con la información de dispositivo de juegos</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt825235">Información del dispositivo de juegos</a></td>
    </tr>
    <tr>
        <td>Mejorar el rendimiento obteniendo acceso exclusivo o acceso prioritario a los recursos de hardware, mediante las API de modo de juego</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt808808">Modo de juego</a></td>
    </tr>
    <tr>
        <td>Obtener Windows Performance Toolkit (WPT) del SDK de Windows10</td>
        <td><a href="https://developer.microsoft.com/windows/downloads/windows-10-sdk">SDK de Windows10</a></td>
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
        <td><a href="https://blogs.msdn.microsoft.com/pix/2017/01/17/introducing-pix-on-windows-beta/">Ajuste del rendimiento y herramienta de depuración para DirectX 12 en Windows</a></td>
    </tr>
    <tr>
        <td>Herramientas de depuración y validación para el desarrollo de D3D12 (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-003">Ajuste del rendimiento D3D12 y depuración con PIX y GPUValidation</a></td>
    </tr>
    <tr>
        <td>Optimización de gráficos y rendimiento (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Advanced-DirectX12-Graphics-and-Performance">Rendimiento y elementos gráficos avanzados de DirectX 12</a></td>
    </tr>
    <tr>
        <td>Depuración de elementos gráficos en DirectX (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Solve-the-Tough-Graphics-Problems-with-your-Game-Using-DirectX-Tools">Solucionar problemas difíciles relacionados con los elementos gráficos de tu juego mediante herramientas de DirectX</a></td>
    </tr>
    <tr>
        <td>Herramientas de Visual Studio 2015 para depurar DirectX 12 (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Series/ConnectOn-Demand/212">Herramientas de DirectX para Windows 10 en Visual Studio 2015</a></td>
    </tr>
    <tr>
        <td>Guía de programación de Direct3D 12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn903821">Guía de programación de Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Combinar DirectX y XAML</td>
        <td><a href="directx-and-xaml-interop.md">Interoperabilidad de DirectX y XAML</a></td>
    </tr>
</table>

### <a name="high-dynamic-range-hdr-content-development"></a>Desarrollo de contenido de alto rango dinámico (HDR)

Crear el contenido del juego que usa las capacidades de color completo del HDR.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Introducción a los conceptos de HDR y color (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2017/P4061">Activación de HDR y color avanzado en DirectX</a></td>
    </tr>
    <tr>
        <td>Aprende a representar contenido HDR y detectar si la pantalla actual es compatible</td>
        <td><a href="https://github.com/Microsoft/DirectX-Graphics-Samples/tree/master/Samples/UWP/D3D12HDR">Ejemplo de HDR</a></td>
    </tr>
    <tr>
        <td>Crear y configurar un color avanzado con DirectX</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DAdvancedColorImages">Ejemplo de representación de imagen en color avanzado Direct2D</a></td>
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
        <td><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt186453.aspx">Directrices de desarrollo para un público global</a></td>
    </tr>
    <tr>
        <td>Puente entre idiomas, culturas y tecnologías</td>
        <td><a href="http://www.microsoft.com/Language/Default.aspx">Recurso en línea para las convenciones de idioma y la terminología estándar de Microsoft</a></td>
    </tr>
</table>

### <a name="security"></a>Seguridad

Crear un entorno donde los jugadores pueden jugar y competir de forma justa. Un juego inscrito en TruePlay se ejecuta en un proceso protegido, que mitiga una clase de ataques comunes. El sistema de supervisión del juego también ayuda a identificar situaciones de trampas comunes. 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Herramientas para combatir las trampas dentro de los juegos de PC</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt808781">TruePlay</a></td>
    </tr>
</table>

## <a name="submitting-and-publishing-your-game"></a>Enviar y publicar juegos

Las siguientes guías e información ayudan a agilizar lo más posible el proceso de publicación y envío.

### <a name="publishing"></a>Publicación

Usarás [El centro de partners](https://partner.microsoft.com/dashboard) para publicar y administrar los paquetes de juego.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Publicación de aplicaciones del centro de partners</td>
        <td><a href="https://dev.windows.com/publish">Publicar aplicaciones de Windows</a></td>
    </tr>
    <tr>
        <td>El centro de partners publicación avanzada (GDN)</td>
        <td><a href="https://developer.xboxlive.com/en-us/windows/documentation/Pages/home.aspx">Guía de publicación avanzada de centro de partners</a></td>
    </tr>
    <tr>
        <td>Usar Azure Active Directory (AAD) para agregar usuarios a tu cuenta del centro de partners</td>
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
        <td>Aprende a usar la instalación de transmisión por secuencias y paquetes opcionales (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2017/B8093">Distribución de aplicaciones de UWP Nextgen: creación de componentizedapps extensible, ampliables,</a></td>
    </tr>
    <tr>
        <td>Dividir y agrupar contenido para habilitar la transmisión por secuencias</td>
        <td><a href="../packaging/streaming-install.md">Instalación en streaming de aplicaciones para UWP</a></td>
    </tr>
    <tr>
        <td>Crear paquetes opcionales como el contenido del juego DLC</td>
        <td><a href="../packaging/optional-packages.md">Paquetes opcionales y creación de conjuntos relacionados</a></td>
    </tr>
    <tr>
        <td>Empaquetar el juego UWP</td>
        <td><a href="../packaging/index.md">Empaquetado de aplicaciones</a></td>
    </tr>
    <tr>
        <td>Empaquetar tu juego DirectX para UWP</td>
        <td><a href="package-your-windows-store-directx-game.md">Empaquetar tu juego DirectX para UWP</a></td>
    </tr>
    <tr>
        <td>Empaquetar el juego como desarrollador de terceros (entrada de blog)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/">Crear paquetes que se puedan cargar sin acceso a la cuenta de la Tienda del publicador</a></td>
    </tr>
    <tr>
        <td>Creación paquetes de aplicaciones y lotes de paquetes de aplicaciones con MakeAppx</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool">Crear paquetes mediante la herramienta de empaquetador de aplicaciones MakeAppx.exe</a></td>
    </tr>
    <tr>
        <td>Firma digital de los archivos con SignTool</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/aa387764">Firma archivos y comprueba las firmas en los archivos con SignTool.</a></td>
    </tr>    
    <tr>
        <td>Carga y control de versiones del juego</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148542">Cargar paquetes de aplicación</a></td>
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
        <td>Contrato de desarrollo de aplicaciones en Microsoft Store</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh694058">Acuerdo para desarrolladores de aplicaciones</a></td>
    </tr>
    <tr>
        <td>Directivas para publicar aplicaciones en Microsoft Store</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn764944">Directivas de Microsoft Store</a></td>
    </tr>
    <tr>
        <td>Cómo evitar algunos problemas de certificación comunes en las aplicaciones</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/jj657968">Evitar errores de certificación comunes</a></td>
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
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt617335">Esquema StoreManifest (Windows 10)</a></td>
    </tr>
</table>
 

## <a name="game-lifecycle-management"></a>Administración del ciclo de vida del juego


Después haber terminado el desarrollo y enviado tu juego, todavía no has terminado. Quizás hayas terminado el desarrollo de la versión uno, pero el camino de tu juego en el mercado tan solo acaba de comenzar. Es recomendable supervisar el uso y los informe de errores, responder a los comentarios de los usuarios y publicar actualizaciones para tu juego.

### <a name="partner-center-analytics-and-promotion"></a>Promoción y análisis del centro de partners

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Análisis del centro de partners</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148522">Analizar el rendimiento de las aplicaciones</a></td>
    </tr>
    <tr>
        <td>Obtén información sobre el uso que hacen tus clientes de las características Xbox en tu juego</td>
        <td><a href="../publish/xbox-analytics-report.md">Informe de análisis de Xbox</a></td>
    </tr>
    <tr>
        <td>Responder a las valoraciones de los clientes</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148546">Responder a las opiniones de los clientes</a></td>
    </tr>
    <tr>
        <td>Formas para promover tu juego</td>
        <td><a href="https://dev.windows.com/store-promotion">Promocionar tus aplicaciones</a></td>
    </tr>
</table>
 

### <a name="visual-studio-application-insights"></a>Visual Studio Application Insights

Visual Studio Application Insights proporciona análisis de rendimiento, telemetría y uso para tu juego publicado. Application Insights te ayuda a detectar y solucionar problemas después del lanzamiento de tu juego, supervisar y mejorar el uso continuamente, así como comprender la forma en que los jugadores interactúan con el juego. Application Insights funciona al agregar un SDK a tu aplicación, que envía datos de telemetría al [portal de Azure](http://portal.azure.com/).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Análisis de rendimiento y uso de la aplicación</td>
        <td><a href="https://azure.microsoft.com/documentation/articles/app-insights-get-started/">Visual Studio Application Insights</a></td>
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
        <td>Comprensión del comportamiento de los jugadores con GameAnalytics</td>
        <td><a href="http://www.gameanalytics.com/">GameAnalytics</a></td>
    </tr>
    <tr>
        <td>Conectar tu juego para UWP con Google Analytics</td>
        <td><a href="https://github.com/dotnet/windows-sdk-for-google-analytics">Obtener Windows SDK para Google Analytics</a></td>
    </tr>
    <tr>
        <td>Aprender a usar Windows SDK para Google Analytics (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-the-Windows-SDK-for-Google-Analytics">Introducción a Windows SDK para Google Analytics</a></td>
    </tr>    
    <tr>
        <td>Usar anuncios de instalaciones de aplicaciones de Facebook para promocionar tu juego a los usuarios de Facebook</td>
        <td><a href="https://github.com/Microsoft/winsdkfb">Obtener Windows SDK para Facebook</a></td>
    </tr>
    <tr>
        <td>Obtén información sobre cómo usar los anuncios de instalaciones de aplicaciones de Facebook (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-Facebook-App-Install-Ads">Introducción a Windows SDK para Facebook</a></td>
    </tr>
    <tr>
        <td>Usar Vungle para agregar anuncios de vídeo en tus juegos</td>
        <td><a href="https://v.vungle.com/sdk">Obtener el Windows SDK para Vungle</a></td>
    </tr>
</table>
 

### <a name="creating-and-managing-content-updates"></a>Crear y administrar actualizaciones de contenidos

Para actualizar tu juego publicado, envía un nuevo paquete de la aplicación con un número de versión superior. Después de que el paquete pase por el proceso de envío y certificación, estará disponible automáticamente para los clientes como una actualización.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Actualizar y controlar las versiones de tu juego</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt188602">Numeración de la versión del paquete</a></td>
    </tr>
    <tr>
        <td>Instrucciones para administrar paquetes de juego</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt188602">Instrucciones para administrar paquetes de la aplicación</a></td>
    </tr>
</table>


## <a name="adding-xbox-live-to-your-game"></a>Agregar Xbox Live a tu juego

Xbox Live es una red de juegos de calidad que conecta a millones de jugadores en todo el mundo. Los desarrolladores obtienen acceso a características de Xbox Live que pueden hacer crecer orgánicamente el número de jugadores, incluyendo la presencia en Xbox Live, los marcadores, el almacenamiento en la nube, el hub de juegos, los clubs, las charlas de grupo, Game DVR y mucho más.

> [!Note]
> Si quieres desarrollar juegos habilitados para Xbox Live, dispones de varias opciones. Para obtener información sobre los distintos programas, consulta [Información general del programa de desarrolladores](../xbox-live/developer-program-overview.md).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Introducción a Xbox Live</td>
        <td><a href="../xbox-live/index.md">Guía del desarrollador de Xbox Live</a></td>
    </tr>
    <tr>
        <td>Conocer qué funciones están disponibles dependiendo del programa</td>
        <td><a href="../xbox-live/developer-program-overview.md#feature-table">Información general del programa de desarrolladores: tabla de funciones</a></td>
    </tr>
    <tr>
        <td>Vínculos a recursos útiles para desarrollar juegos de Xbox Live</td>
        <td><a href="../xbox-live/xbox-live-resources.md">Recursos de Xbox Live.</a></td>
    </tr>
    <tr>
        <td>Obtén información sobre cómo agregar información desde los servicios de Xbox Live</td>
        <td><a href="../xbox-live/introduction-to-xbox-live-apis.md">Introducción a las API de Xbox Live</a></td>
    </tr>
</table>


### <a name="for-developers-in-the-xbox-live-creators-program"></a>Para desarrolladores del Programa de creadores de Xbox Live

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Introducción</td>
        <td><a href="../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md">Introducción al Programa de creadores de Xbox Live</a></td>
    </tr>
    <tr>
        <td>Agregar Xbox Live a tu juego</td>
        <td><a href="../xbox-live/get-started-with-creators/creators-step-by-step-guide.md">Guía paso a paso para integrar el Programa de creadores de Xbox Live</a></td>
    </tr>
    <tr>
        <td>Agregar Xbox Live a tu juego para UWP creado con Unity</td>
        <td><a href="../xbox-live/get-started-with-creators/develop-creators-title-with-unity.md">Introducción al desarrollo de un título del Programa de creadores de Xbox Live con el motor de juego de Unity</a></td>
    </tr>
    <tr>
        <td>Configurar tu espacio aislado de desarrollo</td>
        <td><a href="../xbox-live/get-started-with-creators/xbox-live-sandboxes-creators.md">Introducción a los espacios aislados de Xbox Live</a></td>
    </tr>
    <tr>
        <td>Configurar cuentas para las pruebas</td>
        <td><a href="../xbox-live/get-started-with-creators/authorize-xbox-live-accounts.md">Autorizar a cuentas de Xbox Live en el entorno de pruebas</a></td>
    </tr>
    <tr>
        <td>Muestras para el Programa de creadores de Xbox Live</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/CreatorsSDK">Muestras de código para desarrolladores del Programa de creadores</a></td>
    </tr>
    <tr>
        <td>Más información sobre la integración de experiencias de Xbox Live multiplataforma en juegos UWP (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-005">Programa de creadores de XboxLive</a></td>
    </tr>  
</table>

### <a name="for-managed-partners-and-developers-in-the-idxbox-program"></a>Para asociados administrados y desarrolladores del programa ID@Xbox

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Introducción</td>
        <td><a href="../xbox-live/get-started-with-partner/get-started-with-xbox-live-partner.md">Introducción a Xbox Live como asociado administrado o desarrollador de ID.</a></td>
    </tr>
    <tr>
        <td>Agregar Xbox Live a tu juego</td>
        <td><a href="../xbox-live/get-started-with-partner/partners-step-by-step-guide.md">Guía paso a paso para integrar Xbox Live para asociados administrados y miembros de ID.</a></td>
    </tr>
    <tr>
        <td>Agregar Xbox Live a tu juego para UWP creado con Unity</td>
        <td><a href="../xbox-live/get-started-with-partner/partner-unity-uwp-il2cpp.md">Agregar compatibilidad con Xbox Live a Unity para UWP con back-end de fragmentos de código IL2CPP para ID y socios administrados</a></td>
    </tr>
    <tr>
        <td>Configurar tu espacio aislado de desarrollo</td>
        <td><a href="../xbox-live/get-started-with-partner/advanced-xbox-live-sandboxes.md">Espacios aislados avanzados de Xbox Live</a></td>
    </tr>
    <tr>
        <td>Requisitos para los juegos que usan Xbox Live (GDN)</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=533217">Requisitos de Xbox para Xbox Live en Windows10</a></td>
    </tr>
    <tr>
        <td>Muestras</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK">Muestras de código para desarrolladores ID@Xbox</a></td>
    </tr>  
    <tr>
        <td>Información general sobre el desarrollo de juegos para Xbox Live (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-with-Xbox-Live-for-Windows-10">Desarrollar con Xbox Live para Windows10</a></td>
    </tr>
    <tr>
        <td>Asociación entre plataformas (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Xbox-Live-Multiplayer-Introducing-services-for-cross-platform-matchmaking-and-gameplay">Xbox Live para varios jugadores: introducción a los servicios de asociación y juego entre plataformas</a></td>
    </tr>
    <tr>
        <td>Juegos entre dispositivos en Fable Legends (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Fable-Legends-Cross-device-Gameplay-with-Xbox-Live">Fable Legends: juegos entre dispositivos con Xbox Live</a></td>
    </tr>
    <tr>
        <td>Estadísticas y logros de Xbox Live (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Best-Practices-for-Leveraging-Cloud-Based-User-Stats-and-Achievements-in-Xbox-Live">Procedimientos recomendados para aprovechar las estadísticas y los logros del usuario basados en la nube en Xbox Live</a></td>
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
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/game-development-videos">Vídeos de conferencias principales, como GDC y //build</a></td>
    </tr>
    <tr>
        <td>Desarrollo de juegos independientes (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/New-Opportunities-for-Independent-Developers">Nuevas oportunidades para los desarrolladores independientes</a></td>
    </tr>
    <tr>
        <td>Consideraciones para dispositivos móviles de varios núcleos (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Sustained-gaming-performance-in-multi-core-mobile-devices">Rendimiento mantenido de juegos en dispositivos móviles de varios núcleos</a></td>
    </tr>
    <tr>
        <td>Desarrollar juegos de escritorio para Windows10 (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/PC-Games-for-Windows-10">Juegos de PC para Windows10</a></td>
    </tr>
</table>



 

 

 
