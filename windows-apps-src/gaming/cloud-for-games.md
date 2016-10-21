---
author: joannaleecy
title: Uso de servicios en la nube con juegos para UWP
description: "Obtén más información acerca de cómo implementar la nube como un back-end para tus juegos para UWP."
ms.assetid: 1a7088e0-0d7b-11e6-8e05-0002a5d5c51b
translationtype: Human Translation
ms.sourcegitcommit: 0b2d81daa8bd0fd5694b81fa14fcd064e1600d35
ms.openlocfilehash: b23c33fac9ac8fe5e2d5563a0af6824c82a3969b

---
#  Uso de servicios en la nube con juegos para UWP

La Plataforma universal de Windows (UWP) de Windows 10 ofrece un conjunto de API que pueden usarse para desarrollar juegos en todos los dispositivos de Microsoft. Al desarrollar juegos en todos los dispositivos y plataformas, puede usar un back-end en la nube para ayudar a escalar el juego según la demanda.

##  ¿Qué es la informática de nube?

La informática de nube usa recursos y aplicaciones de TI a petición a través de Internet para almacenar y procesar los datos de los dispositivos. El término _nube_ es una metáfora de la disponibilidad de la enorme cantidad de recursos disponibles (recursos no locales) a los que puedes acceder desde ubicaciones no específicas.
El principio de la informática de nube ofrece una nueva manera de consumir los recursos y el software. Los usuarios ya no tienen que pagar por el producto o los recursos completos de antemano, pero en su lugar pueden consumir la plataforma, el software y los recursos como un servicio. A menudo, los proveedores de nube facturan a sus clientes según el uso o las ofertas del plan de servicio.

##  Motivos para usar servicios en la nube

Una ventaja de usar los servicios en la nube para juegos es que es necesario invertir en servidores de hardware físicos de antemano, si no que solo se debe pagar según el uso o los planes de servicio posteriormente. Es una manera de ayudar a administrar los riesgos que implica el desarrollo de un nuevo título de juego. 

Otra ventaja es que el juego puede aprovechar la gran variedad de recursos de nube para lograr escalabilidad (administrar eficazmente los picos repentinos en el número de jugadores simultáneos, los cálculos de juegos intensos en tiempo real o los requisitos de datos). Esto hace que el rendimiento de juego sea siempre estable. Además, los recursos de nube son accesibles desde cualquier dispositivo que se ejecute en cualquier plataforma y en cualquier lugar del mundo, lo que significa que puede poner el juego a disposición de todo el mundo.

Ofrecer una experiencia de juego increíble a los jugadores es importante. Dado que los servidores de juegos que se ejecutan en la nube son independientes de las actualizaciones de cliente, pueden ofrecer un entorno más seguro y controlado para tu juego en general.   También puedes lograr la coherencia del juego a través de la nube, para lo cual nunca debes confiar en el cliente y tener la lógica del juego del lado servidor. Las conexiones de servicio a servicio también pueden configurarse para permitir una experiencia de juego más integrada; algunos ejemplos incluyen los vínculos de compras desde el juego a diversos métodos de pago, los puentes en distintas redes de juegos y las actualizaciones desde el juego compartidas en portales de redes sociales populares, como Facebook y Twitter. 

También puedes usar servidores de nube dedicados para crear un enorme mundo de juego persistente, crear una comunidad de jugadores, recopilar y analizar datos de jugadores en el tiempo para mejorar el juego y optimizar el modelo de diseño de rentabilidad del juego.

Además, los juegos que requieren funciones de administración de datos de juegos intensivos, como juegos sociales con mecanismos multijugador asincrónicos, pueden implementarse mediante servicios en la nube.

##  ¿Cómo usan las empresas de juegos la tecnología de nube?

Descubre cómo otros desarrolladores han implementado soluciones en la nube en sus juegos.

