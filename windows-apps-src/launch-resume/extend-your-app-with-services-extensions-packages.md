---
title: Ampliar la aplicación con servicios, extensiones y paquetes
description: Describe cómo crear una tarea en segundo plano que se ejecuta cuando se actualiza la aplicación de la tienda de Plataforma universal de Windows (UWP).
ms.date: 05/07/2018
ms.topic: article
keywords: Windows 10, UWP, Extend, componente, App Service, paquete, extensión
ms.localizationpriority: medium
ms.openlocfilehash: 9239178701082012f2878e996ede2f3f56a9a94b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155879"
---
# <a name="extend-your-app-with-services-extensions-and-packages"></a>Ampliar la aplicación con servicios, extensiones y paquetes

Hay muchas tecnologías en Windows 10 para extender y componen la aplicación. Esta tabla debe ayudarle a determinar qué tecnología debe usar en función de los requisitos. Va seguido de una breve descripción de los escenarios y las tecnologías.

| Escenario                           | Paquete de recursos   | Paquete de activos      | Paquete opcional   | Paquete plano        | Extensión de aplicación      | App Service        | Instalación de streaming  |
|------------------------------------|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|
| Complementos de código de terceros            |                    |                    |                    |                    | :heavy_check_mark: |                    |                    |
| Complementos de código en proceso              |                    |                    | :heavy_check_mark: |                    |                    |                    |                    |
| Recursos de experiencia del usuario (cadenas/imágenes)         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Contenido a petición <br/> (por ejemplo, niveles adicionales) |      |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Licencias y adquisición independientes |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: | :heavy_check_mark: |                    |
| Adquisición en la aplicación                 |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |
| Optimizar la hora de instalación              | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Reducir la superficie del disco              | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |                    |                    |
| Optimizar el empaquetado                 |                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |
| Reducir el tiempo de publicación             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |

## <a name="scenario-descriptions-the-rows-in-the-table-above"></a>Descripciones de los escenarios (las filas de la tabla anterior)

**Complementos de terceros**  

Código que se puede descargar de la tienda y ejecutar desde la aplicación. Por ejemplo, extensiones para el explorador Microsoft Edge.

**Complementos de código en proceso**  

Código que se ejecuta en proceso con la aplicación. También puede incluir contenido. Dado que el código se ejecuta en proceso, se supone un mayor nivel de confianza. Puede optar por no exponer este tipo de extensibilidad a un tercero.

**Recursos de experiencia del usuario (cadena/imágenes)**  

Recursos de la interfaz de usuario, como cadenas localizadas, imágenes y cualquier otro contenido de la interfaz de usuario que desee factorizar según la configuración regional o cualquier otro motivo.

**Contenido a petición**  

Contenido que desea descargar en otro momento. Por ejemplo, compras desde la aplicación que le permiten descargar nuevos niveles, máscaras o funcionalidades.

**Licencias y adquisición independientes**  

La capacidad de conceder licencias y adquirir el contenido independientemente de la aplicación.

**Adquisición en la aplicación**  

Indica si hay compatibilidad mediante programación para adquirir el contenido desde dentro de la aplicación.

**Optimizar la hora de instalación**

Proporciona funcionalidad para reducir el tiempo que se tarda en adquirir la aplicación desde la tienda y comenzar a ejecutarse.

**Reducir la superficie del disco** Reduce el tamaño de una aplicación mediante la inclusión de las aplicaciones o los recursos necesarios.

**Optimizar el empaquetado** Optimiza el proceso de empaquetado de aplicaciones para aplicaciones a gran escala o complejas.

**Reducir el tiempo de publicación** Minimice la cantidad de tiempo que se tarda en publicar la aplicación en la tienda, el recurso compartido local o el servidor Web.

## <a name="technology-descriptions-the-columns-in-the-table-above"></a>Descripciones de tecnología (las columnas de la tabla anterior)

**Paquete de recursos**

Los paquetes de recursos son paquetes solo de activos que permiten que la aplicación se adapte a varios tamaños de pantalla y idiomas del sistema. El paquete de recursos tiene como destino el idioma del usuario, la escala del sistema y las características de DirectX, lo que permite adaptar la aplicación a diversos escenarios de usuario. Aunque un paquete de aplicación puede contener varios recursos, el sistema operativo solo descargará los recursos correspondientes por dispositivo de usuario, lo que ahorrará ancho de banda y espacio en disco.

**Paquete de activos** Los paquetes de recursos son un origen común y centralizado de archivos ejecutables o no ejecutables para su uso por parte de la aplicación. Normalmente son archivos que no son de procesador o específicos del idioma. Por ejemplo, esto podría incluir una colección de imágenes en un paquete de recursos y vídeos en otro paquete de recursos, que se usan en la aplicación. Si la aplicación admite varias arquitecturas y varios idiomas, estos recursos podrían incluirse en el paquete de arquitectura o en el paquete de recursos, pero también significa que los recursos se duplicarán varias veces en los distintos paquetes de arquitectura, con lo que se ocupará espacio en disco. Si se usan paquetes de recursos, solo tienen que incluirse en el paquete de aplicación general una vez. Consulte [Introducción a los paquetes de recursos](/windows/msix/package/asset-packages) para obtener más información.

