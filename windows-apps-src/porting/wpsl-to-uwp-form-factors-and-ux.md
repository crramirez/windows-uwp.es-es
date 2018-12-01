---
description: Las aplicaciones de Windows comparten una apariencia común entre PC, dispositivos móviles y muchos otros tipos de dispositivos. La interfaz de usuario, las entradas y los patrones de interacción son muy similares, y un usuario que se mueve entre dispositivos agradecerá la experiencia familiar.
title: Migración WindowsPhone Silverlight a UWP para factor de forma y experiencia del usuario
ms.assetid: 96244516-dd2c-494d-ab5a-14b7dcd2edbd
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0897bd2636f13cfb02568847c0ba40b2d6b218f3
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8338241"
---
#  <a name="porting-windowsphone-silverlight-to-uwp-for-form-factor-and-ux"></a>Migración WindowsPhone Silverlight a UWP para factor de forma y experiencia del usuario


El tema anterior era [Migración de capas empresariales y de datos](wpsl-to-uwp-business-and-data.md).

Las aplicaciones de Windows comparten una apariencia común entre equipos, dispositivos móviles y muchos otros tipos de dispositivos. La interfaz de usuario, las entradas y los patrones de interacción son muy similares, y un usuario que se mueve entre dispositivos agradecerá la experiencia familiar. Las diferencias entre dispositivos como el tamaño físico, la orientación predeterminada y la resolución en píxeles efectivos influyen en la manera en una aplicación de plataforma Universal de Windows (UWP) se presenta en Windows 10. La buena noticia es que gran parte del trabajo pesado se realiza automáticamente por el sistema mediante conceptos inteligentes como píxeles efectivos.

## <a name="different-form-factors-and-user-experience"></a>Diferentes factores de forma y experiencias de usuario

Los diferentes dispositivos tienen varias resoluciones en los modos horizontal y vertical, con varias relaciones de aspecto. ¿Cómo escalarán los aspectos visuales de la interfaz, texto y recursos de la aplicación para UWP? ¿Cómo puedes admitir tanto entradas táctiles, como por mouse y teclado? Y con una aplicación que admite entrada táctil en dispositivos de distintos tamaños con distancias de visualización diferentes, ¿cómo puede un control ser al mismo tiempo un objetivo táctil de tamaño adecuado para diferentes densidades de píxeles *y* mantener su contenido legible desde diferentes distancias? En las secciones siguientes se tratan los aspectos que debes conocer.

## <a name="what-is-the-size-of-a-screen-really"></a>¿Cuál es el tamaño de una pantalla, realmente?

La respuesta corta es que es subjetivo, dado que no solo depende del tamaño de la pantalla objetivo sino también de la distancia a la que te encuentras de la misma. Subjetividad significa que debemos ponernos en el lugar del usuario, aunque es algo que los desarrolladores de buenas aplicaciones hacen en cualquier caso.

Objetivamente, una pantalla se mide en unidades de pulgadas y píxeles físicos (sin procesar). Conocer estas dos métricas te permite saber cuántos píxeles caben en una pulgada. Esa es la densidad de píxeles, también conocida como puntos por pulgada y píxeles por pulgada. Y el valor recíproco de los DPI es el tamaño físico de los píxeles como una fracción de una pulgada. Densidad de píxeles también se conoce como *resolución*, aunque ese término a menudo se usa libremente para referirse al número de píxeles.

