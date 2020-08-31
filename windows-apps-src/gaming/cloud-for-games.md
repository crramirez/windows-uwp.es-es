---
title: Uso de servicios en la nube con juegos para UWP
description: Al desarrollar juegos para UWP en plataformas y dispositivos, use un back-end en la nube para ayudar a escalar los juegos según la demanda.
ms.assetid: 1a7088e0-0d7b-11e6-8e05-0002a5d5c51b
ms.date: 03/27/2018
ms.topic: article
keywords: Windows 10, UWP, juegos, Cloud Services
ms.localizationpriority: medium
ms.openlocfilehash: 7c0cfd98a37c4822d80eded7fe69e23c54bcdc89
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054415"
---
#  <a name="using-cloud-services-for-uwp-games"></a>Uso de servicios en la nube con juegos para UWP

La Plataforma universal de Windows (UWP) de Windows 10 ofrece un conjunto de API que pueden usarse para desarrollar juegos en todos los dispositivos de Microsoft. Al desarrollar juegos en todos los dispositivos y plataformas, puede usar un back-end en la nube para ayudar a escalar el juego según la demanda.

Si busca una solución completa de back-end en la nube para su juego, consulte [software como servicio para el back-end de juegos](#software-as-a-service-for-game-backend).

##  <a name="what-is-cloud-computing"></a>¿Qué es la informática en la nube?

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
        <td><a href="https://www.tencent.com">Juegos de Tencent</a></td>
        <td><b>Tencent Games</b> ha desarrollado una solución innovadora que usa Azure Service fabric que permite la entrega de juegos de equipos tradicionales como servicio. Su solución de juegos en la nube usa un modelo de "ligero cliente + la nube enriquecida" que ejecuta cargas de trabajo como microservicios en el back-end.</td>
        <td>
            <ul>
                <li>Los juegos de equipos tradicionales se entregan como juegos en la nube a usuarios de todo el mundo <li>Proceso de entrega de juegos optimizado <li>Funcionalidades del juego aisladas como microservicios para reducir la complejidad, reducir la repetición de las cargas de trabajo debido a las dependencias y la capacidad de actualizar nuevas características de forma independiente <li>Descarga de paquetes de instalación pequeños para reproducir contenido de juego más reciente (tamaño de paquete reducido de GB a MB) <li>Costo de mantenimiento reducido </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://customers.microsoft.com/story/tencent-telecommunications-azure-service-fabric-windows-server-en">Tencent Games y Microsoft creó la solución de juegos en la nube</a>
                <li><a href="https://channel9.msdn.com/Shows/Cloud+Cover/Episode-228-Building-Games-with-Service-Fabric#time=38m33s">Creación de juegos con Service Fabric: detalles sobre la implementación de Tencent (vídeo)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://www.halowaypoint.com/">343 Industries</a></td>
        <td><b>Halo 5: protecciones</b> implementadas en <a href="https://www.halowaypoint.com/spartan-companies">Halo: Spartan compañías</a> como plataforma de juegos sociales mediante Azure Cosmos dB (a través de la API de DocumentDB), que se seleccionó para su velocidad y flexibilidad debido a sus capacidades de indexación automática.</td>
        <td>
            <ul>
                <li>Capa de datos escalable para controlar la administración y creación de grupos para juegos multijugador <li>Integración de juegos y redes sociales <li>Consultas en tiempo real de datos a través de varios atributos <li>Sincronización de logros y estadísticas de juegos </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/">Juego social implementado mediante Azure Cosmos DB (a través de la API de DocumentDB)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://web.ageofascent.com/">Illyriad Games</a></td>
        <td>Illyriad Games creó <b>Ages of Ascent</b>, un juego en línea multijugador masivo (MMOG) de espacio 3D épico que puede reproducirse en dispositivos con exploradores modernos. Así pues, este juego se puede reproducir en PC, equipos portátiles, teléfonos móviles y otros dispositivos móviles sin complementos. El juego usa ASP.NET Core, HTML5, WebGL y Azure.</td>
        <td>
            <ul>
                <li>Juego multiplataforma basado en explorador <li>Un enorme mundo persistente abierto y único <li>Controla los cálculos de juegos intensivos en tiempo real <li>Escala con el número de jugadores </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://channel9.msdn.com/Shows/Cloud+Cover/Episode-228-Building-Games-with-Service-Fabric#time=06m52s">Creación de juegos con Service Fabric: Age of Ascent ULTRAMMO Game (vídeo)</a>
                <li><a href="https://channel9.msdn.com/Events/Build/2016/KEY02#time=57m20s">Administración de componentes de juegos como microservicios mediante Azure Service Fabric (vídeo)</a> 
                <li><a href="/Blogs/Azure/Age-of-Ascent-from-Illyriad-Powered-by-Azure-Service-Fabric-and-ASPNET">Entrevista con los desarrolladores de Age of Ascent (vídeo)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://www.nextgames.com/">Próximos juegos</a></td>
        <td>Next Games es el creador del videojuego <b>The Walking Dead: No Man's Land</b> basado en la serie original de AMC. El juego Walking Dead usó Azure como back-end. Se realizaron 1.000.000 descargas el fin de semana del lanzamiento y, la primera semana, el juego se convirtió en la aplicación gratuita número 1 para iPhone y iPad en el App Store de Estados Unidos, la aplicación gratuita número 1 en 12 países y el juego gratuito número 1 en 13 países.
        </td>
        <td>
            <ul>
                <li>Multiplataforma <li>Multijugador por turnos <li>Escalar rendimiento de forma elástica <li>Protección contra fraudes de jugadores <li>Entrega de contenido dinámico </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://azure.microsoft.com/blog/how-we-built-it-next-games-global-online-gaming-platform-on-azure/">Cómo se crea: la plataforma de juegos en línea global de próximos juegos en Azure (blog con vídeo)</a>
                <li><a href="https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/">Recorrer los usos inactivos Azure Cosmos DB (a través de la API de DocumentDB) para un ciclo de desarrollo más rápido y un juego más atractivo</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="http://www.crimecoast.com/">Pixel Squad</a></td>
        <td>Pixel Squad desarrolló <b>Crime Coast</b> con el motor de juego de Unity y Azure. <b>Crime Coast</b> es un juego de estrategia social disponible para las plataformas Android, iOS y Windows. En su juego se usó lo siguiente: Almacenamiento de blobs de Azure, Caché en Redis de Azure administrada, una matriz de máquinas virtuales IIS de carga equilibrada y el centro de notificaciones de Microsoft. Descubre cómo administraron la escala y controlaron el aumento de jugadores con 5000 jugadores simultáneos.
        </td>
        <td>
            <ul>
                <li>Multiplataforma <li>Juego multijugador en línea <li>Escala con el número de jugadores </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://channel9.msdn.com/Blogs/The-Game-Blog/BizSpark-Interview-with-Pixel-Squad-How-the-used-Azure-Cloud-Services-to-make-an-MMO-with-a-3-man-te">Cómo el juego ULTRAMMO de delitos de uso de Azure Cloud Services</a>
            </ul>
        </td>
    </tr> 
</table>

    
### <a name="other-links"></a>Otros vínculos

* [Hitman y Azure: cree características de juego como el destino escurridizos que solo son posibles con Cloud](https://channel9.msdn.com/Series/Hitman)
* [Azure como la salsa secreta de Hitcents, Game Troopers e InnoSpark](https://news.microsoft.com/features/game-developers-use-microsoft-azure-as-secret-sauce-for-scale-and-growth-2/)
* [Inicios de juegos en el programa Bizspark con Azure](https://blogs.technet.microsoft.com/bizspark_featured_startups/2015/09/25/azure-open-for-gaming-startups/)


## <a name="how-to-design-your-cloud-backend"></a>Cómo diseñar el back-end en la nube

Mientras que los productores y diseñadores juegos discuten qué características y funcionalidades del juego son necesarias, es recomendable que comiences a considerar cómo quieres diseñar la infraestructura de tu juego. Azure puede usarse como el back-end de juego si quieres desarrollar juegos para varios dispositivos y en las distintas plataformas principales.

### <a name="understanding-iaas-paas-or-saas"></a>Descripción de IaaS, PaaS o SaaS

En primer lugar, debes pensar en el nivel de servicio más idóneo para tu juego. Conocer las diferencias de los siguientes tres servicios puede ayudarte a determinar el enfoque que quieres para crear el back-end.

* [Infraestructura como servicio (IaaS)](https://azure.microsoft.com/overview/what-is-iaas/)

    Infraestructura como servicio (IaaS) es una infraestructura informática instantánea aprovisionada y administrada a través de Internet. Imagina tener la posibilidad de tener muchos equipos disponibles para escalar y reducir verticalmente y con rapidez según la demanda. IaaS te ayuda a evitar el costo y la complejidad de comprar y administrar tus propios servidores físicos y otras infraestructuras de centro de datos.

* [Plataforma como servicio (PaaS)](https://azure.microsoft.com/overview/what-is-paas/)

    Plataforma como servicio (PaaS) es como IaaS, pero también incluye la administración de infraestructuras, como servidores, almacenamiento y redes. Así, además de no tener que comprar servidores físicos e infraestructura de centro de datos, tampoco tendrás que comprar ni administrar licencias de software, una infraestructura de aplicaciones subyacente, software intermedio, herramientas de desarrollo u otros recursos.

* [Software como servicio (SaaS)](https://azure.microsoft.com/overview/what-is-saas/)

    El software como servicio (SaaS) permite que los usuarios se conecten y usen aplicaciones basadas en la nube a través de Internet. Proporciona una solución de software completa que se compra según una base de pago por uso de un proveedor de servicios en la nube.  Algunos ejemplos comunes son el correo electrónico, el calendario y las herramientas de Office (por ejemplo, las aplicaciones de Office Microsoft 365). Alquila el uso de una aplicación para su organización y los usuarios se conectan a ella a través de Internet, normalmente con un explorador Web. Toda la infraestructura subyacente, el software intermedio, el software de aplicación y los datos de aplicación se encuentran en el centro de datos del proveedor de servicios. El proveedor de servicios administra el hardware y el software, y con el contrato de servicio adecuado, garantizará también la disponibilidad y la seguridad del juego y los datos. SaaS permite que la organización empiece rápidamente a funcionar con una aplicación a un costo por adelantado mínimo.


### <a name="design-your-game-infrastructure-using-azure"></a>Diseñar la infraestructura de juego con Azure

A continuación se indican algunas maneras de usar las ofertas de la nube de Azure para un juego. Azure funciona con Windows y Linux, así como con las tecnologías de código abierto conocidas, como Ruby, Python, Java y PHP. Para obtener más información, consulta el sitio web [Azure para juegos](https://azure.microsoft.com/solutions/gaming/).

| Requisitos                 | Escenarios de actividad                            | Oferta de productos                      | Funcionalidades de productos                                    |
|-----------------------------------|-----------------------------------------------|---------------------------------------|----------------------------------------------------|
| Hospedar el dominio en la nube.     | Responder a las consultas DNS de manera eficaz.            | [DNS de Azure](https://azure.microsoft.com/services/dns/) | Hospedar el dominio con alto rendimiento y disponibilidad  |
| Iniciar sesión, verificación de identidad      | El jugador inicia sesión y se autentica la identidad del jugador.  | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) | Inicio de sesión único a cualquier aplicación web en la nube y local con la autenticación multifactor.            | 
| Juego con el modelo de infraestructura como servicio (IaaS)      | El juego se hospeda en máquinas virtuales en la nube.       | [Máquinas virtuales de Azure](https://azure.microsoft.com/services/virtual-machines/) | Escala de 1 a miles de instancias de máquina virtual como servidores de juegos con redes virtuales integradas y equilibrio de carga; coherencia híbrida con sistemas locales.           |
| Juegos web o móviles con el modelo de plataforma como servicio (PaaS)            | El juego se hospeda en una plataforma administrada.                | [Azure App Service](https://azure.microsoft.com/services/app-service/) | PaaS para sitios web o juegos móviles (lo que se refiere a las VM de Azure con software intermedio, herramientas de desarrollo, BI y administración de bases de datos).   |
| Juego en la nube de n niveles escalable y de alta disponibilidad con más control del sistema operativo (PaaS)        | El juego se hospeda en una plataforma administrada.                | [Servicio en la nube de Azure](https://azure.microsoft.com/services/app-service/) | PaaS diseñado para admitir aplicaciones escalables, confiables y baratas de operar   |
| Equilibrio de carga entre regiones para obtener un mejor rendimiento y disponibilidad | Enruta las solicitudes de juegos entrantes. Puede actuar como primer nivel de equilibrio de carga.       | [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) | Ofrece varias opciones de conmutación por error automática y la capacidad de distribuir el tráfico equitativamente o con valores ponderados. Puede combinar sin problemas sistemas locales y en la nube. |
| Almacenamiento en la nube de datos de juegos       | Los datos de los juegos más recientes se almacenan en la nube y se envían a dispositivos cliente. | [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/)| Sin limitación de los tipos de archivo que se pueden almacenar; almacenamiento de objetos para grandes cantidades de datos sin estructurar, como imágenes, audio, vídeo y mucho más.  |
| Tablas de almacenamiento datos temporales| Las transacciones de juegos (cambios en los estados de juego) se almacenan temporalmente en tablas. | [Azure Table Storage](https://azure.microsoft.com/services/storage/tables/)| Los datos de juegos pueden almacenarse en un esquema flexible según las necesidades del juego. |
| Poner en cola transacciones y solicitudes de juegos| Las transacciones de juegos se procesan en forma de cola. | [Azure Queue Storage](https://azure.microsoft.com/services/storage/queues/)| Las colas absorben las ráfagas de tráfico inesperadas y pueden impedir que los servidores se sobrecarguen con una avalancha repentina de solicitudes durante el juego.   |
| Base de datos de juegos relacional escalable| Almacenamiento estructurado de datos relacionales, como las transacciones en el juego, en la base de datos. | [Azure SQL Database](https://azure.microsoft.com/services/sql-database/)| Base de datos SQL como servicio ([comparación con SQL en una máquina virtual](https://azure.microsoft.com/documentation/articles/data-management-azure-sql-database-and-sql-server-iaas/)).  |
| Base de datos de juegos escalable y distribuida de baja latencia| Lectura, escritura y consulta rápidas de datos del juego y el reproductor con flexibilidad de esquema. | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)| Base de datos de documentos NoSQL de baja latencia como servicio.   |
| Uso del centro de datos propio con los servicios de Azure | El juego se recupera del propio centro de datos y se envía a los dispositivos cliente. | [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) | Permite a tu organización ofrecer servicios de Azure del propio centro de datos que te ayudarán a mejorar tus logros.  |
| Transferencia de fragmentos de datos de gran tamaño| Los archivos de gran tamaño, como los de imagen, audio y vídeo de juegos, se pueden enviar a los usuarios desde la ubicación emergente más cercana de la Red de entrega de contenido (CDN) con CDN de Azure.    | [Azure Content Delivery Network](https://azure.microsoft.com/services/cdn/) | Basada en una topología de red moderna de grandes nodos centralizadas, la red CDN de Azure controla picos repentinos de tráfico y cargas elevadas para aumentar considerablemente la velocidad y la disponibilidad, lo que da lugar a mejoras significativas de la experiencia del usuario.  |
| Baja latencia.               | Realiza el almacenamiento en caché para compilar juegos rápidos y escalables con más control y aislamiento garantizado de los datos; también puede usarse para mejorar la característica de creación de coincidencias del juego. | [Azure Redis Cache](https://azure.microsoft.com/services/cache/) | Acceso a datos coherente de alto rendimiento y baja latencia para impulsar aplicaciones de Azure rápidas y escalables.  |
| Alta escalabilidad y baja latencia | Controla fluctuaciones en el número de usuarios de juegos con lectura y escritura de baja latencia. | [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | Capacidad de impulsar los escenarios de uso intensivo de datos y baja latencia más complejos, así como de escalar con confianza para controlar más usuarios a la vez. Service Fabric permite compilar juegos sin tener que crear un almacén independiente o una caché, según sea necesario para aplicaciones sin estado. |
| Capacidad para recopilar millones de eventos por segundo de dispositivos.                         | Registrar millones de eventos por segundo de dispositivos. | [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) | Ingesta de telemetría de escala de nube de juegos, sitios web, aplicaciones y dispositivos.  |
| Procesamiento de datos de juegos en tiempo real  | Realizar un análisis en tiempo real de los datos de jugadores para mejorar el juego.| [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) | Procesamiento de transmisiones en tiempo real en la nube.  |
| Desarrollo de un juego predictivo         | Crear un juego dinámico personalizado según los datos de jugadores.  | [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) | Servicio en la nube totalmente administrado que te permite compilar, implementar y compartir fácilmente soluciones de análisis predictivo.  |
| Recopilación y análisis de datos de juegos| Procesamiento en paralelo masivo de datos de bases de datos relacionales y no relacionales. | [Azure Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/)| Almacenamiento de datos elástico como servicio con funcionalidades de clase empresarial.   |
| Atraer a los usuarios para aumentar el uso y la retención| Envíe notificaciones de envío de destino a cualquier plataforma desde cualquier back-end para generar interés y fomentar acciones de juego específicas | [Centros de notificaciones de Azure](https://azure.microsoft.com/services/notification-hubs/)| Difusión rápida para llegar a millones de dispositivos móviles en todas las plataformas principales: &mdash; iOS, Android, Windows, Kindle, Baidu. El juego se puede hospedar en cualquier &mdash; nube de back-end o en un entorno local.|
| Transmita contenido multimedia a sus audiencias locales e internacionales mientras protege su contenido| Los juegos de calidad de difusión y los clips cinematográficos se pueden inspeccionar desde todos los dispositivos| [Azure Media Services](https://azure.microsoft.com/services/media-services/)| Streaming de vídeo a petición y en directo con capacidades de Content Delivery Network integradas. Usar un reproductor para todas sus necesidades de reproducción, incluye protección y cifrado de contenido.| 
| Desarrollo, distribución y prueba de versiones beta de las aplicaciones móviles | Pruebe y distribuya su aplicación móvil. Rendimiento de la aplicación y administración de la experiencia del usuario. | [HockeyApp](https://azure.microsoft.com/services/hockeyapp/)| Integra informes de bloqueo y métricas de usuario con una plataforma de distribución de aplicaciones y comentarios de usuario. Admite aplicaciones de Android, Cordova, iOS, OS X, Unity, Windows y Xamarin. Además, considere el control de misión de [Visual Studio Mobile Center](https://visualstudio.microsoft.com/app-center/) &mdash; para aplicaciones que combine análisis enriquecidos, informes de errores, notificaciones de envío, distribución de aplicaciones y mucho más. |
| Creación de campañas de marketing para aumentar el uso y la retención  | Enviar notificaciones de inserción a los reproductores de destino para generar interés y fomentar acciones específicas del juego según el análisis de datos. | [Mobile Engagement](https://azure.microsoft.com/services/mobile-engagement/) : se retirará el 2018 de marzo y actualmente solo está disponible para los clientes existentes. |  Aumentar el tiempo de juego y la retención del usuario en las principales plataformas (iOS, Android, Windows y Windows Phone). |


##  <a name="startup-and-developer-resources"></a>Recursos para nuevas empresas y desarrolladores

* [Microsoft for Startups](https://startups.microsoft.com)

    Microsoft for Startups ofrece ventajas de productos, técnicas y de comercialización para ayudar a acelerar el crecimiento de las startups. Una ventaja incluye obtener una cuenta gratuita de Azure. Tiene $200 crédito para explorar servicios durante 30 días, 12 meses de servicios gratuitos populares y siempre hay más de 25 servicios gratis. Para obtener más información, consulte [Cómo hacer que las ideas de su inicio cobren vida con una cuenta gratuita de Azure](https://azure.microsoft.com/free/startups/).
    
* [Programas para desarrolladores](e2e.md#developer-programs)

    Microsoft ofrece varios programas para desarrolladores como [ID@Xbox](https://www.xbox.com/Developers/id) y el programa de creadores de [Xbox Live](https://developer.microsoft.com/games/xbox/xboxlive/creator) para ayudarle a desarrollar y publicar juegos.

## <a name="learning-resources"></a>Recursos de aprendizaje

* compilación 2016: [CodeLabs &mdash; usar Microsoft Azure App Service y back-end de Microsoft SQL Azure para ahorrar puntuación de juego en Unity](https://github.com/Microsoft-Build-2016/CodeLabs-GameDev-6-Azure)
* compilación 2017: [entrega de experiencias de juegos de primera clase mediante Microsoft Azure: lecciones aprendidas de títulos como Halo, Hitman y caminar inactivas (vídeo)](https://channel9.msdn.com/Events/Build/2017/P4062)
* Conjunto reutilizable de bloques de creación, proyectos, servicios y procedimientos recomendados diseñados para admitir cargas de trabajo de juegos comunes con Azure en GitHub: [bloques de creación para juegos en Azure](https://github.com/MicrosoftDX/nether)
* [Servicios de juegos en Azure (vídeos)](https://channel9.msdn.com/Series/Gaming-Services-on-Azure)

## <a name="tools-and-other-useful-links"></a>Herramientas y otros vínculos útiles

* [Foros de MSDN &mdash; plataforma de Azure](https://social.msdn.microsoft.com/Forums/azure/home?category=windowsazureplatform)
* [Herramienta de prueba de carga basada en la nube](https://visualstudio.microsoft.com/team-services/cloud-load-testing/)
* [SDK y herramientas de línea de comandos](https://azure.microsoft.com/downloads/)
    
## <a name="software-as-a-service-for-game-backend"></a>Software como servicio para el back-end del juego

[Playfab](https://playfab.com/) actualmente impulsa más de 1.200 juegos en vivo con 80 millones reproductores activos mensuales. Se trata de una plataforma de back-end completa que incluye LiveOps de pila completa con control en tiempo real. 

Puede integrar esta solución en sus juegos móviles, PC o de consola mediante los SDK. Hay SDK disponibles para todas las plataformas y motores de juegos populares, como Android, iOS, Unreal, Unity y Windows. Para empezar, vea la [documentación](https://api.playfab.com/)de.

Ofrece servicios de juegos como autenticación, administración de datos del reproductor, varios jugadores y análisis en tiempo real para ayudar a su juego a aumentar su base de usuarios. Aproveche el potencial de la canalización de datos en tiempo real y LiveOps para atraer a los usuarios con elementos, eventos y promociones personalizados en el juego. También tiene la posibilidad de realizar pruebas A/B, generar informes, enviar notificaciones de envío y mucho más. 

Estamos innovando y agregando nuevas características constantemente. Para obtener más información, consulte [características](https://playfab.com/features/) y precios, consulte [precios sencillos que se escalan con usted](https://playfab.com/pricing/).

## <a name="related-links"></a>Vínculos relacionados

* [Guía de desarrollo de juegos de Windows 10](https://docs.microsoft.com/windows/uwp/gaming/e2e)
* [Azure para juegos](https://azure.microsoft.com/solutions/gaming/)
* [Playfab](https://playfab.com/)
* [Microsoft for Startups](https://startups.microsoft.com)
* [ID@Xbox](https://www.xbox.com/Developers/id)


 

 
