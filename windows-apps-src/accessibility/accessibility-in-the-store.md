---
author: Xansky
Description: "Describe los requisitos para declarar tu aplicación para Plataforma universal de Windows (UWP) como accesible en la Tienda Windows."
ms.assetid: 59FA3B87-75A6-4B30-BA7C-A0E769D68050
title: Accesibilidad en la Tienda
label: Accessibility in the Store
template: detail.hbs
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: 46dfe4fba383861c704b2ba9070bdd8102b10562

---

# Accesibilidad en la Tienda  



Describe los requisitos para declarar tu aplicación para Plataforma universal de Windows (UWP) como accesible en la Tienda Windows.

Al enviar tu aplicación a la Tienda Windows para su certificación, puedes declarar que es accesible. Con ello, haces que sea más fácil que los usuarios que estén interesados en aplicaciones accesibles la descubran, como aquellos que tienen discapacidades visuales. Los usuarios descubren las aplicaciones accesibles mediante el uso del filtro **Accesible** mientras buscan en la Tienda Windows. Al declarar tu aplicación como accesible también agregas la etiqueta **Accesible** a su descripción.

Al declarar tu aplicación como accesible, estás comunicando que cumple la [información de accesibilidad básica](basic-accessibility-information.md) que necesitan los usuarios en escenarios principales en alguno o varios de los siguientes elementos:

* El teclado
* Un tema de contraste alto
* Una configuración variable de puntos por pulgada (ppp)
* Tecnología de asistencia común como las funciones de accesibilidad de Windows, que incluyen el narrador, la lupa y el teclado en pantalla.

Debes declarar tu aplicación como accesible si la has compilado para ello y has realizado las pruebas de accesibilidad. Esto implica que has hecho lo siguiente:

* Configurar toda la información de accesibilidad relevante para los elementos de la interfaz de usuario, entre ellos, el nombre, el rol, el valor, etc.
* Implementar una accesibilidad de teclado completa, que permita al usuario realizar lo siguiente:
    * alcanzar los principales escenarios de aplicaciones usando únicamente el teclado;
    * saltar entre elementos de la interfaz de usuario en un orden lógico;
    * navegar entre los elementos de la interfaz de usuario en un control con las teclas de dirección;
    * usar métodos abreviados de teclado para alcanzar una funcionalidad de la aplicación principal;
    * usar los gestos táctiles del narrador para la equivalencia con el tabulador y las flechas para dispositivos sin teclado.
* Garantizar que la interfaz de usuario de tu aplicación es accesible visualmente, es decir, que tiene una relación de contraste de texto mínima de 4.5:1, que no depende solamente del color para transmitir la información, etc.
* Usar herramientas de prueba de accesibilidad como [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) y [**UIAVerify**](https://msdn.microsoft.com/library/windows/desktop/Hh920986) para comprobar la implementación que has realizado de la accesibilidad y resolver todos los errores de prioridad 1 notificados por dichas herramientas.
* Comprobar los principales escenarios de la aplicación de principio a fin usando el narrador, la lupa, el teclado en pantalla, un tema de contraste alto y con la opción de ppp ajustada.

Consulta el tema [Lista de comprobación para accesibilidad](accessibility-checklist.md) para obtener un revisión de estos procedimientos y vínculos a recursos que te ayudarán a llevarlos a cabo.

<span id="related_topics"/>
## Temas relacionados    
* [Accesibilidad](accessibility.md)



<!--HONumber=Jun16_HO4-->


