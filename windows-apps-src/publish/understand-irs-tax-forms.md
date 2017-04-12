---
author: jnHs
Description: "Obtén información sobre los formularios fiscales emitidos por Microsoft, incluido quién los recibirá y cuándo estarán disponibles."
title: "Descripción de los formularios fiscales del IRS emitidos por Microsoft"
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 1e475b96-f953-457c-864f-b6f4cb4c309f
ms.openlocfilehash: 068a940a54048b10e8f66bd3267b3a22c42beb50
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="understand-irs-tax-forms-issued-by-microsoft"></a>Descripción de los formularios fiscales del IRS emitidos por Microsoft

Según tu ubicación y el importe de ventas o pagos que recibes, es posible que recibas uno o varios formularios fiscales de Microsoft cada año. Microsoft debe emitir estos formularios y presentarlos ante el Servicio de Impuestos Internos (IRS).

A continuación, explicaremos más sobre estos formularios, incluidos sus destinatarios y su disponibilidad.

## <a name="types-of-tax-forms"></a>Tipos de formularios fiscales

| Formulario fiscal del IRS | Descripción | Disponibilidad |
|--------------|-------------|--------------|
|1099-MISC, 1099-K | Se relacionan con la actividad de venta o los pagos realizados a raíz de la participación en los catálogos de soluciones de Microsoft. | Los formularios impresos se sellarán el **31 de enero** o antes y las copias en formato PDF estarán disponibles en el Centro de desarrollo (**Panel > Configuración de la cuenta > Perfil fiscal**) al mismo tiempo. |
|1042-S | Se relaciona con los pagos que te efectuamos que están sujetos a la retención de impuestos de Estados Unidos. | Los formularios impresos se sellarán el **15 de marzo** o antes y las copias en formato PDF estarán disponibles en el Centro de desarrollo (**Panel > Configuración de la cuenta > Perfil fiscal**) al mismo tiempo. |