**Paquete opcional**

Los paquetes opcionales se usan para complementar o extender la funcionalidad original de un paquete de la aplicación. Es posible publicar una aplicación, después de publicar paquetes opcionales en otro momento, o publicar tanto la aplicación como los paquetes opcionales simultáneamente. Al extender la aplicación a través de un paquete opcional, tiene las ventajas de distribuir y rentabilizar el contenido como un paquete de aplicación independiente. Normalmente, los paquetes opcionales están diseñados para que los desarrolle el desarrollador de la aplicación original, ya que se ejecutan con la identidad de la aplicación principal (a diferencia de las extensiones de aplicación). En función de cómo defina el paquete opcional, puede cargar código, recursos o código y recursos desde el paquete opcional a la aplicación principal. Si necesita mejorar la aplicación con contenido que se puede monetizar, autorizar y distribuir por separado, los paquetes opcionales podrían ser la opción adecuada para usted. Para obtener información detallada sobre la implementación, consulte [paquetes opcionales y creación de conjuntos relacionados](/windows/msix/package/optional-packages).

**Paquete plano** 
 Los [paquetes de aplicación de agrupación plana](/windows/msix/package/flat-bundles) son similares a las agrupaciones de aplicaciones normales, salvo que en lugar de incluir todos los paquetes de la aplicación dentro de la carpeta, el paquete plano solo contiene *referencias* a esos paquetes de aplicaciones. Al incluir las referencias a los paquetes de la aplicación en lugar de los propios archivos, una agrupación plana reducirá la cantidad de tiempo que se tarda en empaquetar y descargar una aplicación.

**Extensión de aplicación**

[Las extensiones de aplicación](/uwp/api/windows.applicationmodel.appextensions) permiten a la aplicación de UWP hospedar el contenido proporcionado por otras aplicaciones de UWP. Descubre, enumera y accede a contenido de solo lectura desde dichas aplicaciones.

Si una aplicación admite extensiones, cualquier desarrollador puede enviar una extensión para la aplicación. Por lo tanto, la aplicación host debe ser sólida cuando carga una extensión de la que no se ha probado previamente. Las extensiones deben considerarse que no son de confianza.

Las aplicaciones no pueden cargar código desde extensiones. Si necesita ejecutar el código, considere la posibilidad de App Services.

**App Service**

Los servicios de aplicaciones de Windows permiten la comunicación de aplicación a aplicación al permitir que la aplicación UWP proporcione servicios a otra aplicación universal de Windows. App Services le permite crear servicios sin interfaz de usuario que las aplicaciones pueden llamar en el mismo dispositivo y a partir de Windows 10, versión 1607, en dispositivos remotos. Consulte [creación y uso de un servicio de aplicaciones](./how-to-create-and-consume-an-app-service.md) para obtener más información.

Los servicios de aplicaciones son aplicaciones para UWP que proporcionan servicios a otras aplicaciones de UWP. Son análogos a los servicios web en un dispositivo. Una aplicación de App Service se ejecuta como una tarea en segundo plano en la aplicación host y puede proporcionar su servicio a otras aplicaciones. Por ejemplo, un servicio de aplicaciones podría proporcionar un servicio de análisis de código de barras que otras aplicaciones podrían usar. O quizás un conjunto de aplicaciones de empresa tiene un servicio de aplicaciones de revisión ortográfica común que está disponible para las demás aplicaciones del conjunto.

**Instalación de streaming de aplicaciones para UWP**

La instalación de streaming es una manera de optimizar el modo en que se entrega la aplicación a los usuarios. En lugar de esperar a que la aplicación completa se descargue antes de poder usarla, los usuarios pueden interactuar con la aplicación en cuanto se haya descargado una parte necesaria. Usted, como desarrollador, puede segmentar la aplicación en una sección necesaria para la activación e inicio básicos y el contenido adicional para el resto de la aplicación. Para obtener más información y detalles de implementación, consulte [instalación de streaming de aplicaciones para UWP](/windows/msix/package/streaming-install) .

## <a name="see-also"></a>Vea también

[Crear y usar un servicio de aplicación](./how-to-create-and-consume-an-app-service.md)  
[Introducción a los paquetes de activos](/windows/msix/package/asset-packages)  
[Creación de paquetes con el diseño del empaquetado](/windows/msix/package/packaging-layout)  
[Creación de paquetes opcionales y conjuntos relacionados](/windows/msix/package/optional-packages)  
[Desarrollar con paquetes de activos y plegado de paquetes](/windows/msix/package/package-folding)  
[Instalación en streaming de aplicaciones para UWP](/windows/msix/package/streaming-install)  
[Paquetes de aplicaciones de conjuntos planos](/windows/msix/package/flat-bundles)  
[Espacio de nombres Windows. ApplicationModel. AppService](/uwp/api/Windows.ApplicationModel.AppService)  
[Espacio de nombres Windows. ApplicationModel. Extensions](/uwp/api/windows.applicationmodel.appextensions)