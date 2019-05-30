---
Description: Enumera los patrones de control de Automatización de la interfaz de usuario de Microsoft, las clases que los clientes usan para acceder a ellos y las interfaces que los proveedores usan para implementarlos.
ms.assetid: 2091883C-5D0C-44ED-936A-709022926A42
title: Interfaces y patrones de control
label: Control patterns and interfaces
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fc8018a4ccf53c106a1e10410593e214d9dcead7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359564"
---
# <a name="control-patterns-and-interfaces"></a>Interfaces y patrones de control  



Enumera los patrones de control de Automatización de la interfaz de usuario de Microsoft, las clases que los clientes usan para acceder a ellos y las interfaces que los proveedores usan para implementarlos.

La tabla de este tema describe los modelos de patrón de control de Automatización de la interfaz de usuario de Microsoft. La tabla también enumera las clases usadas por los clientes de automatización de la interfaz de usuario para acceder a las interfaces y los modelos de patrón de control usados por los proveedores de automatización de la interfaz de usuario para implementarlos. La columna **Control pattern** muestra el nombre del patrón desde la perspectiva del cliente de automatización de la interfaz de usuario, como un valor constante enumerado en [**Control Pattern Availability Property Identifiers**](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-control-pattern-availability-propids). Desde la perspectiva del proveedor de automatización de la interfaz de usuario, cada uno de estos patrones es un nombre de constante [**PatternInterface**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface). La columna **Interfaz de proveedor de clase** muestra el nombre de la interfaz de Windows en tiempo de ejecución que los proveedores implementan para proporcionar este patrón para un control XAML personalizado.

Para más información acerca de cómo implementar sistemas de automatización del mismo nivel personalizados que expongan patrones de control e implemente las interfaces, consulta [Personalizar sistemas de automatización del mismo nivel](custom-automation-peers.md).

Al implementar un patrón de control, debes consultar también la documentación del proveedor de automatización de la interfaz de usuario que explica algunas de las expectativas que los clientes tendrán de un patrón de control, independientemente de qué marco de trabajo de interfaz de usuario se use para implementarlo. Alguna de la información incluida en la documentación del proveedor de automatización de la interfaz de usuario influirá en cómo implementes tus sistemas del mismo nivel y admitas correctamente ese patrón. Consulta el tema sobre cómo [Implementar patrones de control de automatización de la interfaz de usuario](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns) y visita la página que documenta el patrón que quieres implementar.

