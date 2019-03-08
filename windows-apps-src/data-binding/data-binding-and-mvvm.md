---
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: Enlace de datos y MVVM
description: Enlace de datos es el núcleo del patrón de diseño de arquitectura Model-View-ViewModel (MVVM) interfaz de usuario y permite el acoplamiento flexible entre el código de interfaz de usuario y que no son de interfaz de usuario.
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 931f2fcbcdbf58b9dc2ca40403d7466b620a8991
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616740"
---
# <a name="data-binding-and-mvvm"></a>Enlace de datos y MVVM

Model-View-ViewModel (MVVM) es un patrón de diseño de arquitectura de interfaz de usuario para desacoplar el código de interfaz de usuario y que no son de interfaz de usuario. Con MVVM, definir la interfaz de usuario mediante declaración en XAML y usar marcado de enlace de datos para vincularla a otras capas que contiene los datos y comandos. La infraestructura de enlace de datos proporciona un acoplamiento flexible que mantiene la interfaz de usuario y los datos vinculados se sincronizan y enruta la entrada del usuario a los comandos adecuados. 

Ya que proporciona un acoplamiento flexible, el uso del enlace de datos reduce dependencias fuertes entre los distintos tipos de código. Esto facilita cambiar las unidades de código individuales (métodos, las clases, controles, etc.) sin causar efectos secundarios no deseados en otras unidades. Este desacoplamiento es un ejemplo de la *separación de preocupaciones*, que es un concepto importante en muchos patrones de diseño. 

## <a name="benefits-of-mvvm"></a>Ventajas de MVVM

Desacoplar el código tiene muchas ventajas, como:

* Habilitación de un estilo de codificación iterativo y exploratorio. Cambio que está aislado es menos arriesgado y más fácil experimentar con.
* Pruebas unitarias simplificar. Unidades de código que están completamente aisladas entre sí se pueden probar individualmente y fuera de los entornos de producción.
* Admitir la colaboración en equipo. Código desacoplado que se adhiere a las interfaces bien diseñadas puede desarrollado por personas independientes o equipos e integrada más adelante.
* Mejorar la facilidad de mantenimiento. Corrección de errores en código desacoplado es menos probable que causan regresiones en el otro código.

A diferencia de MVVM, una aplicación con una estructura más convencional de "código" normalmente usa el enlace de datos para datos de sólo visualización y responde a la entrada del usuario directamente administrando eventos expuestos por los controles. Los controladores de eventos se implementan en los archivos de código subyacente (por ejemplo, MainPage.xaml.cs) y, a menudo estrechamente a los controles, que normalmente contiene el código que manipula directamente la interfaz de usuario. Esto facilita difíciles o imposibles de reemplazar un control sin tener que actualizar el código de control de eventos. Con esta arquitectura, los archivos de código subyacente acumulan a menudo código que no está directamente relacionados con la interfaz de usuario, como el código de acceso de la base de datos, que termina está duplicado y se puede modificar para su uso con otras páginas.

## <a name="app-layers"></a>Capas de aplicación

Cuando se usa el patrón MVVM, una aplicación se divide en las siguientes capas:

* El **modelo** capa define los tipos que representan los datos empresariales. Esto todo lo necesario para modelar el dominio de aplicación principal incluye y, a menudo incluye lógica de aplicación principal. Esta capa es completamente independiente de la vista y las capas del modelo de vista y, a menudo reside parcialmente en la nube. Dado un nivel de modelo implementada totalmente, puede crear a otro cliente varias aplicaciones si lo prefiere, como aplicaciones web y UWP que funcionan con los mismos datos subyacentes.
* El **vista** capa define la interfaz de usuario mediante marcado XAML. El marcado incluye expresiones de enlace de datos (como [x: Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)) que definen la conexión entre los componentes específicos de la interfaz de usuario y diversos miembros del modelo de vista y modelos. A veces, los archivos de código subyacente se usan como parte de la capa de vista para que contenga código adicional necesario para personalizar o manipular la interfaz de usuario, o para extraer datos de los argumentos de controlador de eventos antes de llamar a un método de modelo de vista que realiza el trabajo. 
* El **vista-modelo** capa proporciona los destinos de enlace de datos para la vista. En muchos casos, el modelo de vista expone el modelo directamente, o proporciona a miembros que contienen miembros de modelo específico. El modelo de vista también puede definir los miembros para realizar el seguimiento de los datos que son pertinentes para la interfaz de usuario pero no para el modelo, como el orden de visualización de una lista de elementos. El modelo de vista también actúa como un punto de integración con otros servicios como código de acceso de la base de datos. En los proyectos sencillos, quizás no necesite un nivel de modelo independiente, pero solo un modelo de vista que encapsula todos los datos que necesita. 

## <a name="basic-and-advanced-mvvm"></a>MVVM básica y avanzada

Al igual que con cualquier modelo de diseño, hay más de una forma de implementar MVVM y muchas técnicas diferentes se consideran parte de MVVM. Por este motivo, hay varios marcos diferentes de MVVM de terceros que admiten las diferentes plataformas basada en XAML, incluidos UWP. Sin embargo, estos marcos de trabajo generalmente incluyen varios servicios para implementar una arquitectura desacoplada, lo que la definición exacta de MVVM un poco ambiguo. 

Aunque sofisticado marcos de MVVM pueden ser muy útiles, especialmente para los proyectos a escala empresarial, normalmente hay un costo asociado a adoptar cualquier patrón determinado o técnica, y los beneficios no siempre están claros, según la escala y el tamaño de el proyecto. Afortunadamente, puede adoptar solo esas técnicas que proporcionan una ventaja clara y tangible y pase por alto otros hasta que la necesite. 

En concreto, puede obtener mucha ventaja basta con comprender y aplicar toda la funcionalidad de enlace de datos y separar la lógica de aplicación en las capas que se ha descrito anteriormente. Esto puede lograrse mediante solo las funcionalidades que ofrece el SDK de Windows, sin usar los marcos de trabajo externos. En concreto, el [extensión de marcado {x: Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) facilita el enlace de datos y mayor rendimiento que en plataformas anteriores de XAML, eliminando la necesidad de un lote del código reutilizable necesario anteriormente.

Para obtener orientación adicional sobre el uso básico, estándar de MVVM, eche un vistazo el [ejemplo de la base de datos de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database) en GitHub. Muchos de los otros [ejemplos de aplicaciones UWP](https://github.com/Microsoft?q=windows-appsample
) también usan una arquitectura básica de MVVM y el [ejemplo de aplicación de tráfico](https://github.com/Microsoft/Windows-appsample-trafficapp) incluye código subyacente y las versiones MVVM, con notas que describe el [conversión MVVM ](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md). 

## <a name="see-also"></a>Consulte también

### <a name="topics"></a>Temas

[Enlace de datos en profundidad](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)  
[extensión de marcado {x: Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)  

### <a name="samples"></a>Muestras

[Ejemplo de la base de datos de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[Ejemplo de inventario de VanArsdel](https://github.com/Microsoft/InventorySample)  
[Ejemplo de aplicación de tráfico](https://github.com/Microsoft/Windows-appsample-trafficapp)  
