---
author: mcleanbyron
Description: "Puedes animar a los clientes a dejar comentarios iniciando el Centro de opiniones desde la aplicación."
title: "Iniciar el Centro de opiniones desde la aplicación"
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
translationtype: Human Translation
ms.sourcegitcommit: de85956c7c1d2a0ba509d61ee8928b412f057f8a
ms.openlocfilehash: ccda01d9bfa4ffdff2bbce5d6c60c78e026270e5

---

# Iniciar el Centro de opiniones desde la aplicación

Puedes animar a los clientes a dejar comentarios agregando un control (como un botón) a la aplicación de la Plataforma universal de Windows (UWP) que inicia el Centro de opiniones. El Centro de opiniones es una aplicación preinstalada que proporciona un único lugar para recopilar comentarios sobre Windows y las aplicaciones instaladas. Todos los comentarios de los clientes sobre la aplicación enviados a través del Centro de opiniones se recopilan y se presentan en el [Informe de comentarios](../publish/feedback-report.md), en el panel del Centro de desarrollo de Windows; por lo tanto, puedes ver los problemas, las sugerencias y los votos a favor que los clientes hayan enviado en un informe.

>**Nota** El informe **Comentarios** actualmente solo está disponible para las cuentas de desarrollador que se hayan unido al [Programa Insider del Centro de desarrollo](../publish/dev-center-insider-program.md). 

Para iniciar el Centro de opiniones desde la aplicación, usa una API que proporciona el [SDK de Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk). Te recomendamos que uses esta API para iniciar el Centro de opiniones desde un elemento de interfaz de usuario en la aplicación que siga nuestras directrices para el diseño.

>**Nota** El Centro de opiniones está disponible solo en dispositivos con Windows 10 versión 10.0.14271 o posterior. Te recomendamos que muestres un control de comentarios en la aplicación solo si el Centro de opiniones está disponible en el dispositivo del usuario. El código de este tema muestra cómo hacerlo.

## Cómo iniciar el Centro de opiniones desde la aplicación

Para iniciar el Centro de opiniones desde la aplicación:

1. Instala el [SDK de Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk). Además de la API para iniciar el Centro de opiniones, este SDK también proporciona API para otras características, como ejecutar experimentos en las aplicaciones con pruebas A/B y mostrar anuncios. Para obtener más información sobre este SDK, consulta [Rentabilizar tu aplicación y atraer a los clientes con el SDK de Microsoft Store Engagement and Monetization](monetize-your-app-with-the-microsoft-store-engagement-and-monetization-sdk.md).
2. Abre el proyecto en Visual Studio.
3. En el Explorador de soluciones, haz clic con el botón secundario en el nodo **Referencias** del proyecto y haz clic en **Agregar referencia**.
4. En el cuadro de diálogo **Administrador de referencias**, expande **Windows Universal** y haz clic en **Extensiones**.
5. En la lista de los SDK, haz clic en la casilla junto a **SDK de Microsoft Store Engagement** y haz clic en **Aceptar**.
6. En el proyecto, agrega el control que quieras mostrar a los usuarios para iniciar el Centro de opiniones, como un botón. Te recomendamos que configures el control de la siguiente manera:
  * Establece la fuente del contenido que se muestra en el control de **Segoe MDL2 Assets**.
  * Establece el texto en el control en el código de carácter Unicode hexadecimal E939. Este es el código de carácter para el icono de comentarios recomendados en la fuente **Segoe MDL2 Assets**.
  * Establece la visibilidad del control en oculto.

    > **Nota**  El Centro de opiniones está disponible solo en dispositivos con Windows 10 versión 10.0.14271 o posterior. Te recomendamos que ocultes el control de comentarios de manera predeterminada y que lo muestres en el código de inicialización solo si el Centro de opiniones está disponible en el dispositivo del usuario. En el paso siguiente se muestra cómo hacerlo.

  El siguiente código muestra la definición XAML de una clase [Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx) que está configurada como se describió anteriormente.
  ```xml
  <Button x:Name="feedbackButton" FontFamily="Segoe MDL2 Assets" Content="&#xE939;" HorizontalAlignment="Left" Margin="138,352,0,0" VerticalAlignment="Top" Visibility="Collapsed"  Click="feedbackButton_Click"/>
  ```
7. En el código de inicialización de la página de la aplicación que hospeda el control de comentarios, usa la propiedad [IsSupported](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.issupported.aspx) de la clase [Feedback](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.aspx) para determinar si el Centro de opiniones está disponible en el dispositivo del usuario. Si esta propiedad devuelve **true**, haz que el control sea visible. El siguiente código muestra cómo hacerlo para una clase [Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx).
```CSharp
if (Microsoft.Services.Store.Engagement.Feedback.IsSupported)
{
        this.feedbackButton.Visibility = Visibility.Visible;
}
```
8. En el controlador de eventos que se ejecuta cuando el usuario hace clic en el control, llama al método [LaunchFeedbackAsync](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.launchfeedbackasync.aspx) estático de la clase [Feedback](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.feedback.aspx) para iniciar la aplicación Centro de opiniones. Hay dos sobrecargas para este método: una sin parámetros y otra que acepta un diccionario de pares clave y valor que contienen los metadatos que quieres asociar a los comentarios. En el ejemplo siguiente se muestra cómo iniciar el Centro de opiniones en el controlador de eventos [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) para una clase [Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx).
```CSharp
private async void feedbackButton_Click(object sender, RoutedEventArgs e)
{
        await Microsoft.Services.Store.Engagement.Feedback.LaunchFeedbackAsync();
}
```

## Recomendaciones de diseño para la interfaz de usuario de información

Para iniciar el Centro de opiniones, te recomendamos que agregues un elemento de interfaz de usuario en la aplicación (por ejemplo, un botón) que muestre el siguiente icono de comentarios estándar de la fuente Segoe MDL2 Assets y el código de carácter E939.

![]Feedback icon](images/feedback_icon.PNG)

También te recomendamos que uses una o varias de las siguientes opciones de ubicación para vincular al Centro de opiniones en la aplicación.
* **Directamente en la barra de la aplicación**. Según la implementación, se recomendará que uses solo el icono o que agregues texto (como se muestra a continuación).

  ![]Feedback icon](images/feedback_appbar_placement.png)

* **En la configuración de la aplicación**. Esta es una manera más sutil de proporcionar acceso al Centro de opiniones. En el siguiente ejemplo, el vínculo Comentarios aparece como uno de los vínculos de la aplicación.

  ![]Feedback icon](images/feedback_settings_placement.png)

* **En un control flotante controlado por eventos**. Esto es útil cuando quieres consultar a los clientes sobre una pregunta específica antes de iniciar el Centro de opiniones sobre Windows. Por ejemplo, después de que la aplicación use una característica determinada, es posible que le hagas al cliente una pregunta específica sobre su satisfacción con esa característica. Si el cliente elige responder, la aplicación inicia el Centro de opiniones.


## Temas relacionados

* [Informe de comentarios](../publish/feedback-report.md)



<!--HONumber=Jun16_HO4-->


