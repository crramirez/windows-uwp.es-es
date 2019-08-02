---
description: En este tutorial se muestra cómo agregar interfaces de usuario XAML de UWP, crear paquetes de MSIX e incorporar otros componentes modernos en la aplicación WPF.
title: Empaquetar e implementar con MSIX
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, Islas XAML
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 961157bc3d3429b56d3da24a46d71cbb5b84e7a3
ms.sourcegitcommit: 3cc6eb3bab78f7e68c37226c40410ebca73f82a9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2019
ms.locfileid: "68729498"
---
# <a name="part-5-package-and-deploy-with-msix"></a>5\.ª parte: Empaquetar e implementar con MSIX

Esta es la parte final de un tutorial que muestra cómo modernizar una aplicación de escritorio WPF de ejemplo denominada gastos de contoso. Para obtener información general sobre el tutorial, los requisitos previos y las instrucciones para descargar la aplicación de [ejemplo, vea Tutorial: Modernizar una aplicación](modernize-wpf-tutorial.md)de WPF. En este artículo se supone que ya ha completado la [parte 4](modernize-wpf-tutorial-4.md).

En la [parte 4](modernize-wpf-tutorial-4.md) aprendió que algunas API de WinRT, incluida la API de notificaciones, requieren la identidad del paquete antes de que se puedan usar en una aplicación. Puede obtener la identidad del paquete mediante el empaquetado de gastos de Contoso con [MSIX](https://docs.microsoft.com/windows/msix), el formato de empaquetado introducido en Windows 10 para empaquetar e implementar aplicaciones de Windows. MSIX ofrece ventajas a la tabla tanto para desarrolladores como para profesionales de ti, incluidos:

- Uso optimizado de la red y espacio de almacenamiento.
- Complete la desinstalación limpia, gracias a un contenedor ligero donde se ejecuta la aplicación. No se han dejado claves del registro ni archivos temporales en el sistema.
- Desacopla las actualizaciones del sistema operativo de las actualizaciones y personalizaciones de la aplicación.
- Simplifica el proceso de instalación, actualización y desinstalación. 

En esta parte del tutorial aprenderá a empaquetar la aplicación de gastos de Contoso en un paquete de MSIX.

## <a name="package-the-application"></a>Empaquetar la aplicación

Visual Studio 2019 proporciona una manera sencilla de empaquetar una aplicación de escritorio mediante el proyecto de paquete de aplicación de Windows. 

1. En **Explorador de soluciones**, haga clic con el botón derecho en la solución **ContosoExpenses** y elija **Agregar > nuevo proyecto**.

    ![Agregar nuevo proyecto](images/wpf-modernize-tutorial/AddNewProject.png)

3. En el cuadro de diálogo **Agregar un nuevo proyecto** , busque `packaging`, elija la plantilla de proyecto proyecto de paquete de aplicación C# de **Windows** en la categoría y haga clic en **siguiente**.

    ![Proyecto de paquete de aplicación de Windows](images/wpf-modernize-tutorial/WAP.png)

4. Asigne un nombre al `ContosoExpenses.Package` nuevo proyecto y haga clic en **crear**.

5. Seleccione **Windows 10, versión 1903 (10,0; Compilación 18362)** para la **versión de destino** y la **versión mínima** , y haga clic en **Aceptar**.

    El proyecto **ContosoExpenses. Package** se agrega a la solución **ContosoExpenses** . Este proyecto incluye un [manifiesto del paquete](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root), que describe la aplicación y algunos activos predeterminados que se usan para elementos como el icono del menú programas y el icono en la pantalla Inicio. Sin embargo, a diferencia de un proyecto de UWP, el proyecto de empaquetado no contiene código. Su finalidad es empaquetar una aplicación de escritorio existente.

6. En el proyecto **ContosoExpenses. Package** , haga clic con el botón secundario en el nodo **aplicaciones** y elija **Agregar referencia**. Este nodo especifica las aplicaciones de la solución que se incluirán en el paquete.

7. En la lista de proyectos, seleccione **ContosoExpenses. Core** y haga clic en **Aceptar**.

8. Expanda el nodo **aplicaciones** y confirme que se hace referencia al proyecto **ContosoExpense. Core** y que se resalta en negrita. Esto significa que se usará como punto de partida para el paquete.

9. Haga clic con el botón derecho en el proyecto **ContosoExpenses. Package** y elija **establecer como proyecto de inicio**.

10. En Explorador de soluciones, haga clic con el botón secundario en el nodo de proyecto **ContosoExpenses. Package** y seleccione **Editar archivo de proyecto**.

11. Busca el elemento `<Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />` en el archivo.

12. Reemplace este elemento por el siguiente código XML.

    ``` xml
    <ItemGroup>
        <SDKReference Include="Microsoft.VCLibs,Version=14.0">
        <TargetedSDKConfiguration Condition="'$(Configuration)'!='Debug'">Retail</TargetedSDKConfiguration>
        <TargetedSDKConfiguration Condition="'$(Configuration)'=='Debug'">Debug</TargetedSDKConfiguration>
        <TargetedSDKArchitecture>$(PlatformShortName)</TargetedSDKArchitecture>
        <Implicit>true</Implicit>
        </SDKReference>
    </ItemGroup>
    <Import Project="$(WapProjPath)\Microsoft.DesktopBridge.targets" />
    <Target Name="_StompSourceProjectForWapProject" BeforeTargets="_ConvertItems">
        <ItemGroup>
        <_TemporaryFilteredWapProjOutput Include="@(_FilteredNonWapProjProjectOutput)" />
        <_FilteredNonWapProjProjectOutput Remove="@(_TemporaryFilteredWapProjOutput)" />
        <_FilteredNonWapProjProjectOutput Include="@(_TemporaryFilteredWapProjOutput)">
            <SourceProject>
            </SourceProject>
        </_FilteredNonWapProjProjectOutput>
        </ItemGroup>
    </Target>
    ```

13. Guarde el archivo de proyecto y ciérrelo.

14. Presione **F5** para iniciar la aplicación empaquetada en el depurador.

En este momento, puede observar algunos cambios que indican que la aplicación se está ejecutando ahora como empaquetada:

- El icono de la barra de tareas o el menú Inicio es ahora el recurso predeterminado que se incluye en cada **proyecto de paquete de aplicación de Windows**.
- Si hace clic con el botón secundario en la aplicación **ContosoExpense. Package** que aparece en el menú Inicio, observará opciones que normalmente se reservan para las aplicaciones descargadas desde el Microsoft Store, como la configuración de la **aplicación**, la **tasa y la revisión** y el **uso compartido.** .

    ![ContosoExpenses en el menú Inicio](images/wpf-modernize-tutorial/StartMenu.png)

- Si desea desinstalar la aplicación, puede hacer clic con el botón secundario en **ContosoExpense. Package** en el menú Inicio yseleccionar desinstalar. La aplicación se quitará de inmediato, sin que haya ningún sobrante en el sistema.

## <a name="test-the-notification"></a>Prueba de la notificación

Ahora que ha empaquetado la aplicación de gastos de Contoso con MSIX, puede probar el escenario de notificación que no estaba funcionando al final de la [parte 4](modernize-wpf-tutorial-4.md).

1. En la aplicación de gastos de Contoso, elija un empleado de la lista y, a continuación, haga clic en el botón **Agregar nuevo gasto** . 
2. Complete todos los campos del formulario y presione **Guardar**.
3. Confirme que ve una notificación del sistema operativo.

![Notificación del sistema](images/wpf-modernize-tutorial/ToastNotification.png)
