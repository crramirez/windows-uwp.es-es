---
title: Configuración del escáner de código de barras basado en cámara
description: Habilitar o deshabilitar el escáner de código de barras basado en cámara.
ms.date: 05/1/2018
ms.topic: article
keywords: windows 10, uwp, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: 6b802dfa44f36768dc2446ee1d15bf9ca6d4f9f3
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2019
ms.locfileid: "9051048"
---
# <a name="enable-or-disable-the-software-decoder-that-ships-with-windows"></a>Habilitar o deshabilitar el descodificador de software que se incluye con Windows
En Windows 10, versión 1803, el descodificador de software está instalado y habilitado de manera predeterminada.  Puedes deshabilitar el descodificador de software que viene con Windows si no quieres usar el escáner de código de barras basado en cámara, o si has adquirido un descodificador diferente que funciona con API Windows.Devices.PointOfService.BarcodeScanner y no deseas usar ambos.

## <a name="enable-or-disable-using-the-system-registry"></a>Habilitar o deshabilitar mediante el registro del sistema
El descodificador de software que se incluye con Windows se puede habilitar o deshabilitar mediante el registro del sistema agregando la clave del Registro *InboxDecoder*, en *HKLM\Software\Microsoft\PointOfService\BarcodeScanner* y configurando el valor *Habilitar* tal y como se describe a continuación.

| Nombre de valor  | Tipo de valor | Valor | Estado |
| ----------- | --------- | -------|--------|
| Habilitar      | DWORD     | 1 (predeterminado)<br/>0 |  Habilita el descodificador de software que se incluye con Windows <br/> Deshabilita el descodificador de software que se incluye con Windows |


Este es un archivo de registro de ejemplo que puedes usar para **deshabilitar** el descodificador de software que se incluye con Windows:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000000


```  
    
Usa este archivo de registro de ejemplo para **habilitar** el descodificador de software que se incluye con Windows:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000001


```  

> [!Warning] 
> Pueden producirse problemas graves si modificas el registro de manera incorrecta.  Para una mayor protección, haz una copia de seguridad del registro antes de modificarlo.  Y así, si se produce algún problema, puedes restaurarlo.  Si deseas obtener información adicional sobre cómo hacer una copia de seguridad del registro y restaurarlo, haz clic en el siguiente número de artículo para obtener acceso al artículo correspondiente en Microsoft Knowledge Base: <br/><br/> [322756](https://support.microsoft.com/kb/322756) Cómo hacer una copia de seguridad y restaurar el registro de Windows.

> [!NOTE]
> El descodificador de software integrado en Windows 10 es cortesía de [**Digimarc Corporation**](https://www.digimarc.com/).
