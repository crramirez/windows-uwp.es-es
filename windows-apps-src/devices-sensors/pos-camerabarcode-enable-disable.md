---
title: Configuración del escáner de códigos de barras de cámara
description: Obtenga información acerca de cómo establecer una clave del registro del sistema en Windows 10 para habilitar o deshabilitar el descodificador de software para el escáner de códigos de barras de la cámara.
ms.date: 04/08/2019
ms.topic: article
keywords: Windows 10, UWP, punto de servicio, pos
ms.localizationpriority: medium
ms.openlocfilehash: fefe15dd36cbc08fcae3b5bc0199eafad400cf1f
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2020
ms.locfileid: "88943069"
---
# <a name="enable-or-disable-the-software-decoder-that-ships-with-windows"></a>Habilitar o deshabilitar el descodificador de software que se distribuye con Windows

En Windows 10, versión 1803, el descodificador de software está instalado y habilitado de forma predeterminada.  Puede deshabilitar el descodificador de software que se incluye con Windows si no desea usar el escáner de códigos de barras de la cámara o si ha adquirido un descodificador de terceros que funciona con las API de Windows. Devices. PointOfService. BarcodeScanner y no desea usar ambos.

## <a name="enable-or-disable-using-the-system-registry"></a>Habilitar o deshabilitar mediante el registro del sistema

El descodificador de software que se incluye con Windows puede habilitarse o deshabilitarse a través del registro del sistema mediante la adición de la clave del registro *InboxDecoder* en *HKLM\Software\Microsoft\PointOfService\BarcodeScanner* y el establecimiento del valor de *enable* tal como se describe a continuación.

| Nombre del valor  | Tipo de valor | Value | Estado |
| ----------- | --------- | -------|--------|
| Habilitar      | DWORD     | 1 (predeterminado)<br/>0 |  Habilita el descodificador de software que se distribuye con Windows <br/> Deshabilita el descodificador de software que se distribuye con Windows |

Este es un archivo de registro de ejemplo que puede usar para **deshabilitar** el descodificador de software que se distribuye con Windows:

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000000
```  

Use este archivo de registro de ejemplo para **Habilitar** el descodificador de software que se distribuye con Windows:

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000001
```  

> [!Warning]
> Es posible que se produzcan problemas graves si el registro se modifica de forma incorrecta.  Como protección adicional, haga una copia de seguridad del Registro antes de modificarlo.  Y así, si se produce algún problema, puede restaurarlo.  Para obtener más información acerca de cómo realizar copias de seguridad y restaurar el registro, haga clic en el número de artículo siguiente para ver el artículo en Microsoft Knowledge Base: <br/><br/> [322756](https://support.microsoft.com/help/322756/how-to-back-up-and-restore-the-registry-in-windows) cómo realizar una copia de seguridad y restaurar el registro en Windows.

> [!NOTE]
> El descodificador de software integrado en Windows 10 se proporciona a cortesía de  [**Digimarc Corporation**](https://www.digimarc.com/).

## <a name="see-also"></a>Consulte también

### <a name="samples"></a>Ejemplos

- [Ejemplo de escáner de código de barras](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
