---
ms.assetid: dd2a1e01-c284-4d62-963e-f59f58dca61a
description: En este artículo se describe cómo importar contenido multimedia de un dispositivo, incluida la búsqueda de orígenes de medios disponibles, la importación de archivos como fotos y archivos sidecar, y la eliminación de los archivos importados del dispositivo de origen.
title: Importar contenido multimedia
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c08612e48eec7989f3b56fba41a17e1c149b2058
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8349244"
---
# <a name="import-media-from-a-device"></a>Importar contenido multimedia desde un dispositivo

En este artículo se describe cómo importar contenido multimedia de un dispositivo, incluida la búsqueda de orígenes multimedia disponibles, la importación de archivos como vídeos, fotos y archivos complementarios, y la eliminación de los archivos importados del dispositivo de origen.

> [!NOTE] 
> El código de este artículo es una adaptación de la [**muestra de la aplicación para UWP MediaImport**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport). Puedes clonar o descargar esta muestra desde el [**repositorio Git de muestras de aplicaciones universales de Windows**](https://github.com/Microsoft/Windows-universal-samples) para ver el código en contexto o usarlo como punto de partida para tu propia aplicación.

## <a name="create-a-simple-media-import-ui"></a>Crear una interfaz de usuario de importación de contenido multimedia sencilla
En el ejemplo de este artículo se usa una interfaz de usuario mínima para habilitar los escenarios de importación de contenido multimedia principales. Para ver cómo crear una interfaz de usuario más resistente para una aplicación de importación de contenido multimedia, consulta la [**muestra de MediaImport**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport). El siguiente archivo XAML crea un panel de pila con los controles siguientes:
* Un control [**Button**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Button) para iniciar la búsqueda de orígenes de los que se puede importar contenido multimedia.
* Un control [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ComboBox) para enumerar y seleccionar los orígenes de importación de contenido multimedia encontrados.
* Un control [**ListView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ListView) para visualizar y seleccionar los elementos multimedia del origen de importación seleccionado.
* Un control **Button** para iniciar la importación de elementos multimedia del origen seleccionado.
* Un control **Button** para iniciar la eliminación de los elementos que se han importado del origen seleccionado.
* Un control **Button** para cancelar una operación de importación de contenido multimedia asincrónica.

[!code-xml[ImportXAML](./code/PhotoImport_Win10/cs/MainPage.xaml#SnippetImportXAML)]

## <a name="set-up-your-code-behind-file"></a>Configurar el código subyacente del archivo
Agrega directivas *using* para incluir los espacios de nombres que se usan en este ejemplo y que no estén incluidos aún en la plantilla de proyecto predeterminada.

[!code-cs[Using](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="set-up-task-cancellation-for-media-import-operations"></a>Configurar la cancelación de tareas para las operaciones de importación de contenido multimedia

Dado que las operaciones de importación de contenido multimedia pueden tardar mucho tiempo, se realizan asincrónicamente mediante [**IAsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206594.aspx). Declara una variable de miembro de clase de tipo [**CancellationTokenSource**](https://msdn.microsoft.com/library/system.threading.cancellationtokensource) que se usará para cancelar una operación en curso si el usuario hace clic en el botón Cancelar.

[!code-cs[DeclareCts](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareCts)]

Implementa un controlador para el botón Cancelar. Los ejemplos que se muestran más adelante en este artículo inicializarán el objeto **CancellationTokenSource** cuando una operación comience y lo establecerán en null cuando esta se complete. En el controlador del botón Cancelar, comprueba si el token es nulo y, si no es así, llama a [**Cancel**](https://msdn.microsoft.com/library/dd321955) para cancelar la operación.

[!code-cs[OnCancel](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetOnCancel)]

## <a name="data-binding-helper-classes"></a>Clases auxiliares de enlace de datos

En un escenario de importación de contenido multimedia típico, se muestra al usuario una lista de elementos multimedia disponibles para importar. Puede haber un gran número de archivos multimedia para elegir y, por lo general, es recomendable mostrar una miniatura para cada elemento multimedia. Por este motivo, en este ejemplo se usan tres clases auxiliares para cargar las entradas de manera incremental en el control ListView a medida que el usuario se desplaza hacia abajo por la lista.

* Clase **IncrementalLoadingBase**: implementa [**IList**](https://msdn.microsoft.com/library/system.collections.ilist), [**ISupportIncrementalLoading**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.data.isupportincrementalloading) y [**INotifyCollectionChanged**](https://msdn.microsoft.com/library/windows/apps/system.collections.specialized.inotifycollectionchanged(v=vs.105).aspx) para proporcionar el comportamiento de carga incremental de base.
* Clase **GeneratorIncrementalLoadingClass**: proporciona una implementación de la clase base de carga incremental.
* Clase **ImportableItemWrapper**: contenedor fino en torno a una clase [**PhotoImportItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItem) para agregar una propiedad [**BitmapImage**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Media.Imaging.BitmapImage) enlazable para la imagen en miniatura de cada elemento importado.

Estas clases se proporcionan en la [**muestra de MediaImport**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MediaImport) y se pueden agregar al proyecto sin modificaciones. Después de agregar las clases auxiliares a tu proyecto, declara una variable de miembro de clase de tipo **GeneratorIncrementalLoadingClass**, que se usará más adelante en este ejemplo.

[!code-cs[GeneratorIncrementalLoadingClass](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetGeneratorIncrementalLoadingClass)]


## <a name="find-available-sources-from-which-media-can-be-imported"></a>Buscar orígenes disponibles de los que se pueda importar contenido multimedia

En el controlador de clic del botón de búsqueda de orígenes, llama al método estático [**PhotoImportManager.FindAllSourcesAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportManager.FindAllSourcesAsync) para iniciar el sistema y buscar dispositivos de los que se pueda importar contenido multimedia. Después de esperar a que finalice la operación, repite el procedimiento con cada objeto [**PhotoImportSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSource) de la lista devuelta; agrega una entrada al control **ComboBox** y establece la propiedad **Tag** en el propio objeto de origen para que se pueda recuperar fácilmente cuando el usuario realice una selección.

[!code-cs[FindSourcesClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetFindSourcesClick)]

Declara una variable de miembro de clase para almacenar el origen de importación seleccionado del usuario.

[!code-cs[DeclareImportSource](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImportSource)]

En el controlador [**SelectionChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.Selector.SelectionChanged) del origen de importación **ComboBox**, establece la variable de miembro de clase en el origen seleccionado y, después, llama al método auxiliar **FindItems**, que se mostrará más adelante en este artículo. 

[!code-cs[SourcesSelectionChanged](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetSourcesSelectionChanged)]

## <a name="find-items-to-import"></a>Buscar elementos para importar

Agrega variables de miembro de clase de tipo [**PhotoImportSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSession) y [**PhotoImportFindItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult), que se usarán en los siguientes pasos.

[!code-cs[DeclareImport](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImport)]

En el método **FindItems**, inicializa la variable **CancellationTokenSource** para que pueda usarse para cancelar la operación de búsqueda si es necesario. En un bloque **try**, llama a [**CreateImportSession**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSource.CreateImportSession) en el objeto [**PhotoImportSource**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportSource) seleccionado por el usuario para crear una nueva sesión de importación. Crea un nuevo objeto [**Progress**](https://msdn.microsoft.com/library/hh193692.aspx) para proporcionar una devolución de llamada y mostrar el progreso de la operación de búsqueda. Luego, llama a **[FindItemsAsync](https://docs.microsoft.com/uwp/api/windows.media.import.photoimportsession.finditemsasync)** para iniciar la operación de búsqueda. Proporciona un valor [**PhotoImportContentTypeFilter**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportContentTypeFilter) para especificar si se deben devolver fotos, vídeos o ambos. Proporciona un valor [**PhotoImportItemSelectionMode**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItemSelectionMode) para especificar si deben devolverse todos los elementos multimedia, ninguno o solo los nuevos con su propiedad [**IsSelected**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItem.IsSelected) establecida en true. Esta propiedad está enlazada a una casilla para cada elemento multimedia de nuestra plantilla de elementos ListBox.

**FindItemsAsync** devuelve una interfaz [**IAsyncOperationWithProgress**](https://msdn.microsoft.com/library/windows/apps/br206594.aspx). El método de extensión [**AsTask**](https://msdn.microsoft.com/library/hh779750.aspx) se usa para crear una tarea que se pueda esperar, que se pueda cancelar con el token de cancelación y que notifique el progreso mediante el objeto **Progress** proporcionado.

A continuación, se inicializa la clase auxiliar de enlace de datos **GeneratorIncrementalLoadingClass**. **FindItemsAsync**, cuando regresa tras su espera, devuelve un objeto [**PhotoImportFindItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult). Este objeto contiene información de estado sobre la operación de búsqueda, que incluye el éxito de la operación y el número de tipos de elementos multimedia diferentes que se han encontrado. La propiedad [**FoundItems**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.FoundItems) contiene una lista de objetos [**PhotoImportItem**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportItem) que representan los elementos multimedia encontrados. El constructor **GeneratorIncrementalLoadingClass** toma como argumentos el recuento total de elementos que se cargarán de forma incremental y una función que genera nuevos elementos para cargar según sea necesario. En este caso, la expresión lambda proporcionada crea una nueva instancia de **ImportableItemWrapper**, que encapsula la clase **PhotoImportItem** e incluye una miniatura de cada elemento. Una vez inicializada la clase de carga incremental, establécela en la propiedad [**ItemsSource**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ItemsControl.ItemsSource) del control **ListView** en la interfaz de usuario. Ahora, los elementos multimedia encontrados se cargarán de forma incremental y se mostrarán en la lista.

A continuación, se muestra la información de estado de la operación de búsqueda. Una aplicación típica mostrará esta información al usuario en la interfaz de usuario, pero, en este ejemplo, la información se muestra simplemente en la consola de depuración. Para terminar, establece el token de cancelación en null porque la operación se ha completado.

[!code-cs[FindItems](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetFindItems)]

## <a name="import-media-items"></a>Importar elementos multimedia

Antes de implementar la operación de importación, declara un objeto [**PhotoImportImportItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult) para almacenar los resultados de la operación de importación. Este se usará más adelante para eliminar los elementos multimedia que se han importado correctamente del origen.

[!code-cs[DeclareImportResult](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeclareImportResult)]

Antes de iniciar la operación de importación de contenido multimedia, inicializa la variable **CancellationTokenSource** y establece el valor del control [**ProgressBar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.ProgressBar) en 0.

Si no hay ningún elemento seleccionado en el control **ListView**, no hay nada para importar. De lo contrario, inicializa un objeto [**Progress**](https://msdn.microsoft.com/library/hh193692.aspx) para proporcionar una devolución de llamada de progreso que actualice el valor del control de barra de progreso. Registra un controlador para el evento [**ItemImported**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ItemImported) de la clase [**PhotoImportFindItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult) devuelto por la operación de búsqueda. Este evento se generará cuando se importe un elemento y, en este ejemplo, muestra el nombre de cada archivo importado en la consola de depuración.

Llama a [**ImportItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ImportItemsAsync) para iniciar la operación de importación. Como sucede con la operación de búsqueda, el método de extensión [**AsTask**](https://msdn.microsoft.com/library/hh779750.aspx) se usa para convertir la operación devuelta en una tarea que se pueda esperar, que notifique el progreso y que se pueda cancelar.

Una vez completada la operación de importación, se puede obtener el estado de la operación del objeto [**PhotoImportImportItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult) devuelto por [**ImportItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ImportItemsAsync). En este ejemplo se muestra la información de estado en la consola de depuración y, después, se establece el token de cancelación en null.

[!code-cs[ImportClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetImportClick)]

## <a name="delete-imported-items"></a>Eliminar elementos importados
Para eliminar los elementos importados correctamente desde el origen del que se importaron, inicializa primero el token de cancelación para que la operación de eliminación pueda cancelarse y establece el valor de la barra de progreso en 0. Asegúrate de que la clase [**PhotoImportImportItemsResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult) que ha devuelto [**ImportItemsAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportFindItemsResult.ImportItemsAsync) no sea nula. De lo contrario, vuelve a crear un objeto [**Progress**](https://msdn.microsoft.com/library/hh193692.aspx) a fin de proporcionar una devolución de llamada de progreso para la operación de eliminación. Llama a [**DeleteImportedItemsFromSourceAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportImportItemsResult.DeleteImportedItemsFromSourceAsync) para comenzar a eliminar los elementos importados. Usa **AsTask** para convertir el resultado en una tarea que admita await con funcionalidades de progreso y cancelación. Después de esperar, el objeto [**PhotoImportDeleteImportedItemsFromSourceResult**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Import.PhotoImportDeleteImportedItemsFromSourceResult) devuelto puede usarse para obtener y mostrar información de estado sobre la operación de eliminación.

[!code-cs[DeleteClick](./code/PhotoImport_Win10/cs/MainPage.xaml.cs#SnippetDeleteClick)]








 


