---
description: Proporciona una lista de comprobación para ayudarle a asegurarse de que la aplicación de Windows esté accesible.
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
title: Lista de comprobación de accesibilidad
label: Accessibility checklist
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fd16c93b3914987741a486e4f40d4b60e0b274ee
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032718"
---
# <a name="accessibility-checklist"></a>Lista de comprobación de accesibilidad

Proporciona una lista de comprobación para ayudarle a asegurarse de que la aplicación de Windows esté accesible.

Aquí proporcionamos una lista de comprobación que te ayudará a garantizar que la aplicación sea accesible.

1. Establece el nombre accesible (obligatorio) y la descripción accesible (opcional) para los elementos de la interfaz de usuario interactivos y de contenido de la aplicación.

    Un nombre accesible es una cadena de texto descriptiva y corta que un lector de pantalla usa para anunciar un elemento de la interfaz de usuario. Algunos elementos de la interfaz de usuario como [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) y [**TextBox**](/uwp/api/Windows.UI.Xaml.Controls.TextBox) promueven su contenido de texto como el nombre accesible predeterminado; vea [información básica de accesibilidad](basic-accessibility-information.md#name_from_inner_text).

    Debes establecer el nombre accesible de forma explícita para imágenes y otros controles que no promueven contenido de texto interno como nombre accesible implícito. Debes usar etiquetas para los elementos de formulario, de modo que el texto de la etiqueta se pueda usar como destino [**LabeledBy**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)) en el modelo de Automatización de la interfaz de usuario de Microsoft para la correspondencia de etiquetas y entradas. Si quieres proporcionar a los usuarios más información sobre la interfaz de usuario que la que suele incluir el nombre accesible, las descripciones accesibles e información sobre herramientas ayudan a los usuarios a comprender la interfaz de usuario.

    Para obtener más información, consulta [Nombre accesible](basic-accessibility-information.md#accessible_name) y [Descripción accesible](basic-accessibility-information.md).

2. Implementar accesibilidad de teclado:

    * Prueba el orden de los índices de tabulación para una interfaz de usuario. Si es necesario, ajusta el orden de los índices de texto, lo que puede requerir la activación o desactivación de determinados controles o la modificación de los valores predeterminados de [**TabIndex**](/uwp/api/windows.ui.xaml.controls.control.tabindex) en algunos de los elementos de la interfaz de usuario.
    * Usa controles que admitan la navegación por teclas de dirección para elementos compuestos. En los controles predeterminados, la navegación por teclas de dirección suele venir implementada.
    * Usa controles que admitan la activación del teclado. En los controles predeterminados, en especial aquellos que admiten el modelo [**Invoke**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) de automatización de la interfaz de usuario, la activación del teclado suele estar disponible; consulta la documentación de ese control.
    * Define teclas de acceso o teclas de aceleración para partes específicas de la interfaz de usuario que admitan interacción.
    * Para cualquier control personalizado que uses en la interfaz de usuario, comprueba que hayas implementado estos controles con la compatibilidad adecuada de [**AutomationPeer**](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) para la activación y que hayas definido invalidaciones para controlar las teclas según sea necesario a fin de admitir teclas de activación, recorrido y acceso o aceleración.

    Para obtener más información, consulte [interacciones de teclado](../input/keyboard-interactions.md).

3. Asegurarse de que el texto es de tamaño legible

    * Windows incluye varias herramientas de accesibilidad y configuraciones en las que los usuarios pueden aprovechar las ventajas y adaptarse a sus propias necesidades y preferencias para leer texto. Se incluyen los siguientes:
        * La herramienta ampliador, que amplía un área seleccionada de la interfaz de usuario. Debe asegurarse de que el diseño de texto de la aplicación no dificulta el uso de la lupa para la lectura.
        * Configuración de escala y resolución global en **configuración->>de pantalla->de la escala y el diseño** . Exactamente las opciones de ajuste de tamaño disponibles pueden variar, ya que depende de las capacidades del dispositivo de pantalla.
        * Configuración de tamaño de texto en **Configuración: >la facilidad de acceso >pantalla** . Ajuste la configuración de **hacer que el texto sea más grande** para especificar solo el tamaño del texto en los controles auxiliares en todas las aplicaciones y pantallas (todos los controles de texto de UWP admiten la experiencia de escalado de texto sin ninguna personalización o plantilla).
        > [!NOTE]
        > La configuración **hacer todo es mayor permite que** un usuario especifique su tamaño preferido para el texto y las aplicaciones en general solo en su pantalla principal.

4. Comprueba visualmente tu interfaz de usuario para asegurarte de que el contraste de texto sea suficiente, que los elementos se representen correctamente en los temas de contraste alto y que los colores se usen correctamente.

    * Usa una herramienta de análisis de color para comprobar que la relación de contraste del texto visual sea de al menos 4.5:1.
    * Cambia a un tema de contraste alto y comprueba que la interfaz de usuario de la aplicación pueda leerse y usarse.
    * Asegúrate de que tu interfaz de usuario no use el color como el único modo de transmitir información.

    Para obtener más información, vea [temas de contraste alto](high-contrast-themes.md) y [requisitos de texto accesible](accessible-text-requirements.md).

5. Ejecuta herramientas de accesibilidad, soluciona problemas notificados y comprueba la experiencia de lectura de pantalla.

    Usa herramientas como [**Inspect**](/windows/desktop/WinAuto/inspect-objects) para comprobar el acceso mediante programación, ejecuta herramientas como [**AccChecker**](/windows/desktop/WinAuto/ui-accessibility-checker) para descubrir errores comunes y comprueba la experiencia de lectura en pantalla con la característica Narrador.

    Para obtener más información, vea [pruebas de accesibilidad](accessibility-testing.md).

6. Asegúrate de que la configuración del manifiesto de la aplicación siga las instrucciones de accesibilidad.

7. Declare la aplicación como accesible en el Microsoft Store.

    Si ha implementado la compatibilidad con la accesibilidad de línea de base, la declaración de la aplicación como accesible en el Microsoft Store puede ayudar a llegar a más clientes y obtener calificaciones correctas adicionales.

    Para obtener más información, consulte [accesibilidad en el almacén](accessibility-in-the-store.md).

## <a name="related-topics"></a>Temas relacionados  

* [Requisitos de texto accesible](accessible-text-requirements.md)
* [Ajuste de escala de texto](../input/text-scaling.md)
* [Accesibilidad](accessibility.md)
* [Diseño de accesibilidad](./accessibility-overview.md)
* [Prácticas que se deben evitar](practices-to-avoid.md)