> **Nota**: La dirección que aparece en los formularios fiscales del IRS proviene de la dirección que consta en el [Perfil fiscal](setting-up-your-payout-account-and-tax-forms.md#tax-forms). Si ha cambiado tu dirección, asegúrate de actualizarla en tu **Perfil fiscal**.

## <a name="for-developers-located-in-the-united-states"></a>Para los desarrolladores que se encuentran en los Estados Unidos

<table>
  <tr>
     <th>Si soy un desarrollador de Estados Unidos que vende aplicaciones de pago y... </th>
     <th> Debo recibir este formulario</th>
  </tr>
  <tr> 
     <td valign="top">Tuve **ventas superiores a 200 aplicaciones** con un importe total de compra de estas ventas **superior a 20000USD** en el año fiscal aplicable (**sin** contar las ventas realizadas en Brasil y China a través de la Tienda de Windows10).</td>
    <td valign="top">**1099-K**:<br>
Contribuyente: Microsoft Corporation<br>
CIF/NIF: \*\*\*\*\*4442<br>
<br>
**Importante**: El formulario 1099-K contiene importes de **compra bruta**, no los pagos que se te efectuaron.</td>
  </tr>
  <tr> 
     <td valign="top">Recibí **al menos 10USD por pagos** de ventas de aplicaciones realizadas en Brasil y China a través de la Tienda de Windows10.<br>
<br>
**O bien,**<br>
<br>
Recibí al menos 600USD por pagos no relacionados con las ventas de aplicaciones de Microsoft en el año fiscal aplicable (por ejemplo, pagos de incentivos o pagos de un concurso o una promoción).</td>
    <td valign="top">**1099-MISC**:<br>
Pagador: Microsoft Corporation<br>
CIF/NIF: \*\*\*\*\*4442<br>
<br>
**Importante**: Determinadas entidades comerciales no recibirán formularios 1099-MISC, independientemente de los importes de pago recibidos de Microsoft.  Para obtener más información, consulta a tu profesional en materia fiscal.</td>
  </tr>
  <tr>
    <td valign="top">No se aplica ninguna de las anteriores.</td>
    <td valign="top">Ninguno</td>
  </tr>
  <tr>
    <td valign="top">&nbsp;</td>
    <td valign="top">&nbsp;</td>
  </tr>
  <tr>
     <th>Si soy un desarrollador de Estados Unidos que vende aplicaciones de pago y... </th>
     <th> Debo recibir este formulario</th>
  </tr>
  <tr> 
     <td valign="top">Recibí **al menos 600USD en pagos** provenientes de anuncios en aplicaciones (Microsoft Advertising) en el año fiscal aplicable.</td>
    <td valign="top">**1099-MISC**:<br>
Pagador: Microsoft Online, Inc.<br>
CIF/NIF: \*\*\*\*\*0505<br>
<br>
**Importante**: Determinadas entidades comerciales no recibirán formularios 1099-MISC, independientemente de los importes de pago recibidos de Microsoft.  Para obtener más información, consulta a tu profesional en materia fiscal.  </td>
  </tr>
  <tr> 
     <td valign="top">Recibí **menos de 600USD en pagos** provenientes de anuncios en aplicaciones (Microsoft Advertising) en el año fiscal aplicable.</td>
     <td valign="top">Ninguno</td>
  </tr>
</table>


## <a name="for-developers-located-outside-of-the-united-states"></a>Para los desarrolladores que se encuentran fuera de Estados Unidos

<table>
  <tr>
    <td valign="top">**Recibí un formulario 1042-S de Microsoft. ¿Para qué es?**</td>
    <td valign="top">Microsoft te ha proporcionado uno o varios formularios 1042-S porque se te han pagado ingresos que deben declararse a las autoridades fiscales de los Estados Unidos y estaban sujetos a retención de impuestos.  El formulario 1042-S se usa para este requisito de declaración fiscal.</td>
  </tr>
  <tr>
    <td valign="top">**¿Qué debo hacer con los formularios?**</td>
    <td valign="top">Por lo general, no se requiere ninguna acción específica de tu parte. El formulario 1042-S quizá te resulte útil si quieres solicitar a las autoridades tributarias locales cualquier tipo de crédito fiscal.  Debes consultar a tus asesores fiscales para obtener más información sobre este tema.</td>
  </tr>
  <tr>
    <td valign="top">**¿Por qué se retuvieron impuestos en mis pagos si he completado un formulario W8?**</td>
    <td valign="top">La retención de impuestos se realiza si:<br>
     1. No completaste correctamente la sección de tratado fiscal del formulario W8 o<br>
     2. Resides en un país que no tiene un tratado fiscal con los Estados Unidos.

     You can visit Dev Center at any time to submit an updated W8 form.<br>
     <br>
     **Note:** Not all income is subject to tax withholding.</td>
  </tr>
  <tr>
    <td valign="top">**Envié un formulario W8 actualizado con información válida sobre el tratado. ¿Puede Microsoft reembolsarme los impuestos retenidos?**</td>
    <td valign="top">Una vez que se han retenido los impuestos, no pueden reembolsarse. Debes consultar con tus asesores fiscales si puedes solicitar un crédito local por estos impuestos o si puedes solicitar un reembolso al IRS.</td>
  </tr>
  <tr>
    <td valign="top">**¿Qué ventas se declaran en el formulario 1042-S?**</td>
    <td valign="top">Solo se notifican las ventas realizadas **a los compradores que se encuentren en los Estados Unidos que estuvieran clasificadas como sujetas a retención de impuestos**.  Todas las demás ventas no deben declararse.</td>
  </tr>
  <tr>
    <td valign="top">**¿Por qué recibí tres copias del mismo formulario 1042-S en un mismo sobre?**</td>
    <td valign="top">Las disposiciones del IRS estipulan que se deben proporcionar tres copias del formulario:
<ul>
<li>una para el destinatario;</li>
<li>una para presentar una declaración de impuestos federal de Estados Unidos (si procede);</li>
<li>una para presentar una declaración de impuestos estatal de Estados Unidos (si procede).</li>
</ul></td>
  </tr>
</table>


> **Nota**: Si tienes más preguntas o dudas relacionadas con los **formularios fiscales del IRS**, crea una [incidencia de soporte técnico](http://aka.ms/storesupport). Microsoft no puede responder preguntas relacionadas con tus circunstancias fiscales específicas. Para tales preguntas, consulta a tu profesional en materia fiscal.
