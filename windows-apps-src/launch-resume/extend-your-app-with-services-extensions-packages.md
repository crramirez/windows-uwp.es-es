---
title: Ampliar la aplicación con servicios de aplicaciones, extensiones y paquetes
description: Aprende a crear una tarea en segundo plano que se ejecute cuando se actualice la aplicación de la tienda de la Plataforma universal de Windows (UWP).
ms.date: 05/7/2018
ms.topic: article
keywords: windows 10, uwp, ampliar, separar por componentes, servicio de aplicaciones, paquete, ampliación
ms.localizationpriority: medium
ms.openlocfilehash: 110fa8ec9feba1f54155d41b4c726ffb28f71630
ms.sourcegitcommit: 681c70f964210ab49ac5d06357ae96505bb78741
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2018
ms.locfileid: "7703079"
---
# <a name="extend-your-app-with-services-extensions-and-packages"></a>Ampliar tu aplicación con servicios de aplicaciones, extensiones y paquetes

Hay distintas tecnologías en Windows 10 que te ayudarán a ampliar y separar tu aplicación por componentes. Esta tabla debe ayudarte a determinar qué tecnología debes usas para tu escenario. Le sigue un breve descripción de los escenarios y las tecnologías.

| Escenario                           | Paquete de recursos   | Paquete de activos      | Paquete opcional   | Lote plano        | Extensión de aplicación      | Servicio de aplicaciones        | Instalación en streaming  |
|------------------------------------|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|
| Complementos de código de terceros            |                    |                    |                    |                    | :heavy_check_mark: |                    |                    |
| Complementos de código dentro del proceso              |                    |                    | :heavy_check_mark: |                    |                    |                    |                    |
| Activos de la experiencia de usuario (cadenas o imágenes)         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Contenido a petición <br/> (por ejemplo, niveles adicionales) |      |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Licencias independientes y adquisición |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: | :heavy_check_mark: |                    |
| Adquisición desde la aplicación                 |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |
| Optimizar el tiempo de instalación              | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Reducir la superficie del disco              | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |                    |                    |
| Optimizar el empaquetado                 |                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |
| Reducir el tiempo de publicación             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |

## <a name="scenario-descriptions-the-rows-in-the-table-above"></a>Descripciones de escenarios (filas de la tabla anterior)

**Complementos de terceros**  

Código que se puede descargar de la tienda y ejecutarse desde tu aplicación. Por ejemplo, extensiones para el explorador Microsoft Edge.

**Complementos de código dentro del proceso**  

Código que se ejecuta dentro de proceso con la aplicación. Solo se admite C++. También puede incluir contenido. Dado que el código se ejecuta en proceso, se supone un mayor nivel de confianza. Puedes elegir no abrir este tipo de extensibilidad a un tercero.

**Activos de la experiencia de usuario (cadena o imágenes)**  

Activos de la interfaz de usuario, como cadenas localizadas, imágenes y cualquier otro contenido de la interfaz de usuario que quieras repartir en función de la configuración regional o de cualquier otro motivo.

**Contenido a petición**  

Contenido que quieres descargar en un momento posterior. Por ejemplo, en compras desde la aplicación que te permitan descargar nuevos niveles, máscaras o funcionalidad.

**Licencias independientes y adquisición**  

La capacidad de conceder licencias y adquirir el contenido independientemente de la aplicación.

**Adquisición desde la aplicación**  

Indica si hay soporte de programación para adquirir el contenido desde dentro de la aplicación.

**Optimizar el tiempo de instalación**

Proporciona funcionalidad para reducir el tiempo que tarda en adquirir la aplicación en la tienda y empezar la ejecución.

**Reducir la superficie del disco** Reduce el tamaño de una aplicación incluyendo solo las aplicaciones o los recursos necesarios.

**Optimizar el empaquetado** Optimiza el proceso de empaquetado de la aplicación para aplicaciones complejas o a gran escala.

**Reducir el tiempo de publicación** Minimizar la cantidad de tiempo que se tarda en publicar la aplicación en la Store, el recurso compartido local o el servidor web.

## <a name="technology-descriptions-the-columns-in-the-table-above"></a>Descripciones de tecnología (las columnas de la tabla anterior)

**Paquete de recursos**

Los paquetes de recursos son paquetes únicamente de activos que permiten a tu aplicación adaptarse a varios tamaños de pantalla e idiomas del sistema. El paquete de recursos tiene como objetivo el idioma del usuario, la escala de sistema y características de DirectX, lo que permite a la aplicación adaptarse a una variedad de escenarios de usuario. Aunque un paquete de la aplicación puede contener varios recursos, el sistema operativo solo descargará los recursos pertinentes por dispositivo de usuario, lo que ahorrará espacio en disco y ancho de banda.

**Paquete de activos** Los paquetes de activos tienen una fuente centralizada común de archivos ejecutables o no ejecutable usados por la aplicación. Estos son normalmente archivos específicos de idioma o no de procesador. Por ejemplo, esto podría incluir una colección de imágenes en un paquete de activos y vídeos de otro paquete de activos, ambos usados por la aplicación. Por ejemplo, esto podría incluir una colección de imágenes en un paquete de activos y vídeos de otro paquete de activos. Si tu aplicación admite varias arquitecturas y varios idiomas, estos activos podrían incluirse en el paquete de la arquitectura o el paquete de recursos, pero eso también significa que los activos se duplicarían varias veces en los paquetes de arquitectura diferentes ocupando espacio en disco. Si se usan paquetes de activos, solo hay que incluirlos una vez en el paquete general de la aplicación. Consulta [Introducción a los paquetes de activos](../packaging/asset-packages.md) para obtener más información.

