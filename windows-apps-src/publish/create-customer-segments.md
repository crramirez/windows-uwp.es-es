---
author: shawjohn
Description: "Aprende a crear segmentos de clientes para poder dirigirte a un subgrupo de tu base de clientes para fines promocionales o de participación."
title: Crear segmentos de clientes
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.assetid: 58185f6c-d61f-478b-ab24-753d8986cd5a
ms.openlocfilehash: 2ba423d7f7624575e7cf743e9589d31a90d05295
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="create-customer-segments"></a>Crear segmentos de clientes

En ocasiones, puede que quieras dirigirte a un subgrupo de tu base de clientes para fines promocionales y de participación. Puedes hacerlo en el Centro de desarrollo de Windows creando un tipo de [grupo de clientes](create-customer-groups.md) denominado *segmento* que incluya a los clientes de Windows 10 que cumplan con los criterios demográficos o de ingresos que elijas.

Por ejemplo, podrías crear un segmento que incluya solo a clientes de 50 años o más, o que incluya a clientes que han gastado más de 10 USD en la Tienda Windows. También podrías combinar esos criterios y crear un segmento que incluya a todos los clientes de más de 50 años que han gastado más de 10 USD en la Tienda. Proporcionamos algunas plantillas de segmento para que puedas empezar a trabajar, pero puedes definir y combinar los criterios que quieras.

> **Sugerencia:** se pueden usar segmentos para [enviar notificaciones push dirigidas](send-push-notifications-to-your-apps-customers.md) a un grupo de clientes dentro de una campaña de participación.

## <a name="to-create-a-customer-segment"></a>Para crear un segmento de clientes

1.    En el [panel del Centro de desarrollo de Windows](https://developer.microsoft.com/dashboard/overview), selecciona **Clientes** en el menú superior.
2.    En la página **Grupos de clientes**, sigue uno de estos procedimientos:
 - En la sección **Mis grupos de clientes**, selecciona **Crear nuevo grupo** para definir un segmento desde cero. Asegúrate de que la opción **Segmento** está seleccionada en la lista desplegable **Tipo de grupo**.
 - En la sección **Segment templates**, selecciona **Copy to use a predefined segment**, una copia que puedes usar tal cual o modificar según tus necesidades.
3.    En la lista **Include customers from this app**, selecciona una de las aplicaciones de destino.
4.    En la casilla **Nombre del segmento**, elige un nombre para el segmento.
5.    En la sección **Definir condiciones de inclusión**, selecciona los criterios de filtro para el segmento.

    Puedes elegir entre diversos criterios de filtro, incluidos **Origen de adquisición**, **Adquisiciones**, **Datos demográficos**, **Clasificación**, **Adquisiciones de la Tienda**, **Compras de la tienda** y **Gasto de la Tienda**.

    Por ejemplo, si quieres crear un segmento que solo incluya a los clientes de la aplicación que tengan de 18 a 24 años, selecciona los criterios de filtro [**Datos demográficos**] [**Grupo de edad**] [**es**] [**18 a 24**] en las listas desplegables.

    Puedes generar segmentos más complejos mediante consultas AND/OR para incluir o excluir clientes en función de diversos atributos. Para agregar una consulta OR, selecciona **+ OR statement**. Para agregar una consulta ADD, selecciona **Add another filter**.

    Por lo tanto, si quieres precisar que el segmento solo incluya clientes varones en el intervalo de edad especificado, selecciona **Add another filter**, a continuación, selecciona los criterios de filtro adicionales [**Datos demográficos**] [**Sexo**] [**es**] [**Hombre**]. En este ejemplo, la **Definición de segmento** mostraría **Grupo de edad == 18 a 24 & & Sexo == Hombre**.

    ![Ejemplo de criterios de filtro para un segmento](images/create-segment-inclusions.png)
6. Selecciona **Guardar**.

> **Importante:** no podrás usar un segmento que incluya muy pocos clientes. Si tu definición de segmento no incluye suficientes clientes, puedes ajustar los criterios del segmento o intentarlo de nuevo más tarde, cuando la aplicación puede haber adquirido más clientes que cumplan los criterios de tu segmento.

Cosas que tener en cuenta sobre segmentos de clientes:
- Después de guardar un segmento, se tarda 24 horas en poder usarlo para [notificaciones push dirigidas](send-push-notifications-to-your-apps-customers.md).
- Los resultados de segmentos se actualizan a diario, por lo que es posible que veas que el recuento total de clientes de un segmento cambia de un día para otro, a medida que haya más o menos clientes que cumplan los requisitos.
- La mayoría de estos atributos se calculan con todos los datos de historial, aunque hay algunas excepciones. Por ejemplo, **App acquisition date**, **Id. de campaña**, **Store page view date** y **Referrer URI domain** están limitados a los últimos 90 días de datos.
- El segmento solo incluirá a clientes que hayan adquirido la aplicación en Windows 10. Si la aplicación es compatible con versiones anteriores del sistema operativo, los clientes que usen esas versiones no se incluirán en los segmentos que crees.
- Los segmentos excluirán automáticamente a todos los clientes menores de 17 años.


## <a name="app-statistics"></a>Estadísticas de la aplicación

La sección **Estadísticas de la aplicación** del segmento proporciona información sobre la aplicación, así como el tamaño del segmento que acabas de crear.

Ten en cuenta que **Available app customers** no refleja el número real de clientes que han adquirido la aplicación, sino el número de clientes que se pueden incluir en segmentos (es decir, clientes que se puede determinar que cumplen los requisitos de edad, que han adquirido la aplicación en Windows 10 y que están asociados con una cuenta de Microsoft válida).

Si ves los resultados y **Customers in this segment** dice **Pequeño**, el segmento no incluye suficientes clientes y está marcado como inactivo. Los segmentos inactivos no pueden usarse para notificaciones u otras características. Es posible que puedas activar y usar un segmento mediante una de estas acciones:

- En la sección **Define inclusion conditions**, ajusta los criterios de filtro para que el segmento incluya más clientes.
- En la página **Grupos de clientes**, en la sección **Inactive segments**, selecciona **Actualizar** para ver si el segmento contiene actualmente suficientes clientes. Esta táctica podría dar resultados, por ejemplo, si más clientes que cumplen tus criterios de segmento han descargado tu aplicación desde que creaste el segmento.
