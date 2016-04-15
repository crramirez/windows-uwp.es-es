---
Description: Proporciona una lista de comprobación que te ayudará a garantizar que tu aplicación para Plataforma universal de Windows (UWP) sea accesible.
title: Lista de comprobación de accesibilidad
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
label: Checklist
template: detail.hbs
---

Lista de comprobación de accesibilidad
===================================================================================

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Proporciona una lista de comprobación que te ayudará a garantizar que tu aplicación para Plataforma universal de Windows (UWP) sea accesible.

Aquí proporcionamos una lista de comprobación que te ayudará a garantizar que la aplicación sea accesible.

1.  Establece el nombre accesible (obligatorio) y la descripción accesible (opcional) para los elementos de la interfaz de usuario interactivos y de contenido de la aplicación.

    Un nombre accesible es una cadena de texto descriptiva y corta que un lector de pantalla usa para anunciar un elemento de la interfaz de usuario. Algunos de los elementos de la interfaz de usuario, como [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) y [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683), promueven su contenido de texto como el nombre accesible predeterminado. Consulta [Información básica de accesibilidad](basic-accessibility-information.md#name_from_inner_text).

    Debes establecer el nombre accesible de forma explícita para imágenes y otros controles que no promueven contenido de texto interno como nombre accesible implícito. Debes usar etiquetas para los elementos de formulario, de modo que el texto de la etiqueta se pueda usar como destino [**LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) en el modelo de Automatización de la interfaz de usuario de Microsoft para la correspondencia de etiquetas y entradas. Si quieres proporcionar a los usuarios más información sobre la interfaz de usuario que la que suele incluir el nombre accesible, las descripciones accesibles e información sobre herramientas ayudan a los usuarios a comprender la interfaz de usuario.

    Para obtener más información, consulta [Nombre accesible](basic-accessibility-information.md#accessible_name) y [Descripción accesible](basic-accessibility-information.md).

2.  Implementar accesibilidad de teclado:


    -   Test the default tab index order for a UI. Adjust the tab index order if necessary, which may require enabling or disabling certain controls, or changing the default values of [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) on some of the UI elements.
    -   Use controls that support arrow-key navigation for composite elements. For default controls, the arrow-key navigation is typically already implemented.
    -   Use controls that support keyboard activation. For default controls, particularly those that support the UI Automation [**Invoke**](https://msdn.microsoft.com/library/windows/apps/BR242582) pattern, keyboard activation is typically available; check the documentation for that control.
    -   Set access keys or implement accelerator keys for specific parts of the UI that support interaction.
    -   For any custom controls that you use in your UI, verify that you have implemented these controls with correct [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) support for activation, and defined overrides for key handling as needed to support activation, traversal and access or accelerator keys.

    For more info, see [Keyboard interactions](https://msdn.microsoft.com/library/windows/apps/Mt185607).

3.  Comprueba visualmente tu interfaz de usuario para asegurarte de que el contraste de texto sea suficiente, que los elementos se presenten correctamente en los temas de contraste alto y que los colores se usen correctamente.

    -   Usa las opciones de pantalla del sistema para ajustar el valor de puntos por pulgada (ppp) de la pantalla y asegúrate de que la interfaz de usuario de la aplicación se escala correctamente cuando cambie el valor de ppp. (Algunos usuarios cambian los valores de ppp como una opción de accesibilidad que encontrarás en **Accesibilidad**).
    -   Usa una herramienta de análisis de color para comprobar que la relación de contraste del texto visual sea de al menos 4.5:1.
    -   Cambia a un tema de contraste alto y comprueba que la interfaz de usuario de la aplicación pueda leerse y usarse.
    -   Asegúrate de que tu interfaz de usuario no use el color como el único modo de transmitir información.

    Para más información, consulta [Temas de contraste alto](high-contrast-themes.md) y [Requisitos de texto accesible](accessible-text-requirements.md).

4.  Ejecuta herramientas de accesibilidad, soluciona problemas notificados y comprueba la experiencia de lectura de pantalla.

    Usa herramientas como [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) para comprobar el acceso mediante programación, ejecuta herramientas como [**AccChecker**](https://msdn.microsoft.com/library/windows/desktop/Hh920985) para descubrir errores comunes y comprueba la experiencia de lectura en pantalla con la característica Narrador.

    Para más información, consulta [Pruebas de accesibilidad](accessibility-testing.md).

5.  Asegúrate de que la configuración del manifiesto de la aplicación siga las instrucciones de accesibilidad.

6.  Declara como accesible a tu aplicación en la Tienda Windows.

    Si implementaste la compatibilidad de accesibilidad de línea base, declarar tu aplicación como accesible en la Tienda Windows puede ayudarte a llegar a más clientes y obtener más cantidad de buenas clasificaciones.

    Para obtener más información, consulta [Accesibilidad en la Tienda](accessibility-in-the-store.md).

<span id="related_topics"></span>Temas relacionados
-----------------------------------------------

* [Accesibilidad](accessibility.md)
* [Diseño de accesibilidad](https://msdn.microsoft.com/library/windows/apps/Hh700407)
* [Procedimientos que deben evitarse](practices-to-avoid.md)
 

 





<!--HONumber=Mar16_HO3-->


