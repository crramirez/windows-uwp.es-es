---
author: JnHs
Description: Learn how to create customer segments so you can target a subset of your customer base for promotional or engagement purposes.
title: Crear segmentos de clientes
ms.author: wdg-dev-content
ms.date: 07/26/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, segmento, segmentos, grupo dirigido, clientes
ms.assetid: 58185f6c-d61f-478b-ab24-753d8986cd5a
ms.localizationpriority: medium
ms.openlocfilehash: 4ec3bdd1d46927c9d6037906ebdb07e7a5c8f3f1
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2018
ms.locfileid: "4431259"
---
# <a name="create-customer-segments"></a>Crear segmentos de clientes

En ocasiones, puede que quieras dirigirte a un subgrupo de tu base de clientes para fines promocionales y de participación. Puedes hacerlo en el Centro de desarrollo de Windows creando un tipo de [grupo de clientes](create-customer-groups.md) denominado *segmento* que incluya a los clientes de Windows 10 que cumplan con los criterios demográficos o de ingresos que elijas.

Por ejemplo, podrías crear un segmento que incluya solo a clientes de 50 años o más, o que incluya a clientes que han gastado más de 10 USD en Microsoft Store. También podrías combinar esos criterios y crear un segmento que incluya a todos los clientes de más de 50 años que han gastado más de 10 USD en la Tienda. 

Proporcionamos algunas plantillas de segmento para que puedas empezar a trabajar, pero puedes definir y combinar los criterios que quieras.

> [!TIP]
> Se pueden utilizar segmentos para enviar [notificaciones dirigidas](send-push-notifications-to-your-apps-customers.md) u [ofertas dirigidas](use-targeted-offers-to-maximize-engagement-and-conversions.md) a un grupo de clientes como parte de tus campañas de participación.

Cosas que tener en cuenta sobre segmentos de clientes:
- Después de guardar un segmento, se tarda 24 horas en poder usarlo para [notificaciones push dirigidas](send-push-notifications-to-your-apps-customers.md).
- Los resultados de segmentos se actualizan a diario, por lo que es posible que veas que el recuento total de clientes de un segmento cambia de un día para otro, a medida que haya más o menos clientes que cumplan los requisitos.
- La mayoría de los segmentos se calculan con todos los datos históricos, aunque hay algunas excepciones. Por ejemplo, **App acquisition date**, **Id. de campaña**, **Store page view date** y **Referrer URI domain** están limitados a los últimos 90 días de datos.
- Los segmentos solo incluyen los clientes que adquieren la aplicación en Windows10 teniendo iniciada sesión con una cuenta de Microsoft válida. 
- Los segmentos no incluyen todos los clientes menores de 17 años.

## <a name="to-create-a-customer-segment"></a>Para crear un segmento de clientes

1.  En el panel de información del Centro de desarrollo de Windows, expande **Interactuar** en el panel de navegación izquierdo y luego selecciona **Grupos de clientes**.
2.  En la página **Grupos de clientes**, sigue uno de estos procedimientos:
 - En la sección **Mis grupos de clientes**, selecciona **Crear nuevo grupo** para definir un segmento desde cero. En la página siguiente, selecciona el botón de opción **Segmento**.
 - En la sección **Segment templates**, selecciona **Copiar** junto a uno de los segmentos predefinidos (que puedes usar tal cual o modificar según tus necesidades).
3.  En la casilla **Nombre de grupo**, escribe un nombre para el segmento.
4.  En la lista **Include customers from this app**, selecciona una de las aplicaciones de destino.
5.  En la sección **Definir condiciones de inclusión**, especifica los criterios de filtro para el segmento.

    Puedes elegir entre diversos criterios de filtro, incluidos **Adquisiciones**, **Origen de adquisición**, **Uso**, **Datos demográficos**, **Clasificación**, **Predicción de las renovaciones**, **Compras de la Tienda**, **Adquisiciones de la Tiendas** y **Gastos de la Tienda**.

    Por ejemplo, si quieres crear un segmento que solo incluya a los clientes de la aplicación que tengan de 18 a 24 años, selecciona los criterios de filtro [**Datos demográficos**] [**Grupo de edad**] [**es**] [**18 a 24**] en las listas desplegables.

    Puedes generar segmentos más complejos mediante consultas AND/OR para incluir o excluir clientes en función de diversos atributos. Para agregar una consulta OR, selecciona **+ OR statement**. Para agregar una consulta ADD, selecciona **Add another filter**.

    Por lo tanto, si quieres precisar que el segmento solo incluya clientes varones en el intervalo de edad especificado, selecciona **Add another filter**, a continuación, selecciona los criterios de filtro adicionales [**Datos demográficos**] [**Sexo**] [**es**] [**Hombre**]. En este ejemplo, la **Definición de segmento** mostraría **Grupo de edad == 18 a 24 & & Sexo == Hombre**.

    ![Ejemplo de criterios de filtro para un segmento](images/create-segment-inclusions.png)
6. Selecciona **Guardar**.

> [!IMPORTANT]
> No podrás usar un segmento que incluya muy pocos clientes. Si tu definición de segmento no incluye suficientes clientes, puedes ajustar los criterios del segmento o intentarlo de nuevo más tarde, cuando la aplicación puede haber adquirido más clientes que cumplan los criterios de tu segmento.


## <a name="app-statistics"></a>Estadísticas de la aplicación

La sección **Estadísticas de la aplicación** del segmento proporciona información sobre la aplicación, así como el tamaño del segmento que acabas de crear.

Ten en cuenta que **Available app customers** no refleja el número real de clientes que han adquirido la aplicación, sino el número de clientes que se pueden incluir en segmentos (es decir, clientes que se puede determinar que cumplen los requisitos de edad, que han adquirido la aplicación en Windows 10 y que están asociados con una cuenta de Microsoft válida).

Si ves los resultados y **Customers in this segment** indica **Pequeño**, el segmento no incluye suficientes clientes y está marcado como inactivo. Los segmentos inactivos no pueden usarse para notificaciones u otras características. Es posible que puedas activar y usar un segmento mediante una de estas acciones:

- En la sección **Define inclusion conditions**, ajusta los criterios de filtro para que el segmento incluya más clientes.
- En la página **Grupos de clientes**, en la sección **Inactive segments**, selecciona **Actualizar** para ver si el segmento incluye ahora suficientes clientes (por ejemplo, si más clientes que cumplen tus criterios de segmento han descargado tu aplicación desde que creaste el segmento o si más clientes existentes cumplen ahora tus criterios de segmento).
