---
title: Ampliar la aplicación con servicios, extensiones y paquetes
description: Describe cómo crear una tarea en segundo plano que se ejecuta cuando se actualiza la aplicación de tienda Universal Windows Platform (UWP).
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, ampliar, separar por componentes, servicio de aplicaciones, paquete, ampliación
ms.localizationpriority: medium
ms.openlocfilehash: 47ab6491d09775bf86f0f484fc96d85bd07f53a4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590680"
---
# <a name="extend-your-app-with-services-extensions-and-packages"></a>Ampliar la aplicación con servicios, extensiones y paquetes

Hay muchas tecnologías de Windows 10 para extender y crear componentes de la aplicación. Esta tabla le ayudarán a determinar qué tecnología se debe utilizar según los requisitos. Le sigue un breve descripción de los escenarios y las tecnologías.

| Escenario                           | Paquete de recursos   | Paquete de activos      | Paquete opcional   | Lote plano        | Extensión de aplicación      | Servicio de aplicaciones        | Instalación en streaming  |
|------------------------------------|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|
| Los complementos de código de terceros            |                    |                    |                    |                    | :heavy_check_mark: |                    |                    |
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

**Los complementos de terceros**  

Código que se puede descargar de la tienda y ejecutarse desde tu aplicación. Por ejemplo, extensiones para el explorador Microsoft Edge.

**Complementos de código en proceso**  

Código que se ejecuta dentro de proceso con la aplicación. Solo se admite C++. También puede incluir contenido. Dado que el código se ejecuta en proceso, se supone un mayor nivel de confianza. Es posible que elija no exponer este tipo de extensibilidad a un tercero.

**Experiencia de usuario activos (cadena o imágenes)**  

Activos de la interfaz de usuario, como cadenas localizadas, imágenes y cualquier otro contenido de la interfaz de usuario que quieras repartir en función de la configuración regional o de cualquier otro motivo.

**En el contenido a petición**  

Contenido que quieres descargar en un momento posterior. Por ejemplo, en compras desde la aplicación que te permitan descargar nuevos niveles, máscaras o funcionalidad.

**Separar la adquisición y licencias**  

La capacidad de conceder licencias y adquirir el contenido independientemente de la aplicación.

**Adquisición de la aplicación**  

Indica si hay soporte de programación para adquirir el contenido desde dentro de la aplicación.

**Optimizar el tiempo de instalación**

Proporciona funcionalidad para reducir el tiempo que tarda en adquirir la aplicación en la tienda y empezar la ejecución.

**Reducir la superficie del disco** Reduce el tamaño de una aplicación incluyendo solo las aplicaciones o los recursos necesarios.

**Optimizar el empaquetado** optimiza el proceso de empaquetado de aplicaciones para las aplicaciones a gran escala o complejos.

**Reducir el tiempo de publicación** Minimizar la cantidad de tiempo que se tarda en publicar la aplicación en la Store, el recurso compartido local o el servidor web.

## <a name="technology-descriptions-the-columns-in-the-table-above"></a>Descripciones de tecnología (las columnas de la tabla anterior)

**Paquete de recursos**

Los paquetes de recursos son paquetes únicamente de activos que permiten a tu aplicación adaptarse a varios tamaños de pantalla e idiomas del sistema. El paquete de recursos tiene como objetivo el idioma del usuario, la escala de sistema y características de DirectX, lo que permite a la aplicación adaptarse a una variedad de escenarios de usuario. Aunque un paquete de la aplicación puede contener varios recursos, el sistema operativo solo descargará los recursos pertinentes por dispositivo de usuario, lo que ahorrará espacio en disco y ancho de banda.

**Paquete de recurso** los paquetes de activos son una fuente común y centralizada de ejecutable o archivos ejecutables que no son para su uso por la aplicación. Estos suelen ser archivos que no son de procesador o específico del idioma. Por ejemplo, esto podría incluir una colección de imágenes en un paquete de activos y vídeos de otro paquete de activos, ambos usados por la aplicación. Si la aplicación admite varios idiomas y varias arquitecturas, estos recursos se podrían incluir en el paquete de la arquitectura o el paquete de recursos, pero que también significa que los activos se duplicaría varias veces a través de los distintos paquetes de arquitectura, teniendo espacio en disco. Si se usan paquetes de activos, solo hay que incluirlos una vez en el paquete general de la aplicación. Consulta [Introducción a los paquetes de activos](../packaging/asset-packages.md) para obtener más información.

**Paquete opcional**

