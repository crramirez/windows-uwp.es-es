---
title: Trabajar con búferes de almacenamiento conectado
description: Obtenga información sobre cómo trabajar con búferes de almacenamiento conectado.
ms.assetid: 1d9d1b52-4bfe-4cd9-8b80-50ca6c0e9ae1
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, juegos, uwp, windows 10, xbox, almacenamiento conectado
ms.localizationpriority: medium
ms.openlocfilehash: 3df95e4807e8d3457143e67eebfb62011bf365cc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595030"
---
# <a name="working-with-connected-storage-buffers"></a>Trabajar con búferes de almacenamiento conectado

Utiliza la API de almacenamiento conectado **Windows::Storage::Streams::Buffer** instancias para pasar datos a y desde una aplicación. Dado que los tipos de WinRT no pueden exponer punteros sin formato, acceso a los datos de una instancia de búfer se produce a través de **DataReader** y **DataWriter** clases. Sin embargo, **búfer** también implementa la interfaz COM **IBufferByteAccess**, lo que hace posible obtener un puntero directamente a los datos del búfer.

### <a name="to-get-a-pointer-to-a-buffer-instances-data"></a>Para obtener un puntero a datos de la instancia de búfer

1.  Use **reinterpretar\_cast** para convertir la instancia de búfer como **IUnknown**.

```cpp
        IUnknown* unknown = reinterpret_cast<IUnknown*>(buffer);
```

2.  Consulta el **IUnknown** interfaz para el **IBufferByteAccess** interfaz COM.

```cpp
        Microsoft::WRL::ComPtr<IBufferByteAccess> bufferByteAccess;
        HRESULT hr = unknown->QueryInterface(_uuidof(IBufferByteAccess), &bufferByteAccess);
        if (FAILED(hr))
        return nullptr;
```

3.  Use **IBufferByteAccess::Buffer** para obtener un puntero directamente a los datos del búfer.

```cpp
        byte* bytes = nullptr;
        bufferByteAccess->Buffer(&bytes);
```

Por ejemplo, el ejemplo de código siguiente muestra cómo crear un búfer que contiene la hora actual del sistema. Puesto que los búferes tienen un valor independiente de la capacidad y la longitud es necesario establecer explícitamente la capacidad y la longitud. De forma predeterminada, la longitud es 0.

```cpp
    inline byte* GetBufferData(Windows::Storage::Streams::IBuffer^ buffer)
    {
        using namespace Windows::Storage::Streams;
        IUnknown* unknown = reinterpret_cast<IUnknown*>(buffer);
        Microsoft::WRL::ComPtr<IBufferByteAccess> bufferByteAccess;
        HRESULT hr = unknown->QueryInterface(_uuidof(IBufferByteAccess), &bufferByteAccess);
        if (FAILED(hr))
        return nullptr;
        byte* bytes = nullptr;
        bufferByteAccess->Buffer(&bytes);
        return bytes;
    }

    IBuffer^ WrapRawBuffer( void* ptr, size_t size )
    {
        using namespace Windows::Storage::Streams;

        //uint32 size = sizeof(FILETIME);
        Buffer^ buffer = ref new Buffer(size);
        buffer->Length = size;

        memcpy(GetBufferData(buffer),ptr,size);


        return buffer;

    };
```
