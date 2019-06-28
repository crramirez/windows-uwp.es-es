---
description: Este tutorial muestra cómo agregar interfaces de usuario de UWP XAML y crear paquetes MSIX para incorporar otros componentes modernos en su aplicación WPF.
title: Agregar notificaciones y las actividades de usuario de Windows 10
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, uwp, formularios windows forms, wpf, Islas de xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 8443ac25ba678986046b967a90a8899eaffb76aa
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420116"
---
# <a name="part-4-add-windows-10-user-activities-and-notifications"></a>Parte 4: Agregar notificaciones y las actividades de usuario de Windows 10

Se trata de la cuarta parte de un tutorial que muestra cómo modernizar una aplicación de escritorio de WPF de ejemplo denominada Contoso gastos. Para obtener información general del tutorial, los requisitos previos y las instrucciones para descargar la aplicación de ejemplo, vea [Tutorial: Modernizar una aplicación de WPF](modernize-wpf-tutorial.md). En este artículo se da por supuesto que ya ha completado [parte 3](modernize-wpf-tutorial-3.md).

En las partes anteriores de este tutorial, ha agregado que UWP XAML controla la aplicación con islas de XAML. Como un producto de esto, también habilitar la aplicación a llamar a cualquier API de WinRT. Se abrirá la oportunidad para la aplicación pueda utilizar muchas otras características que ofrece Windows 10, no solo los controles de UWP XAML.

En el escenario ficticio de este tutorial, el equipo de desarrollo de Contoso ha decidido agregar dos nuevas características a la aplicación: las actividades y las notificaciones. Esta parte del tutorial muestra cómo implementar estas características.

## <a name="add-a-user-activity"></a>Agregar una actividad de usuario

En Windows 10, las aplicaciones pueden realizar un seguimiento de las actividades realizadas por el usuario como la apertura de un archivo o mostrar una página específica. Estas actividades, a continuación, se ponen a disposición a través de la escala de tiempo, una característica incluida en Windows 10, versión 1803, que permite al usuario volver al pasado y reanudar una actividad anteriormente a trabajar rápidamente.

![Imagen de escala de tiempo de Windows](images/wpf-modernize-tutorial/WindowsTimeline.png)

