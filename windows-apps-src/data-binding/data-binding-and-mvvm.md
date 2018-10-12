---
author: KarlErickson
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: Enlace de datos y MVVM
description: Enlace de datos es el núcleo del patrón de diseño de la arquitectura de la interfaz de usuario de Model-View-ViewModel (MVVM) y permite imprecisa entre el código de la interfaz de usuario y que no son de interfaz de usuario.
ms.author: karler
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: eda370db8b68232066052cca3d0abfa6e3876167
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "4538272"
---
# <a name="data-binding-and-mvvm"></a>Enlace de datos y MVVM

Model-View-ViewModel (MVVM) es un patrón de diseño de la arquitectura de la interfaz de usuario para separar el código de la interfaz de usuario y que no son de interfaz de usuario. Con MVVM, puedes definen la interfaz de usuario mediante declaración en XAML y usan el marcado de enlace de datos para vincular a otras capas que contiene datos y comandos. La infraestructura de enlace de datos proporciona un acoplamiento flexible que mantiene la interfaz de usuario y los datos vinculados sincronizan y enruta la entrada del usuario a los comandos adecuados. 

Porque proporciona imprecisa, el uso del enlace de datos reduce el disco duros dependencias entre distintos tipos de código. Esto facilita la cambiar las unidades de código individuales (métodos, clases, controles, etc.) sin provocar efectos secundarios no deseados en otras unidades. Esta separación es un ejemplo de la *separación de problemas*, que es un concepto importante en muchos patrones de diseño. 

## <a name="benefits-of-mvvm"></a>Ventajas de MVVM

Separar el código tiene muchas ventajas, incluidos:

* Habilitación de un estilo de codificación de exploración, iterativo. Cambio que está aislado es menos arriesgados y fácil de experimentar con.
* Las pruebas unitarias simplificar. Unidades de código que están aisladas entre sí se pueden probar por separado como fuera de los entornos de producción.
* Soporte de colaboración de equipo. Código desacoplado conforme a interfaces bien diseñadas puede desarrollado por personas independientes o equipos y se integra más adelante.
* Mejorar la facilidad de mantenimiento. Corrección de errores en código desacoplado es menos probable que causan regresiones en otro código.

A diferencia de MVVM, una aplicación con una estructura de "código subyacente" más convencional por lo general, usa el enlace de datos solo de visualización de datos y responde a la entrada del usuario directamente controlar eventos que se exponen por los controles. Los controladores de eventos se implementan en los archivos de código subyacente (como MainPage.xaml.cs) y son a menudo estrechamente con los controles, por lo general, que contiene el código que manipula directamente de la interfaz de usuario. Esto hace difícil o imposible reemplazar un control sin tener que actualizar el código de control de eventos. Con esta arquitectura, los archivos de código subyacente a menudo acumularán código que no está relacionada directamente con la interfaz de usuario, por ejemplo, el código de acceso de la base de datos, que termina que se va a duplicar y modificar para su uso con otras páginas.

## <a name="app-layers"></a>Capas de la aplicación

Al usar el patrón MVVM, una aplicación se divide en los siguientes niveles:

* La capa de **modelo** define los tipos que representan los datos empresariales. Esto todo lo necesario para el dominio de la aplicación principal del modelo incluye y suele incluir lógica de la aplicación. Este nivel es completamente independiente de la vista y modelo de vista capas y, a menudo reside parcialmente en la nube. Dada una capa de modelo implementado totalmente, puedes crear a varios cliente diferentes aplicaciones si lo desea, como UWP y aplicaciones web que funcionan con los mismos datos subyacentes.
* La capa de **vistas** define la interfaz de usuario con el marcado XAML. El marcado incluye expresiones de enlace de datos (por ejemplo, [x: Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)) que definen la conexión entre componentes específicos de la interfaz de usuario y diversos miembros de modelo de vista y modelo. A veces, los archivos de código subyacente se usan como parte de la capa de vista para contener el código adicional necesario para personalizar o manipular la interfaz de usuario, o para extraer datos de los argumentos del evento de controlador antes de llamar a un método de modelo de vista que realiza el trabajo. 
* La capa de **modelo de vista** proporciona los destinos de enlace de datos para la vista. En muchos casos, el modelo de vista expone el modelo directamente o proporciona a los miembros que se ajustan los miembros de un modelo específico. El modelo de vista también puede definir miembros para realizar un seguimiento de los datos que es relevantes para la interfaz de usuario, pero no en el modelo, por ejemplo, el orden de visualización de una lista de elementos. El modelo de vista también actúa como un punto de integración con otros servicios como código de acceso de la base de datos. Para proyectos simples, no tengas una capa de modelo independiente, pero solo un modelo de vista que encapsula todos los datos que necesita. 

## <a name="basic-and-advanced-mvvm"></a>MVVM básico y avanzado

Al igual que con cualquier patrón de diseño, hay más de una manera de implementar MVVM y técnicas diferentes que se consideran parte de MVVM. Por este motivo, hay varios marcos diferentes de MVVM de terceros compatible con las diversas plataformas basada en XAML, incluida UWP. Sin embargo, estos marcos por lo general, incluyen varios servicios de implementación de arquitectura desacoplado, lo que la definición exacta de MVVM un poco ambiguo. 

Aunque los marcos MVVM sofisticados pueden ser muy útiles, especialmente para proyectos de escala de la empresa, por lo general, hay un costo asociado con la adopción de cualquier patrón determinado o técnica y los beneficios no siempre son evidentes, según la escala y el tamaño de el proyecto. Afortunadamente, puede adoptar solo estas técnicas que proporcionan una ventaja tangible y clara y omitir otros hasta que sea necesario. 

En concreto, puedes obtener una gran cantidad de ventaja simplemente mediante la comprensión y la aplicación toda la potencia de enlace de datos y separar la lógica de la aplicación en las capas que se ha descrito anteriormente. Esto se logra usando solo las funcionalidades proporcionadas por el SDK de Windows y sin usar ningún marco externo. En concreto, la [extensión de marcado {x: Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) facilita el enlace de datos y requiere mayor rendimiento que en las plataformas anteriores de XAML, lo que elimina la necesidad de una gran cantidad del código reutilizable anteriormente.

Para obtener instrucciones adicionales sobre el uso de MVVM básica, de cuadro, echa un vistazo a la [muestra de base de datos de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database) en GitHub. Muchas de las otras [muestras de aplicaciones para UWP](https://github.com/Microsoft?q=windows-appsample
) también usan una arquitectura MVVM básica y la [muestra de la aplicación de tráfico](https://github.com/Microsoft/Windows-appsample-trafficapp) incluye código subyacente y las versiones MVVM, con las notas que describe la [conversión de MVVM](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md). 

## <a name="see-also"></a>Ver también

### <a name="topics"></a>Temas

[Enlace de datos en profundidad](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)  
[Extensión de marcado {x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)  

### <a name="samples"></a>Ejemplos

[Muestra de base de datos de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[Muestra de inventario de VanArsdel](https://github.com/Microsoft/InventorySample)  
[Muestra de la aplicación de tráfico](https://github.com/Microsoft/Windows-appsample-trafficapp)  