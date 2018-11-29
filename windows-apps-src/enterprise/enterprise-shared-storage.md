---
ms.assetid: B48E21AB-0EA5-444B-8333-393DD8D1B76D
title: Almacenamiento compartido de empresa
description: El almacenamiento compartido de empresa define las ubicaciones de los datos locales de las aplicaciones de línea de negocio para compartir datos.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 006507d4665f5578310b8d3e31fb8f7fba4117a2
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "7984830"
---
# <a name="enterprise-shared-storage"></a>Almacenamiento compartido de empresa

El almacenamiento compartido consta de dos ubicaciones en las que las aplicaciones con la funcionalidad restringida **enterpriseDeviceLockdown** y un certificado de empresa tienen acceso completo de lectura y escritura. Ten en cuenta que la funcionalidad **enterpriseDeviceLockdown** permite a las aplicaciones usar la API de bloqueo del dispositivo y acceder a las carpetas de almacenamiento compartido de empresa. Para obtener más información acerca de la API, consulta el espacio de nombres [**Windows.Embedded.DeviceLockdown**](http://go.microsoft.com/fwlink/?LinkId=699331).  

Estas ubicaciones se establecen en la unidad local:
- \Data\SharedData\Enterprise\Persistent
- \Data\SharedData\Enterprise\Non-Persistent

## <a name="scenarios"></a>Escenarios

El almacenamiento compartido de empresa proporciona compatibilidad con los siguientes escenarios.

- Puedes compartir datos en una instancia de una aplicación, entre instancias de la misma aplicación, o incluso entre aplicaciones, suponiendo que ambas tengan el certificado y la funcionalidad adecuados.
- Puedes almacenar datos en la unidad de disco duro local de la carpeta \Data\SharedData\Enterprise\Persistent y esta persistirá incluso después de restablecer el dispositivo.
- Manipula, así como lee, escribe y elimina, archivos en un dispositivo a través del servicio de administración de dispositivos móviles (MDM). Para obtener más información sobre cómo usar el almacenamiento compartido de empresa a través del servicio MDM, consulta [EnterpriseExtFileSystem CSP (CSP de EnterpriseExtFileSystem)](http://go.microsoft.com/fwlink/?LinkId=699333).

## <a name="access-enterprise-shared-storage"></a>Acceder al almacenamiento compartido de empresa

En el siguiente ejemplo se muestra cómo declarar la funcionalidad para acceder al almacenamiento compartido de empresa en el manifiesto del paquete, y cómo tener acceso a las carpetas de almacenamiento compartido mediante la clase Windows.Storage.StorageFolder.

En el manifiesto del paquete de la aplicación, incluye la funcionalidad siguiente:

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap mp rescap">

…

<Capabilities>
    <rescap:Capability Name="enterpriseDeviceLockdown"/>
</Capabilities>
```

Para acceder a la ubicación de datos compartida, la aplicación debe usar el siguiente código.

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;
using Windows.Storage;

…

// Get the Enterprise Shared Storage folder.
var enterprisePersistentFolderRoot = @"C:\Data\SharedData\Enterprise\Persistent";

StorageFolder folder =
    await StorageFolder.GetFolderFromPathAsync(enterprisePersistentFolderRoot);

// Get the files in the folder.
IReadOnlyList<StorageFile> sortedItems =
    await folder.GetFilesAsync();

// Iterate over the results and print the list of files
// to the Visual Studio Output window.
foreach (StorageFile file in sortedItems)
    Debug.WriteLine(file.Name + ", " + file.DateCreated);
```

