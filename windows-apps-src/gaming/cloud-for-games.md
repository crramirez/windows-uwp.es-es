---
author: joannaleecy
title: Uso de servicios en la nube con juegos para UWP
description: Obtén más información acerca de cómo implementar la nube como un back-end para tus juegos para UWP.
ms.assetid: 1a7088e0-0d7b-11e6-8e05-0002a5d5c51b
ms.author: joanlee
ms.date: 03/27/2018
ms.topic: article
keywords: Windows 10, UWP, juegos, servicios en la nube
ms.localizationpriority: medium
ms.openlocfilehash: 5d15d3e6b6beb773a8d606db7a5d8a17544270be
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "6035743"
---
#  <a name="using-cloud-services-for-uwp-games"></a>Uso de servicios en la nube con juegos para UWP

La Plataforma universal de Windows (UWP) de Windows 10 ofrece un conjunto de API que pueden usarse para desarrollar juegos en todos los dispositivos de Microsoft. Al desarrollar juegos en todos los dispositivos y plataformas, puede usar un back-end en la nube para ayudar a escalar el juego según la demanda.

Si buscas una solución back-end completa y en la nube para tu juego, consulta [Software como servicio para el back-end del juego](#software-as-a-service-for-game-backend).

##  <a name="what-is-cloud-computing"></a>¿Qué es la informática de nube?

La informática de nube usa recursos y aplicaciones de TI a petición a través de Internet para almacenar y procesar los datos de los dispositivos. El término _nube_ es una metáfora de la disponibilidad de la enorme cantidad de recursos disponibles (recursos no locales) a los que puedes acceder desde ubicaciones no específicas.
El principio de la informática de nube ofrece una nueva manera de consumir los recursos y el software. Los usuarios ya no tienen que pagar por el producto o los recursos completos de antemano, pero en su lugar pueden consumir la plataforma, el software y los recursos como un servicio. A menudo, los proveedores de nube facturan a sus clientes según el uso o las ofertas del plan de servicio.

##  <a name="why-use-cloud-services"></a>Motivos para usar servicios en la nube

Una ventaja de usar los servicios en la nube para juegos es que es necesario invertir en servidores de hardware físicos de antemano, si no que solo se debe pagar según el uso o los planes de servicio posteriormente. Es una manera de ayudar a administrar los riesgos que implica el desarrollo de un nuevo título de juego. 

Otra ventaja es que el juego puede aprovechar la gran variedad de recursos de nube para lograr escalabilidad (administrar eficazmente los picos repentinos en el número de jugadores simultáneos, los cálculos de juegos intensos en tiempo real o los requisitos de datos). Esto hace que el rendimiento de juego sea siempre estable. Además, los recursos de nube son accesibles desde cualquier dispositivo que se ejecute en cualquier plataforma y en cualquier lugar del mundo, lo que significa que puede poner el juego a disposición de todo el mundo.

Ofrecer una experiencia de juego increíble a los jugadores es importante. Dado que los servidores de juegos que se ejecutan en la nube son independientes de las actualizaciones de cliente, pueden ofrecer un entorno más seguro y controlado para tu juego en general.   También puedes lograr la coherencia del juego a través de la nube, para lo cual nunca debes confiar en el cliente y tener la lógica del juego del lado servidor. Las conexiones de servicio a servicio también pueden configurarse para permitir una experiencia de juego más integrada; algunos ejemplos incluyen los vínculos de compras desde el juego a diversos métodos de pago, los puentes en distintas redes de juegos y las actualizaciones desde el juego compartidas en portales de redes sociales populares, como Facebook y Twitter. 

También puedes usar servidores de nube dedicados para crear un enorme mundo de juego persistente, crear una comunidad de jugadores, recopilar y analizar datos de jugadores en el tiempo para mejorar el juego y optimizar el modelo de diseño de rentabilidad del juego.

Además, los juegos que requieren funciones de administración de datos de juegos intensivos, como juegos sociales con mecanismos multijugador asincrónicos, pueden implementarse mediante servicios en la nube.

##  <a name="how-game-companies-use-the-cloud-technology"></a>¿Cómo usan las empresas de juegos la tecnología de nube?

Descubre cómo otros desarrolladores han implementado soluciones en la nube en sus juegos.

<table>
    <colgroup>
    <col width="10%" />
    <col width="30%" />
    <col width="30%" />
    <col width="30%" />
    </colgroup>
    <tr class="header" align="left">
        <th>Desarrollador</th>
        <th>Descripción</th>
        <th>Escenarios clave de juegos</th>
        <th>Más información</th>
    </tr>
    <tr>
        <td><a href="https://www.tencent.com">Tencent Games</a></td>
        <td><b>Tencent Games</b> tiene desarrollada una solución innovadora con Azure Service Fabric que posibilita que los juegos de PC tradicionales se distribuyan como un servicio. Su solución de juegos en la nube usa un modelo 'cliente ligero + nube enriquecida' que ejecuta las cargas de trabajo como microservicios en el back-end.</td>
        <td>
            <ul>
                <li>Los juegos para PC tradicionales se distribuyen como juegos en la nube a usuarios de todo el mundo <li>Proceso de distribución optimizada de juegos <li>Las funcionalidades de juegos aisladas como microservicios para reducir la complejidad, reducir la repetición de las cargas de trabajo debido a dependencias y la posibilidad de actualizar de forma independiente nuevas funciones <li>Se descarga un paquete de instalación pequeño para jugar el contenido más reciente del juego (tamaño de paquete reducido desde los GB a los MB) <li>Costo de mantenimiento reducido </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://customers.microsoft.com/story/tencent-telecommunications-azure-service-fabric-windows-server-en">Tencent Games y Microsoft crean la solución de juego de la nube</a>
                <li><a href="https://channel9.msdn.com/Shows/Cloud+Cover/Episode-228-Building-Games-with-Service-Fabric#time=38m33s">Crear juegos con Service Fabric: detalles sobre la implementación de Tencent (vídeo)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://www.halowaypoint.com/">343 Industries</a></td>
        <td><b>Halo 5: Guardians</b> implementó <a href="https://www.halowaypoint.com/spartan-companies">Halo: Spartan Companies</a> como su plataforma social de juegos mediante el uso de Azure Cosmos DB (via API de DocumentDB), que fue elegido por su velocidad y flexibilidad debido a sus funciones de indexación automática.</td>
        <td>
            <ul>
                <li>Capa de datos escalable para controlar la administración y creación de grupos para juegos multijugador <li>Integración de juegos y redes sociales <li>Consultas en tiempo real de datos a través de varios atributos <li>Sincronización de logros y estadísticas de juegos </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/">Juego social implementado con Azure Cosmos DB (vía API de DocumentDB)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="http://web.ageofascent.com/">Illyriad Games</a></td>
        <td>Illyriad Games creó <b>Ages of Ascent</b>, un juego en línea multijugador masivo (MMOG) de espacio 3D épico que puede reproducirse en dispositivos con exploradores modernos. Por lo tanto, este juego se puede reproducir en PC, portátiles, teléfonos móviles y otros dispositivos móviles sin complementos. El juego usa ASP.NET Core, HTML5, WebGL y Azure.</td>
        <td>
            <ul>
                <li>Juego multiplataforma basado en navegador <li>Un enorme mundo persistente abierto y único <li>Controla los cálculos de juegos intensivos en tiempo real <li>Escalas con el número de jugadores </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://channel9.msdn.com/Shows/Cloud+Cover/Episode-228-Building-Games-with-Service-Fabric#time=06m52s">Crear juegos con Service Fabric: juego Age of Ascent MMO (vídeo)</a>
                <li><a href="https://channel9.msdn.com/Events/Build/2016/KEY02#time=57m20s">Administrar componentes de juegos como microservicios con Azure Service Fabric (vídeo)</a> 
                <li><a href="https://channel9.msdn.com/Shows/Azure-Friday/Age-of-Ascent-from-Illyriad-Powered-by-Azure-Service-Fabric-and-ASPNET">Entrevista con los desarrolladores de Age of Ascent (vídeo)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="http://www.nextgames.com/">Next Games</a></td>
        <td>Next Games es el creador del videojuego <b>The Walking Dead: No Man's Land</b> basado en la serie original de AMC. El juego Walking Dead usó Azure como back-end. Se realizaron 1.000.000 descargas el fin de semana del lanzamiento y, la primera semana, el juego se convirtió en la aplicación gratuita número 1 para iPhone y iPad en el App Store de Estados Unidos, la aplicación gratuita número 1 en 12 países y el juego gratuito número 1 en 13 países.
        </td>
        <td>
            <ul>
                <li>Multiplataforma <li>Multijugador por turnos <li>Escalar el rendimiento de forma elástica <li>Protección contra el fraude al jugador <li>Entrega de contenido dinámica </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://azure.microsoft.com/blog/how-we-built-it-next-games-global-online-gaming-platform-on-azure/">Cómo se ha creado: plataforma de juegos en línea global Next Games en Azure (blog con vídeo)</a>
                <li><a href="https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/">Walking Dead usa Azure Cosmos DB (vía API de DocumentDB) para ofrecer un ciclo de desarrollo más rápido y un juego más atractivo.</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="http://www.crimecoast.com/">Pixel Squad</a></td>
        <td>Pixel Squad desarrolló <b>Crime Coast</b> con el motor de juego de Unity y Azure. <b>Crime Coast</b> es un juego de estrategia social disponible en las plataformas Android, iOS y Windows. En su juego se usó lo siguiente: Almacenamiento de blobs de Azure, Caché en Redis de Azure administrada, una matriz de máquinas virtuales IIS de carga equilibrada y el centro de notificaciones de Microsoft. Descubre cómo administraron la escala y controlaron el aumento de jugadores con 5000 jugadores simultáneos.
        </td>
        <td>
            <ul>
                <li>Multiplataforma <li>Juego multijugador en línea <li>Escala con el número de jugadores </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://channel9.msdn.com/Blogs/The-Game-Blog/BizSpark-Interview-with-Pixel-Squad-How-the-used-Azure-Cloud-Services-to-make-an-MMO-with-a-3-man-te">Cómo usó el juego MMOG Crime Coast los Servicios en la nube de Azure</a>
            </ul>
        </td>
    </tr> 
</table>

    
### <a name="other-links"></a>Otros vínculos

* [Hitman y Azure: crear funciones de juegos como Elusive Target, que solo son posibles con la nube](https://channel9.msdn.com/Series/Hitman)
* [Azure es la salsa secreta de Hitcents, Game Troopers e InnoSpark](http://news.microsoft.com/features/game-developers-use-microsoft-azure-as-secret-sauce-for-scale-and-growth-2/)
* [Inicios de juegos en el programa Bizspark con Azure](https://blogs.technet.microsoft.com/bizspark_featured_startups/2015/09/25/azure-open-for-gaming-startups/)


## <a name="how-to-design-your-cloud-backend"></a>Cómo diseñar el back-end en la nube

Mientras que los productores y diseñadores juegos discuten qué características y funcionalidades del juego son necesarias, es recomendable que comiences a considerar cómo quieres diseñar la infraestructura de tu juego. Azure puede usarse como el back-end del juego si quieres desarrollar juegos para varios dispositivos y en las distintas plataformas principales.

### <a name="understanding-iaas-paas-or-saas"></a>Descripción de IaaS, PaaS o SaaS

En primer lugar, debes pensar en el nivel de servicio más idóneo para tu juego. Conocer las diferencias de los siguientes tres servicios puede ayudarte a determinar el enfoque que quieres para crear el back-end.

* [Infraestructura como servicio (IaaS)](https://azure.microsoft.com/overview/what-is-iaas/)

    Infraestructura como servicio (IaaS) es una infraestructura informática instantánea aprovisionada y administrada a través de Internet. Imagina tener la posibilidad de tener muchos equipos disponibles para escalar y reducir verticalmente y con rapidez según la demanda. IaaS te ayuda a evitar el costo y la complejidad de comprar y administrar tus propios servidores físicos y otras infraestructuras de centro de datos.

* [Plataforma como servicio (PaaS)](https://azure.microsoft.com/overview/what-is-paas/)

    Plataforma como servicio (PaaS) es como IaaS, pero también incluye la administración de infraestructuras, como servidores, almacenamiento y redes. Así, además de no tener que comprar servidores físicos e infraestructura de centro de datos, tampoco tendrás que comprar ni administrar licencias de software, una infraestructura de aplicaciones subyacente, software intermedio, herramientas de desarrollo u otros recursos.

* [Software como servicio (SaaS)](https://azure.microsoft.com/overview/what-is-saas/)

    Software como servicio (SaaS) permite a los usuarios conectarse y usar aplicaciones basadas en la nube a través de Internet. Proporciona una solución completa de software que se compra en forma de pagar por uso desde un proveedor de servicios de nube.  Algunos ejemplos usuales son las herramientas de correo electrónico, el calendario y office (por ejemplo, Microsoft Office 365). Se alquila el uso de una aplicación para la organización y los usuarios se conectan a ella a través de Internet, por lo general con un navegador web. Toda la infraestructura subyacente, el software intermedio, el software de aplicaciones y los datos de las aplicaciones se encuentran en el centro de datos del proveedor de servicios. El proveedor de servicios administra el hardware y el software, y con el contrato de servicio apropiado se garantiza la disponibilidad y la seguridad del juego y también de los datos. SaaS permite que la organización arranque rápidamente y funcione con una aplicación, con un coste de equipo mínimo.


### <a name="design-your-game-infrastructure-using-azure"></a>Diseñar la infraestructura del juego con Azure

A continuación se indican algunas maneras de usar las ofertas de la nube de Azure para un juego. Azure funciona con Windows y Linux, así como con las tecnologías de código abierto conocidas, como Ruby, Python, Java y PHP. Para obtener más información, consulta el sitio web [Azure para juegos](https://azure.microsoft.com/solutions/gaming/).

| Requisitos                 | Escenarios de actividad                            | Oferta de productos                      | Funcionalidades de productos                                    |
|-----------------------------------|-----------------------------------------------|---------------------------------------|----------------------------------------------------|
| Hospedar el dominio en la nube.     | Responder a las consultas DNS de manera eficaz.            | [DNS de Azure](https://azure.microsoft.com/services/dns/) | Hospedar el dominio con alto rendimiento y disponibilidad  |
| Iniciar sesión, verificación de identidad      | El jugador inicia sesión y se autentica la identidad del jugador.  | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) | Inicio de sesión único a cualquier aplicación web en la nube y local con la autenticación multifactor.            | 
| Juego con el modelo de infraestructura como servicio (IaaS)      | El juego se hospeda en máquinas virtuales en la nube.       | [VM de Azure](https://azure.microsoft.com/services/virtual-machines/) | Escala de 1 a miles de instancias de máquina virtual como servidores de juegos con redes virtuales integradas y equilibrio de carga; coherencia híbrida con sistemas locales.           |
| Juegos web o móviles con el modelo de plataforma como servicio (PaaS)            | El juego se hospeda en una plataforma administrada.                | [Servicio de aplicaciones de Azure](https://azure.microsoft.com/services/app-service/) | PaaS para sitios web o juegos móviles (lo que se refiere a las VM de Azure con software intermedio, herramientas de desarrollo, BI y administración de bases de datos).   |
| Juego en la nube de n niveles, altamente disponible y escalable, con mayor control del sistema operativo (PaaS)        | El juego se aloja en una plataforma administrada.                | [Servicio Azure en la nube](https://azure.microsoft.com/services/app-service/) | PaaS está diseñado para admitir aplicaciones que sean escalables, confiables y baratas de operar   |
| Equilibrio de carga en las regiones, para mejor rendimiento y disponibilidad | Encamina las solicitudes entrantes de juego. Puede actuar como el primer nivel de equilibrio de carga.       | [Administrador de tráfico de Azure](https://azure.microsoft.com/en-us/services/traffic-manager/) | Ofrece varias opciones de conmutación automática por error y la capacidad de distribuir el tráfico de forma equitativa o con valores ponderados. Puede combinarse sin problemas en los sistemas locales y en la nube. |
| Almacenamiento en la nube de datos de juegos       | Los datos de los juegos más recientes se almacenan en la nube y se envían a dispositivos cliente. | [Almacenamiento de blobs de Azure](https://azure.microsoft.com/services/storage/blobs/)| Sin limitación de los tipos de archivo que se pueden almacenar; almacenamiento de objetos para grandes cantidades de datos sin estructurar, como imágenes, audio, vídeo y mucho más.  |
| Tablas de almacenamiento datos temporales| Las transacciones de juegos (cambios en los estados de juego) se almacenan temporalmente en tablas. | [Almacenamiento de tablas de Azure](https://azure.microsoft.com/services/storage/tables/)| Los datos de juegos pueden almacenarse en un esquema flexible según las necesidades del juego. |
| Poner en cola transacciones y solicitudes de juegos| Las transacciones de juegos se procesan en forma de cola. | [Almacenamiento en cola de Azure](https://azure.microsoft.com/services/storage/queues/)| Las colas absorben las ráfagas de tráfico inesperadas y pueden impedir que los servidores se sobrecarguen con una avalancha repentina de solicitudes durante el juego.   |
| Base de datos de juegos relacional escalable| Almacenamiento estructurado de datos relacionales, como las transacciones en el juego, en la base de datos. | [Base de datos SQL de Azure](https://azure.microsoft.com/services/sql-database/)| Base de datos SQL como servicio ([comparación con SQL en una máquina virtual](https://azure.microsoft.com/documentation/articles/data-management-azure-sql-database-and-sql-server-iaas/)).  |
| Base de datos de juegos escalable y distribuida de baja latencia| Lectura, escritura y consulta rápidas de datos del juego y del reproductor con flexibilidad de esquema. | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)| Base de datos de documentos NoSQL de baja latencia como servicio.   |
| Uso del centro de datos propio con los servicios de Azure | El juego se recupera del propio centro de datos y se envía a los dispositivos cliente. | [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) | Permite a tu organización ofrecer servicios de Azure del propio centro de datos que te ayudarán a mejorar tus logros.  |
| Transferencia de fragmentos de datos de gran tamaño| Los archivos de gran tamaño, como los de imagen, audio y vídeo de juegos, se pueden enviar a los usuarios desde la ubicación emergente más cercana de la Red de entrega de contenido (CDN) con CDN de Azure.    | [Red de entrega de contenido de Azure](https://azure.microsoft.com/services/cdn/) | Basada en una topología de red moderna de grandes nodos centralizadas, la red CDN de Azure controla picos repentinos de tráfico y cargas elevadas para aumentar considerablemente la velocidad y la disponibilidad, lo que da lugar a mejoras significativas de la experiencia del usuario.  |
| Baja latencia               | Realiza el almacenamiento en caché para compilar juegos rápidos y escalables con más control y aislamiento garantizado de los datos; también puede usarse para mejorar la característica de creación de coincidencias del juego. | [Caché en Redis de Azure](https://azure.microsoft.com/services/cache/) | Acceso a datos coherente de alto rendimiento y baja latencia para impulsar aplicaciones de Azure rápidas y escalables.  |
| Alta escalabilidad y baja latencia | Controla fluctuaciones en el número de usuarios de juegos con lectura y escritura de baja latencia. | [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | Capacidad de impulsar los escenarios de uso intensivo de datos y baja latencia más complejos, así como de escalar con confianza para controlar más usuarios a la vez. Service Fabric permite compilar juegos sin tener que crear un almacén independiente o una caché, según sea necesario para aplicaciones sin estado. |
| Capacidad para recopilar millones de eventos por segundo de dispositivos.                         | Registrar millones de eventos por segundo de dispositivos. | [Centros de eventos de Azure](https://azure.microsoft.com/services/event-hubs/) | Ingesta de telemetría de escala de nube de juegos, sitios web, aplicaciones y dispositivos.  |
| Procesamiento de datos de juegos en tiempo real  | Realizar un análisis en tiempo real de los datos de jugadores para mejorar el juego.| [Análisis de transmisiones de Azure](https://azure.microsoft.com/services/stream-analytics/) | Procesamiento de transmisiones en tiempo real en la nube.  |
| Desarrollo de un juego predictivo         | Crear un juego dinámico personalizado según los datos de jugadores.  | [Aprendizaje automático de Azure](https://azure.microsoft.com/services/machine-learning/) | Servicio en la nube totalmente administrado que te permite compilar, implementar y compartir fácilmente soluciones de análisis predictivo.  |
| Recopilación y análisis de datos de juegos| Procesamiento en paralelo masivo de datos de bases de datos relacionales y no relacionales. | [Almacenamiento de datos de Azure](https://azure.microsoft.com/services/sql-data-warehouse/)| Almacenamiento de datos elástico como servicio con funciones de clase empresarial.   |
| Atraer a los usuarios a aumentar el uso y la retención| Enviar notificaciones push dirigidas a cualquier plataforma desde cualquier back-end para generar interés y fomentar acciones específicas del juego. | [Centros de notificaciones de Azure](https://azure.microsoft.com/services/notification-hubs/)| Difusión rápida de inserción para llegar a millones de dispositivos móviles en todas las plataformas principales &mdash; iOS, Android, Windows, Kindle, Baidu. Tu juego puede alojarse en cualquier back-end de la &mdash; nube o local.|
| Streaming del contenido multimedia a tu público local y de todo el mundo, protegiendo tu contenido| Difusión de tráileres de juegos y clips cinemáticos de calidad que pueden verse desde cualquier dispositivo.| [Servicios Azure Media](https://azure.microsoft.com/services/media-services/)| Streaming de vídeo bajo demanda y en directo, con funcionalidades integradas de red de entrega de contenido. Uso de un jugador para todas tus necesidades de reproducción, que incluye cifrado y protección de contenido.| 
| Desarrollar, distribuir y realizar pruebas beta de tus aplicaciones móviles | Probar y distribuir tu aplicación móvil. Rendimiento de aplicaciones y administración de la experiencia de usuario. | [HockeyApp](https://azure.microsoft.com/services/hockeyapp/)| Integra las métricas de informes de bloqueo y de usuarios con una plataforma de comentarios de usuario y distribución de aplicaciones. Admite las aplicaciones de Android, Cordova, iOS, OS X, Unity, Windows y Xamarin. Además, considera el control de misiones de [Visual Studio Mobile Center](https://www.visualstudio.com/vs/mobile-center/) &mdash; para aplicaciones que combinen análisis enriquecido, informes de notificaciones de inserción, distribución de aplicaciones y mucho más. |
| Creación de campañas de marketing para aumentar el uso y la retención  | Enviar notificaciones de inserción a los reproductores de destino para generar interés y fomentar acciones específicas del juego según el análisis de datos. | [Interacción móvil](https://azure.microsoft.com/services/mobile-engagement/): se retirará en marzo de 2018 y solo está disponible actualmente para los clientes existentes |  Aumentar el tiempo de juego y la retención del usuario en las principales plataformas (iOS, Android, Windows y Windows Phone) |


##  <a name="startup-and-developer-resources"></a>Recursos para nuevas empresas y desarrolladores

* [Microsoft for Startups](https://startups.microsoft.com)

    Microsoft for Startups ofrece ventajas técnicas y de salida al mercado para productos, a fin de acelerar el desarrollo de nuevas empresas. Una ventaja incluye obtener una cuenta gratuita de Azure. Tienes un crédito de 200 $ explorar servicios durante 30 días, 12meses a partir de los servicios gratuitos populares y más de 25 servicios gratuitos para siempre. Para obtener más información, consulta [Haz realidad tus ideas para nuevas empresas con una cuenta gratuita de Azure](https://azure.microsoft.com/free/startups/).
    
* [Programas para desarrolladores](e2e.md#developer-programs)

    Microsoft ofrece varios programas para desarrolladores, como [ID@Xbox](http://www.xbox.com/Developers/id) y el [Programa de creadores de Xbox Live](https://developer.microsoft.com/games/xbox/xboxlive/creator) para ayudarte a desarrollar y publicar juegos.

## <a name="learning-resources"></a>Recursos de aprendizaje

* //build 2016 [Codelabs &mdash;: Usa Microsoft Azure App Service y el back-end de Microsoft SQL Azure para guardar las puntuaciones del juego en Unity](https://github.com/Microsoft-Build-2016/CodeLabs-GameDev-6-Azure)
* Build 2017: [experiencias de juego entregar con Microsoft Azure: lecciones aprendidas de títulos, como Halo, Hitman y WalkingDead (vídeo)](https://channel9.msdn.com/Events/Build/2017/P4062)
* Conjunto reutilizable de bloques de creación, proyectos, servicios y procedimientos recomendados diseñados para admitir cargas de trabajo comunes de juegos usando Azure en GitHub: [Creación de bloques de juegos en Azure](https://github.com/MicrosoftDX/nether)
* [Servicios de juegos en Azure (vídeos)](https://channel9.msdn.com/Series/Gaming-Services-on-Azure)

## <a name="tools-and-other-useful-links"></a>Herramientas y otros vínculos útiles

* [Foros de MSDN &mdash; Plataforma de Azure](https://social.msdn.microsoft.com/Forums/azure/home?category=windowsazureplatform)
* [Herramienta de pruebas de carga en la nube](https://www.visualstudio.com/team-services/cloud-load-testing/)
* [Herramientas de SDK y línea de comandos](https://azure.microsoft.com/downloads/)
    
## <a name="software-as-a-service-for-game-backend"></a>Software como servicio para el back-end del juego

[Playfab](https://playfab.com/) actualmente da cabida a más de 1200 juegos dinámicos con 80 millones de jugadores activos mensuales. Es una plataforma back-end completa que incluye la pila completa de LiveOps con control en tiempo real. 

Puedes integrar esta solución en el móvil, PC o juegos de la consola con el SDK. Hay SDK disponibles para todos los motores de juegos y plataformas populares, como Android, iOS, Unreal, Unity y Windows. Para empezar, consulta [Documentation](https://api.playfab.com/).

Ofrece servicios de juego como autenticación, administración de datos del jugador, análisis multijugador y en tiempo real para ayudar a que tu juego aumente su base de usuarios. Aprovecha la capacidad de la canalización de datos en tiempo real y LiveOps para atraer a tus usuarios con los elementos personalizados integrados en el juego, eventos y promociones. También tienes la posibilidad de llevar a cabo pruebas A/B, generar informes, enviar notificaciones de inserción y mucho más. 

Estamos continuamente innovando y añadiendo características nuevas. Para obtener más información, consulta [Características](https://playfab.com/features/) y para ver precios, consulta [Precios sencillos a tu medida](https://playfab.com/pricing/).

## <a name="related-links"></a>Vínculos relacionados

* [Guía de desarrollo de juegos para Windows 10](https://msdn.microsoft.com/windows/uwp/gaming/e2e)
* [Azure para juegos](https://azure.microsoft.com/solutions/gaming/)
* [Playfab](https://playfab.com/)
* [Microsoft for Startups](https://startups.microsoft.com)
* [ID@Xbox](http://www.xbox.com/Developers/id)


 

 
