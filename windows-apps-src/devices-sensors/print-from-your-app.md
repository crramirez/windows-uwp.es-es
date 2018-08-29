---
author: PatrickFarley
ms.assetid: 9A0F1852-A76B-4F43-ACFC-2CC56AAD1C03
title: Imprimir desde tu aplicación
description: Aprende a imprimir documentos desde aplicaciones universales de Windows. En este tema también se muestra cómo imprimir páginas específicas.
ms.author: pafarley
ms.date: 01/29/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, impresión
ms.localizationpriority: medium
ms.openlocfilehash: cff96c0b8daf9f3ef32815437b510a5b94641527
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "2913553"
---
# <a name="print-from-your-app"></a>Imprimir desde tu aplicación



**API importantes**

-   [**Windows.Graphics.Printing**](https://msdn.microsoft.com/library/windows/apps/BR226489)
-   [**Windows.UI.Xaml.Printing**](https://msdn.microsoft.com/library/windows/apps/BR243325)
-   [**PrintDocument**](https://msdn.microsoft.com/library/windows/apps/BR243314)

Aprende a imprimir documentos desde aplicaciones universales de Windows. En este tema también se muestra cómo imprimir páginas específicas. Para cambios más avanzados de la interfaz de usuario de vista previa de impresión, consulta [Personalizar la interfaz de usuario de vista previa de impresión](customize-the-print-preview-ui.md).

> [!TIP]
> La mayoría de los ejemplos de este tema se basan en la muestra de impresión. Para ver el código completo, descarga la [muestra de impresión de la Plataforma universal de Windows (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619984) desde [Windows-universal-samples repo (Repositorio de muestras universales de Windows)](http://go.microsoft.com/fwlink/p/?LinkId=619979) en GitHub.

## <a name="register-for-printing"></a>Registro para la impresión

El primer paso para agregar impresión a tu aplicación es registrarte en el contrato de Imprimir. Tu aplicación debe realizar esta acción en cada pantalla desde la que desees que el usuario pueda imprimir. Solo puedes registrar para impresión la pantalla que se muestra al usuario. Si una pantalla de la aplicación se registró para realizar la impresión, debes anular su registro cuando termine la operación de impresión. Si se reemplaza con otra pantalla, la siguiente pantalla debe registrarse para un nuevo contrato de Imprimir al abrirse.

> [!TIP]
> Si necesitas soporte para imprimir desde más de una página en tu aplicación, puedes colocar este código de impresión en una clase auxiliar común y hacer que las páginas de la aplicación vuelvan a usarla. Para ver un ejemplo de cómo hacerlo, consulta la clase `PrintHelper` de la [muestra de impresión de UWP](http://go.microsoft.com/fwlink/p/?LinkId=619984).

Primero, debes declarar [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426) y [**PrintDocument**](https://msdn.microsoft.com/library/windows/apps/BR243314). El tipo **PrintManager** se encuentra en el espacio de nombres [**Windows.Graphics.Printing**](https://msdn.microsoft.com/library/windows/apps/BR226489) junto con los tipos que admiten otras funciones de impresión de Windows. El tipo **PrintDocument** se encuentra en el espacio de nombres [**Windows.UI.Xaml.Printing**](https://msdn.microsoft.com/library/windows/apps/BR243325) junto con otros tipos que admiten la preparación de contenido XAML para su impresión. Puedes simplificar la escritura de código de impresión agregando las instrucciones **using** o **Imports** a la página.

```csharp
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
```

La clase [**PrintDocument**](https://msdn.microsoft.com/library/windows/apps/BR243314) se usa para controlar gran parte de la interacción entre la aplicación y la clase [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426), pero expone varias devoluciones de llamada propias. Durante el registro, debes crear instancias de **PrintManager** y **PrintDocument**, y se registrar los controladores de los eventos de impresión.

En la [muestra de impresión para UWP](http://go.microsoft.com/fwlink/p/?LinkId=619984), el método `RegisterForPrinting` es el que realiza el registro.

```csharp
public virtual void RegisterForPrinting()
{
   printDocument = new PrintDocument();
   printDocumentSource = printDocument.DocumentSource;
   printDocument.Paginate += CreatePrintPreviewPages;
   printDocument.GetPreviewPage += GetPrintPreviewPage;
   printDocument.AddPages += AddPrintPages;

   PrintManager printMan = PrintManager.GetForCurrentView();
   printMan.PrintTaskRequested += PrintTaskRequested;
}
```

Cuando el usuario va a una página compatible con la impresión se inicia el registro dentro del método `OnNavigatedTo`.

```csharp
protected override void OnNavigatedTo(NavigationEventArgs e)
{
   // Initialize common helper class and register for printing
   printHelper = new PrintHelper(this);
   printHelper.RegisterForPrinting();

   // Initialize print content for this scenario
   printHelper.PreparePrintContent(new PageToPrint());

   // Tell the user how to print
   MainPage.Current.NotifyUser("Print contract registered with customization, use the Print button to print.", NotifyType.StatusMessage);
}
```

Cuando el usuario abandona la página, se desconectan los controladores de eventos de impresión. Si tienes una aplicación de múltiples páginas y no desconectas la impresión, se generará una excepción cuando el usuario abandone la página y regrese a ella.

```csharp
protected override void OnNavigatedFrom(NavigationEventArgs e)
{
   if (printHelper != null)
   {
         printHelper.UnregisterForPrinting();
   }
}
```
## <a name="create-a-print-button"></a>Crear un botón de impresión

Agrega un botón de impresión a la pantalla de tu aplicación en el lugar deseado. Asegúrate de que no interfiera con el contenido que deseas imprimir.

```xml
<Button x:Name="InvokePrintingButton" Content="Print" Click="OnPrintButtonClick"/>
```

A continuación, agrega un controlador de eventos al código de la aplicación para controlar el evento "Click". Usa el método [**ShowPrintUIAsync**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing.printmanager.showprintuiasync) para iniciar la impresión desde la aplicación. **ShowPrintUIAsync** es un método asincrónico que muestra la ventana de impresión apropiada. Te recomendamos llamar al método [**IsSupported**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Printing.PrintManager.IsSupported) primero con el fin de comprobar que la aplicación se ejecuta en un dispositivo que admite impresión (y controlar en caso de que no la admita). Si no se puede realizar la impresión en ese momento por cualquier otro motivo, **ShowPrintUIAsync** iniciará una excepción. Te recomendamos capturar estas excepciones y notificar al usuario cuando no se pueda continuar con la impresión.

```csharp
async private void OnPrintButtonClick(object sender, RoutedEventArgs e)
{
    if (Windows.Graphics.Printing.PrintManager.IsSupported())
    {
        try
        {
            // Show print UI
            await Windows.Graphics.Printing.PrintManager.ShowPrintUIAsync();

        }
        catch
        {
            // Printing cannot proceed at this time
            ContentDialog noPrintingDialog = new ContentDialog()
            {
                Title = "Printing error",
                Content = "\nSorry, printing can' t proceed at this time.", PrimaryButtonText = "OK"
            };
            await noPrintingDialog.ShowAsync();
        }
    }
    else
    {
        // Printing is not supported on this device
        ContentDialog noPrintingDialog = new ContentDialog()
        {
            Title = "Printing not supported",
            Content = "\nSorry, printing is not supported on this device.",PrimaryButtonText = "OK"
        };
        await noPrintingDialog.ShowAsync();
    }
}
```

En este ejemplo, se muestra una ventana de impresión en el controlador de eventos para un clic de botón. Si el método genera una excepción (porque no se puede realizar la impresión en ese momento), el control [**ContentDialog**](https://msdn.microsoft.com/library/windows/apps/Dn633972) informa al usuario de la situación.

## <a name="format-your-apps-content"></a>Aplicar formato al contenido de la aplicación

Cuando llamas a **ShowPrintUIAsync**, se genera el evento [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597). El controlador de eventos **PrintTaskRequested** que se muestra en este paso, crea una clase [**PrintTask**](https://msdn.microsoft.com/library/windows/apps/BR226436) llamando al método [**PrintTaskRequest.CreatePrintTask**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing.printtaskrequest.createprinttask.aspx); a continuación, pasa el título de la página de impresión y el nombre de un delegado [**PrintTaskSourceRequestedHandler**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing.printtask.source). Ten en cuenta que en este ejemplo, el delegado **PrintTaskSourceRequestedHandler** se define en línea. Igualmente, el delegado **PrintTaskSourceRequestedHandler** te proporciona el contenido formateado para imprimir; más adelante lo describiremos con más detalle.

En este ejemplo, se define también un controlador de finalización para capturar errores. Es recomendable controlar los eventos de finalización, ya que luego tu aplicación puede informar al usuario si ocurrió un error y proporcionar posibles soluciones. De la misma manera, la aplicación puede usar el evento de finalización para indicar los pasos que el usuario debe seguir después de que el trabajo de impresión se haya completado correctamente.

```csharp
protected virtual void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequested =>
   {
         // Print Task event handler is invoked when the print job is completed.
         printTask.Completed += async (s, args) =>
         {
            // Notify the user when the print operation fails.
            if (args.Completion == PrintTaskCompletion.Failed)
            {
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     MainPage.Current.NotifyUser("Failed to print.", NotifyType.ErrorMessage);
               });
            }
         };

         sourceRequested.SetSource(printDocumentSource);
   });
}
```

Una vez creada la tarea de impresión, el evento [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426) solicita una colección de páginas de impresión para mostrarlas en la interfaz de usuario de la vista previa de impresión; para ello, genera el evento [**Paginate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.paginate). Esto corresponde al método **Paginate** de la interfaz **IPrintPreviewPageCollection**. En ese momento, se llamará al controlador de eventos que creaste durante el registro.

> [!IMPORTANT]
> Si el usuario cambia la configuración de impresión, se llamará nuevamente al controlador de eventos de paginación para permitir la redistribución del contenido. Para ofrecer la mejor experiencia de usuario, te recomendamos que compruebes la configuración antes de redistribuir el contenido, para evitar reiniciar el contenido paginado cuando no sea necesario.

En el controlador de eventos [**Paginate**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.paginate) (el método `CreatePrintPreviewPages` de la [muestra de impresión para UWP](http://go.microsoft.com/fwlink/p/?LinkId=619984)), crea las páginas que se muestran en la interfaz de usuario de la vista previa de impresión y que se envían a la impresora. El código que usas para preparar el contenido de tu aplicación para impresión es específico de la aplicación y del contenido que imprimes. Consulta la [muestra de impresión para UWP](http://go.microsoft.com/fwlink/p/?LinkId=619984) para ver cómo se aplica el formato al contenido de impresión.

```csharp
protected virtual void CreatePrintPreviewPages(object sender, PaginateEventArgs e)
{
   // Clear the cache of preview pages
   printPreviewPages.Clear();

   // Clear the print canvas of preview pages
   PrintCanvas.Children.Clear();

   // This variable keeps track of the last RichTextBlockOverflow element that was added to a page which will be printed
   RichTextBlockOverflow lastRTBOOnPage;

   // Get the PrintTaskOptions
   PrintTaskOptions printingOptions = ((PrintTaskOptions)e.PrintTaskOptions);

   // Get the page description to deterimine how big the page is
   PrintPageDescription pageDescription = printingOptions.GetPageDescription(0);

   // We know there is at least one page to be printed. passing null as the first parameter to
   // AddOnePrintPreviewPage tells the function to add the first page.
   lastRTBOOnPage = AddOnePrintPreviewPage(null, pageDescription);

   // We know there are more pages to be added as long as the last RichTextBoxOverflow added to a print preview
   // page has extra content
   while (lastRTBOOnPage.HasOverflowContent && lastRTBOOnPage.Visibility == Windows.UI.Xaml.Visibility.Visible)
   {
         lastRTBOOnPage = AddOnePrintPreviewPage(lastRTBOOnPage, pageDescription);
   }

   if (PreviewPagesCreated != null)
   {
         PreviewPagesCreated.Invoke(printPreviewPages, null);
   }

   PrintDocument printDoc = (PrintDocument)sender;

   // Report the number of preview pages created
   printDoc.SetPreviewPageCount(printPreviewPages.Count, PreviewPageCountType.Intermediate);
}
```

Cuando una página determinada se va a mostrar en la ventana de vista previa de impresión, [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426) genera el evento [**GetPreviewPage**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.getpreviewpage). Esto corresponde al método **MakePage** de la interfaz **IPrintPreviewPageCollection**. En ese momento, se llama al controlador de eventos que creaste durante el registro.

En el controlador de eventos [**GetPreviewPage**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.getpreviewpage) (el método `GetPrintPreviewPage` de la [muestra de impresión para UWP](http://go.microsoft.com/fwlink/p/?LinkId=619984)), establece la página correspondiente en el documento de impresión.

```csharp
protected virtual void GetPrintPreviewPage(object sender, GetPreviewPageEventArgs e)
{
   PrintDocument printDoc = (PrintDocument)sender;
   printDoc.SetPreviewPage(e.PageNumber, printPreviewPages[e.PageNumber - 1]);
}
```

Por último, una vez que el usuario hace clic en el botón de impresión, la clase [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426) solicita la colección final de páginas que se enviará a la impresora mediante una llamada al método **MakeDocument** de la interfaz **IDocumentPageSource**. En XAML, esto genera el evento [**AddPages**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.addpages). En ese momento, se llama al controlador de eventos que creaste durante el registro.

En el controlador de eventos [**AddPages**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.printing.printdocument.addpages) (el método `AddPrintPages` de la [muestra de impresión para UWP](http://go.microsoft.com/fwlink/p/?LinkId=619984)), agrega páginas de la colección de páginas al objeto [**PrintDocument**](https://msdn.microsoft.com/library/windows/apps/BR243314) que se va a enviar a la impresora. Si un usuario especifica páginas concretas o un intervalo de páginas para imprimir, dicha información se usa aquí para agregar solamente las páginas que se enviarán a la impresora.

```csharp
protected virtual void AddPrintPages(object sender, AddPagesEventArgs e)
{
   // Loop over all of the preview pages and add each one to  add each page to be printied
   for (int i = 0; i < printPreviewPages.Count; i++)
   {
         // We should have all pages ready at this point...
         printDocument.AddPage(printPreviewPages[i]);
   }

   PrintDocument printDoc = (PrintDocument)sender;

   // Indicate that all of the print pages have been provided
   printDoc.AddPagesComplete();
}
```

## <a name="prepare-print-options"></a>Preparar las opciones de impresión

A continuación, hay que preparar las opciones de impresión. Por ejemplo, esta sección describe cómo establecer la opción de intervalo de página para permitir la impresión de páginas específicas. Para opciones más avanzadas, consulta [Personalizar la interfaz de usuario de vista previa de impresión](customize-the-print-preview-ui.md).

En este paso se crea una nueva opción de impresión, se define una lista de los valores compatibles con la opción y después se agrega la opción a la interfaz de usuario de vista previa de impresión. La opción de intervalo de páginas tiene tres opciones de configuración:

| Nombre de la opción          | Acción |
|----------------------|--------|
| **Imprimir todo**        | Imprimir todas las páginas del documento.|
| **Imprimir selección**  | Imprimir solo el contenido seleccionado por el usuario.|
| **Intervalo de impresión**      | Mostrar un control de edición en que el usuario pueda especificar las páginas que va a imprimir.|

En primer lugar, modifica el controlador de eventos [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597) para agregar el código que te permitirá obtener un objeto [**PrintTaskOptionDetails**](https://msdn.microsoft.com/library/windows/apps/Hh701256).

```csharp
PrintTaskOptionDetails printDetailedOptions = PrintTaskOptionDetails.GetFromPrintTaskOptions(printTask.Options);
```

A continuación, borra la lista de opciones que se muestran en la interfaz de usuario de la vista previa de impresión y agrega las opciones que aparecerán cuando el usuario quiera imprimir desde la aplicación.

> [!NOTE]
> Las opciones aparecen en la interfaz de usuario de vista previa de impresión en el mismo orden en que se anexaron, con la primera opción en la parte superior de la ventana.

```csharp
IList<string> displayedOptions = printDetailedOptions.DisplayedOptions;

displayedOptions.Clear();
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.ColorMode);
```

Crea la nueva opción de impresión e inicializa la lista de valores de opción.

```csharp
// Create a new list option
PrintCustomItemListOptionDetails pageFormat = printDetailedOptions.CreateItemListOption("PageRange", "Page Range");
pageFormat.AddItem("PrintAll", "Print all");
pageFormat.AddItem("PrintSelection", "Print Selection");
pageFormat.AddItem("PrintRange", "Print Range");
```

Agrega la opción de impresión personalizada y asigna el controlador de eventos. La opción personalizada se anexa al final para que aparezca en la parte inferior de la lista de opciones. No obstante, puedes colocarla en cualquier parte de la lista; no es necesario agregar las opciones de impresión personalizadas al final.

```csharp
// Add the custom option to the option list
displayedOptions.Add("PageRange");

// Create new edit option
PrintCustomTextOptionDetails pageRangeEdit = printDetailedOptions.CreateTextOption("PageRangeEdit", "Range");

// Register the handler for the option change event
printDetailedOptions.OptionChanged += printDetailedOptions_OptionChanged;
```

El método [**CreateTextOption**](https://msdn.microsoft.com/library/windows/apps/windows.graphics.printing.optiondetails.printtaskoptiondetails.createtextoption) crea el cuadro de texto **Intervalo**. En ese cuadro, el usuario indica las páginas específicas que quiere imprimir cuando selecciona la opción **Intervalo de impresión**.

## <a name="handle-print-option-changes"></a>Controlar los cambios de opciones de impresión

El controlador de eventos **OptionChanged** realiza dos tareas. En primer lugar, muestra y oculta el campo de edición de texto del intervalo de páginas en función de la opción de intervalo de páginas que seleccione el usuario. En segundo lugar, prueba el texto especificado en el cuadro de texto del intervalo de páginas para asegurarse de que el intervalo de páginas del documento sea válido.

En este ejemplo se muestra la manera en que la [muestra de impresión para UWP](http://go.microsoft.com/fwlink/p/?LinkId=619984) controla los eventos de cambio.

```csharp
async void printDetailedOptions_OptionChanged(PrintTaskOptionDetails sender, PrintTaskOptionChangedEventArgs args)
{
   if (args.OptionId == null)
   {
         return;
   }

   string optionId = args.OptionId.ToString();

   // Handle change in Page Range Option
   if (optionId == "PageRange")
   {
         IPrintOptionDetails pageRange = sender.Options[optionId];
         string pageRangeValue = pageRange.Value.ToString();

         selectionMode = false;

         switch (pageRangeValue)
         {
            case "PrintRange":
               // Add PageRangeEdit custom option to the option list
               sender.DisplayedOptions.Add("PageRangeEdit");
               pageRangeEditVisible = true;
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     ShowContent(null);
               });
               break;
            case "PrintSelection":
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     Scenario4PageRange page = (Scenario4PageRange)scenarioPage;
                     PageToPrint pageContent = (PageToPrint)page.PrintFrame.Content;
                     ShowContent(pageContent.TextContentBlock.SelectedText);
               });
               RemovePageRangeEdit(sender);
               break;
            default:
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     ShowContent(null);
               });
               RemovePageRangeEdit(sender);
               break;
         }

         Refresh();
   }
   else if (optionId == "PageRangeEdit")
   {
         IPrintOptionDetails pageRange = sender.Options[optionId];
         // Expected range format (p1,p2...)*, (p3-p9)* ...
         if (!Regex.IsMatch(pageRange.Value.ToString(), @"^\s*\d+\s*(\-\s*\d+\s*)?(\,\s*\d+\s*(\-\s*\d+\s*)?)*$"))
         {
            pageRange.ErrorText = "Invalid Page Range (eg: 1-3, 5)";
         }
         else
         {
            pageRange.ErrorText = string.Empty;
            try
            {
               GetPagesInRange(pageRange.Value.ToString());
               Refresh();
            }
            catch (InvalidPageException ipex)
            {
               pageRange.ErrorText = ipex.Message;
            }
         }
   }
}
```

> [!TIP]
> Consulta el método `GetPagesInRange` en la [Muestra de impresión para UWP](http://go.microsoft.com/fwlink/p/?LinkId=619984), a fin de obtener detalles sobre cómo analizar el intervalo de páginas que especifica el usuario en el cuadro de texto Intervalo.

## <a name="preview-selected-pages"></a>Obtener una vista previa de las páginas seleccionadas

El modo en que se aplica formato al contenido de tu aplicación para impresión depende de la naturaleza de la aplicación y su contenido. La [muestra de impresión para UWP](http://go.microsoft.com/fwlink/p/?LinkId=619984) usa la clase auxiliar de impresión para aplicar formato al contenido que se va a imprimir.

Al imprimir un subconjunto de las páginas, existen varias formas de mostrar el contenido en la vista previa de impresión. Independientemente del método que elijas para mostrar el intervalo de páginas de la vista previa de impresión, el resultado impreso debe contener solo las páginas seleccionadas.

-   Mostrar todas las páginas de la vista previa de impresión ya se especifique o no un intervalo de páginas, permitiendo que el usuario sepa qué páginas se van a imprimir realmente.
-   Mostrar solo las páginas seleccionadas por el intervalo de páginas del usuario en la vista previa de impresión, actualizando la pantalla cuando el usuario cambie el intervalo de páginas.
-   Mostrar todas las páginas de la vista previa de impresión, pero desactivar las páginas que no estén en el intervalo de páginas seleccionado por el usuario.

## <a name="related-topics"></a>Temas relacionados

* [Directrices de diseño de impresión](https://msdn.microsoft.com/library/windows/apps/Hh868178)
* [//Vídeo de compilación 2015: Developing apps that print in Windows 10 (Desarrollar aplicaciones que imprimen en Windows 10)](https://channel9.msdn.com/Events/Build/2015/2-94)
* [Muestra de impresión para UWP](http://go.microsoft.com/fwlink/p/?LinkId=619984)