<table>
    <colgroup>
    <col width="10%" />
    <col width="30%" />
    <col width="30%" />
    <col width="30%" />
    </colgroup>
    <tr class="header">
        <th>Desarrollador</th>
        <th>Descripción</th>
        <th>Escenarios clave de juegos</th>
        <th>Más información</th>
    </tr>
    <tr>
        <td>[343 Industries](https://www.halowaypoint.com/)</td>
        <td>_Halo 5: Guardians_ implementó [Halo: Spartan Companies](https://www.halowaypoint.com/spartan-companies) como su plataforma social de juegos con el uso de Microsoft Azure DocumentDB, que fue elegido por su velocidad y flexibilidad debido a sus funciones de indexación automática.</td>
        <td>
            <ul>
                <li>Capa de datos escalable para controlar la administración y creación de grupos para juegos multijugador <li>Integración de juegos y redes sociales <li>Consultas en tiempo real de datos a través de varios atributos <li>Sincronización de logros y estadísticas de juegos </ul>
        </td>
        <td>
            <ul>
                <li>[Juego social implementado con Azure DocumentDB](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/)</td>
            </ul>
    </tr>
    <tr>
        <td>[Illyriad Games](http://web.ageofascent.com/)</td>
        <td>Illyriad Games creó _Ages of Ascent_, un juego en línea multijugador masivo (MMOG) de espacio 3D épico que puede reproducirse en dispositivos con exploradores modernos. Por lo tanto, este juego se puede reproducir en equipos, portátiles, teléfonos móviles y otros dispositivos móviles sin complementos. El juego usa ASP.NET Core, HTML5, WebGL y Microsoft Azure.</td>
        <td>
            <ul>
                <li>Juego multiplataforma basado en explorador <li>Un enorme mundo persistente abierto y único <li>Controla los cálculos de juegos intensivos en tiempo real <li>Escala con el número de jugadores </ul>
        </td>
        <td>
            <ul>
                <li>[Administrar componentes de juegos como microservicios con Azure Service Fabric (vídeo)](https://channel9.msdn.com/Events/Build/2016/KEY02#time=57m20s)  
                <li>[Entrevista con los desarrolladores de Age of Ascent (vídeo)](https://channel9.msdn.com/Shows/Azure-Friday/Age-of-Ascent-from-Illyriad-Powered-by-Azure-Service-Fabric-and-ASPNET)
            </ul>
        </td>
    </tr>
    <tr>
        <td>[Next Games](http://www.nextgames.com/)</td>
        <td>Next Games es el creador del videojuego _The Walking Dead: No Man's Land_ basado en la serie original de AMC. El juego Walking Dead usó Azure como back-end. Se realizaron 1.000.000 descargas el fin de semana del lanzamiento y, la primera semana, el juego se convirtió en la aplicación gratuita número 1 para iPhone y iPad en el App Store de Estados Unidos, la aplicación gratuita número 1 en 12 países y el juego gratuito número 1 en 13 países.
        </td>
        <td>
            <ul>
                <li>Multiplataforma <li>Multijugador por turnos <li>Escalar rendimiento de forma elástica </ul>
        </td>
        <td>
            <ul>
                <li>[Entrevista con Kalle Hiitola, CTO de Next Games (vídeo)](https://channel9.msdn.com/Blogs/AzureDocumentDB/azure-documentdb-walking-dead)
                <li>[Walking Dead usa DocumentDB para ofrecer un ciclo de desarrollo más rápido y un juego más atractivo.](https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/)
            </ul>
    </tr>
    </td>
        <td>[Pixel Squad](http://www.crimecoast.com/)</td>
        <td>Pixel Squad desarrolló _Crime Coast_ con el motor de juego de Unity y Azure. _Crime Coast_ es un juego de estrategia social disponible en las plataformas Android, iOS y Windows. En su juego se usó lo siguiente: Almacenamiento de blobs de Azure, Caché en Redis de Azure administrada, una matriz de máquinas virtuales IIS de carga equilibrada y el centro de notificaciones de Microsoft. Descubre cómo administraron la escala y controlaron el aumento de jugadores con 5000 jugadores simultáneos.
        </td>
        <td>
            <ul>
                <li>Multiplataforma <li>Juego multijugador en línea <li>Escala con el número de jugadores </ul>
        </td>
        <td>
            <ul>
                <li>[Cómo usó el juego MMOG Crime Coast los Servicios en la nube de Azure](https://channel9.msdn.com/Blogs/The-Game-Blog/BizSpark-Interview-with-Pixel-Squad-How-the-used-Azure-Cloud-Services-to-make-an-MMO-with-a-3-man-te)
            </ul>
        </td>
    </tr> 
</table>

    
### Otros vínculos

* [Azure como la salsa secreta de Hitcents, Game Troopers e InnoSpark](http://news.microsoft.com/features/game-developers-use-microsoft-azure-as-secret-sauce-for-scale-and-growth-2/)
* [Inicios de juegos en el programa Bizspark con Azure](https://blogs.technet.microsoft.com/bizspark_featured_startups/2015/09/25/azure-open-for-gaming-startups/)


## Cómo diseñar el back-end en la nube

Mientras que los productores y diseñadores juegos discuten qué características y funcionalidades del juego son necesarias, es recomendable que comiences a considerar cómo quieres diseñar tu infraestructura de juego. La nube de Azure puede usarse como el back-end de juego si quieres desarrollar juegos para varios dispositivos y en las distintas plataformas principales.

### Guías de aprendizaje paso a paso

* [Build 2016 Codelabs: Use Microsoft Azure App Service and Microsoft SQL Azure backend to save game score (Build 2016 - Codelabs: Usar el Servicio de aplicaciones de Microsoft Azure y el back-end de Microsoft SQL Azure para guardar la puntuación del juego)](https://github.com/Microsoft-Build-2016/CodeLabs-GameDev-6-Azure)
* [Diseñar la estrategia de interacción móvil del juego](https://azure.microsoft.com/documentation/articles/mobile-engagement-gaming-scenario/)
* [Uso de Azure Mobile Engagement para la implementación de Unity iOS](https://azure.microsoft.com/documentation/articles/mobile-engagement-unity-ios-get-started/)

### Descripción de IaaS, PaaS o SaaS

En primer lugar, debes pensar en el nivel de servicio más idóneo para tu juego. Conocer las diferencias de los siguientes tres servicios puede ayudarte a determinar el enfoque que quieres para crear el back-end.

* [Infraestructura como servicio (IaaS)](https://azure.microsoft.com/overview/what-is-iaas/)

    Infraestructura como servicio (IaaS) es una infraestructura informática instantánea aprovisionada y administrada a través de Internet. Imagina tener la posibilidad de tener muchos equipos disponibles para escalar y reducir verticalmente y con rapidez según la demanda. IaaS te ayuda a evitar el costo y la complejidad de comprar y administrar tus propios servidores físicos y otras infraestructuras de centro de datos.

* [Plataforma como servicio (PaaS)](https://azure.microsoft.com/overview/what-is-paas/)

    Plataforma como servicio (PaaS) es como IaaS, pero también incluye la administración de infraestructuras, como servidores, almacenamiento y redes. Así, además de no tener que comprar servidores físicos e infraestructura de centro de datos, tampoco tendrás que comprar ni administrar licencias de software, una infraestructura de aplicaciones subyacente, software intermedio, herramientas de desarrollo u otros recursos.

* Software como servicio (SaaS)

    Software como servicio suele referirse a una aplicación ya creada para ti y hospedada en una plataforma en la nube existente. Se ha diseñado para que aún te resulte más fácil empezar a ejecutar el juego en sus servicios.


### Diseñar la infraestructura de juego con Azure

A continuación se indican algunas maneras de usar las ofertas de la nube de Azure para un juego. Azure funciona con Windows y Linux, así como con las tecnologías de código abierto conocidas, como Ruby, Python, Java y PHP. Para obtener más información, consulta el sitio web de la [nube de Azure](https://azure.microsoft.com).

| Requisitos                 | Escenarios de actividad                            | Oferta de productos                      | Funcionalidades de productos                               |
|-----------------------------------|-----------------------------------------------|---------------------------------------|----------------------------------------------------|
| Hospedar el dominio en la nube.     | Responder a las consultas DNS de manera eficaz.            | [DNS de Azure](https://azure.microsoft.com/services/dns/) | Hospedar el dominio con alto rendimiento y disponibilidad  |
| Iniciar sesión, verificación de identidad      | El jugador inicia sesión y se autentica la identidad del jugador.  | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) | Inicio de sesión único a cualquier aplicación web en la nube y local con la autenticación multifactor.            |
| Juego con el modelo de infraestructura como servicio (IaaS)      | El juego se hospeda en máquinas virtuales en la nube.       | [VM de Azure](https://azure.microsoft.com/services/virtual-machines/) | Escala de 1 a miles de instancias de máquina virtual como servidores de juegos con redes virtuales integradas y equilibrio de carga; coherencia híbrida con sistemas locales.           |
| Juegos web o móviles con el modelo de plataforma como servicio (PaaS)            | El juego se hospeda en una plataforma administrada.                | [Servicio de aplicaciones de Azure](https://azure.microsoft.com/services/app-service/) | PaaS para sitios web o juegos móviles (lo que se refiere a las VM de Azure con software intermedio, herramientas de desarrollo, BI y administración de bases de datos).   |
| Almacenamiento en la nube de datos de juegos       | Los datos de los juegos más recientes se almacenan en la nube y se envían a dispositivos cliente. | [Almacenamiento de blobs de Azure](https://azure.microsoft.com/services/storage/blobs/)| Sin limitación de los tipos de archivo que se pueden almacenar; almacenamiento de objetos para grandes cantidades de datos sin estructurar, como imágenes, audio, vídeo y mucho más.  |
| Tablas de almacenamiento datos temporales| Las transacciones de juegos (cambios en los estados de juego) se almacenan temporalmente en tablas. | [Almacenamiento de tablas de Azure](https://azure.microsoft.com/services/storage/tables/)| Los datos de juegos pueden almacenarse en un esquema flexible según las necesidades del juego. |
| Poner en cola transacciones y solicitudes de juegos| Las transacciones de juegos se procesan en forma de cola. | [Almacenamiento en cola de Azure](https://azure.microsoft.com/services/storage/queues/)| Las colas absorben las ráfagas de tráfico inesperadas y pueden impedir que los servidores se sobrecarguen con una avalancha repentina de solicitudes durante el juego.   |
| Base de datos de juegos relacional escalable| Almacenamiento estructurado de datos relacionales, como las transacciones en el juego, en la base de datos. | [Base de datos SQL de Azure](https://azure.microsoft.com/services/sql-database/)| Base de datos SQL como servicio ([comparación con SQL en una máquina virtual](https://azure.microsoft.com/documentation/articles/data-management-azure-sql-database-and-sql-server-iaas/)).  |
| Base de datos de juegos escalable y distribuida de baja latencia| Lectura, escritura y consulta rápidas de datos del juego y el reproductor con flexibilidad de esquema. | [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)| Base de datos de documentos NoSQL de baja latencia como servicio.   |
| Uso del centro de datos propio con los servicios de Azure | El juego se recupera del propio centro de datos y se envía a los dispositivos cliente. | [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) | Permite a tu organización ofrecer servicios de Azure del propio centro de datos que te ayudarán a mejorar tus logros.  |
| Transferencia de fragmentos de datos de gran tamaño| Los archivos de gran tamaño, como los de imagen, audio y vídeo de juegos, se pueden enviar a los usuarios desde la ubicación emergente más cercana de la Red de entrega de contenido (CDN) con CDN de Azure.    | [Red de entrega de contenido de Azure](https://azure.microsoft.com/services/cdn/) | Basada en una topología de red moderna de grandes nodos centralizadas, la red CDN de Azure controla picos repentinos de tráfico y cargas elevadas para aumentar considerablemente la velocidad y la disponibilidad, lo que da lugar a mejoras significativas de la experiencia del usuario.  |
| Baja latencia               | Realiza el almacenamiento en caché para compilar juegos rápidos y escalables con más control y aislamiento garantizado de los datos; también puede usarse para mejorar la característica de creación de coincidencias del juego. | [Caché en Redis de Azure](https://azure.microsoft.com/services/cache/) | Acceso a datos coherente de alto rendimiento y baja latencia para impulsar aplicaciones de Azure rápidas y escalables.  |
| Alta escalabilidad y baja latencia | Controla fluctuaciones en el número de usuarios de juegos con lectura y escritura de baja latencia. | [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | Capacidad de impulsar los escenarios de uso intensivo de datos y baja latencia más complejos, así como de escalar con confianza para controlar más usuarios a la vez. Service Fabric permite compilar juegos sin tener que crear un almacén independiente o una caché, según sea necesario para aplicaciones sin estado. |
| Capacidad para recopilar millones de eventos por segundo de dispositivos.                         | Registrar millones de eventos por segundo de dispositivos. | [Centros de eventos de Azure](https://azure.microsoft.com/services/event-hubs/) | Ingesta de telemetría de escala de nube de juegos, sitios web, aplicaciones y dispositivos.  |
| Procesamiento de datos de juegos en tiempo real  | Realizar un análisis en tiempo real de los datos de jugadores para mejorar el juego.| [Análisis de transmisiones de Azure](https://azure.microsoft.com/services/stream-analytics/) | Procesamiento de transmisiones en tiempo real en la nube.  |
| Desarrollo de un juego predictivo         | Crear un juego dinámico personalizado según los datos de jugadores.  | [Aprendizaje automático de Azure](https://azure.microsoft.com/services/machine-learning/) | Servicio en la nube totalmente administrado que te permite compilar, implementar y compartir fácilmente soluciones de análisis predictivo.  |
| Recopilación y análisis de datos de juegos| Procesamiento en paralelo masivo de datos de bases de datos relacionales y no relacionales. | [Almacenamiento de datos de Azure](https://azure.microsoft.com/services/sql-data-warehouse/)| Almacenamiento de datos elástico como servicio con funcionalidades de clase empresarial.   |
| Creación de campañas de marketing para aumentar el uso y la retención  | Enviar notificaciones de inserción a los reproductores de destino para generar interés y fomentar acciones específicas del juego según el análisis de datos. | [Interacción móvil](https://azure.microsoft.com/services/mobile-engagement/) |  Aumentar el tiempo de juego y la retención del usuario en las principales plataformas (iOS, Android, Windows y Windows Phone). |


##  Recursos para nuevas empresas y desarrolladores

* [Microsoft BizSpark](https://www.microsoft.com/bizspark/)

    Microsoft BizSpark es un programa global que ayuda a las nuevas empresas a ofrecer acceso gratuito a soporte técnico, software y servicios en la nube de Azure. Los miembros de BizSpark reciben cinco suscripciones de Visual Studio Enterprise con MSDN, cada una con un crédito de Azure de 150 USD mensuales. Esto suma un total de 750 USD al mes entre los cinco desarrolladores para invertir en servicios de Azure. BizSpark está disponible para nuevas empresas privadas, de menos de 5 años de antigüedad con unos ingresos anuales inferiores a 1 millón USD. Microsoft cree que, al ayudar a las nuevas empresas, contribuye a entablar una relación de gran valor a largo plazo.
    
* [ID@Xbox](http://www.xbox.com/Developers/id)

    Si quieres agregar características de Xbox Live, como un juego multijugador, matchmaking multiplataforma, puntuación de jugador, logros y marcadores a tu juego de Windows 10, suscríbete con ID@Xbox para obtener las herramientas y el soporte técnico que necesitas para dar rienda suelta a tu creatividad y maximizar tu éxito. Antes de realizar la solicitud a ID@Xbox, registra una cuenta de desarrollador en el [Centro de desarrollo de Windows](https://developer.microsoft.com/windows/programs/join).

## Software como servicio para el back-end del juego

Estas son algunas de las empresas que ofrecen back-end de nube para los juegos basado en los principales proveedores de servicios en la nube para permitirte centrarte en el desarrollo de tu juego.

* [GameSparks](http://www.gamesparks.com/)

    GameSparks es una plataforma de desarrollo basada en la nube para desarrolladores de juegos que les permite crear el lado de servidor de sus juegos.

* [Photon Engine](https://www.photonengine.com/en/Photon)

    Photon es una plataforma multijugador y de motor de redes independiente para juegos. Incluye Photon Cloud, que ofrece software como servicio (SaaS) y, como tal, es un servicio totalmente administrado. Puedes concentrarte completamente en el cliente de la aplicación mientras la hospedas; las operaciones de servidor y ajuste de escala las lleva a cabo Exit Games.

* [Playfab](https://playfab.com/)

    Playfab lleva tecnología excepcional de administración de juegos dinámicos y back-end a su móvil, PC o videoconsola de manera rápida y sencilla.

## Vínculos relacionados

* [Guía de desarrollo de juegos para Windows 10](https://msdn.microsoft.com/windows/uwp/gaming/e2e)
* [Microsoft Azure](https://azure.microsoft.com/)
* [Microsoft BizSpark](https://www.microsoft.com/bizspark/)
* [ID@Xbox](http://www.xbox.com/Developers/id)


 

 



<!--HONumber=Aug16_HO3-->