Los paquetes opcionales se usan para complementar o ampliar la funcionalidad original de un paquete de la aplicación. Es posible publicar una aplicación, seguida de la publicación de paquetes opcionales posteriormente, o publicar tanto la aplicación como paquetes opcionales al mismo tiempo. Al ampliar la aplicación mediante un paquete opcional, tienes las ventajas de distribuir y rentabilizar el contenido como paquete de la aplicación independiente. Los paquetes opcionales está pensados normalmente para ser desarrollados por el desarrollador de aplicaciones, ya que se ejecutan con la identidad de la aplicación principal (a diferencia de las extensiones de para aplicaciones). En función de cómo definas el paquete opcional, puedes cargar el código, los activos, o el código y los activos desde el paquete opcional a la aplicación principal. Si necesita mejorar su aplicación con contenido que se puede monetizar, con licencia, y los paquetes distribuidos por separado, es opcionales, a continuación, podrían ser la opción adecuada para usted. Para obtener detalles de implementación, consulta [Paquetes opcionales y creación de conjuntos relacionados](https://docs.microsoft.com/windows/uwp/packaging/optional-packages).

**Lote plano**
[Paquetes de aplicación de lote plano](../packaging/flat-bundles.md) son similares a los lotes de aplicaciones normales, salvo que en lugar de incluir todos los paquetes de aplicación dentro de la carpeta, el lote plano solo contiene *referencias* a esos paquetes de la aplicación. Al contener referencias a los paquetes de la aplicación en lugar de los propios archivos, un lote plano reducirá la cantidad de tiempo que se tarda en empaquetar y descargar una aplicación.

**Extensión de la aplicación**

[Extensiones de aplicación](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions) permite que tu aplicación para UWP hospede contenido proporcionado por otras aplicaciones para UWP. Descubre, enumera y accede a contenido de solo lectura desde dichas aplicaciones.

Si una aplicación admite extensiones, cualquier desarrollador puede enviar una extensión para la aplicación. Por tanto, la aplicación host debe ser eficaz cuando cargue una extensión con la que no se haya probado previamente. Las extensiones deben considerarse como de no confianza.

Las aplicaciones no pueden cargar código de las extensiones. Si necesitas ejecución de código, ten en cuenta App Services.

**Aplicación de servicio**

Servicios de aplicaciones de Windows habilita la comunicación de aplicación a la aplicación al permitir que la aplicación UWP proporcionar servicios a otra aplicación de Windows Universal. Los servicios de aplicaciones le permiten crear servicios que no tienen interfaz de usuario que las aplicaciones pueden llamar en el mismo dispositivo y, a partir de Windows 10, versión 1607, en dispositivos remotos. Consulta [Crear y usar un servicio de aplicación](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service) para obtener más información.

Los servicios de aplicaciones son aplicaciones para UWP que ofrecen servicios a otras aplicaciones para UWP. Son análogos a los servicios web en un dispositivo. Un servicio de aplicaciones se ejecuta como tarea en segundo plano en la aplicación host y puede proporcionar su servicio a otras aplicaciones. Por ejemplo, un servicio de aplicaciones podría proporcionar un servicio de escáner de códigos de barras que podrían usar otras aplicaciones. O quizás un conjunto de aplicaciones Enterprise tiene un servicio de aplicación de corrector ortográfico que está disponible para las demás aplicaciones del conjunto de aplicaciones.

**Instalación de transmisión por secuencias de la aplicación para UWP**

La transmisión en streaming es una forma de optimizar la forma en que se entrega la aplicación a los usuarios. En lugar de esperar a que se descargue toda la aplicación antes de usarla, los usuarios pueden interactuar con la aplicación tan pronto como se haya descargado una parte necesaria. Depende de ti, como desarrollador, segmentar la aplicación en una sección requerida para el lanzamiento y la activación básica y contenido adicional para el resto de la aplicación. Consulta [Instalación en streaming para aplicaciones para UWP](https://docs.microsoft.com/windows/uwp/packaging/streaming-install) para obtener más detalles de implementación e información.

## <a name="see-also"></a>Consulta también

[Crear y usar un servicio de aplicación](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)  
[Introducción a los paquetes de activos](../packaging/asset-packages.md)  
[Creación del paquete con el diseño de empaquetado](../packaging/packaging-layout.md)  
[Creación de paquetes opcionales y conjuntos relacionados](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)  
[Desarrollo con paquetes de activo y plegamiento de paquete](../packaging/package-folding.md)  
[Instalación en streaming de aplicaciones para UWP](https://docs.microsoft.com/windows/uwp/packaging/streaming-install)  
[Paquetes de aplicación sin formato](../packaging/flat-bundles.md)  
[Espacio de nombres Windows.ApplicationModel.AppService](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService)  
[Espacio de nombres Windows.ApplicationModel.Extensions](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)  
