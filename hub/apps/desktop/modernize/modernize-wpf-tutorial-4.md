---
description: En este tutorial se muestra cómo agregar características de actividad y notificación a la aplicación.
title: Incorporación de actividades y notificaciones del usuario de Windows 10
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, islas xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 3acc5638115932f6536eccb3be5e7222ef53fbb7
ms.sourcegitcommit: 0c4bbaf1c119a84002748cdcf02e1449835559c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2020
ms.locfileid: "92133058"
---
# <a name="part-4-add-windows-10-user-activities-and-notifications"></a>4\.ª parte: Incorporación de actividades y notificaciones del usuario de Windows 10

Esta es la cuarta parte de un tutorial que muestra cómo modernizar una aplicación de escritorio de WPF de ejemplo llamada Contoso Expenses. Para obtener información general sobre el tutorial, los requisitos previos y las instrucciones para descargar la aplicación de ejemplo, consulta [Tutorial: Modernización de una aplicación WPF](modernize-wpf-tutorial.md). En este artículo se da por supuesto que ya has completado la [parte 3](modernize-wpf-tutorial-3.md).

En las partes anteriores de este tutorial, has agregado controles XAML de UWP a la aplicación con islas XAML. Indirectamente, también has habilitado la aplicación para que llame a cualquier API de WinRT. Esto abre la oportunidad de que la aplicación use muchas otras características ofrecidas por Windows 10, no solo los controles XAML de UWP.

En el escenario ficticio de este tutorial, el equipo de desarrollo de Contoso ha decidido agregar dos nuevas características a la aplicación: actividades y notificaciones. En esta parte del tutorial se muestra cómo implementar estas características.

## <a name="add-a-user-activity"></a>Agregar una actividad de usuario

En Windows 10, las aplicaciones pueden realizar un seguimiento de las actividades realizadas por el usuario, como abrir un archivo o mostrar una página específica. Estas actividades están disponibles después mediante la línea de tiempo, una característica incorporada en la versión 1803 de Windows 10, que permite al usuario volver rápidamente al pasado y reanudar una actividad que se había iniciado anteriormente.

![Imagen de la línea de tiempo de Windows](images/wpf-modernize-tutorial/WindowsTimeline.png)