**Paquete opcional**

Los paquetes opcionales se usan para complementar o ampliar la funcionalidad original de un paquete de la aplicación. Es posible publicar una aplicación, seguida de la publicación de paquetes opcionales posteriormente, o publicar tanto la aplicación como paquetes opcionales al mismo tiempo. Al ampliar la aplicación mediante un paquete opcional, tienes las ventajas de distribuir y rentabilizar el contenido como paquete de la aplicación independiente. Los paquetes opcionales está pensados normalmente para ser desarrollados por el desarrollador de aplicaciones, ya que se ejecutan con la identidad de la aplicación principal (a diferencia de las extensiones de para aplicaciones). En función de cómo definas el paquete opcional, puedes cargar el código, los activos, o el código y los activos desde el paquete opcional a la aplicación principal. Si buscas mejorar tu aplicación con contenido que se pueda rentabilizar, para el que se pueda conceder licencia y distribuirse por separado, los paquetes opcionales podrían ser la elección correcta para ti. Para obtener detalles de implementación, consulta [Paquetes opcionales y creación de conjuntos relacionados](https://docs.microsoft.com/windows/uwp/packaging/optional-packages).

**Lote plano**
[Paquetes de aplicación de lote plano](../packaging/flat-bundles.md) son similares a los lotes de aplicaciones normales, salvo que en lugar de incluir todos los paquetes de aplicación dentro de la carpeta, el lote plano solo contiene *referencias* a esos paquetes de la aplicación. Al contener referencias a los paquetes de la aplicación en lugar de los propios archivos, un lote plano reducirá la cantidad de tiempo que se tarda en empaquetar y descargar una aplicación.

**Extensión de aplicación**

[Extensiones de aplicación](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions) permite que tu aplicación para UWP hospede contenido proporcionado por otras aplicaciones para UWP. Descubre, enumera y accede a contenido de solo lectura desde dichas aplicaciones.

Si una aplicación admite extensiones, cualquier desarrollador puede enviar una extensión para la aplicación. Por tanto, la aplicación host debe ser eficaz cuando cargue una extensión con la que no se haya probado previamente. Las extensiones deben considerarse como de no confianza.

Las aplicaciones no pueden cargar código de las extensiones. Si necesitas ejecución de código, ten en cuenta App Services.

**Servicio de aplicaciones**

Los servicios de aplicaciones de Windows permiten la comunicación entre aplicaciones al permitir que tu aplicación para UWP proporcione servicios a otras aplicaciones universales de Windows. Los servicios de aplicaciones le permiten crear servicios que no tienen interfaz de usuario que las aplicaciones pueden llamar en el mismo dispositivo y, a partir de Windows 10, versión 1607, en dispositivos remotos. Consulta [Crear y usar un servicio de aplicación](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service) para obtener más información.

Los servicios de aplicaciones son aplicaciones para UWP que ofrecen servicios a otras aplicaciones para UWP. Son similares a servicios web, en un dispositivo. Un servicio de aplicaciones se ejecuta como tarea en segundo plano en la aplicación host y puede proporcionar su servicio a otras aplicaciones. Por ejemplo, un servicio de aplicaciones podría proporcionar un servicio de escáner de códigos de barras que podrían usar otras aplicaciones. O quizás un conjunto de aplicaciones Enterprise tiene un servicio de aplicación de corrector ortográfico que está disponible para las demás aplicaciones del conjunto de aplicaciones.

**Instalación en streaming de aplicaciones para UWP**

La transmisión en streaming es una forma de optimizar la forma en que se entrega la aplicación a los usuarios. En lugar de esperar a que se descargue toda la aplicación antes de usarla, los usuarios pueden interactuar con la aplicación tan pronto como se haya descargado una parte necesaria. Depende de ti, como desarrollador, segmentar la aplicación en una sección requerida para el lanzamiento y la activación básica y contenido adicional para el resto de la aplicación. Consulta [Instalación en streaming para aplicaciones para UWP](https://docs.microsoft.com/windows/uwp/packaging/streaming-install) para obtener más detalles de implementación e información.

## <a name="see-also"></a>Ver también

[Crear y usar un servicio de aplicación](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)  
[Introducción a los paquetes de activos](../packaging/asset-packages.md)  
[Creación del paquete con el diseño del empaquetado](../packaging/packaging-layout.md)  
[Creación de paquetes opcionales y de conjuntos relacionados](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)  
[Desarrollar con paquetes de activos y plegado de paquete](../packaging/package-folding.md)  
[Instalación en streaming de aplicaciones para UWP](https://docs.microsoft.com/windows/uwp/packaging/streaming-install)  
[Paquetes de aplicaciones de lotes planos](../packaging/flat-bundles.md)  
[Espacio de nombres Windows.ApplicationModel.AppService](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService)  
[Espacio de nombres de Windows.ApplicationModel.Extensions](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)  