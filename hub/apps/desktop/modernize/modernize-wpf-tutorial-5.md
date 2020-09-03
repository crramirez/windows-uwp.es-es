---
description: En este tutorial se muestra cómo agregar interfaces de usuario de XAML en UWP, crear paquetes MSIX e incorporar otros componentes actuales en la aplicación de WPF.
title: Empaquetar e implementar con MSIX
ms.topic: article
ms.date: 01/23/2020
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, islas xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 18b89caa0de947d2b95b46c3deb11378912b6012
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161429"
---
# <a name="part-5-package-and-deploy-with-msix"></a>5\.ª parte: Empaquetar e implementar con MSIX

Esta es la parte final de un tutorial en el que se muestra cómo modernizar una aplicación de escritorio de WPF de ejemplo denominada Contoso Expenses. Para obtener información general sobre el tutorial, los requisitos previos y las instrucciones para descargar la aplicación de ejemplo, consulta [Tutorial: modernizar una aplicación de WPF](modernize-wpf-tutorial.md). En este artículo se da por supuesto que ya has completado la [parte 4](modernize-wpf-tutorial-4.md).

En la [parte 4](modernize-wpf-tutorial-4.md) has aprendido que algunas API de WinRT, incluida la API de notificaciones, requieren la identidad del paquete antes de que se puedan usar en una aplicación. Puedes obtener la identidad del paquete con el empaquetado de Contoso Expenses mediante [MSIX](/windows/msix), el formato de empaquetado introducido en Windows 10 para empaquetar e implementar aplicaciones de Windows. MSIX ofrece ventajas a los desarrolladores y profesionales de TI, entre las que se incluyen:

- Uso de red y espacio de almacenamiento optimizados.
- Desinstalación limpia completa, gracias a un contenedor ligero donde se ejecuta la aplicación. No se han dejado claves del Registro ni archivos temporales en el sistema.
- Desacopla las actualizaciones del sistema operativo de las actualizaciones y personalizaciones de la aplicación.
- Simplifica el proceso de instalación, actualización y desinstalación.

En esta parte del tutorial, aprenderás a empaquetar la aplicación Contoso Expenses en un paquete MSIX.

## <a name="package-the-application"></a>Empaquetado de la aplicación

Visual Studio 2019 proporciona una manera sencilla de empaquetar una aplicación de escritorio mediante el proyecto de paquete de aplicación de Windows. 

1. En el **Explorador de soluciones**, haz clic con el botón derecho en la solución **ContosoExpenses** y elige **Agregar -> Nuevo proyecto**.

    ![Agregar nuevo proyecto](images/wpf-modernize-tutorial/AddNewProject.png)

3. En el cuadro de diálogo **Agregar un nuevo proyecto**, busca `packaging`, elige la plantilla de proyecto **Proyecto de paquete de aplicación de Windows** en la categoría de C# y haz clic en **Siguiente**.

    ![Proyecto de paquete de aplicación de Windows](images/wpf-modernize-tutorial/WAP.png)

4. Asigna un nombre al nuevo proyecto `ContosoExpenses.Package` y haz clic en **Crear**.

5. Selecciona **Windows 10, versión 1903 (10.0; compilación 18362)** para la **versión de destino** y la **versión mínima** y haz clic en **Aceptar**.

    El proyecto **ContosoExpenses.Package** se agrega a la solución **ContosoExpenses**. Este proyecto incluye un [manifiesto de paquete](/uwp/schemas/appxpackage/uapmanifestschema/schema-root), en el que se describen la aplicación y algunos recursos predeterminados que se usan para elementos como el icono del menú Programas y el icono de la pantalla Inicio. Sin embargo, a diferencia de un proyecto de UWP, el proyecto de empaquetado no contiene código. Su finalidad es empaquetar una aplicación de escritorio existente.

6. En el proyecto **ContosoExpenses.Package**, haz clic con el botón derecho en el nodo **Aplicaciones** y elige **Agregar referencia**. Este nodo especifica qué aplicaciones de la solución se incluirán en el paquete.

6. En la lista de proyectos, selecciona **ContosoExpenses.Core** y haz clic en **Aceptar**.

7. Expande el nodo **Aplicaciones** y confirma que se hace referencia al proyecto **ContosoExpense.Core** y que está resaltado en negrita. Esto significa que se usará como punto de partida para el paquete.

8. Haz clic con el botón derecho en el proyecto **ContosoExpenses.Package** y elige **Establecer como proyecto de inicio**.

9. Presiona **F5** para iniciar la aplicación empaquetada en el depurador.

En este momento, puedes observar algunos cambios que indican que la aplicación se está ejecutando ahora como empaquetada:

- El icono de la barra de tareas o el menú Inicio es ahora el recurso predeterminado que se incluye en cada **proyecto de paquete de aplicación de Windows**.
- Si haces clic con el botón derecho en la aplicación **ContosoExpense.Package** que aparece en el menú Inicio, observarás las opciones que normalmente se reservan para las aplicaciones descargadas de Microsoft Store, como **Configuración de aplicaciones**, **Calificar y opinar** y **Compartir**.

    ![ContosoExpenses en el menú Inicio](images/wpf-modernize-tutorial/StartMenu.png)

- Si deseas desinstalar la aplicación, puedes hacer clic con el botón derecho en **ContosoExpense.Package** en el menú Inicio y elegir **Desinstalar**. La aplicación se quitará de inmediato, sin que haya ningún sobrante en el sistema.

## <a name="test-the-notification"></a>Prueba de la notificación

Ahora que has empaquetado la aplicación Contoso Expenses con MSIX, puedes probar el escenario de notificación que no estaba funcionando al final de la [parte 4](modernize-wpf-tutorial-4.md).

1. En la aplicación Contoso Expenses, elige un empleado de la lista y, a continuación, haz clic en el botón **Agregar gasto nuevo**.
2. Completa todos los campos del formulario y presiona **Guardar**.
3. Confirma que ves una notificación del sistema operativo.

![Notificación del sistema](images/wpf-modernize-tutorial/ToastNotification.png)