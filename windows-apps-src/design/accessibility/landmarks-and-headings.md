---
description: Aprenda a usar las características de puntos de referencia y encabezados de la automatización de la interfaz de usuario para definir secciones de contenido en la aplicación, mejorar la accesibilidad y ayudar a los usuarios de tecnología de asistencia (en) a navegar por la interfaz de usuario.
ms.assetid: 019CC63D-D915-4EBD-9442-DE899AB973C9
title: Puntos de referencia y encabezados
label: Landmarks and Headings
template: detail.hbs
ms.date: 01/24/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 65af556180e39ea66bbeb553a50c0746f952a08c
ms.sourcegitcommit: 5481bb34def681bc60fbfa42d9779053febec468
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/02/2020
ms.locfileid: "89304747"
---
# <a name="landmarks-and-headings"></a>Puntos de referencia y encabezados

Una interfaz de usuario se organiza normalmente de forma visualmente eficaz, lo que permite a un usuario con visión rápida hojear lo que le interesa sin tener que ralentizarse para leer *todo* el contenido. Un usuario lector de pantalla debe tener esta misma capacidad de desnatamiento. Los puntos de referencia y los encabezados definen secciones de una interfaz de usuario que ayudan en una navegación más eficaz para los usuarios de tecnología de asistencia (en). Al marcar el contenido en puntos de referencia y encabezados, se proporciona un usuario de lector de pantalla con la opción de hojear el contenido de forma similar a como lo haría un usuario de la vista.

Los conceptos de [puntos de referencia de Aria](https://www.w3.org/WAI/GL/wiki/Using_ARIA_landmarks_to_identify_regions_of_a_page), [encabezados de Aria](https://www.w3.org/TR/WCAG20-TECHS/ARIA12.html)y [encabezados HTML](https://www.w3.org/TR/2016/NOTE-WCAG20-TECHS-20161007/H42.html) se han usado en el contenido web durante años para permitir una navegación más rápida por los usuarios del lector de pantalla. Las páginas web usan puntos de referencia y encabezados para que el contenido sea más fácil de usar, ya que permite al usuario obtener acceso rápidamente al fragmento grande (punto de referencia) y al fragmento más pequeño (título). En concreto, los lectores de pantalla tienen comandos que permiten a los usuarios saltar entre puntos de referencia y saltar entre los encabezados (nivel de encabezado siguiente o anterior o específico). Es importante tener en cuenta los puntos de referencia y los encabezados de las aplicaciones por las mismas razones.

Los puntos de referencia permiten agrupar el contenido en diversas categorías, como la búsqueda, la navegación, el contenido principal, etc. Una vez agrupado, el usuario en puede desplazarse rápidamente entre los grupos. Esta navegación rápida permite al usuario omitir cantidades potencialmente substanciales de contenido que antes se había tenido que navegar por elementos por elemento para una experiencia deficiente.

Por ejemplo, al usar un panel de pestañas, considere este punto de referencia de navegación. Cuando use un cuadro de edición de búsqueda, tenga en cuenta este punto de referencia de búsqueda y considere la posibilidad de configurar el contenido principal como punto de referencia de contenido principal. Ya sea dentro de un punto de referencia o incluso fuera de un punto de referencia, considere la posibilidad de establecer subelementos como encabezados con niveles de encabezado lógicos.

Considere la página **accesibilidad** en la aplicación configuración de Windows.

![Página de accesibilidad en la aplicación de configuración de Windows](images/EaseOfAccessSettings.png)  

Hay un cuadro de edición de búsqueda que se ajusta dentro de un punto de referencia de búsqueda. Los elementos de navegación de la izquierda se ajustan dentro de un punto de referencia de navegación y el contenido principal de la derecha se ajusta dentro de un punto de referencia de contenido principal. Tomando esto más en el punto de referencia de navegación hay un encabezado de grupo principal denominado **accesibilidad** , que es un nivel de encabezado 1. En, son las subopciones **vison**, **audición**, etc. Tienen un nivel de encabezado 2. La configuración de los encabezados continúa en el contenido principal de nuevo al establecer el asunto principal, **Mostrar**, como nivel de encabezado 1 y subgrupos, por ejemplo, **hacer que todo sea mayor** que el nivel de encabezado 2.

La aplicación de configuración sería accesible sin puntos de referencia y encabezados, pero es más fácil de usar con ellas. Un usuario de lector de pantalla puede llegar rápida y fácilmente al grupo (punto de referencia) que necesitan y, a continuación, llegar rápidamente al subgrupo (encabezado).

Use [AutomationProperties. LandmarkTypeProperty](/uwp/api/windows.ui.xaml.automation.automationproperties.LandmarkTypeProperty) para configurar el elemento de la interfaz de usuario como el [tipo de referencia](/windows/desktop/WinAuto/landmark-type-identifiers) que desea. Este elemento de la interfaz de usuario de punto de referencia encapsularía todos los demás elementos de la interfaz de usuario que tienen sentido para ese punto de referencia.

Use [AutomationProperties. LocalizedLandmarkTypeProperty](/uwp/api/windows.ui.xaml.automation.automationproperties.LocalizedLandmarkTypeProperty) para asignar el nombre específico al punto de referencia. Si selecciona un tipo de punto de referencia predefinido como principal o de navegación, se usarán estos nombres para el nombre de punto de referencia. Sin embargo, si establece el tipo de punto de referencia en personalizado, debe asignar nombre específicamente al punto de referencia a través de esta propiedad. También puede usar esta propiedad para invalidar los nombres predeterminados de los tipos de puntos de referencia no personalizados.

Use [AutomationProperties. HeadingLevel](/uwp/api/windows.ui.xaml.automation.automationproperties.headinglevelproperty) para establecer el elemento de la interfaz de usuario como un encabezado de un nivel específico de *Level1* a *Level9*.

## <a name="examples"></a>Ejemplos

Para obtener numerosos ejemplos de código que muestran cómo resolver muchos problemas comunes de accesibilidad de programación en las aplicaciones de escritorio de Windows, consulte [ejemplos de código para resolver problemas comunes de accesibilidad de programación en aplicaciones de escritorio de Windows](/accessibility-tools-docs/).

[Microsoft Accessibility Insights para Windows](https://github.com/microsoft/accessibility-insights-windows)hace referencia directamente a estos ejemplos de código, lo que puede ayudar a destacar muchos problemas de accesibilidad en la interfaz de usuario.
