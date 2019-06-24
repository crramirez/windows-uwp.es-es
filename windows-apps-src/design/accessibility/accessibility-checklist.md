---
Description: Proporciona una lista de comprobación que te ayudará a garantizar que tu aplicación para Plataforma universal de Windows (UWP) sea accesible.
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
title: Lista de comprobación de accesibilidad
label: Accessibility checklist
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9b789102b90e63e5456c030f6eb90df372354d7a
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67319001"
---
# <a name="accessibility-checklist"></a>Lista de comprobación de accesibilidad

Proporciona una lista de comprobación que te ayudará a garantizar que tu aplicación para Plataforma universal de Windows (UWP) sea accesible.

Aquí proporcionamos una lista de comprobación que te ayudará a garantizar que la aplicación sea accesible.

1. Establece el nombre accesible (obligatorio) y la descripción accesible (opcional) para los elementos de la interfaz de usuario interactivos y de contenido de la aplicación.

    Un nombre accesible es una cadena de texto descriptiva y corta que un lector de pantalla usa para anunciar un elemento de la interfaz de usuario. Algunos de los elementos de la interfaz de usuario, como [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) y [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox), promueven su contenido de texto como el nombre accesible predeterminado. Consulta [Información básica de accesibilidad](basic-accessibility-information.md#name_from_inner_text).

    Debes establecer el nombre accesible de forma explícita para imágenes y otros controles que no promueven contenido de texto interno como nombre accesible implícito. Debes usar etiquetas para los elementos de formulario, de modo que el texto de la etiqueta se pueda usar como destino [**LabeledBy**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)) en el modelo de Automatización de la interfaz de usuario de Microsoft para la correspondencia de etiquetas y entradas. Si quieres proporcionar a los usuarios más información sobre la interfaz de usuario que la que suele incluir el nombre accesible, las descripciones accesibles e información sobre herramientas ayudan a los usuarios a comprender la interfaz de usuario.

    Para obtener más información, consulta [Nombre accesible](basic-accessibility-information.md#accessible_name) y [Descripción accesible](basic-accessibility-information.md).

2. Implementar accesibilidad de teclado:

    * Prueba el orden de los índices de tabulación para una interfaz de usuario. Si es necesario, ajusta el orden de los índices de texto, lo que puede requerir la activación o desactivación de determinados controles o la modificación de los valores predeterminados de [**TabIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.tabindex) en algunos de los elementos de la interfaz de usuario.
    * Usa controles que admitan la navegación por teclas de dirección para elementos compuestos. En los controles predeterminados, la navegación por teclas de dirección suele venir implementada.
    * Usa controles que admitan la activación del teclado. En los controles predeterminados, en especial aquellos que admiten el modelo [**Invoke**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) de automatización de la interfaz de usuario, la activación del teclado suele estar disponible; consulta la documentación de ese control.
    * Define teclas de acceso o teclas de aceleración para partes específicas de la interfaz de usuario que admitan interacción.
    * Para cualquier control personalizado que uses en la interfaz de usuario, comprueba que hayas implementado estos controles con la compatibilidad adecuada de [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) para la activación y que hayas definido invalidaciones para controlar las teclas según sea necesario a fin de admitir teclas de activación, recorrido y acceso o aceleración.

    Si quieres obtener más información, consulta las [Interacciones de teclado](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions).

3. Asegúrese de texto es un tamaño de lectura

    * Windows incluyen varias herramientas de accesibilidad y la configuración que los usuarios pueden aprovechar y ajustarse a sus propias necesidades y preferencias para la lectura de texto. Entre ellos se incluyen los siguientes:
        * La herramienta Ampliador, que aumenta el tamaño de un área seleccionada de la interfaz de usuario. Debe asegurarse el diseño del texto en la aplicación no hacen que sea difícil utilizar el Ampliador para leerlo.
        * Configuración global de escala y la resolución en **configuración -> sistema -> Mostrar -> escala y el diseño**. Exactamente qué opciones de ajuste de tamaño están disponibles pueden variar en función depende de las capacidades de la pantalla.
        * Configuración de tamaño de texto en **facilidad de acceso de -> Configuración -> Mostrar**. Ajustar el **agrandar el texto** configuración para especificar solo el tamaño del texto en la compatibilidad con controles en todas las aplicaciones y las pantallas (todos los controles de texto UWP admiten el escalado experiencia sin ninguna personalización o la creación de plantillas de texto).
        > [!NOTE]
        > El **asegurarse todo mayor** configuración permite a los usuarios especificar su tamaño preferido de texto y las aplicaciones en general en su pantalla principal solo.

4. Comprueba visualmente tu interfaz de usuario para asegurarte de que el contraste de texto sea suficiente, que los elementos se representen correctamente en los temas de contraste alto y que los colores se usen correctamente.

    * Usa una herramienta de análisis de color para comprobar que la relación de contraste del texto visual sea de al menos 4.5:1.
    * Cambia a un tema de contraste alto y comprueba que la interfaz de usuario de la aplicación pueda leerse y usarse.
    * Asegúrate de que tu interfaz de usuario no use el color como el único modo de transmitir información.

    Para más información, consulta [Temas de contraste alto](high-contrast-themes.md) y [Requisitos de texto accesible](accessible-text-requirements.md).

5. Ejecuta herramientas de accesibilidad, soluciona problemas notificados y comprueba la experiencia de lectura de pantalla.

    Usa herramientas como [**Inspect**](https://docs.microsoft.com/windows/desktop/WinAuto/inspect-objects) para comprobar el acceso mediante programación, ejecuta herramientas como [**AccChecker**](https://docs.microsoft.com/windows/desktop/WinAuto/ui-accessibility-checker) para descubrir errores comunes y comprueba la experiencia de lectura en pantalla con la característica Narrador.

    Para más información, consulta [Pruebas de accesibilidad](accessibility-testing.md).

6. Asegúrate de que la configuración del manifiesto de la aplicación siga las instrucciones de accesibilidad.

7. Declara que tu aplicación es accesible en Microsoft Store.

    Si implementaste la compatibilidad de accesibilidad de línea base, declarar que aplicación es accesible en Microsoft Store puede ayudarte a llegar a más clientes y obtener más cantidad de buenas clasificaciones.

    Para obtener más información, consulta [Accesibilidad en la Tienda](accessibility-in-the-store.md).

## <a name="related-topics"></a>Temas relacionados  

* [Requisitos de texto accesible](accessible-text-requirements.md)
* [Texto escalado](../input/text-scaling.md)
* [Accesibilidad](accessibility.md)
* [Diseño para mejorar la accesibilidad](https://docs.microsoft.com/windows/uwp/accessibility/accessibility-overview)
* [Prácticas que se deben evitar](practices-to-avoid.md)
