---
ms.assetid: E9ADC88F-BD4F-4721-8893-0E19EA94C8BA
title: Emparejamientos fuera de banda
description: El emparejamiento fuera de banda permite que las aplicaciones se conecten a un punto de servicio periférico sin tener que usar el modo de detección.
---
# Emparejamientos fuera de banda

El emparejamiento fuera de banda permite que las aplicaciones se conecten a un punto de servicio periférico sin tener que usar el modo de detección. Las aplicaciones deben usar el espacio de nombres [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/en-us/library/windows/apps/windows.devices.pointofservice.aspx) y pasar una cadena con formato especial (blob de fuera de banda) al método **FromIdAsync** que corresponda al periférico designado. Cuando ejecutes **FromIdAsync**, el dispositivo de host se emparejará y conectará al periférico antes de que la operación vuelva al autor de la llamada.

## Formato del blob de fuera de banda

```
    "connectionKind":"Network",
    "physicalAddress":"AA:BB:CC:DD:EE:FF",
    "connectionString":"192.168.1.1:9001",
    "peripheralKinds":"{C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33}",
    "providerId":"{02FFF12E-7291-4A5D-ADFA-DA8FB7769CD2}",
    "providerName":"PrinterProtocolProvider.dll"
```

**connectionKind**: es el tipo de conexión. Los valores válidos son "Network" y "Bluetooth".
    
**physicalAddress**: es la dirección MAC del periférico. Por ejemplo, si tienes una impresora de red, esta sería la dirección MAC que puedes encontrar en la hoja de pruebas de la impresora, con el formato AA:BB:CC:DD:EE:FF.

**connectionString**: es la cadena de conexión del periférico. Por ejemplo, si tienes una impresora de red, este valor correspondería a la dirección IP que puedes encontrar en la hoja de pruebas de la impresora, con el formato 192.168.1.1:9001. Este campo se omite en todos los dispositivos periféricos con Bluetooth.

**peripheralKinds**: es el GUID que indica el tipo de dispositivo. Los valores válidos son:

|  |  |
| ---- | ---- |
| *Impresora POS* | C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33 |
| *Escáner de códigos de barras* | C243FFBD-3AFC-45E9-B3D3-2BA18BC7EBC5 |
| *Caja registradora* | 772E18F2-8925-4229-A5AC-6453CB482FDA |

**providerId**: es el GUID que indica la clase del proveedor de protocolo. Los valores válidos son:

|  |  |
| ---- | ---- |
| *Impresora de red genérica ESC/POS* | 02FFF12E-7291-4A5D-ADFA-DA8FB7769CD2 |
| *Impresora genérica ESC/POS BT* | CCD5B810-95B9-4320-BA7E-78C223CAF418 |
| *Impresora Epson BT* | 94917594-544F-4AF8-B53B-EC6D9F8A4464 |
| *Impresora de red Epson* | 9F0F8BE3-4E59-4520-BFBA-AF77614A31CE |
| *Impresora de red Star* | 1E3A32C2-F411-4B8C-AC91-CC2C5FD21996 |
| *Escáner BT de socket* | 6E7C8178-A006-405E-85C3-084244885AD2 |
| *Cajón de red APG* | E619E2FE-9489-4C74-BF57-70AED670B9B0 |
| *Cajón BT APG* | 332E6550-2E01-42EB-9401-C6A112D80185 |

 
**providerName**: es el nombre de la DLL del proveedor. Los proveedores predeterminados son:

|  |  |
| ---- | ---- |
| Impresora | PrinterProtocolProvider.dll |
| Caja registradora | CashDrawerProtocolProvider.dll |
| Escáner | BarcodeScannerProtocolProvider.dll |

## Ejemplo de uso: impresora de red

```csharp
String oobBlobNetworkPrinter =
  "{\"connectionKind\":\"Network\"," +
  "\"physicalAddress\":\"AA:BB:CC:DD:EE:FF\"," +
  "\"connectionString\":\"192.168.1.1:9001\"," +
  "\"peripheralKinds\":\"{C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33}\"," +
  "\"providerId\":\"{02FFF12E-7291-4A5D-ADFA-DA8FB7769CD2}\"," +
  "\"providerName\":\"PrinterProtocolProvider.dll\"}";
    
printer = await PosPrinter.FromIdAsync(oobBlobNetworkPrinter);
```

## Ejemplo de uso: impresora Bluetooth

```csharp
string oobBlobBTPrinter =
    "{\"connectionKind\":\"Bluetooth\"," +
    "\"physicalAddress\":\"AA:BB:CC:DD:EE:FF\"," +
    "\"peripheralKinds\":\"{C7BC9B22-21F0-4F0D-9BB6-66C229B8CD33}\"," +
    "\"providerId\":\"{CCD5B810-95B9-4320-BA7E-78C223CAF418}\"," +
    "\"providerName\":\"PrinterProtocolProvider.dll\"}";

printer = await PosPrinter.FromIdAsync(oobBlobBTPrinter);

```







<!--HONumber=Mar16_HO4-->


