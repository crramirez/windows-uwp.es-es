---
description: Este tutorial muestra cómo agregar interfaces de usuario de UWP XAML y crear paquetes MSIX para incorporar otros componentes modernos en su aplicación WPF.
title: Empaquetar e implementar con MSIX
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, uwp, formularios windows forms, wpf, Islas de xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: d11ef296b690297d33ebd5d366c2594f70b6d10b
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420103"
---
# <a name="part-5-package-and-deploy-with-msix"></a>Parte 5: Empaquetar e implementar con MSIX

Esta es la parte final de un tutorial que muestra cómo modernizar una aplicación de escritorio de WPF de ejemplo denominada Contoso gastos. Para obtener información general del tutorial, los requisitos previos y las instrucciones para descargar la aplicación de ejemplo, vea [Tutorial: Modernizar una aplicación de WPF](modernize-wpf-tutorial.md). En este artículo se da por supuesto que ya ha completado [parte 4](modernize-wpf-tutorial-4.md).

En [parte 4](modernize-wpf-tutorial-4.md) aprendió que algunas API de WinRT, incluidas las notificaciones de API, requiere la identidad del paquete antes de que se pueden usar en una aplicación. Puede obtener la identidad del paquete al empaquetar los gastos de Contoso mediante [MSIX](https://docs.microsoft.com/windows/msix), el formato de empaquetado que se introdujo en Windows 10 para empaquetar e implementar aplicaciones de Windows. MSIX ofrece ventajas a la tabla tanto para desarrolladores y profesionales de TI, incluidos:

- Optimiza el espacio de almacenamiento y uso de la red.
- Completar limpiar desinstalar, gracias a un contenedor ligero que se ejecuta la aplicación. No hay claves del registro y los archivos temporales se dejan en el sistema.
- Desacopla las actualizaciones del SO de las actualizaciones de la aplicación y personalizaciones.
- Simplifica la instalación, actualización y el proceso de desinstalación. 

En esta parte del tutorial obtendrá información sobre cómo empaquetar la aplicación de gastos de Contoso en un paquete MSIX.

## <a name="package-the-application"></a>Empaquetar la aplicación

2019 de Visual Studio proporciona una manera sencilla de empaquetar una aplicación de escritorio mediante el proyecto de paquete de aplicación de Windows. 

1. En **el Explorador de soluciones**, haga clic en el **ContosoExpenses** solución y elija **Agregar -> Nuevo proyecto**.

    ![Agregar nuevo proyecto](images/wpf-modernize-tutorial/AddNewProject.png)

3. En el **agregar un nuevo proyecto** cuadro de diálogo, busque `packaging`, elija el **proyecto de empaquetado de aplicaciones de Windows** plantilla de proyecto en el C# categoría y haga clic en **siguiente** .

    ![Proyecto de empaquetado de aplicaciones de Windows](images/wpf-modernize-tutorial/WAP.png)

4. Asigne al nuevo proyecto `ContosoExpenses.Package` y haga clic en **crear**.

5. Seleccione **Windows 10, versión 1903 (10.0; Compilación 18362)** tanto para el **versión de destino** y **versión mínima** y haga clic en **Aceptar**.

    El **ContosoExpenses.Package** proyecto se agrega a la **ContosoExpenses** solución. Este proyecto incluye un [manifiesto del paquete](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root), que describe la aplicación y algunos activos de forma predeterminada que se usan para elementos como el icono en el menú Programas y el icono en la pantalla Inicio. Sin embargo, a diferencia de un proyecto de UWP, el proyecto de empaquetado no contiene código. Su objetivo es empaquetar una aplicación de escritorio existente.

6. En el **ContosoExpenses.Package** del proyecto, haga clic en el **aplicaciones** nodo y elija **Agregar referencia**. Este nodo especifica qué aplicaciones en la solución se incluirá en el paquete.

7. En la lista de proyectos, seleccione **ContosoExpenses.Core** y haga clic en **Aceptar**.

8. Expanda el **aplicaciones** nodo y confirme que del **ContosoExpense.Core** proyecto se hace referencia y resaltado en negrita. Esto significa que se usará como punto de partida para el paquete.

9. Haga clic en el **ContosoExpenses.Package** proyecto y elija **establecer como proyecto de inicio**.

10. Presione **F5** para iniciar la aplicación empaquetada en el depurador.

En este momento, se pueden observar algunos cambios que indican que la aplicación está ahora empaquetado que se ejecuta como:

- El icono en la barra de tareas o en el menú Inicio es ahora el recurso predeterminado que se incluye en cada **proyecto de empaquetado de aplicaciones de Windows**.
- Si hace doble clic en el **ContosoExpense.Package** aplicación aparezca en el menú Inicio, verá las opciones que normalmente se reservan para aplicaciones descargadas desde la Microsoft Store, como **configuración de la aplicación**, **Tasa y revisión** y **Share**.

    ![ContosoExpenses en el menú Inicio](images/wpf-modernize-tutorial/StartMenu.png)

- Si desea desinstalar la aplicación, haga clic en **ContosoExpense.Package** en el menú Inicio y elija **desinstalar**. La aplicación inmediatamente quitará, sin dejar los restos en el sistema.

## <a name="test-the-notification"></a>La notificación de prueba

Ahora que ha empaquetado la aplicación de gastos de Contoso con MSIX, puede probar el escenario de notificación que no funcionaba al final de [parte 4](modernize-wpf-tutorial-4.md).

1. En la aplicación de gastos de Contoso, elija un empleado en la lista y, a continuación, haga clic en el **Agregar gasto nuevo** botón. 
2. Complete todos los campos en el formulario y presione **guardar**.
3. Confirme que ve una notificación del sistema operativo.

![Notificación del sistema](images/wpf-modernize-tutorial/ToastNotification.png)