Se realiza el seguimiento de las actividades del usuario mediante [Microsoft Graph](https://developer.microsoft.com/graph/). Sin embargo, cuando se crea una aplicación de Windows 10, no necesita interactuar directamente con los puntos de conexión REST proporcionados por Microsoft Graph. En su lugar, puede usar un conjunto práctico de APIs de WinRT. Vamos a usar estas APIs WinRT en la aplicación Contoso gastos para realizar un seguimiento cada vez que el usuario abre un gasto dentro de la aplicación y usar las tarjetas adaptables para permitir que los usuarios crear la actividad.

### <a name="introduction-to-adaptive-cards"></a>Introducción a las tarjetas adaptables

Esta sección proporciona una breve introducción a [las tarjetas adaptables](https://docs.microsoft.com/adaptive-cards/). Si no necesita esta información, puede omitir este paso y vaya directamente a la [agregar una tarjeta adaptable](#add-an-adaptive-card) instrucciones.

Las tarjetas adaptables permiten a los desarrolladores intercambiar el contenido de la tarjeta de una manera común y coherente. Una tarjeta adaptable se describe mediante una carga JSON que define su contenido, que puede incluir texto, imágenes, acciones y mucho más.

Una tarjeta adaptable define sólo el contenido y no la apariencia visual del contenido. La plataforma donde se recibe la tarjeta adaptable puede representar el contenido usando el estilo más apropiado. Es la manera en que están diseñadas las tarjetas adaptables a través de [un representador](https://docs.microsoft.com/adaptive-cards/rendering-cards/getting-started), que es capaz de realizar la carga de JSON y convertirla en la interfaz de usuario nativa. Por ejemplo, la interfaz de usuario podría ser XAML para un WPF o UWP app, AXML para una aplicación Android o HTML para un sitio Web o un chat bot.

Este es un ejemplo de una carga de la tarjeta adaptable simple.

```json
{
    "type": "AdaptiveCard",
    "body": [
        {
            "type": "Container",
            "items": [
                {
                    "type": "TextBlock",
                    "size": "Medium",
                    "weight": "Bolder",
                    "text": "Publish Adaptive Card schema"
                },
                {
                    "type": "ColumnSet",
                    "columns": [
                        {
                            "type": "Column",
                            "items": [
                                {
                                    "type": "Image",
                                    "style": "Person",
                                    "url": "https://pbs.twimg.com/profile_images/3647943215/d7f12830b3c17a5a9e4afcc370e3a37e_400x400.jpeg",
                                    "size": "Small"
                                }
                            ],
                            "width": "auto"
                        },
                        {
                            "type": "Column",
                            "items": [
                                {
                                    "type": "TextBlock",
                                    "weight": "Bolder",
                                    "text": "Matt Hidinger",
                                    "wrap": true
                                },
                                {
                                    "type": "TextBlock",
                                    "spacing": "None",
                                    "text": "Created {{DATE(2017-02-14T06:08:39Z,SHORT)}}",
                                    "isSubtle": true,
                                    "wrap": true
                                }
                            ],
                            "width": "stretch"
                        }
                    ]
                }
            ]
        }
    ],
    "actions": [
        {
            "type": "Action.ShowCard",
            "title": "Set due date",
            "card": {
                "type": "AdaptiveCard",
                "style": "emphasis",
                "body": [
                    {
                        "type": "Input.Date",
                        "id": "dueDate"
                    },
                    {
                        "type": "Input.Text",
                        "id": "comment",
                        "placeholder": "Add a comment",
                        "isMultiline": true
                    }
                ],
                "actions": [
                    {
                        "type": "Action.OpenUrl",
                        "title": "OK",
                        "url": "http://adaptivecards.io"
                    }
                ],
                "$schema": "http://adaptivecards.io/schemas/adaptive-card.json"
            }
        },
        {
            "type": "Action.OpenUrl",
            "title": "View",
            "url": "http://adaptivecards.io"
        }
    ],
    "$schema": "http://adaptivecards.io/schemas/adaptive-card.json",
    "version": "1.0"
}
```

La imagen siguiente muestra cómo este JSON se representa de maneras diferentes mediante ta canal de Teams, Cortana y una notificación de Windows.

![Imagen de representación tarjeta adaptable](images/wpf-modernize-tutorial/AdaptiveCards.png)

Las tarjetas adaptables desempeñan un papel importante en escala de tiempo porque es la manera en que Windows procesa las actividades. Cada vista en miniatura que aparece dentro de la escala de tiempo es realmente una tarjeta adaptable. Por lo tanto, cuando vas a crear una actividad de usuario dentro de la aplicación, se le pedirá que proporcione una tarjeta adaptable para representarlo.

> [!NOTE]
> Está usando una excelente manera de pensar y establecer el diseño de una tarjeta adaptable [el diseñador en línea](https://adaptivecards.io/designer/). Tendrá la oportunidad de diseñar la tarjeta con bloques de creación (imágenes, textos, columnas, etcetera) y recibir el JSON correspondiente. Una vez que tenga una idea del diseño final, puede usar una biblioteca denominada [las tarjetas adaptables](https://www.nuget.org/packages/AdaptiveCards/) para que sea más fácil generar su tarjeta adaptable mediante C# clases en lugar de JSON sin formato, que puede ser difícil de depurar y compilar.

### <a name="add-an-adaptive-card"></a>Agregar una tarjeta adaptable

1. Haga clic con el botón derecho en el **ContosoExpenses.Core** proyecto en el Explorador de soluciones y elija **administrar paquetes NuGet**.

2. En el **Administrador de paquetes de NuGet** ventana, haga clic en **examinar**. Busque el `Newtonsoft.Json` empaquetar e instalar la última versión disponible. Esto es una conocida biblioteca de manipulación de JSON que va a utilizar para ayudar a mainipulate las cadenas JSON requeridas por las tarjetas adaptables.

    ![Paquete NewtonSoft.Json NuGet](images/wpf-modernize-tutorial/JsonNetNuGet.png)

    > [!NOTE]
    > Si no instala el `Newtonsoft.Json` paquete por separado, la biblioteca de las tarjetas adaptables hará referencia a una versión anterior de la `Newtonsoft.Json` paquete que no es compatible con .NET Core 3.0.

2. En el **Administrador de paquetes de NuGet** ventana, haga clic en **examinar**. Busque el `AdaptiveCards` empaquetar e instalar la última versión disponible.

    ![Paquete de NuGet de las tarjetas adaptable](images/wpf-modernize-tutorial/AdaptiveCardsNuGet.png)

3. En **el Explorador de soluciones**, haga clic en el **ContosoExpenses.Core** del proyecto, elija **Agregar -> clase**. Nombre de la clase **TimelineService.cs** y haga clic en **Aceptar**.

4. En el **TimelineService.cs** , agregue las siguientes instrucciones a la parte superior del archivo.

    ```csharp
    using AdaptiveCards;
    using ContosoExpenses.Data.Models;
    ```

5. Cambiar el espacio de nombres declarado en el archivo de `ContosoExpenses.Core` a `ContosoExpenses`.

5. Agregue el método siguiente a la `TimelineService` clase.

   ```csharp
    private string BuildAdaptiveCard(Expense expense)
    {
        AdaptiveCard card = new AdaptiveCard("1.0");

        AdaptiveTextBlock title = new AdaptiveTextBlock
        {
            Text = expense.Description,
            Size = AdaptiveTextSize.Medium,
            Wrap = true
        };

        AdaptiveColumnSet columnSet = new AdaptiveColumnSet();
        AdaptiveColumn photoColumn = new AdaptiveColumn
        {
            Width = "auto"
        };

        AdaptiveImage image = new AdaptiveImage
        {
            Url = new Uri("https://appmodernizationworkshop.blob.core.windows.net/contosoexpenses/Contoso192x192.png"),
            Size = AdaptiveImageSize.Small,
            Style = AdaptiveImageStyle.Default
        };
        photoColumn.Items.Add(image);

        AdaptiveTextBlock amount = new AdaptiveTextBlock
        {
            Text = expense.Cost.ToString(),
            Weight = AdaptiveTextWeight.Bolder,
            Wrap = true
        };

        AdaptiveTextBlock date = new AdaptiveTextBlock
        {
            Text = expense.Date.Date.ToShortDateString(),
            IsSubtle = true,
            Spacing = AdaptiveSpacing.None,
            Wrap = true
        };

        AdaptiveColumn expenseColumn = new AdaptiveColumn
        {
            Width = "stretch"
        };
        expenseColumn.Items.Add(amount);
        expenseColumn.Items.Add(date);

        columnSet.Columns.Add(photoColumn);
        columnSet.Columns.Add(expenseColumn);

        card.Body.Add(title);
        card.Body.Add(columnSet);

        string json = card.ToJson();
        return json;
    }
    ```

#### <a name="about-the-code"></a>Acerca del código

Este método recibe una **gastos** compila un nuevo objeto con toda la información sobre los gastos para representar y **AdaptiveCard** objeto. El método agrega lo siguiente a la tarjeta:

- Un título, que utiliza la descripción del gasto.
- Una imagen, que es el logotipo de Contoso.
- El importe del gasto.
- La fecha del gasto.

Los 3 últimos elementos se dividen en dos columnas diferentes, por lo que el logotipo de Contoso y los detalles sobre los gastos pueden colocarse en paralelo. Una vez creado el objeto, el método devuelve la cadena JSON correspondiente con la Ayuda de la **ToJson** método.

### <a name="define-the-user-activity"></a>Definir la actividad del usuario

Ahora que ha definido la tarjeta adaptable, puede crear una actividad de usuario basada en él.

1. Agregue las siguientes instrucciones al principio del **TimelineService.cs** archivo:

    ```csharp
    using Windows.ApplicationModel.UserActivities;
    using System.Threading.Tasks;
    using Windows.UI.Shell;
    ```

    > [!NOTE]
    > Estos son los espacios de nombres UWP. Estos resolver porque el `Microsoft.Toolkit.Wpf.UI.Controls` paquete de NuGet que instaló en el paso 2 incluye una referencia a la `Microsoft.Windows.SDK.Contracts` empaquetar, lo que permite el **ContosoExpenses.Core** proyecto referencia WinRT APIs incluso aunque se trate de .NET Proyecto de Core 3.

2. Agregue las siguientes declaraciones de campo para el `TimelineService` clase.

    ```csharp
    private UserActivityChannel _userActivityChannel;
    private UserActivity _userActivity;
    private UserActivitySession _userActivitySession;
    ```

3. Agregue el método siguiente a la `TimelineService` clase.

    ```csharp
    public async Task AddToTimeline(Expense expense)
    {
        _userActivityChannel = UserActivityChannel.GetDefault();
        _userActivity = await _userActivityChannel.GetOrCreateUserActivityAsync($"Expense-{expense.ExpenseId}");

        _userActivity.ActivationUri = new Uri($"contosoexpenses://expense/{expense.ExpenseId}");
        _userActivity.VisualElements.DisplayText = "Contoso Expenses";

        string json = BuildAdaptiveCard(expense);

        _userActivity.VisualElements.Content = AdaptiveCardBuilder.CreateAdaptiveCardFromJson(json);

        await _userActivity.SaveAsync();
        _userActivitySession?.Dispose();
        _userActivitySession = _userActivity.CreateSession();
    }
    ```

4. Guarde los cambios en **TimelineService.cs**.

#### <a name="about-the-code"></a>Acerca del código

El `AddToTimeline` método obtiene primero un **UserActivityChannel** objeto que es necesario para almacenar las actividades del usuario. A continuación, crea una nueva actividad de usuario mediante el **GetOrCreateUserActivityAsync** método, que requiere un identificador único. De este modo, si ya existe una actividad, la aplicación puede actualizarlo; en caso contrario, creará uno nuevo. El identificador para pasar depende por el tipo de aplicación que estamos creando:

* Si desea actualizar siempre la misma actividad para que la escala de tiempo, solo se mostrarán una más reciente, puede usar un identificador fijo (como **gastos**).
* Si desea realizar un seguimiento de todas las actividades como uno diferente, para que la escala de tiempo mostrará todas ellas, puede usar un identificador dinámico.

En este escenario, la aplicación realizará el seguimiento de cada gasto abierto como una actividad de usuario diferente, por lo que el código crea cada identificador con la palabra clave **gastos -** seguido por el identificador único gastos.

Después de que el método crea un **UserActivity** objeto, rellena el objeto con la siguiente información:

* Un **ActivationUri** que se invoca cuando el usuario hace clic en la actividad en la escala de tiempo. El código usa un protocolo personalizado denominado **contosoexpenses** que va a controlar la aplicación más adelante.
* El **VisualElements** objeto, que contiene un conjunto de propiedades que definen la apariencia visual de la actividad. Este código establece la **DisplayText** (que es el título que aparece encima de la entrada en la escala de tiempo) y el **contenido**. 

Esto es donde la tarjeta adaptable que se definió anteriormente desempeña un papel. La aplicación pasa la tarjeta adaptable diseñada anteriormente como contenido para el método. Sin embargo, Windows 10 usa un objeto diferente para representar una tarjeta en comparación con el utilizado por el `AdaptiveCards` paquete NuGet. Por lo tanto, el método vuelve a crear la tarjeta usando el **CreateAdaptiveCardFromJson** método expuesto por el **AdaptiveCardBuilder** clase. Después de la actividad del usuario que crea el método, guarda la actividad y crea una nueva sesión.

Cuando un usuario hace clic en una actividad en la escala de tiempo, el **contosoexpenses: / /** se activará el protocolo y la dirección URL incluirá la información de la aplicación debe recuperar el gasto seleccionado. Como una tarea opcional, podría implementar la activación de protocolos para que la aplicación reacciona correctamente cuando el usuario usa la escala de tiempo.

### <a name="integrate-the-application-with-timeline"></a>Integrar la aplicación con la escala de tiempo

Ahora que ha creado una clase que interactúa con la escala de tiempo, nos podemos empezar a usar para mejorar la experiencia de la aplicación. El mejor lugar para usar el **AddToTimeline** método expuesto por el **TimelineService** clase es cuando el usuario abre la página de detalles de gasto.

1. En el **ContosoExpenses.Core** del proyecto, expanda el **ViewModels** carpeta y abra el **ExpenseDetailViewModel.cs** archivo. Se trata de ViewModel que admite la ventana de detalle de gastos.

2. Busque el constructor público de la **ExpenseDetailViewModel** de clases y agregue el código siguiente al final del constructor. Cuando se abre la ventana de gastos, el método se llama a la **AddToTimeline** método y pasa el gasto actual. El **TimelineService** clase utiliza esta información para crear una actividad de usuario con la información de gastos.

    ```csharp
    TimelineService timeline = new TimelineService();
    timeline.AddToTimeline(expense);
    ```

    Cuando haya terminado, el constructor debería parecerse a esto.

    ```csharp
    public ExpensesDetailViewModel(IDatabaseService databaseService, IStorageService storageService)
    {
        var expense = databaseService.GetExpense(storageService.SelectedExpense);

        ExpenseType = expense.Type;
        Description = expense.Description;
        Location = expense.Address;
        Amount = expense.Cost;

        TimelineService timeline = new TimelineService();
        timeline.AddToTimeline(expense);
    }
    ```

3. Presione F5 para compilar y ejecutar la aplicación en el depurador. Elija a un empleado en la lista y, a continuación, elija un gasto. En la página de detalles, tenga en cuenta la descripción de los gastos, la fecha y la cantidad.

4. Presione **inicio + TAB** para abrir la escala de tiempo.

5. Desplácese hacia abajo en la lista de aplicaciones actualmente abiertas hasta que vea la sección titulada **hoy**. En esta sección se muestra algunas de las actividades de usuario más reciente. Haga clic en el **ver todas las actividades** vínculo junto a la **hoy** encabezado.

6. Confirme que ve una tarjeta nueva con la información sobre el gasto que acaba de seleccionar en la aplicación.

    ![Escala de tiempo de los gastos de Contoso](images/wpf-modernize-tutorial/ContosoExpensesTimeline.png)

7. Si ahora abre otros gastos, verá nuevas tarjetas que se va a agregar como las actividades del usuario. Recuerde que el código usa un identificador diferente para cada actividad, por lo que crea una tarjeta para cada gasto abrir en la aplicación.

8. Cierra la aplicación.

## <a name="add-a-notification"></a>Agregar una notificación

La segunda característica en que el equipo de desarrollo de Contoso desea agregar es una notificación que se muestra al usuario cada vez que se guarda un gasto nuevo en la base de datos. Para ello, puede aprovechar el sistema de notificaciones integradas en Windows 10, que se expone a los desarrolladores a través de WinRT APIs. Este sistema de notificación tiene muchas ventajas:

- Las notificaciones son coherentes con el resto del sistema operativo.
- Son útiles.
- Se almacenan en el centro de actividades por lo que se pueden revisar más adelante.

Para agregar una notificación a la aplicación:

1. En **el Explorador de soluciones**, haga clic en el **ContosoExpenses.Core** del proyecto, elija **Agregar -> clase**. Nombre de la clase **NotificationService.cs** y haga clic en **Aceptar**.

2. En el **NotificationService.cs** , agregue las siguientes instrucciones a la parte superior del archivo.

    ```csharp
    using Windows.Data.Xml.Dom;
    using Windows.UI.Notifications;
    ```

3. Cambiar el espacio de nombres declarado en el archivo de `ContosoExpenses.Core` a `ContosoExpenses`.

4. Agregue el método siguiente a la `NotificationService` clase.

    ```csharp
    public void ShowNotification(string description, double amount)
    {
        string xml = $@"<toast>
                          <visual>
                            <binding template='ToastGeneric'>
                              <text>Expense added</text>
                              <text>Description: {description} - Amount: {amount} </text>
                            </binding>
                          </visual>
                        </toast>";

        XmlDocument doc = new XmlDocument();
        doc.LoadXml(xml);

        ToastNotification toast = new ToastNotification(doc);
        ToastNotificationManager.CreateToastNotifier().Show(toast);
    }
    ```

    Notificaciones del sistema se representan mediante una carga XML, que puede incluir texto, imágenes, acciones y mucho más. Puede encontrar todos los elementos admitidos [aquí](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/toast-schema). Este código utiliza un esquema muy simple con dos líneas de texto: el título y el cuerpo. Después de que el código define la carga XML y los carga en un **XmlDocument** objeto que encapsula el código XML en un **ToastNotification** objeto y se muestra mediante un el **ToastNotificationManager** clase.

5. En el **ContosoExpenses.Core** del proyecto, expanda el **ViewModels** carpeta y abra el **AddNewExpenseViewModel.cs** archivo. 

6. Busque el `SaveExpenseCommand` método, que se desencadena cuando el usuario presiona el botón para guardar un gasto nuevo. Agregue el código siguiente a este método, justo después de la llamada a la `SaveExpense` método.

    ```csharp
    NotificationService notificationService = new NotificationService();
    notificationService.ShowNotification(expense.Description, expense.Cost);
    ```

    Cuando haya terminado, la `SaveExpenseCommand` método debe tener un aspecto similar al siguiente.

    ```csharp
    private RelayCommand _saveExpenseCommand;
    public RelayCommand SaveExpenseCommand
    {
        get
        {
            if (_saveExpenseCommand == null)
            {
                _saveExpenseCommand = new RelayCommand(() =>
                {
                    Expense expense = new Expense
                    {
                        Address = Address,
                        City = City,
                        Cost = Cost,
                        Date = Date,
                        Description = Description,
                        EmployeeId = storageService.SelectedEmployeeId,
                        Type = ExpenseType
                    };

                    databaseService.SaveExpense(expense);

                    NotificationService notificationService = new NotificationService();
                    notificationService.ShowNotification(expense.Description, expense.Cost);

                    Messenger.Default.Send<UpdateExpensesListMessage>(new UpdateExpensesListMessage());
                    Messenger.Default.Send<CloseWindowMessage>(new CloseWindowMessage());
                }, () => IsFormFilled
                );
            }

            return _saveExpenseCommand;
        }
    }
    ```

7. Presione F5 para compilar y ejecutar la aplicación en el depurador. Elija un empleado en la lista y, a continuación, haga clic en el **Agregar gasto nuevo** botón. Complete todos los campos en el formulario y presione **guardar**.

8. Recibirá la siguiente excepción.

    ![Error de notificación del sistema](images/wpf-modernize-tutorial/ToastNotificationError.png)

Esta excepción se produce por el hecho de que la aplicación de gastos de Contoso aún no tiene la identidad del paquete. Algunas APIs WinRT, incluidas las notificaciones de API, requieren la identidad del paquete antes de que se pueden usar en una aplicación. Las aplicaciones UWP recibir identidad del paquete de forma predeterminada porque solo se pueden distribuir a través de paquetes MSIX. Otros tipos de aplicaciones de Windows, incluidas las aplicaciones WPF, también pueden implementarse a través de paquetes MSIX para obtener la identidad del paquete. La siguiente parte de este tutorial explorará cómo hacerlo.

## <a name="next-steps"></a>Pasos siguientes

En este punto del tutorial, ha agregado correctamente una actividad de usuario a la aplicación que se integra con la escala de tiempo de Windows, y también ha agregado una notificación a la aplicación que se desencadena cuando los usuarios crean un gasto nuevo. Sin embargo, la notificación todavía no funciona porque la aplicación requiere la identidad del paquete para usar las notificaciones de API. Para obtener información sobre cómo crear un paquete MSIX para la aplicación obtener la identidad del paquete y obtener otros beneficios de la implementación, consulte [parte 5: Empaquetar e implementar con MSIX](modernize-wpf-tutorial-5.md).
