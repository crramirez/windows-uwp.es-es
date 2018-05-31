---
author: TylerMSFT
title: Ampliar la aplicación con servicios de aplicaciones, extensiones y paquetes
description: Aprende a crear una tarea en segundo plano que se ejecute cuando se actualice la aplicación de la tienda de la Plataforma universal de Windows (UWP).
ms.author: twhitney
ms.date: 05/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, ampliar, separar por componentes, servicio de aplicaciones, paquete, ampliación
ms.localizationpriority: medium
ms.openlocfilehash: 2721f9d8f768cabb0e07c0cd2cfcfcbf9255cd70
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/28/2018
ms.locfileid: "1689621"
---
# <a name="extend-your-app-with-services-extensions-and-packages"></a>Ampliar tu aplicación con servicios de aplicaciones, extensiones y paquetes

Hay distintas tecnologías en Windows 10 que te ayudarán a ampliar y separar tu aplicación por componentes. Esta tabla debe ayudarte a determinar qué tecnología debes usas para tu escenario. Le sigue un breve descripción de los escenarios y las tecnologías.


| Escenario                           | Paquete de recursos | Paquete opcional | Extensión de aplicación    | Servicio de aplicaciones      | Instalación en streaming |
|------------------------------------|:----------------:|:----------------:|:----------------:|:----------------:|:-----------------:|
| Complementos de código de terceros            |                  |                  | :heavy_check_mark: |                  |                   |
| Complementos de código dentro del proceso              |                  | :heavy_check_mark: |                  |                  |                   |
| Activos de la experiencia de usuario (cadenas o imágenes)         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                  | :heavy_check_mark: |
| Contenido a petición <br/> (por ejemplo, niveles adicionales) |    | :heavy_check_mark: | :heavy_check_mark: |                  | :heavy_check_mark: |
| Licencias independientes y adquisición |                  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                   |
| Adquisición desde la aplicación                 |                  | :heavy_check_mark: | :heavy_check_mark: |                  |                   |
| Optimizar el tiempo de instalación              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                  | :heavy_check_mark: |

## <a name="scenario-descriptions-rows-in-the-table"></a>Descripciones de escenarios (filas de la tabla)

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

## <a name="technology-descriptions-columns-in-the-table"></a>Descripciones de tecnología (columnas de la tabla)

**Paquete de recursos**

Los paquetes de recursos son paquetes únicamente de activos que permiten a tu aplicación adaptarse a varios tamaños de pantalla e idiomas del sistema. El paquete de recursos tiene como objetivo el idioma del usuario, la escala de sistema y características de DirectX, lo que permite a la aplicación adaptarse a una variedad de escenarios de usuario. Aunque un paquete de la aplicación puede contener varios recursos, el sistema operativo solo descargará los recursos pertinentes por dispositivo de usuario, lo que ahorrará espacio en disco y ancho de banda.

**Paquete opcional**

Los paquetes opcionales se usan para complementar o ampliar la funcionalidad original de un paquete de la aplicación. Es posible publicar una aplicación, seguida de la publicación de paquetes opcionales posteriormente, o publicar tanto la aplicación como paquetes opcionales al mismo tiempo. Al ampliar la aplicación mediante un paquete opcional, tienes las ventajas de distribuir y rentabilizar el contenido como paquete de la aplicación independiente. Los paquetes opcionales está pensados normalmente para ser desarrollados por el desarrollador de aplicaciones, ya que se ejecutan con la identidad de la aplicación principal (a diferencia de las extensiones de para aplicaciones). En función de cómo definas el paquete opcional, puedes cargar el código, los activos, o el código y los activos desde el paquete opcional a la aplicación principal. Si buscas mejorar tu aplicación con contenido que se pueda rentabilizar, para el que se pueda conceder licencia y distribuirse por separado, los paquetes opcionales podrían ser la elección correcta para ti. Para obtener detalles de implementación, consulta [Paquetes opcionales y creación de conjuntos relacionados](https://docs.microsoft.com/windows/uwp/packaging/optional-packages).

**Extensión de aplicación**

[Extensiones de aplicación](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions) permite que tu aplicación para UWP hospede contenido proporcionado por otras aplicaciones para UWP. Descubre, enumera y accede a contenido de solo lectura desde dichas aplicaciones.

Si una aplicación admite extensiones, cualquier desarrollador puede enviar una extensión para la aplicación. Por tanto, la aplicación host debe ser eficaz cuando cargue una extensión con la que no se haya probado previamente. Las extensiones deben considerarse como de no confianza.

Las aplicaciones no pueden cargar código de las extensiones. Si necesitas ejecución de código, ten en cuenta App Services.

**Servicio de aplicaciones**

Los servicios de aplicaciones de Windows permiten la comunicación entre aplicaciones al permitir que tu aplicación para UWP proporcione servicios a otras aplicaciones universales de Windows. Los servicios de aplicaciones le permiten crear servicios que no tienen interfaz de usuario que las aplicaciones pueden llamar en el mismo dispositivo y, a partir de Windows 10, versión 1607, en dispositivos remotos. Consulta [Crear y usar un servicio de aplicación](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service) para obtener más información.

Los servicios de aplicaciones son aplicaciones para UWP que ofrecen servicios a otras aplicaciones para UWP. Son similares a servicios web, en un dispositivo. Un servicio de aplicaciones se ejecuta como tarea en segundo plano en la aplicación host y puede proporcionar su servicio a otras aplicaciones. Por ejemplo, un servicio de aplicaciones podría proporcionar un servicio de escáner de códigos de barras que podrían usar otras aplicaciones. O quizás un conjunto de aplicaciones Enterprise tiene un servicio de aplicación de corrector ortográfico que está disponible para las demás aplicaciones del conjunto de aplicaciones.

**Instalación en streaming de aplicaciones para UWP**

La transmisión en streaming es una forma de optimizar la forma en que se entrega la aplicación a los usuarios. En lugar de esperar a que se descargue toda la aplicación antes de usarla, los usuarios pueden interactuar con la aplicación tan pronto como se haya descargado una parte necesaria. Depende de ti, como desarrollador, segmentar la aplicación en una sección requerida para el lanzamiento y la activación básica y contenido adicional para el resto de la aplicación. Consulta [Instalación en streaming para aplicaciones para UWP](https://docs.microsoft.com/windows/uwp/packaging/streaming-install) para obtener más detalles de implementación e información.

## <a name="see-also"></a>Consulte también

[Crear y usar un servicio de aplicación](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)  
[Paquetes opcionales y creación de conjuntos relacionados](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)  
[Espacio de nombres de Windows.ApplicationModel.Extensions](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)  
[Instalación en streaming de aplicaciones para UWP](https://docs.microsoft.com/windows/uwp/packaging/streaming-install)  
[Espacio de nombres Windows.ApplicationModel.AppService](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService)    
