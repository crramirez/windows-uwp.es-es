---
author: Xansky
Description: "Proporciona una lista de comprobación que te ayudará a garantizar que tu aplicación para la Plataforma universal de Windows (UWP) sea accesible."
ms.assetid: BB8399E2-7013-4F77-AF2C-C1A0E5412856
title: "Lista de comprobación de accesibilidad"
label: Accessibility checklist
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, UWP
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: c5db2b89c52f77cb92da06c246e07a215fd2d02d
ms.lasthandoff: 02/07/2017

---

# <a name="accessibility-checklist"></a>Lista de comprobación de accesibilidad



Proporciona una lista de comprobación que te ayudará a garantizar que tu aplicación para Plataforma universal de Windows (UWP) sea accesible.

Aquí proporcionamos una lista de comprobación que te ayudará a garantizar que la aplicación sea accesible.

1.  Establece el nombre accesible (obligatorio) y la descripción accesible (opcional) para los elementos de la interfaz de usuario interactivos y de contenido de la aplicación.

    Un nombre accesible es una cadena de texto descriptiva y corta que un lector de pantalla usa para anunciar un elemento de la interfaz de usuario. Algunos de los elementos de la interfaz de usuario, como [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) y [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683), promueven su contenido de texto como el nombre accesible predeterminado. Consulta [Información básica de accesibilidad](basic-accessibility-information.md#name_from_inner_text).

    Debes establecer el nombre accesible de forma explícita para imágenes y otros controles que no promueven contenido de texto interno como nombre accesible implícito. Debes usar etiquetas para los elementos de formulario, de modo que el texto de la etiqueta se pueda usar como destino [**LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) en el modelo de Automatización de la interfaz de usuario de Microsoft para la correspondencia de etiquetas y entradas. Si quieres proporcionar a los usuarios más información sobre la interfaz de usuario que la que suele incluir el nombre accesible, las descripciones accesibles e información sobre herramientas ayudan a los usuarios a comprender la interfaz de usuario.

    Para obtener más información, consulta [Nombre accesible](basic-accessibility-information.md#accessible_name) y [Descripción accesible](basic-accessibility-information.md).

2.  Implementar accesibilidad de teclado:

    * Prueba el orden de los índices de tabulación para una interfaz de usuario. Si es necesario, ajusta el orden de los índices de texto, lo que puede requerir la activación o desactivación de determinados controles o la modificación de los valores predeterminados de [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) en algunos de los elementos de la interfaz de usuario.
    * Usa controles que admitan la navegación por teclas de dirección para elementos compuestos. En los controles predeterminados, la navegación por teclas de dirección suele venir implementada.
    * Usa controles que admitan la activación del teclado. En los controles predeterminados, en especial aquellos que admiten el modelo [**Invoke**](https://msdn.microsoft.com/library/windows/apps/BR242582) de automatización de la interfaz de usuario, la activación del teclado suele estar disponible; consulta la documentación de ese control.
    * Define teclas de acceso o teclas de aceleración para partes específicas de la interfaz de usuario que admitan interacción.
    * Para cualquier control personalizado que uses en la interfaz de usuario, comprueba que hayas implementado estos controles con la compatibilidad adecuada de [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) para la activación y que hayas definido invalidaciones para controlar las teclas según sea necesario a fin de admitir teclas de activación, recorrido y acceso o aceleración.

    Si quieres obtener más información, consulta las [Interacciones de teclado](https://msdn.microsoft.com/library/windows/apps/Mt185607).

3.  Comprueba visualmente tu interfaz de usuario para asegurarte de que el contraste de texto sea suficiente, que los elementos se representen correctamente en los temas de contraste alto y que los colores se usen correctamente.

    * Usa las opciones de pantalla del sistema para ajustar el valor de puntos por pulgada (ppp) de la pantalla y asegúrate de que la interfaz de usuario de la aplicación se escala correctamente cuando cambie el valor de ppp. (Algunos usuarios cambian los valores de ppp como una opción de accesibilidad que encontrarás en **Accesibilidad**).
    * Usa una herramienta de análisis de color para comprobar que la relación de contraste del texto visual sea de al menos 4.5:1.
    * Cambia a un tema de contraste alto y comprueba que la interfaz de usuario de la aplicación pueda leerse y usarse.
    * Asegúrate de que tu interfaz de usuario no use el color como el único modo de transmitir información.

    Para más información, consulta [Temas de contraste alto](high-contrast-themes.md) y [Requisitos de texto accesible](accessible-text-requirements.md).

4.  Ejecuta herramientas de accesibilidad, soluciona problemas notificados y comprueba la experiencia de lectura de pantalla.

    Usa herramientas como [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) para comprobar el acceso mediante programación, ejecuta herramientas como [**AccChecker**](https://msdn.microsoft.com/library/windows/desktop/Hh920985) para descubrir errores comunes y comprueba la experiencia de lectura en pantalla con la característica Narrador.

    Para más información, consulta [Pruebas de accesibilidad](accessibility-testing.md).

5.  Asegúrate de que la configuración del manifiesto de la aplicación siga las instrucciones de accesibilidad.

6.  Declara como accesible a tu aplicación en la Tienda Windows.

    Si implementaste la compatibilidad de accesibilidad de línea base, declarar tu aplicación como accesible en la Tienda Windows puede ayudarte a llegar a más clientes y obtener más cantidad de buenas clasificaciones.

    Para obtener más información, consulta [Accesibilidad en la Tienda](accessibility-in-the-store.md).

<span id="related_topics"/>
## <a name="related-topics"></a>Temas relacionados  
* [Accesibilidad](accessibility.md)
* [Diseño de accesibilidad](https://msdn.microsoft.com/library/windows/apps/Hh700407)
* [Procedimientos que deben evitarse](practices-to-avoid.md) 

