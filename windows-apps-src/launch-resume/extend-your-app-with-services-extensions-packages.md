---
title: Ampliar la aplicación con servicios, extensiones y paquetes
description: Describe cómo crear una tarea en segundo plano que se ejecuta cuando se actualiza la aplicación de la tienda de Plataforma universal de Windows (UWP).
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, ampliar, separar por componentes, servicio de aplicaciones, paquete, ampliación
ms.localizationpriority: medium
ms.openlocfilehash: d9a98ef8e0ec53668277face05d83c08f6421cb7
ms.sourcegitcommit: c7e10793cbef55ace959ac8fc6ddd08e683602bd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2019
ms.locfileid: "73329507"
---
# <a name="extend-your-app-with-services-extensions-and-packages"></a>Ampliar la aplicación con servicios, extensiones y paquetes

Hay muchas tecnologías en Windows 10 para extender y componen la aplicación. Esta tabla debe ayudarle a determinar qué tecnología debe usar en función de los requisitos. Le sigue un breve descripción de los escenarios y las tecnologías.

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

**Complementos de código en proceso**  

Código que se ejecuta dentro de proceso con la aplicación. También puede incluir contenido. Dado que el código se ejecuta en proceso, se supone un mayor nivel de confianza. Puede optar por no exponer este tipo de extensibilidad a un tercero.

**Recursos de experiencia del usuario (cadena/imágenes)**  

Activos de la interfaz de usuario, como cadenas localizadas, imágenes y cualquier otro contenido de la interfaz de usuario que quieras repartir en función de la configuración regional o de cualquier otro motivo.

**Contenido a petición**  

Contenido que quieres descargar en un momento posterior. Por ejemplo, en compras desde la aplicación que te permitan descargar nuevos niveles, máscaras o funcionalidad.

**Licencias y adquisición independientes**  

La capacidad de conceder licencias y adquirir el contenido independientemente de la aplicación.

**Adquisición en la aplicación**  

Indica si hay soporte de programación para adquirir el contenido desde dentro de la aplicación.

**Optimizar la hora de instalación**

Proporciona funcionalidad para reducir el tiempo que tarda en adquirir la aplicación en la tienda y empezar la ejecución.

**Reducir la superficie del disco** Reduce el tamaño de una aplicación incluyendo solo las aplicaciones o los recursos necesarios.

**Optimizar el empaquetado** Optimiza el proceso de empaquetado de aplicaciones para aplicaciones a gran escala o complejas.

**Reducir el tiempo de publicación** Minimizar la cantidad de tiempo que se tarda en publicar la aplicación en la Store, el recurso compartido local o el servidor web.

## <a name="technology-descriptions-the-columns-in-the-table-above"></a>Descripciones de tecnología (las columnas de la tabla anterior)

**Paquete de recursos**

Los paquetes de recursos son paquetes únicamente de activos que permiten a tu aplicación adaptarse a varios tamaños de pantalla e idiomas del sistema. El paquete de recursos tiene como objetivo el idioma del usuario, la escala de sistema y características de DirectX, lo que permite a la aplicación adaptarse a una variedad de escenarios de usuario. Aunque un paquete de la aplicación puede contener varios recursos, el sistema operativo solo descargará los recursos pertinentes por dispositivo de usuario, lo que ahorrará espacio en disco y ancho de banda.

**Paquete de activos** Los paquetes de recursos son un origen común y centralizado de archivos ejecutables o no ejecutables para su uso por parte de la aplicación. Normalmente son archivos que no son de procesador o específicos del idioma. Por ejemplo, esto podría incluir una colección de imágenes en un paquete de activos y vídeos de otro paquete de activos, ambos usados por la aplicación. Si la aplicación admite varias arquitecturas y varios idiomas, estos recursos podrían incluirse en el paquete de arquitectura o en el paquete de recursos, pero también significa que los recursos se duplicarán varias veces en los diversos paquetes de arquitectura, tomando espacio en disco. Si se usan paquetes de activos, solo hay que incluirlos una vez en el paquete general de la aplicación. Consulta [Introducción a los paquetes de activos](/windows/msix/package/asset-packages) para obtener más información.

**Paquete opcional**

