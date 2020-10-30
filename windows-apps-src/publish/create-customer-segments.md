---
description: Aprende a crear segmentos de clientes para poder dirigirte a un subgrupo de tu base de clientes para fines promocionales o de participación.
title: Crear segmentos de clientes
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, segmento, segmentos, grupo de destino, clientes
ms.assetid: 58185f6c-d61f-478b-ab24-753d8986cd5a
ms.localizationpriority: medium
ms.openlocfilehash: 563cf6767a9e7b175c861b11bd125ae6f45a3bba
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029758"
---
# <a name="create-customer-segments"></a>Crear segmentos de clientes

En ocasiones, puede que quieras dirigirte a un subgrupo de tu base de clientes para fines promocionales y de participación. Para ello, en el [centro de Partners](https://partner.microsoft.com/dashboard) , cree un tipo de [grupo de clientes](create-customer-groups.md) denominado *segmento* que incluya a los clientes de Windows 10 que cumplan los criterios demográficos o de ingresos que elija.

Por ejemplo, puede crear un segmento que incluya solo los clientes que tengan una antigüedad de 50 o anterior, o que incluya a los clientes que hayan pasado más de $10 en el Microsoft Store. También podrías combinar esos criterios y crear un segmento que incluya a todos los clientes de más de 50 años que han gastado más de 10 USD en la Tienda. 

Proporcionamos algunas plantillas de segmento para que puedas empezar a trabajar, pero puedes definir y combinar los criterios que quieras.

> [!TIP]
> Los segmentos se pueden usar para enviar [notificaciones de destino](send-push-notifications-to-your-apps-customers.md) o [ofertas de destino](use-targeted-offers-to-maximize-engagement-and-conversions.md) a un grupo de clientes como parte de las campañas de Engagement.

Cosas que tener en cuenta sobre segmentos de clientes:
- Después de guardar un segmento, se tarda 24 horas en poder usarlo para [notificaciones push dirigidas](send-push-notifications-to-your-apps-customers.md).
- Los resultados de segmentos se actualizan a diario, por lo que es posible que veas que el recuento total de clientes de un segmento cambia de un día para otro, a medida que haya más o menos clientes que cumplan los requisitos.
- La mayoría de los atributos de segmento se calculan mediante todos los datos históricos, aunque hay algunas excepciones. Por ejemplo, **App acquisition date** , **Id. de campaña** , **Store page view date** y **Referrer URI domain** están limitados a los últimos 90 días de datos.
- Los segmentos solo incluyen a los clientes que adquirieron su aplicación en Windows 10 mientras inician sesión con un cuenta de Microsoft válido. 
- Los segmentos no incluyen a los clientes que tengan menos de 17 años de antigüedad.

## <a name="to-create-a-customer-segment"></a>Para crear un segmento de clientes

1.  En el [centro de Partners](https://partner.microsoft.com/dashboard), expanda **participar** en el menú de navegación izquierdo y, a continuación, seleccione grupos de **clientes** .
2.  En la página **Grupos de clientes** , sigue uno de estos procedimientos:
 - En la sección **Mis grupos de clientes** , selecciona **Crear nuevo grupo** para definir un segmento desde cero. En la página siguiente, seleccione el botón de radio **segmento** .
 - En la sección **plantillas de segmento** , seleccione **copiar** junto a uno de los segmentos predefinidos (que puede usar tal cual o modificar para que se ajuste a sus necesidades).
3.  En el cuadro **nombre de grupo** , escriba un nombre para el segmento.
4.  En la lista **Include customers from this app** , selecciona una de las aplicaciones de destino.
5.  En la sección **definir condiciones de inclusión** , especifique los criterios de filtro para el segmento.

    Puede elegir entre diversos criterios de filtro, entre los que se incluyen las **adquisiciones** , el **origen de adquisición** , los datos **demográficos** , la **clasificación** , la **predicción de renovación** , las **compras** de tiendas, las **adquisiciones de tiendas** y los gastos de **tienda** .

    Por ejemplo, si quieres crear un segmento que solo incluya a los clientes de la aplicación que tengan de 18 a 24 años, selecciona los criterios de filtro [ **Datos demográficos** ] [ **Grupo de edad** ] [ **es** ] [ **18 a 24** ] en las listas desplegables.

    Puedes generar segmentos más complejos mediante consultas AND/OR para incluir o excluir clientes en función de diversos atributos. Para agregar una consulta OR, selecciona **+ OR statement** . Para agregar una consulta ADD, selecciona **Add another filter** .

    Por lo tanto, si quieres precisar que el segmento solo incluya clientes varones en el intervalo de edad especificado, selecciona **Add another filter** , a continuación, selecciona los criterios de filtro adicionales [ **Datos demográficos** ] [ **Sexo** ] [ **es** ] [ **Hombre** ]. En este ejemplo, la **Definición de segmento** mostraría **Grupo de edad == 18 a 24 & & Sexo == Hombre** .

    ![Ejemplo de criterios de filtro para un segmento](images/create-segment-inclusions.png)
6. Seleccione **Guardar** .

> [!IMPORTANT]
> No podrá usar un segmento que incluya pocos clientes. Si tu definición de segmento no incluye suficientes clientes, puedes ajustar los criterios del segmento o intentarlo de nuevo más tarde, cuando la aplicación puede haber adquirido más clientes que cumplan los criterios de tu segmento.


## <a name="app-statistics"></a>Estadísticas de la aplicación

La sección **Estadísticas de la aplicación** del segmento proporciona información sobre la aplicación, así como el tamaño del segmento que acabas de crear.

Ten en cuenta que **Available app customers** no refleja el número real de clientes que han adquirido la aplicación, sino el número de clientes que se pueden incluir en segmentos (es decir, clientes que se puede determinar que cumplen los requisitos de edad, que han adquirido la aplicación en Windows 10 y que están asociados con una cuenta de Microsoft válida).

Si ve los resultados y los **clientes de este segmento** son **pequeños** , el segmento no incluye suficientes clientes y el segmento está marcado como inactivo. Los segmentos inactivos no pueden usarse para notificaciones u otras características. Es posible que puedas activar y usar un segmento mediante una de estas acciones:

- En la sección **Define inclusion conditions** , ajusta los criterios de filtro para que el segmento incluya más clientes.
- En la página **grupos de clientes** , en la sección **segmentos inactivos** , seleccione **Actualizar** para ver si el segmento ahora contiene suficientes clientes (por ejemplo, si más clientes que cumplen los criterios del segmento han descargado la aplicación desde la primera vez que creó el segmento, o si hay más clientes existentes que ahora cumplen los criterios del segmento).
