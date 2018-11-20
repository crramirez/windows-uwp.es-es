---
author: jwmsft
ms.assetid: 3A477380-EAC5-44E7-8E0F-18346CC0C92F
title: Virtualización de datos de ListView y GridView
description: Mejora el rendimiento y el tiempo de inicio de las clases ListView y GridView mediante la virtualización de datos.
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 92b81c79eb1be9e21aa7c306ef31b0b3bb62e7d1
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2018
ms.locfileid: "7285513"
---
# <a name="listview-and-gridview-data-virtualization"></a>Virtualización de datos de ListView y GridView


**Nota**para obtener más información, consulta la sesión de //build/ [Considerablemente aumentar el rendimiento cuando los usuarios interactúan con grandes cantidades de datos de ListView y GridView](https://channel9.msdn.com/Events/Build/2013/3-158).

Mejorar el rendimiento y el tiempo de inicio de [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) y [**GridView**](https://msdn.microsoft.com/library/windows/apps/BR242705) mediante la virtualización de datos. Para la virtualización de la interfaz de usuario, reducción de elementos y la actualización progresiva de elementos, consulta el tema [Optimización de ListView y GridView UI](optimize-gridview-and-listview.md).

Se necesita un método de virtualización de datos para un conjunto de datos que sea tan grande que no se pueda o no se deba almacenar en la memoria de una sola vez. Cargas una porción inicial en la memoria (disco local, red o nube) y aplicas la virtualización de la interfaz de usuario a este conjunto de datos parcial. Más tarde, se pueden cargar datos de manera incremental o desde puntos arbitrarios del conjunto de datos maestro (acceso aleatorio) a petición. Determinar si la virtualización es apropiada para tu caso depende de muchos factores.

-   El tamaño del conjunto de datos
-   El tamaño de cada elemento
-   El origen del conjunto de datos (disco local, red o nube)
-   El consumo de memoria total de la aplicación

**Nota**Ten en cuenta que una característica está habilitada de manera predeterminada para ListView y GridView que muestra elementos visuales de marcador de posición temporales mientras el usuario realiza movimiento panorámico/desplazamiento rápido. A medida que se cargan los datos, estos elementos visuales de marcador de posición se reemplazan por la plantilla de elemento. Para desactivar la característica, puedes establecer [**ListViewBase.ShowsScrollingPlaceholders**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listviewbase.showsscrollingplaceholders) en false, aunque si lo haces, te recomendamos que uses el atributo x:Phase para representar de manera progresiva los elementos de la plantilla de elemento. Consulta [Actualizar los elementos ListView y GridView de forma progresiva](optimize-gridview-and-listview.md#update-items-incrementally).

Aquí encontrarás más información acerca de las técnicas de virtualización de datos incremental y de acceso aleatorio.

## <a name="incremental-data-virtualization"></a> Virtualización de datos incremental

La virtualización de datos incremental carga los datos en secuencia. Un [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) que usa la virtualización de datos incremental puede usarse para ver una colección de un millón de elementos, pero solo 50 elementos se cargan inicialmente. A medida que el usuario realiza un movimiento panorámico/desplazamiento, se cargan los 50 siguientes. A medida que se cargan los elementos, se reduce el tamaño del control de la barra de desplazamiento. Para este tipo de virtualización de datos, debes escribir una clase de origen de datos que implemente estas interfaces.

-   [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx)
-   [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) (C#/VB) o [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052) (C++/CX)
-   [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/Hh701916)

Un origen de datos como el siguiente, es una lista en memoria que se puede extender continuamente. El control de elementos solicitará elementos con el indexador [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx) estándar y las propiedades de recuento. El recuento debe representar el número de elementos localmente, no el tamaño real del conjunto de datos.

Cuando el control de elementos se acerque al final de los datos existentes, llamará a [**ISupportIncrementalLoading.HasMoreItems**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.isupportincrementalloading.hasmoreitems). Si devuelves **true**, a continuación, llamará a [**ISupportIncrementalLoading.LoadMoreItemsAsync**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.isupportincrementalloading.loadmoreitemsasync) pasando un número de elementos para cargar recomendado. Dependiendo desde dónde se están cargando datos (disco local, red o nube), puedes cargar un número diferente de elementos que el recomendado. Por ejemplo, si el servicio admite lotes de 50 elementos, pero el control de elementos solo solicita 10, puede cargar 50. Carga los datos desde el backend, agrégalos a la lista y genera una notificación de cambio a través de la interfaz [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) o [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052), para que el control de elementos reconozca los elementos nuevos. Asimismo, también devuelve un recuento de los elementos cargados realmente. Si cargas menos elementos que lo recomendado o el control de elementos se ha movido panorámicamente/desplazado incluso más aún en el transcurso, se llamará nuevamente al origen de datos para obtener más elementos y el ciclo continuará. Puedes obtener más al descargar la [muestra de enlace de datos XAML](https://code.msdn.microsoft.com/windowsapps/Data-Binding-7b1d67b5) para Windows8.1 y volver a usar su código fuente en la aplicación de Windows 10.

## <a name="random-access-data-virtualization"></a>Virtualización de datos de acceso aleatorio

La virtualización de datos de acceso aleatorio permite cargar desde un punto arbitrario del conjunto de datos. Un [**ListView**](https://msdn.microsoft.com/library/windows/apps/BR242878) que usa la virtualización de datos de acceso aleatorio, usado para ver una colección de un millón de elementos, puede cargar los elementos del 100000 al 100050. Si el usuario se mueve al principio de la lista, el control carga los elementos del 1 al 50. En cualquier momento, el control de la barra de desplazamiento indica que **ListView** contiene un millón de elementos. El control de posición de la barra de desplazamiento se ubica en relación con el lugar donde se encuentran los elementos visibles en el conjunto de datos completo de la colección. Este tipo de virtualización de datos puede reducir significativamente los requisitos de memoria y los tiempos de carga de la colección. Para habilitarla, debes escribir una clase de origen de datos que recupere los datos a petición y administre una memoria caché local e implemente estas interfaces.

-   [**IList**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.ilist.aspx)
-   [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) (C#/VB) o [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052) (C++/CX)
-   (Opcional) [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070)
-   (Opcional) [**ISelectionInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877074)

[**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070) proporciona información sobre los elementos que está usando activamente el control. El control de elementos llamará a este método siempre que cambie su vista e incluirá estos dos conjuntos de intervalos.

-   El conjunto de elementos que existe en la ventanilla.
-   Un conjunto de elementos no virtualizados que está usando el control que puede que no estén en la ventanilla.
    -   Un búfer de elementos de la ventanilla que el control de elementos mantiene para que el movimiento panorámico táctil sea suave.
    -   El elemento especificado.
    -   El primer elemento.

Al implementar [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070), el origen de datos sabe qué elementos deben capturarse y almacenarse en caché, y cuándo eliminarse de los datos de caché que ya no son necesarios. **IItemsRangeInfo** usa objetos [**ItemIndexRange**](https://msdn.microsoft.com/library/windows/apps/Dn877081) para describir un conjunto de elementos en función del índice de la colección. Esto permite evitar que se usen punteros de elemento, que podrían no ser correctos o estables. **IItemsRangeInfo** está diseñado para usarse en una sola instancia de un control de elementos, porque se basa en la información de estado para ese control de elementos. Si varios controles de elementos necesitan acceso a los mismos datos, necesitarás una instancia por separado del origen de datos para cada uno. Pueden compartir una memoria caché común, pero la lógica de depuración de la caché será más complicada.

Esta es la estrategia básica para el origen de datos de la virtualización de datos de acceso aleatorio.

-   Cuando se solicite un elemento
    -   Si está disponible en la memoria, devuélvelo.
    -   Si no lo está, devuelve null o un elemento de marcador de posición.
    -   Usa la solicitud de un elemento (o la información del rango de [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070)) para saber qué elementos son necesarios y para capturar datos de los elementos del backend de forma asincrónica. Después de recuperar los datos, genera una notificación de cambio a través de [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/xaml/system.collections.specialized.inotifycollectionchanged.aspx) o [**IObservableVector&lt;T&gt;**](https://msdn.microsoft.com/library/windows/apps/BR226052) para que el control de elementos sepa del nuevo elemento.
-   (Opcionalmente) A medida que la ventanilla del control de elementos cambia, identifica qué elementos son necesarios del origen de datos a través de la implementación de [**IItemsRangeInfo**](https://msdn.microsoft.com/library/windows/apps/Dn877070).

Además, la estrategia de cuándo cargar elementos de datos, la cantidad de carga y qué elementos mantener en memoria depende de tu aplicación. Algunas consideraciones generales que se deben tener en cuenta:

-   Realiza solicitudes asincrónicas para datos; no bloquees el subproceso de la interfaz de usuario.
-   Encuentra el punto óptimo en el tamaño de los lotes en los que capturas elementos. Se recomienda que sea más grande que pequeño. No tan pequeña que realizas demasiadas solicitudes pequeñas; ni tan grande que tarda demasiado tiempo en recuperarse.
-   Considera la posibilidad de cuántas solicitudes que desees tener pendientes al mismo tiempo. Realizar una a la vez es más fácil, pero puede ser demasiado lento si el tiempo de procesamiento es alto.
-   ¿Puedes cancelar las solicitudes de datos?
-   Si usas un servicio hospedado, ¿existe un costo por transacción?
-   ¿Qué tipo de notificaciones proporciona el servicio cuando se cambian los resultados de una consulta? ¿Sabrás si se inserta un elemento en el índice 33? Si el servicio admite consultas basadas en tecla-más-desplazamiento, podría ser mejor que el uso de un índice.
-   ¿Qué tan inteligente deseas ser en la captura previa elementos? ¿Probarás realizar un seguimiento de la dirección y velocidad de desplazamiento para predecir qué elementos se necesitan?
-   ¿Con qué dinamismo quieres purgar la memoria caché? Esto es un equilibrio entre el uso de la memoria frente a la experiencia.




