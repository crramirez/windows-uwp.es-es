---
author: jnHs
Description: "La página Propiedades de la aplicación del proceso de envío de aplicaciones te permite definir la categoría de la aplicación e indicar las preferencias de hardware u otras declaraciones."
title: "Introducir las propiedades de la aplicación"
ms.assetid: CDE4AF96-95A0-4635-9D07-A27B810CAE26
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 008c073844332aadbc17def774aae16f3ae01513
ms.lasthandoff: 02/07/2017

---

# <a name="enter-app-properties"></a>Introducir las propiedades de la aplicación

La página **Propiedades de la aplicación** del [proceso de envío de aplicaciones](app-submissions.md) te permite definir la categoría de la aplicación e indicar las preferencias de hardware u otras declaraciones. A continuación, te guiaremos a través de las opciones de esta página y todo lo que debes tener en cuenta al escribir la información.

> **Nota**  Las clasificaciones por edades son ahora una página independiente del proceso de envío. Para más información, consulta [Clasificaciones por edades](age-ratings.md).

## <a name="category-and-subcategory"></a>Categoría y subcategoría

En esta sección, debes indicar la categoría (y la subcategoría, si procede) que debe usar la Tienda para clasificar la aplicación. Es necesario especificar una categoría para enviar la aplicación.

Para más información, consulta [Tabla de categoría y subcategoría](category-and-subcategory-table.md).

## <a name="product-declarations"></a>Declaraciones de producto

Puedes marcar casillas en esta sección para indicar si alguna de las declaraciones se aplica a la aplicación. Esto puede afectar a la manera en que se muestra la aplicación, a si se ofrece a determinados clientes o al modo en que los clientes pueden usarla.

Para obtener más información, consulta [Declaraciones de la aplicación](app-declarations.md).

## <a name="system-requirements"></a>Requisitos del sistema

En esta sección, tienes la opción de indicar si determinadas características de hardware son necesarias o recomendables para que la aplicación se ejecute correctamente y para interactuar con ella. Puedes marcar la casilla (o indicar la opción adecuada) para cada elemento de hardware donde quieras especificar **Requisitos mínimos de hardware** o **Hardware recomendado**.

Si haces selecciones para **Hardware recomendado**, esos elementos se mostrarán en la descripción de la Tienda del producto como hardware recomendado para los clientes de Windows 10, versión 1607 o posterior. Los clientes con versiones anteriores del sistema operativo no verán esta información.

Si haces selecciones para **Requisitos mínimos de hardware**, esos elementos se mostrarán en la descripción de la Tienda del producto como hardware requerido para los clientes de Windows 10, versión 1607 o posterior. Los clientes con versiones anteriores del sistema operativo no verán esta información. La Tienda también puede mostrar una advertencia a los clientes que vean la descripción de la aplicación en un dispositivo que no tenga el hardware necesario. Esto no impedirá que los usuarios descarguen la aplicación en dispositivos que no tengan el hardware apropiado, pero no podrán evaluarla ni opinar sobre ella desde esos dispositivos. 

El comportamiento para los clientes dependerá de los requisitos específicos y la versión del cliente de Windows:

- **Para los clientes de Windows 10, versión 1607 o posterior:**
     - Todos los requisitos mínimos y recomendados se mostrará en la descripción de la Tienda.
     - La Tienda buscará todos los requisitos mínimos y mostrará una advertencia a los clientes que usen un dispositivo que no cumpla los requisitos.
- **Para los clientes de versiones anteriores de Windows 10:**
     - Para la mayoría de los clientes, todos los requisitos de hardware mínimos y recomendados se mostrarán en la descripción de la Tienda (aunque los clientes que visualicen una versión anterior de cliente de la Tienda solo verán los requisitos mínimos de hardware).
     - La Tienda intentará comprobar los elementos que designes como **Requisitos mínimos de hardware**, con la excepción de **Memoria**, **DirectX**, **Memoria de vídeo**, **Gráficos** y **Procesador**. Estos no se comprobarán y los clientes no verán ninguna advertencia en los dispositivos que no cumplan estos requisitos. 
- **Para los clientes de Windows 8.x y versiones anteriores, o de Windows Phone 8.x y versiones anteriores:**
     - Si activas la casilla **Requisitos mínimos de hardware** para **Pantalla táctil**, este requisito se mostrará en la descripción de la Tienda de la aplicación y los clientes con dispositivos sin pantalla táctil verán una advertencia si intentan descargar la aplicación. No se comprobarán otros requisitos ni se mostrarán en la descripción de la Tienda.

También recomendamos agregar a la aplicación comprobaciones en tiempo de ejecución para el hardware especificado, dado que la Tienda no siempre puede detectar si al dispositivo de un cliente le faltan las características seleccionadas. De todos modos, el cliente podrá descargar la aplicación, aunque se le muestre una advertencia.

> **Sugerencia**  Si quieres evitar completamente que tu aplicación para UWP se descargue en un dispositivo que no cumpla con los requisitos mínimos de memoria o nivel de DirectX, puedes designar los requisitos mínimos en un archivo XML de StoreManifest. Para obtener más información, consulta [Esquema StoreManifest (Windows 10)](https://msdn.microsoft.com/library/windows/apps/mt617335).