| Patrón de control | Interfaz de proveedor de clase | Descripción |
|-----------------|--------------------------|-------------|
| **Anotación** | [**IAnnotationProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IAnnotationProvider) | Se usa para exponer las propiedades de una anotación en un documento. |
| **Acoplar** | [**IDockProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IDockProvider) | Se usa para controles que pueden acoplarse en un contenedor de acoplamiento. Por ejemplo, barras de herramientas o paletas herramientas. |
| **Arrastre** | [**IDragProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IDragProvider) | Se usa para admitir controles arrastrables o controles con elementos arrastrables. |
| **DropTarget** | [**IDropTargetProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IDropTargetProvider) | Se usa para admitir controles que pueden ser el destino para una operación de arrastrar y colocar. |
| **ExpandCollapse** | [**IExpandCollapseProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IExpandCollapseProvider) | Se usa para admitir controles que se expanden visualmente para mostrar más contenido y se reducen para ocultar el contenido. |
| **Grid** | [**IGridProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridProvider) | Se usa para los controles que admiten funcionalidad de cuadrícula, como la variación de tamaño y el desplazamiento a una celda especificada. Ten en cuenta que la cuadrícula en sí no implementa este patrón, porque proporciona diseño pero no es un control |
| **GridItem** | [**IGridItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridItemProvider) | Se usa para controles que tienen celdas dentro de cuadrículas. |
| **Invoke** | [**IInvokeProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) | Se usa para los controles que se pueden invocar, como  [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button). |
| **ItemContainer** | [**IItemContainerProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IItemContainerProvider) | Permite que las aplicaciones encuentren un elemento en un contenedor, como una lista virtualizada. |
| **MultipleView** | [**IMultipleViewProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IMultipleViewProvider) | Se usa para los controles que pueden cambiar entre múltiples representaciones del mismo conjunto de información, datos o elementos secundarios. |
| **ObjectModel** | [**IObjectModelProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IObjectModelProvider) | Se usa para exponer un puntero al modelo de objetos subyacente de un documento. |
| **RangeValue** | [**IRangeValueProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) | Se usa para los controles que tienen un intervalo de valores que pueden aplicarse al control. Por ejemplo, un control de número que contiene años puede tener un intervalo desde 1900 al año actual, mientras que otro control de número que presenta meses tendría un intervalo de 1 a 12. |
| **Scroll** | [**IScrollProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollProvider) | Se usa para los controles que pueden desplazarse. Por ejemplo, un control que tiene barras de desplazamiento que están activas cuando existe más información que se puede mostrar en un área visualizable del control. |
| **ScrollItem** | [**IScrollItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollItemProvider) | Se usa para controles que tienen elementos individuales en una lista que se desplaza. Por ejemplo, un control de lista que tiene elementos individuales en la lista de desplazamiento, como un control de cuadro combinado. |
| **Selección** | [**ISelectionProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionProvider) | Se usa para controles de contenedor de selección. Por ejemplo, [**ListBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) y [**ComboBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox). |
| **SelectionItem** | [**ISelectionItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionItemProvider) | Se usa para elementos individuales en controles de contenedor de selección, como cuadros de lista y cuadros combinados. |
| **Spreadsheet** | [**ISpreadsheetProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetProvider) | Se usa para exponer el contenido de una hoja de cálculo u otro documento basado en cuadrícula. |
| **SpreadsheetItem** | [**ISpreadsheetItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetItemProvider) | Se usa para exponer las propiedades de una celda de una hoja de cálculo u otro documento basado en cuadrícula. |
| **Estilos** | [**IStylesProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IStylesProvider) | Se usa para describir un elemento de interfaz de usuario que tiene un estilo, color de relleno, patrón de relleno o forma específicos. |
| **SynchronizedInput** | [**ISynchronizedInputProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISynchronizedInputProvider) | Permite a las aplicaciones cliente de automatización de la interfaz de usuario dirigir la entrada de mouse o teclado a un elemento específico de la interfaz de usuario. |
| **Table** | [**ITableProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableProvider) | Se usa para controles que tienen una cuadrícula así como información de encabezado. Por ejemplo, un control de calendario tabular. |
| **TableItem** | [**ITableItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableItemProvider) | Se usa para elementos de una tabla. |
| **Texto** | [**ITextProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITextProvider) | Se usa para editar documentos y controles que exponen información textual. Consulta también [**ITextRangeProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider) y [**ITextProvider2**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextprovider2). |
| **TextChild** | [**ITextChildProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextchildprovider) | Se usa para acceder al antecesor más próximo de un elemento que admite el patrón de control **Text**. |
| **TextEdit** | No hay ninguna clase administrada disponible | Proporciona acceso a un control que modifica texto, por ejemplo, un control que realiza autocorrección o habilita la composición de entrada mediante un editor de métodos de entrada (IME). |
| **TextRange** | [**ITextRangeProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider) | Proporciona acceso a un intervalo de texto continuo en un contenedor de texto que implementa [**ITextProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextprovider). Consulta también [**ITextRangeProvider2**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider2). |
| **Alternancia** | [**IToggleProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IToggleProvider) | Se usa para los controles donde puede alternarse el estado. Por ejemplo, elementos de menú y [**CheckBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox) que pueden comprobarse. |
| **Transformación** | [**ITransformProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITransformProvider) | Se usa para los controles cuyo tamaño puede cambiarse, que pueden moverse y girarse. Los usos típicos del patrón de control de transformación son diseñadores, formularios, editores gráficos y aplicaciones de dibujo. |
| **Valor** | [**IValueProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IValueProvider) | Permite que los clientes obtengan o establezcan un valor en los controles que no admiten un intervalo de valores. |
| **VirtualizedItem** | [**IVirtualizedItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IVirtualizedItemProvider) | Expone elementos dentro de contenedores que están virtualizados y deben ponerse totalmente a disposición como elementos de automatización de la interfaz de usuario. |
| **Window** | [**IWindowProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IWindowProvider) | Expone información específica de windows, un concepto fundamental para el sistema operativo Microsoft Windows. Ejemplos de controles que son ventanas son los cuadros de diálogo y las ventanas secundarias. |

> [!NOTE]
> No encontrarás necesariamente implementaciones de todos estos patrones en los controles XAML existentes. Algunos de los patrones tienen interfaces únicamente para admitir la paridad con la definición de patrones del marco de trabajo de automatización de la interfaz de usuario y para admitir los escenarios de sistemas de automatización del mismo nivel que requerirán una implementación personalizada para admitir dicho patrón.

> [!NOTE]
> Las aplicaciones de la Tienda de Windows Phone no admiten todos los patrones de control de Automatización de la interfaz de usuario que figuran en la lista. **Annotation**, **Dock**, **Drag**, **DropTarget**, **ObjectModel** son algunos de los patrones no admitidos.

<span id="related_topics"/>

## <a name="related-topics"></a>Temas relacionados  
* [Automatización del mismo nivel personalizado](custom-automation-peers.md)
* [Accesibilidad](accessibility.md) 