El seguimiento de las actividades de usuario se realiza mediante [Microsoft Graph](https://developer.microsoft.com/graph/). Sin embargo, cuando se compila una aplicación de Windows 10, no es necesario interactuar directamente con los puntos de conexión de REST proporcionados por Microsoft Graph. En su lugar, puedes usar un conjunto de API de WinRT adecuado. Vamos a usar estas API de WinRT en la aplicación Contoso Expenses para realizar un seguimiento cada vez que el usuario cree un gasto en la aplicación, y usar tarjetas adaptables que los usuarios puedan crear la actividad.

### <a name="introduction-to-adaptive-cards"></a>Introducción a las tarjetas adaptables

En esta sección se proporciona una breve introducción a las [tarjetas adaptables](/adaptive-cards/). Si no necesitas esta información, puedes ir directamente a las instrucciones sobre cómo [agregar una tarjeta adaptable](#add-an-adaptive-card).

Las tarjetas adaptables permiten a los desarrolladores intercambiar contenido de tarjetas de una manera común y coherente. Una tarjeta adaptable se describe mediante una carga JSON que define su contenido, que puede incluir texto, imágenes, acciones, etc.

Una tarjeta adaptable define solo el contenido y no la apariencia visual del contenido. La plataforma donde se recibe la tarjeta adaptable puede representar el contenido con el estilo más adecuado. Las tarjetas adaptables se diseñan con un [representador](/adaptive-cards/rendering-cards/getting-started), que puede tomar la carga de JSON y convertirla en una interfaz de usuario nativa. Por ejemplo, la interfaz de usuario podría ser XAML para una aplicación para WPF o UWP, AXML para una aplicación Android o HTML para un sitio web o un chat de bot.

Este es un ejemplo de una carga sencilla de tarjeta adaptable.

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

En la imagen siguiente se muestra cómo representan este JSON un canal de Teams, Cortana y una notificación de Windows.

![Imagen de la representación de una tarjeta adaptable](images/wpf-modernize-tutorial/AdaptiveCards.png)

Las tarjetas adaptables desempeñan un papel importante en Línea de tiempo porque es la forma que tiene Windows de representar las actividades. Cada miniatura mostrada dentro de la línea de tiempo es realmente una tarjeta adaptable. Por lo tanto, cuando vayas a crear una actividad de usuario dentro de la aplicación, se te pedirá que proporciones una tarjeta adaptable para representarla.

> [!NOTE]
> Una excelente manera de recopilar el diseño de una tarjeta adaptable es usar el [diseñador en línea](https://adaptivecards.io/designer/). Tendrás la oportunidad de diseñar la tarjeta con bloques de creación (imágenes, textos, columnas, etc.) y obtener el código JSON correspondiente. Cuando tengas la idea del diseño final, puedes usar una biblioteca [Tarjetas adaptables](https://www.nuget.org/packages/AdaptiveCards/) para crear tu tarjeta adaptable fácilmente usando las clases de C# en lugar de JSON sin formato, que podría resultar difícil de depurar y compilar.

### <a name="add-an-adaptive-card"></a>Agregar una tarjeta adaptable

1. En el Explorador de soluciones, haz clic con el botón derecho en el proyecto **ContosoExpenses.Core** y elige **Administrar paquetes NuGet**.

2. En la ventana **Administrador de paquetes NuGet**, haz clic en **Examinar**. Busca el paquete `Newtonsoft.Json` e instala la versión más reciente disponible. Esta es una conocida biblioteca de manipulación de JSON que se usará para ayudar a manipular las cadenas JSON requeridas por las tarjetas adaptables.

    ![Paquete NuGet NewtonSoft.Json](images/wpf-modernize-tutorial/JsonNetNuGet.png)

    > [!NOTE]
    > Si no instalas el paquete `Newtonsoft.Json` por separado, la biblioteca de tarjetas adaptables hará referencia a una versión anterior del paquete `Newtonsoft.Json` que no es compatible con .NET Core 3.0.

2. En la ventana **Administrador de paquetes NuGet**, haz clic en **Examinar**. Busca el paquete `AdaptiveCards` e instala la versión más reciente disponible.

    ![Paquete NuGet de tarjetas adaptables](images/wpf-modernize-tutorial/AdaptiveCardsNuGet.png)

3. En el **Explorador de soluciones**, haz clic con el botón derecho en el proyecto **ContosoExpenses.Core** y elige **Agregar -> Clase**. Asigna a la clase el nombre **TimelineService.cs** y haz clic en **Aceptar**.

4. En el archivo **TimelineService.cs**, agrega las siguientes instrucciones en la parte superior del archivo.

    ```csharp
    using AdaptiveCards;
    using ContosoExpenses.Data.Models;
    ```

5. Cambia el espacio de nombres `ContosoExpenses.Core` declarado en el archivo por `ContosoExpenses`.

5. Agrega el método siguiente a la clase `TimelineService`:

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

Este método recibe un objeto **Expense** con toda la información sobre el gasto que se va a representar y genera un nuevo objeto **AdaptiveCard**. El método agrega lo siguiente a la tarjeta:

- Un título, que usa la descripción del gasto.
- Una imagen, que es el logotipo de Contoso.
- El importe del gasto.
- La fecha del gasto.

Los tres últimos elementos se dividen en dos columnas diferentes, por lo que el logotipo de Contoso y los detalles sobre el gasto pueden colocarse en paralelo. Una vez compilado el objeto, el método devuelve la cadena JSON correspondiente con la ayuda del método **ToJson**.

### <a name="define-the-user-activity"></a>Definir la actividad del usuario

Ahora que has definido la tarjeta adaptable, puedes crear una actividad de usuario basada en ella.

1. Agrega las siguientes instrucciones en la parte superior del archivo **TimelineService.cs**:

    ```csharp
    using Windows.ApplicationModel.UserActivities;
    using System.Threading.Tasks;
    using Windows.UI.Shell;
    ```

    > [!NOTE]
    > Estos son espacios de nombres de UWP. Se resuelven porque el paquete NuGet `Microsoft.Toolkit.Wpf.UI.Controls` que instalaste en el paso 2 incluye una referencia al paquete `Microsoft.Windows.SDK.Contracts`, que permite que el proyecto **ContosoExpenses.Core** haga referencia a las API de WinRT aunque sea un proyecto de .NET Core 3.

2. Agrega las siguientes declaraciones de campo a la clase `TimelineService`.

    ```csharp
    private UserActivityChannel _userActivityChannel;
    private UserActivity _userActivity;
    private UserActivitySession _userActivitySession;
    ```

3. Agrega el método siguiente a la clase `TimelineService`:

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

4. Guarda los cambios en **TimelineService.cs**.

#### <a name="about-the-code"></a>Acerca del código

El método `AddToTimeline` obtiene primero un objeto **UserActivityChannel** que es necesario para almacenar las actividades de usuario. A continuación, crea una nueva actividad de usuario con el método **GetOrCreateUserActivityAsync**, que requiere un identificador único. Así, si una actividad ya existe, la aplicación puede actualizarla. En caso contrario, creará una nueva. El identificador que se va a pasar depende del tipo de aplicación que se está compilando:

* Si quieres actualizar siempre la misma actividad para que la línea de tiempo muestre solo la más reciente, puedes usar un identificador fijo (como **Expenses**).
* Si quieres hacer el seguimiento de cada actividad por separado, para que la línea de tiempo las muestre todas, puedes usar un identificador dinámico.

En este escenario, la aplicación realizará un seguimiento de los gastos abiertos como una actividad de usuario diferente, por lo que el código crea cada identificador usando la palabra clave **Expense-** seguida del identificador de gastos único.

Después de que el método crea un objeto **UserActivity**, lo rellena con la siguiente información:

* **ActivationUri**, que se invoca cuando el usuario hace clic en la actividad en la línea de tiempo. El código usa un protocolo personalizado llamado **contosoexpenses** que la aplicación controlará más adelante.
* El objeto **VisualElements**, que contiene un conjunto de propiedades que definen la apariencia visual de la actividad. Este código establece los valores de **DisplayText** (que es el título que se muestra en la parte superior de la entrada en la línea de tiempo) y **Content**. 

Aquí es donde entra en juego la tarjeta adaptable que definió anteriormente. La aplicación pasa la tarjeta adaptable que diseñaste como contenido al método. Sin embargo, para representar una tarjeta, Windows 10 usa un objeto diferente del que usa el paquete NuGet `AdaptiveCards`. Por lo tanto, el método vuelve a crear la tarjeta con el método **CreateAdaptiveCardFromJson** expuesto por la clase **AdaptiveCardBuilder**. Después de que el método crea la actividad de usuario, guarda la actividad y crea una nueva sesión.

Cuando un usuario haga clic en una actividad de la línea de tiempo, se activará el protocolo **contosoexpenses://** y la dirección URL incluirá la información que la aplicación necesita para recuperar el gasto seleccionado. Como tarea opcional, podrías implementar la activación del protocolo para que la aplicación reaccione correctamente cuando el usuario utiliza la línea de tiempo.

### <a name="integrate-the-application-with-timeline"></a>Integración de la aplicación con la línea de tiempo

Ahora que has creado una clase que interactúa con la línea de tiempo, podemos empezar a usarla para mejorar la experiencia de la aplicación. El mejor lugar para usar el método **AddToTimeline** expuesto por la clase **TimelineService** es cuando el usuario abre la página de detalles de un gasto.

1. En el proyecto **ContosoExpenses.Core**, expande la carpeta **ViewModels** y abre el archivo **ExpenseDetailViewModel.cs**. Este es el control ViewModel que admite la ventana del detalle de gastos.

2. Busca el constructor público de la clase **ExpenseDetailViewModel** y agrega el código siguiente al final del constructor. Cada vez que se abre la ventana de gastos, el método llama al método **AddToTimeline** y pasa el gasto actual. La clase **TimelineService** usa esta información para crear una actividad de usuario con la información de los gastos.

    ```csharp
    TimelineService timeline = new TimelineService();
    timeline.AddToTimeline(expense);
    ```

    Cuando termines, el constructor tendrá el siguiente aspecto.

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

3. Presiona F5 para compilar y ejecutar la aplicación en el depurador. Elige un empleado en la lista y uno de los gastos. En la página de detalles, anota la descripción del gasto, la fecha y el importe.

4. Presiona **Inicio + TAB** para abrir la línea de tiempo.

5. Baja por la lista de aplicaciones abiertas actualmente hasta que veas la sección **Hoy**. En esta sección se muestran algunas de las actividades de usuario más recientes. Haz clic en el vínculo **Ver todas las actividades** junto al encabezado **Hoy**.

6. Confirma que ves una nueva tarjeta con la información sobre los gastos que acabas de seleccionar en la aplicación.

    ![Línea de tiempo de Contoso Expenses](images/wpf-modernize-tutorial/ContosoExpensesTimeline.png)

7. Si ahora abres otros gastos, verás que se agregan nuevas tarjetas como actividades de usuario. Recuerda que el código usa un identificador diferente para cada actividad, por lo que crea una tarjeta para cada gasto que abra en la aplicación.

8. Cierra la aplicación.

## <a name="add-a-notification"></a>Agregar una notificación

La segunda característica que el equipo de desarrollo de Contoso quiere agregar es una notificación que se muestre al usuario cada vez que se guarde un nuevo gasto en la base de datos. Para ello, puede aprovechar el sistema de notificaciones integradas de Windows 10, que se expone a los desarrolladores mediante las API de WinRT. Este sistema de notificación tiene muchas ventajas:

- Las notificaciones son coherentes con el resto del sistema operativo.
- Son accionables.
- Se almacenan en el Centro de actividades para que se puedan revisar más adelante.

Para agregar una notificación a la aplicación:

1. En el **Explorador de soluciones**, haz clic con el botón derecho en el proyecto **ContosoExpenses.Core** y elige **Agregar -> Clase**. Asigna a la clase el nombre **NotificationService.cs** y haz clic en **Aceptar**.

2. En el archivo **NotificationService.cs**, agrega las siguientes instrucciones en la parte superior del archivo.

    ```csharp
    using Windows.Data.Xml.Dom;
    using Windows.UI.Notifications;
    ```

3. Cambia el espacio de nombres `ContosoExpenses.Core` declarado en el archivo por `ContosoExpenses`.

4. Agrega el método siguiente a la clase `NotificationService`:

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

    Las notificaciones del sistema se representan mediante una carga XML, que puede incluir texto, imágenes, acciones, etc. Encontrarás todos los elementos admitidos [aquí](/windows/uwp/design/shell/tiles-and-notifications/toast-schema). Este código usa un esquema muy sencillo con dos líneas de texto: el título y el cuerpo. Una vez que el código define la carga XML y la carga en un objeto **XmlDocument**, ajusta el código XML en un objeto **ToastNotification** y lo muestra mediante una clase **ToastNotificationManager**.

5. En el proyecto **ContosoExpenses.Core**, expande la carpeta **ViewModels** y abre el archivo **AddNewExpenseViewModel.cs**. 

6. Busca el método `SaveExpenseCommand`, que se desencadena cuando el usuario presiona el botón para guardar un nuevo gasto. Agrega el código siguiente a este método, justo después de la llamada al método `SaveExpense`.

    ```csharp
    NotificationService notificationService = new NotificationService();
    notificationService.ShowNotification(expense.Description, expense.Cost);
    ```

    Cuando hayas terminado, el método `SaveExpenseCommand` tendrá este aspecto.

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

7. Presiona F5 para compilar y ejecutar la aplicación en el depurador. Elige un empleado de la lista y, a continuación y haz clic en el botón **Add new expense** (Agregar gasto nuevo). Completa todos los campos del formulario y presiona **Save** (Guardar).

8. Recibirás la siguiente excepción.

    ![Error de notificación del sistema](images/wpf-modernize-tutorial/ToastNotificationError.png)

Esta excepción se debe al hecho de que la aplicación Contoso Expenses todavía no tiene la identidad del paquete. Algunas API de WinRT, incluida la API de notificaciones, requieren la identidad del paquete antes de que se puedan usar en una aplicación. Las aplicaciones UWP reciben la identidad de paquete de forma predeterminada porque solo se pueden distribuir a través de paquetes MSIX. Otros tipos de aplicaciones de Windows, incluidas las aplicaciones de WPF, también se pueden implementar a través de paquetes MSIX para obtener la identidad del paquete. En la siguiente parte de este tutorial se explica cómo hacerlo.

## <a name="next-steps"></a>Pasos siguientes

En este punto del tutorial, has agregado correctamente una actividad de usuario a la aplicación que se integra con la línea de tiempo de Windows, y has agregado también una notificación a la aplicación que se desencadena cuando los usuarios crean un nuevo gasto. Sin embargo, la notificación todavía no funciona porque la aplicación requiere que la identidad del paquete use la API de notificaciones. Para obtener información sobre cómo crear un paquete MSIX para que la aplicación obtenga la identidad del paquete, así como otras ventajas de implementación, consulta la [parte 5: Empaquetar e implementar con MSIX](modernize-wpf-tutorial-5.md).