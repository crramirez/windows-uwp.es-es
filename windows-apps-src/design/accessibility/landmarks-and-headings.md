---
Description: Describes the landmarks and headings features of accessibility.
ms.assetid: 019CC63D-D915-4EBD-9442-DE899AB973C9
title: Puntos de referencia y encabezados
label: Landmarks and Headings
template: detail.hbs
ms.date: 01/24/2018
ms.topic: article
keywords: windows10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d81957c379bd948a50d08b980ff20debc6c223c5
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "8753678"
---
# <a name="landmarks-and-headings"></a>Puntos de referencia y encabezados

Normalmente una interfaz de usuario se organiza de una manera visualmente eficiente, lo que permite a los usuarios sin problemas de visión encontrar lo que le interesa sin tener que perder tiempo leyendo *todos* el contenido. Un usuario que emplee un lector de pantalla debe tener esta misma capacidad de discernir rápidamente. Los puntos de referencia y los encabezados definen secciones de una interfaz de usuario que ayudan a que la navegación sea más eficaz para los usuarios de tecnología de asistencia (AT). Marcar el contenido con puntos de referencia y encabezados proporciona a un usuario de un lector de pantalla la opción de discernir rápidamente el contenido de forma similar a como lo hace un usuario sin problemas de visión.

Los conceptos de [puntos de referencia ARIA](https://www.w3.org/WAI/GL/wiki/Using_ARIA_landmarks_to_identify_regions_of_a_page), [encabezados ARIA](https://www.w3.org/TR/WCAG20-TECHS/ARIA12.html) y [encabezados HTML](https://www.w3.org/TR/2016/NOTE-WCAG20-TECHS-20161007/H42.html) llevan años utilizándose en el contenido web para permitir una navegación más rápida a los usuarios de lectores de pantalla. Las páginas web usan los puntos de referencia y los encabezados para que su contenido sea más utilizable, ya que permiten a los usuarios de tecnología de asistencia acceder rápidamente al fragmento grande (punto de referencia) y al fragmento más pequeño (encabezado). En concreto, los lectores de pantalla cuentan con comandos que permiten a los usuarios saltar entre puntos de referencia y entre encabezados (siguiente o anterior, o un nivel de encabezado específico). Por estos mismos motivos, es importante tener en cuenta los puntos de referencia y los encabezados en tus aplicaciones.

Los puntos de referencia permiten agrupar el contenido en distintas categorías, como búsqueda, navegación, contenido principal, etc. Una vez agrupado, el usuario de tecnología de asistencia puede navegar rápidamente entre los grupos. Esta navegación rápida permite al usuario omitir cantidades potencialmente significativas de contenido que anteriormente podría haber tenido que recorrer punto por punto, lo que resultaba en una experiencia de baja calidad. 

Por ejemplo, si usas un panel de pestañas, considéralo un punto de referencia de navegación. Si usas un cuadro de edición de búsqueda, considéralo un punto de referencia de búsqueda, y piensa en la posibilidad de configurar el contenido principal como un punto de referencia de contenido principal. Tanto si está dentro de algún punto de referencia como si está fuera, considera la posibilidad de establecer subelementos como encabezados de niveles de título lógicos. 

Piensa en la página **Accesibilidad** de la aplicación Configuración de Windows. 

![La página Accesibilidad de la aplicación Configuración de Windows](images/EaseOfAccessSettings.png)  

Hay un cuadro de edición de búsqueda ajustado dentro de un punto de referencia de búsqueda. Los elementos de navegación de la izquierda están ajustados en un punto de referencia de navegación, y el contenido principal de la derecha está ajustado en un punto de referencia de contenido principal. Si vamos un poco más allá, en el punto de referencia navegación hay un encabezado de grupo principal llamado **Accesibilidad**, que es un nivel 1 de encabezado. Debajo están las opciones secundarias **Visión**, **Audición**, etc. Estas tienen un nivel 2 de encabezado. En el contenido principal se siguen estableciendo encabezados, y el asunto principal, **Pantalla**, se establece como nivel 1 de encabezado y los subgrupos como **Hacer todo más grande** como nivel 2 de encabezado. 

La aplicación Configuración sería accesible sin puntos de referencia y encabezados, pero resulta más fácil de usar con ellos. Un usuario de lector de pantalla puede ir de manera rápida y fácil al grupo (punto de referencia) que necesite y luego acceder igual de rápido al subgrupo (encabezado). 

Usa [AutomationProperties.LandmarkTypeProperty](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.LandmarkTypeProperty) para configurar el elemento de interfaz de usuario como el [tipo de punto de referencia](https://msdn.microsoft.com/library/windows/desktop/mt759299) que quieras. Este elemento de la interfaz de usuario de punto de referencia encapsula todos los demás elementos de la interfaz de usuario que tienen sentido para ese punto de referencia. 

Usa [AutomationProperties.LocalizedLandmarkTypeProperty](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.LocalizedLandmarkTypeProperty) para dar un nombre específicamente al punto de referencia. Si se selecciona un tipo de punto de referencia predefinido como principal o de navegación, estos nombres se usarán para el nombre del punto de referencia. Sin embargo, si se establece el tipo de punto de referencia como personalizada, es necesario dar un nombre específicamente al punto de referencia mediante esta propiedad. También puedes usar esta propiedad para reemplazar los nombres predeterminados de los tipos de punto de referencia no personalizados. 

Usa [AutomationProperties.HeadingLevel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.headinglevelproperty) para establecer el elemento de la interfaz de usuario como un encabezado de un nivel específico desde *Level1* a *Level9*.

