---
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: Enlace de datos y MVVM
description: El enlace de datos es el núcleo de la arquitectura de vista de modelos (MVVM) de la interfaz de usuario, y permite el acoplamiento flexible entre la interfaz de usuario y el código que no es de la interfaz de usuario.
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ad0595fa070a1970e4890ce7e95627c06385ba6a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154219"
---
# <a name="data-binding-and-mvvm"></a>Enlace de datos y MVVM

Vista de modelos (MVVM) es un patrón de diseño de arquitectura de interfaz de usuario para desacoplar el código de la interfaz de usuario y el código que no es de la interfaz de usuario. Con MVVM, se define la interfaz de usuario mediante una declaración en XAML y se usa el marcado de enlace de datos para vincularlo a otras capas que contienen datos y comandos. La infraestructura de enlace de datos ofrece un acoplamiento flexible que mantiene la interfaz de usuario y los datos vinculados sincronizados y enruta la entrada del usuario a los comandos adecuados. 

Dado que proporciona un acoplamiento flexible, el uso del enlace de datos reduce las dependencias fuertes entre los distintos tipos de código. Esto facilita el cambio de unidades de código individuales (métodos, clases, controles, etc.) sin causar efectos secundarios imprevistos en otras unidades. Este desacoplamiento es un ejemplo de la *separación de preocupaciones*, que es un concepto importante en muchos patrones de diseño. 

## <a name="benefits-of-mvvm"></a>Ventajas de MVVM

Desacoplar el código tiene muchas ventajas, entre las que se incluyen:

* Permite un estilo de codificación exploratorio iterativo. Los cambios aislados son menos arriesgados y es más fácil experimentar con ellos.
* Simplifica las pruebas unitarias. Las unidades de código que están aisladas entre sí se pueden probar de forma individual y fuera de los entornos de producción.
* Admite el trabajo en equipo. El código desacoplado que se adhiere a interfaces bien diseñadas pueden desarrollarlo usuarios individuales o equipos, e integrarse posteriormente.
* Mejora el mantenimiento. La corrección de errores en el código desacoplado es menos probable que provoque regresiones en otro código.

A diferencia de MVVM, una aplicación con una estructura de "código subyacente" más convencional normalmente usa el enlace de datos para los datos de solo presentación, y responde a la entrada del usuario mediante el control directo de los eventos expuestos por los controles. Los controladores de eventos se implementan en archivos de código subyacente (como MainPage.xaml.cs) y, a menudo, se acoplan estrechamente a los controles, que normalmente contienen código que manipula directamente la interfaz de usuario. Esto hace que sea difícil o imposible reemplazar un control sin tener que actualizar el código de control de eventos. Con esta arquitectura, los archivos de código subyacente suelen acumular código que no está directamente relacionado con la interfaz de usuario, como el código de acceso a la base de datos, y terminan duplicándose y modificándose para usarlos con otras páginas.

## <a name="app-layers"></a>Capas de aplicación

Al usar el patrón MVVM, una aplicación se divide en las siguientes capas:

* La capa del **modelo** define los tipos que representan los datos empresariales. Esto incluye todo lo necesario para modelar el dominio de la aplicación principal y, a menudo, incluye la lógica de la aplicación principal. Esta capa es totalmente independiente de las capas de vista y de vista de modelos, y a menudo reside parcialmente en la nube. Con una capa de modelo totalmente implementada, puedes crear varias aplicaciones cliente diferentes si lo prefieres, como aplicaciones web y de UWP que funcionen con los mismos datos subyacentes.
* La capa de **vista** define la interfaz de usuario mediante el marcado XAML. El marcado incluye expresiones de enlace de datos (como [x:Bind](../xaml-platform/x-bind-markup-extension.md)) que definen la conexión entre componentes de interfaz de usuario específicos y varias vistas de modelos y miembros de modelo. Los archivos de código subyacente a veces se utilizan como parte de la capa de vista para contener el código adicional necesario para personalizar o manipular la interfaz de usuario, o para extraer datos de los argumentos del controlador de eventos antes de llamar a un método de vista de modelos que realiza el trabajo. 
* La capa **vista de modelos** proporciona los destinos del enlace de datos para la vista. En muchos casos, la vista de modelos expone el modelo directamente o proporciona miembros que contienen miembros de modelo específicos. La vista de modelos también puede definir miembros para realizar el seguimiento de los datos que son relevantes para la interfaz de usuario, pero no para el modelo, como el orden de presentación de una lista de elementos. La vista de modelos también sirve como punto de integración con otros servicios, como el código de acceso a la base de datos. En el caso de los proyectos sencillos, es posible que no necesites una capa de modelo independiente, sino solo una vista de modelos que encapsule todos los datos que necesitas. 

## <a name="basic-and-advanced-mvvm"></a>MVVM básico y avanzado

Como en cualquier patrón de diseño, hay más de una manera de implementar MVVM, y muchas técnicas diferentes se consideran parte de MVVM. Por este motivo, hay varios marcos de trabajo de MVVM de terceros que admiten las distintas plataformas basadas en XAML, incluida UWP. Sin embargo, estos marcos suelen incluir varios servicios para implementar la arquitectura desacoplada, lo que permite que la definición exacta de MVVM sea algo ambigua. 

Aunque los marcos de trabajo de MVVM sofisticados pueden ser muy útiles, especialmente en el caso de los proyectos de escala empresarial, normalmente la adopción de un patrón o técnica determinados conlleva un costo y las ventajas no siempre están claras, en función de la escala y el tamaño del proyecto. Afortunadamente, puedes adoptar solo las técnicas que proporcionan una ventaja clara y tangible, y omitir otras hasta que las necesites. 

En concreto, puedes disfrutar de muchas ventajas tan solo con comprender y aplicar todas las posibilidades del enlace de datos, y separar la lógica de la aplicación en las capas descritas anteriormente. Esto se puede lograr usando solo las funcionalidades proporcionadas por Windows SDK y sin usar marcos de trabajo externos. En concreto, la [extensión de marcado {x:Bind}](../xaml-platform/x-bind-markup-extension.md) hace que el enlace de datos sea más sencillo y superior que en las plataformas XAML anteriores, lo que elimina la necesidad de crear la gran cantidad de código reutilizable que se requería anteriormente.

Para más información sobre el uso de la arquitectura MVVM básica lista para usar, consulta el [ejemplo de base de datos de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database), en GitHub. Muchos otros [ejemplos de aplicaciones para UWP](https://github.com/Microsoft?q=windows-appsample
) usan también una arquitectura MVVM básica, y el ejemplo de aplicación de [tráfico](https://github.com/Microsoft/Windows-appsample-trafficapp) incluye tanto la versión de código subyacente como la versión MVVM, con notas que describen la [conversión a MVVM](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md). 

## <a name="see-also"></a>Consulta también

### <a name="topics"></a>Temas

[Enlace de datos en profundidad](./data-binding-in-depth.md)  
[Extensión de marcado {x:Bind}](../xaml-platform/x-bind-markup-extension.md)  

### <a name="samples"></a>Muestras

[Ejemplo de base de datos de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[Ejemplo de inventario de VanArsdel](https://github.com/Microsoft/InventorySample)  
[Ejemplo de aplicación de tráfico](https://github.com/Microsoft/Windows-appsample-trafficapp)