Cuando la distancia de visión aumenta, todas esas métricas objetivo *parecen* más pequeñas y se resuelven en el *tamaño eficaz* de la pantalla y su *resolución eficaz*. El teléfono normalmente se mantiene más cercano a los ojos; después la tableta, a continuación el monitor del equipo y los más alejados son los dispositivos [Surface Hub](http://www.microsoft.com/microsoft-surface-hub) y los televisores. Para compensarlo, los dispositivos tienden a ser objetivamente más grandes con la distancia de visualización. Cuando configuras los tamaños de los elementos de interfaz de usuario, estableces esos tamaños en unidades denominadas píxeles efectivos (epx). Y Windows 10 tendrá en cuenta PPP y la distancia típica desde un dispositivo, para calcular el mejor tamaño de los elementos de la interfaz de usuario en píxeles físicos para ofrecer la mejor experiencia de visualización. Consulta [Píxeles de visualización o efectivos, distancia de visualización y factores de escala](wpsl-to-uwp-porting-xaml-and-ui.md).

Aun así, te recomendamos probar la aplicación con muchos dispositivos distintos, para poder confirmar cada experiencia por ti mismo.

## <a name="touch-resolution-and-viewing-resolution"></a>Resolución táctil y de visualización

Las prestaciones (widgets de la interfaz de usuario) deben ser del tamaño correcto para la entrada táctil. Por tanto, un objetivo táctil debe mantener su tamaño físico de forma parecida entre distintos dispositivos que pueden tener densidades de píxeles diferentes. Los píxeles funcionales también te ayudarán: se escalan en diferentes dispositivos (teniendo en cuenta la densidad de píxeles) para lograr un tamaño físico más o menos constante que sea ideal para los objetivos táctiles.

El texto debe tener el tamaño correcto para que se pueda leer (una buena regla general es un texto de 12 puntos a una distancia de visualización de 20 pulgadas) y una imagen debe tener el tamaño y la resolución correctos para la distancia de visualización. En diferentes dispositivos, ese mismo escalado de píxeles efectivos mantiene los elementos de la interfaz de usuario con un tamaño adecuado y legibles. El texto y otros gráficos vectoriales se escalan automáticamente, y lo hacen muy bien. Los gráficos basados en tramas (mapas de bits) también se escalan automáticamente si el desarrollador proporciona un recurso en un solo tamaño grande. Pero es preferible que el desarrollador proporcione cada recurso en una variedad de tamaños para que se pueda cargar automáticamente el apropiado para el factor de escala de un dispositivo de destino. Para obtener más información sobre esto, vea [Píxeles de visualización o efectivos, distancia de visualización y factores de escala](wpsl-to-uwp-porting-xaml-and-ui.md).

## <a name="layout-and-adaptive-visual-state-manager"></a>Diseño y Visual State Manager adaptativo

Hemos descrito los factores que intervienen en la comprensión del tamaño de pantalla. Ahora pensemos en el diseño de la aplicación y en cómo hacer uso de estado real de la pantalla extra cuando está disponible. Considera esta página de una aplicación muy sencilla que se ha diseñado para usarse en un dispositivo móvil estrecho. ¿Cómo debería mostrarse esta página en una pantalla mayor?

![la aplicación de la Tienda de Windows Phone portada](images/wpsl-to-uwp-case-studies/c01-04-uni-phone-app-ported.png)

La versión móvil está limitada solo a la orientación vertical porque es la mejor relación de aspecto para la lista de libros; y haríamos lo mismo para una página de texto, que es mejor mantenerla con una sola columna en dispositivos móviles. Pero las pantallas de PC y de las tabletas son grandes en cualquier orientación, por lo que la restricción del dispositivo móvil parece una limitación innecesaria en dispositivos más grandes.

Si se hace zoom óptico de la aplicación para que se parezca a la versión móvil, pero más grande, no se aprovecha el dispositivo ni su espacio adicional, y eso no suele gustarle al usuario. Deberíamos considerar mostrar más contenido, en lugar del mismo contenido más grande. Incluso en un tabléfono, podríamos mostrar más filas de contenido. Podríamos usar espacio extra para mostrar contenido diferente, como anuncios, o podríamos cambiar el cuadro de lista por una vista de lista y usarlo para agrupar elementos en varias columnas, cuando sea posible, usar el espacio de esa forma. Consulta [Directrices para controles de lista y cuadrícula](https://msdn.microsoft.com/library/windows/apps/mt186889).

Además de los controles nuevos, como la vista de lista y vista de cuadrícula, la mayoría de los tipos de diseño establecidos de WindowsPhone Silverlight tiene equivalentes en la plataforma Universal de Windows (UWP). Por ejemplo, [**Canvas**](https://msdn.microsoft.com/library/windows/apps/br209267), [**Grid**](https://msdn.microsoft.com/library/windows/apps/br242704) y [**StackPanel**](https://msdn.microsoft.com/library/windows/apps/br209635). La migración de gran parte de la interfaz de usuario que usa estos tipos debe ser un proceso sencillo, pero busca siempre formas de sacar provecho de las funcionalidades de diseño dinámico de estos paneles para que automáticamente se cambie el tamaño y se vuelva a diseñar en dispositivos de diferentes tamaños.

Intenta ir más allá del diseño dinámico integrado en los controles del sistema y paneles de diseño, podemos usar una nueva característica de Windows 10 denominada [Visual State Manager adaptativo](wpsl-to-uwp-porting-xaml-and-ui.md).

## <a name="input-modalities"></a>Modalidades de entrada

Una interfaz WindowsPhone Silverlight es táctil. Y, por supuesto, la interfaz de la aplicación portada debe admitir la entrada táctil, pero puedes elegir admitir también otras modalidades de entrada, como el mouse y el teclado. En UWP, se unifican mouse, lápiz y entrada táctil como *entrada del puntero*. Para obtener más información, consulta [Controlar la entrada de puntero](https://msdn.microsoft.com/library/windows/apps/mt404610) e [Interacciones de teclado](https://msdn.microsoft.com/library/windows/apps/mt185607).

## <a name="maximizing-markup-and-code-re-use"></a>Maximización de la reutilización de código y marcado

Vuelve a consultar la lista [Maximización de la reutilización de código y marcado](wpsl-to-uwp-porting-to-a-uwp-project.md) para ver técnicas de uso compartido de la interfaz de usuario para dispositivos con un amplio intervalo de factores de forma.

## <a name="more-info-and-design-guidelines"></a>Más información y pautas de diseño

-   [Diseñar aplicaciones para UWP](http://dev.windows.com/design)
-   [Directrices sobre fuentes](https://msdn.microsoft.com/library/windows/apps/hh700394)
-   [Programación de diferentes factores de forma](https://msdn.microsoft.com/library/windows/apps/dn958435)

## <a name="related-topics"></a>Temas relacionados

* [Asignaciones de espacios de nombres y clases](wpsl-to-uwp-namespace-and-class-mappings.md)

