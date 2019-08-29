---
description: Aprende a lograr que tu aplicación sea inclusiva y accesible para personas de todo el mundo.
keywords: accesibilidad de las aplicaciones para UWP, globalización, diseñar aplicaciones inclusivas, requisitos de aplicaciones de accesibilidad
title: 'Facilidad de uso para aplicaciones para UWP: desarrollo de aplicaciones de Windows'
layout: LandingPage
template: detail.hbs
ms.date: 10/18/2017
ms.topic: article
ms.assetid: e6bb3464-dd8e-402c-9c56-dd9e51002a49
ms.localizationpriority: medium
ms.openlocfilehash: a6912932b7ad71fd3d04c038eab7e0aa4dd6cb11
ms.sourcegitcommit: 2fa2d2236870eaabc95941a95fd4e358d3668c0c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70076387"
---
# <a name="usability-for-uwp-apps"></a>Facilidad de uso para aplicaciones para UWP

Los pequeños toques, una especial atención a los detalles, pueden transformar una buena experiencia del usuario en una experiencia del usuario realmente inclusiva que satisfaga las necesidades de usuarios de todo el mundo.

Las instrucciones de diseño y codificación de esta sección pueden hacer que tu aplicación para UWP resulte más inclusiva al agregar características de accesibilidad, habilitar la globalización y la localización, permitir a los usuarios personalizar su experiencia y proporcionarles ayuda cuando lo necesiten.

## <a name="accessibility"></a>Accesibilidad

La accesibilidad consiste en hacer que el uso de una aplicación resulte más fácil para personas que presentan limitaciones que les impiden el uso de interfaces de usuario convencionales. En algunos casos, los requisitos de accesibilidad están impuestos por ley. Sin embargo, resulta buena idea abordar los problemas de accesibilidad independientemente de los requisitos legales, de modo que tus aplicaciones lleguen al mayor público posible.

[Portal de accesibilidad](../accessibility/accessibility.md)

<!--
<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-overview.md">Accessibility overview</a></b> <br/> This article is an overview of the concepts and technologies related to accessibility scenarios for UWP apps.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/designing-inclusive-software.md">Designing inclusive software</a></b><br/>Learn about evolving inclusive design with Universal Windows Platform (UWP) apps for Windows 10.  Design and build inclusive software with accessibility in mind.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/developing-inclusive-windows-apps.md">Developing inclusive Windows apps</a></b><br/> This article is a roadmap for developing accessible UWP apps.</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-testing.md">Accessibility testing</a> </b><br/>Testing procedures to follow to ensure that your UWP app is accessible.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-in-the-store.md">Accessibility in the Store</a></b><br/>Describes the requirements for declaring your UWP app as accessible in the Microsoft Store.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-checklist.md">Accessibility checklist</a></b><br/>Provides a checklist to help you ensure that your UWP app is accessible.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>        
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/basic-accessibility-information.md">Expose basic accessibility information</a></b><br/>Basic accessibility info is often categorized into name, role, and value. This topic describes code to help your app expose the basic information that assistive technologies need.</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/keyboard-accessibility.md">Keyboard accessibility</a></b><br/>If your app does not provide good keyboard access, users who are blind or have mobility issues can have difficulty using your app or may not be able to use it at all.</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/high-contrast-themes.md">High-contrast themes</a></b><br/>Describes the steps needed to ensure your UWP app is usable when a high-contrast theme is active. </p>
                    </div>
                </div>
            </div>
        </div>
    </li>         
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessible-text-requirements.md">Accessible text requirements</a></b><br/>This topic describes best practices for accessibility of text in an app, by assuring that colors and backgrounds satisfy the necessary contrast ratio. This topic also discusses the Microsoft UI Automation roles that text elements in a UWP app can have, and best practices for text in graphics.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/practices-to-avoid.md">Accessibility practices to avoid</a></b><br/>Lists the practices to avoid if you want to create an accessible UWP app.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/custom-automation-peers.md">Custom automation peers</a></b><br/>Describes the concept of automation peers for UI Automation, and how you can provide automation support for your own custom UI class.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/control-patterns-and-interfaces.md">Control patterns and interfaces</a></b><br/>Lists the Microsoft UI Automation control patterns, the classes that clients use to access them, and the interfaces providers use to implement them.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
</ul>
-->

## <a name="app-settings"></a>Configuración de aplicaciones

Configuración de aplicaciones permite al usuario personalizar la aplicación con el fin de optimizarla para necesidades y preferencias individuales. El hecho de proporcionar la configuración adecuada y almacenarla correctamente puede mejorar una experiencia del usuario ya excelente de por sí.

<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../app-settings/guidelines-for-app-settings.md">Instrucciones</a></b><br/>Procedimientos recomendados para crear y mostrar la configuración de la aplicación.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../app-settings/store-and-retrieve-app-data.md">Almacenar y recuperar datos de la aplicación</a></b><br/>Cómo almacenar y recuperar datos de aplicaciones locales, móviles y temporales.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul>

## <a name="globalization-and-localization"></a>Globalización y localización

Windows se usa en todo el mundo por personas de diferentes culturas, regiones e idiomas. Los usuarios hablan varios idiomas diferentes y en una diversidad de países y regiones diferentes. Algunos usuarios hablan más de un idioma. Por lo tanto, tu aplicación se ejecuta en configuraciones que implican muchas permutaciones en la configuración del sistema para el idioma, la región y la referencia cultural. Diseña tu aplicación para que sea fácilmente adaptable mediante la *globalización* y la *localización* y así aumentar su mercado potencial.

<a href="../globalizing/globalizing-portal.md">Portal de globalización y localización</a>

## <a name="in-app-help"></a>Ayuda desde la aplicación
Por muy bien que hayas diseñado una aplicación, algunos usuarios necesitarán un poco más de ayuda.

<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/guidelines-for-app-help.md">Directrices para la ayuda de la aplicación</a></b><br/>Las aplicaciones pueden ser complejas, por lo que proporcionar una ayuda eficaz a los usuarios puede mejorar enormemente su experiencia.
</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/instructional-ui.md">Interfaz de usuario informativa</a></b><br/>Algunas veces, puede resultar útil informar al usuario acerca de las funciones de la aplicación que quizá no sean obvias para ellos, tales como interacciones táctiles específicas. En estos casos, es necesario presentar instrucciones al usuario a través de la interfaz de usuario para que pueda descubrir y usar esas características que posiblemente no conozcan.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/in-app-help.md">Ayuda desde la aplicación</a></b><br/>La mayoría de las veces, es mejor que la ayuda se muestre dentro la aplicación y que aparezca cuando el usuario solicite verla. Ten en cuenta las siguientes directrices a la hora de crear la ayuda en la aplicación.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/external-help.md">Ayuda externa</a></b><br/>La mayoría de las veces, es mejor que la ayuda se muestre dentro la aplicación y que aparezca cuando el usuario solicite verla. Ten en cuenta las siguientes directrices a la hora de crear la ayuda en la aplicación.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>        
</ul>