Los paquetes opcionales se usan para complementar o ampliar la funcionalidad original de un paquete de la aplicación. Es posible publicar una aplicación, seguida de la publicación de paquetes opcionales posteriormente, o publicar tanto la aplicación como paquetes opcionales al mismo tiempo. Al ampliar la aplicación mediante un paquete opcional, tienes las ventajas de distribuir y rentabilizar el contenido como paquete de la aplicación independiente. Los paquetes opcionales está pensados normalmente para ser desarrollados por el desarrollador de aplicaciones, ya que se ejecutan con la identidad de la aplicación principal (a diferencia de las extensiones de para aplicaciones). En función de cómo definas el paquete opcional, puedes cargar el código, los activos, o el código y los activos desde el paquete opcional a la aplicación principal. Si necesita mejorar la aplicación con contenido que se puede monetizar, autorizar y distribuir por separado, los paquetes opcionales podrían ser la opción adecuada para usted. Para obtener detalles de implementación, consulta [Paquetes opcionales y creación de conjuntos relacionados](/windows/msix/package/optional-packages).

**Lote plano**
[Paquetes de aplicación de lote plano](/windows/msix/package/flat-bundles.md) son similares a los lotes de aplicaciones normales, salvo que en lugar de incluir todos los paquetes de aplicación dentro de la carpeta, el lote plano solo contiene *referencias* a esos paquetes de la aplicación. Al contener referencias a los paquetes de la aplicación en lugar de los propios archivos, un lote plano reducirá la cantidad de tiempo que se tarda en empaquetar y descargar una aplicación.

**Extensión de aplicación**

[Extensiones de aplicación](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions) permite que tu aplicación para UWP hospede contenido proporcionado por otras aplicaciones para UWP. Descubre, enumera y accede a contenido de solo lectura desde dichas aplicaciones.

Si una aplicación admite extensiones, cualquier desarrollador puede enviar una extensión para la aplicación. Por tanto, la aplicación host debe ser eficaz cuando cargue una extensión con la que no se haya probado previamente. Las extensiones deben considerarse como de no confianza.

Las aplicaciones no pueden cargar código de las extensiones. Si necesitas ejecución de código, ten en cuenta App Services.

**App Service**

Los servicios de aplicaciones de Windows permiten la comunicación de aplicación a aplicación al permitir que la aplicación UWP proporcione servicios a otra aplicación universal de Windows. Los servicios de aplicaciones le permiten crear servicios que no tienen interfaz de usuario que las aplicaciones pueden llamar en el mismo dispositivo y, a partir de Windows 10, versión 1607, en dispositivos remotos. Consulta [Crear y usar un servicio de aplicación](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service) para obtener más información.

Los servicios de aplicaciones son aplicaciones para UWP que ofrecen servicios a otras aplicaciones para UWP. Son análogos a los servicios web en un dispositivo. Un servicio de aplicaciones se ejecuta como tarea en segundo plano en la aplicación host y puede proporcionar su servicio a otras aplicaciones. Por ejemplo, un servicio de aplicaciones podría proporcionar un servicio de escáner de códigos de barras que podrían usar otras aplicaciones. O quizás un conjunto de aplicaciones Enterprise tiene un servicio de aplicación de corrector ortográfico que está disponible para las demás aplicaciones del conjunto de aplicaciones.

**Instalación de streaming de aplicaciones para UWP**

La transmisión en streaming es una forma de optimizar la forma en que se entrega la aplicación a los usuarios. En lugar de esperar a que se descargue toda la aplicación antes de usarla, los usuarios pueden interactuar con la aplicación tan pronto como se haya descargado una parte necesaria. Depende de ti, como desarrollador, segmentar la aplicación en una sección requerida para el lanzamiento y la activación básica y contenido adicional para el resto de la aplicación. Consulta [Instalación en streaming para aplicaciones para UWP](/windows/msix/package/streaming-install) para obtener más detalles de implementación e información.

## <a name="see-also"></a>Consulta también

[Crear y usar un servicio de aplicación](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)  
[Introducción a los paquetes de activos](/windows/msix/package/asset-packages)  
[Creación de paquetes con el diseño de empaquetado](/windows/msix/package/packaging-layout)  
[Creación de paquetes opcionales y conjuntos relacionados](/windows/msix/package/optional-packages)  
[Desarrollo con paquetes de recursos y plegamientos de paquetes](/windows/msix/package/package-folding)  
[Instalación en streaming de aplicaciones para UWP](/windows/msix/package/streaming-install)  
[Paquetes de aplicaciones de agrupación plana](/windows/msix/package/flat-bundles)  
[Espacio de nombres Windows. ApplicationModel. AppService](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService)  
[Espacio de nombres Windows. ApplicationModel. Extensions](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)  
