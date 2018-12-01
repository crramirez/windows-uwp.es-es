---
ms.assetid: 88132B6F-FB50-4B03-BC21-233988746230
title: Personalizar la interfaz de usuario de vista previa de impresión
description: En esta sección se describe cómo personalizar las opciones de impresión y la configuración de la interfaz de usuario de vista previa de impresión.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, impresión
ms.localizationpriority: medium
ms.openlocfilehash: 16fca2fc98450375ec2dcf7a0d10775b2bba1bf2
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8351077"
---
# <a name="customize-the-print-preview-ui"></a>Personalizar la interfaz de usuario de vista previa de impresión



**API importantes**

-   [**Windows.Graphics.Printing**](https://msdn.microsoft.com/library/windows/apps/BR226489)
-   [**Windows.UI.Xaml.Printing**](https://msdn.microsoft.com/library/windows/apps/BR243325)
-   [**PrintManager**](https://msdn.microsoft.com/library/windows/apps/BR226426)

En esta sección se describe cómo personalizar las opciones de impresión y la configuración de la interfaz de usuario de vista previa de impresión. Para obtener más información sobre la impresión, consulta [Imprimir desde tu aplicación](print-from-your-app.md).

**Sugerencia**la mayoría de los ejemplos de este tema se basa en la muestra de impresión. Para ver el código completo, descarga la [muestra de impresión de la Plataforma universal de Windows (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619984) desde el [Repositorio de muestras universales de Windows](http://go.microsoft.com/fwlink/p/?LinkId=619979) en GitHub.

 

## <a name="customize-print-options"></a>Personalizar las opciones de impresión

De forma predeterminada, la interfaz de usuario de vista previa de impresión muestra las opciones de impresión [**ColorMode**](https://msdn.microsoft.com/library/windows/apps/BR226478), [**Copies**](https://msdn.microsoft.com/library/windows/apps/BR226479) y [**Orientation**](https://msdn.microsoft.com/library/windows/apps/BR226486). Además de estas opciones, existen varias opciones de impresora comunes que podrás agregar a la interfaz de usuario de vista previa de impresión:

-   [**Enlace**](https://msdn.microsoft.com/library/windows/apps/BR226476)
-   [**Collation**](https://msdn.microsoft.com/library/windows/apps/BR226477)
-   [**Duplex**](https://msdn.microsoft.com/library/windows/apps/BR226480)
-   [**HolePunch**](https://msdn.microsoft.com/library/windows/apps/BR226481)
-   [**InputBin**](https://msdn.microsoft.com/library/windows/apps/BR226482)
-   [**MediaSize**](https://msdn.microsoft.com/library/windows/apps/BR226483)
-   [**MediaType**](https://msdn.microsoft.com/library/windows/apps/BR226484)
-   [**NUp**](https://msdn.microsoft.com/library/windows/apps/BR226485)
-   [**PrintQuality**](https://msdn.microsoft.com/library/windows/apps/BR226487)
-   [**Staple**](https://msdn.microsoft.com/library/windows/apps/BR226488)

Estas opciones se definen en la clase [**StandardPrintTaskOptions**](https://msdn.microsoft.com/library/windows/apps/BR226475). Puedes agregar o quitar opciones de la lista que se muestra en la interfaz de usuario de vista previa de impresión. También puedes cambiar el orden en que aparecen las opciones y establecer la configuración predeterminada que se muestra al usuario.

Pero los cambios que realices mediante este método afectan solo a la interfaz de usuario de vista previa de impresión. Los usuarios pueden acceder siempre a todas las opciones compatibles con la impresora presionando **Más configuraciones** en la interfaz de usuario de vista previa de impresión.

**Nota**aunque la aplicación puede especificar las opciones de impresión que se muestre, solo aquellos que son compatibles con la impresora seleccionada se muestran en la interfaz de usuario de vista previa de impresión. La interfaz de impresión no mostrará opciones que no sean compatibles con la impresora seleccionada.

 

### <a name="define-the-options-to-display"></a>Definir las opciones que se van a mostrar

Una vez que se carga la pantalla de la aplicación, se registra para el contrato de Imprimir. El registro incluye la definición del controlador de eventos [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597). El código para personalizar las opciones que aparecen en la interfaz de usuario de vista previa de impresión se agrega al controlador de eventos **PrintTaskRequested**.

Modifica el controlador de eventos [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597) para incluir las instrucciones [**printTask.options**](https://msdn.microsoft.com/library/windows/apps/BR226469) que configuran las opciones de impresión que deseas mostrar en la interfaz de usuario de vista previa de impresión. En la pantalla de la aplicación para la que quieres mostrar una lista de opciones de impresión personalizadas, anula el controlador de eventos **PrintTaskRequested** de la clase auxiliar para incluir código que especifique las opciones que se mostrarán cuando se imprima la pantalla.

``` csharp
protected override void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequestedArgs =>
   {
         IList<string> displayedOptions = printTask.Options.DisplayedOptions;

         // Choose the printer options to be shown.
         // The order in which the options are appended determines the order in which they appear in the UI
         displayedOptions.Clear();
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.MediaSize);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Collation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Duplex);

         // Preset the default value of the printer option
         printTask.Options.MediaSize = PrintMediaSize.NorthAmericaLegal;

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

         sourceRequestedArgs.SetSource(printDocumentSource);
   });
}
```

**Importante**llamar a () [**displayedOptions.clear**](https://msdn.microsoft.com/library/windows/apps/BR226453)quita todas las opciones de impresión de la interfaz de usuario, incluido el vínculo de **opciones más** de vista previa de impresión. Asegúrate de anexar las opciones que deseas mostrar en la interfaz de usuario de vista previa de impresión.

### <a name="specify-default-options"></a>Especificar las opciones predeterminadas

También puedes establecer los valores predeterminados de las opciones de la interfaz de usuario de vista previa de impresión. La siguiente línea de código, tomada del ejemplo anterior, establece el valor predeterminado de la opción [**MediaSize**](https://msdn.microsoft.com/library/windows/apps/BR226483).

``` csharp
         // Preset the default value of the printer option
         printTask.Options.MediaSize = PrintMediaSize.NorthAmericaLegal;
```         

## <a name="add-new-print-options"></a>Agregar nuevas opciones de impresión

En esta sección se muestra cómo crear una nueva opción de impresión, cómo definir una lista de valores compatibles con la opción y, después, cómo agregar la opción a la interfaz de usuario de vista previa de impresión. Tal como se ha mostrado en la sección anterior, agrega la nueva opción de impresión en el controlador de eventos [**PrintTaskRequested**](https://msdn.microsoft.com/library/windows/apps/br206597).

Primero, obtén un objeto [**PrintTaskOptionDetails**](https://msdn.microsoft.com/library/windows/apps/Hh701256). Este objeto se usa para agregar la nueva opción de impresión a la interfaz de usuario de vista previa de impresión. Después, borra la lista de opciones que se muestran en la interfaz de usuario de vista previa de impresión y agrega las opciones que desees que aparezcan cuando el usuario quiera imprimir desde la aplicación. A continuación, crea la nueva opción de impresión e inicializa la lista de valores de opción. Por último, agrega la nueva opción y asigna un controlador para el evento **OptionChanged**.

``` csharp
protected override void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequestedArgs =>
   {
         PrintTaskOptionDetails printDetailedOptions = PrintTaskOptionDetails.GetFromPrintTaskOptions(printTask.Options);
         IList<string> displayedOptions = printDetailedOptions.DisplayedOptions;

         // Choose the printer options to be shown.
         // The order in which the options are appended determines the order in which they appear in the UI
         displayedOptions.Clear();

         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.ColorMode);

         // Create a new list option
         PrintCustomItemListOptionDetails pageFormat = printDetailedOptions.CreateItemListOption("PageContent", "Pictures");
         pageFormat.AddItem("PicturesText", "Pictures and text");
         pageFormat.AddItem("PicturesOnly", "Pictures only");
         pageFormat.AddItem("TextOnly", "Text only");

         // Add the custom option to the option list
         displayedOptions.Add("PageContent");

         printDetailedOptions.OptionChanged += printDetailedOptions_OptionChanged;

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

         sourceRequestedArgs.SetSource(printDocumentSource);
   });
}
```

Las opciones aparecen en la interfaz de usuario de vista previa de impresión en el mismo orden en que se anexaron, con la primera opción en la parte superior de la ventana. En este ejemplo, la opción personalizada se anexa al final para que aparezca en la parte inferior de la lista de opciones. No obstante, puedes colocarla en cualquier parte de la lista; no es necesario agregar las opciones de impresión personalizadas al final.

Cuando el usuario cambia la opción seleccionada en la opción personalizada, actualiza la imagen de vista previa de impresión. Llama al método [**InvalidatePreview**](https://msdn.microsoft.com/library/windows/apps/Hh702146) para dibujar la imagen en la interfaz de usuario de vista previa de impresión, como se muestra a continuación.

``` csharp
async void printDetailedOptions_OptionChanged(PrintTaskOptionDetails sender, PrintTaskOptionChangedEventArgs args)
{
   // Listen for PageContent changes
   string optionId = args.OptionId as string;
   if (string.IsNullOrEmpty(optionId))
   {
         return;
   }

   if (optionId == "PageContent")
   {
         await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
         {
            printDocument.InvalidatePreview();
         });
   }
}
```

## <a name="related-topics"></a>Temas relacionados

* [Directrices de diseño de impresión](https://msdn.microsoft.com/library/windows/apps/Hh868178)
* [//Vídeo de compilación 2015: Developing apps that print in Windows 10 (Desarrollar aplicaciones que imprimen en Windows 10)](https://channel9.msdn.com/Events/Build/2015/2-94)
* [Muestra de impresión para UWP](http://go.microsoft.com/fwlink/p/?LinkId=619984)
