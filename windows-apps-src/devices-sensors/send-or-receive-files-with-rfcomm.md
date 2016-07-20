---
author: msatranjr
ms.assetid: 5B3A6326-15EE-4618-AA8C-F1C7FB5232FB
title: Bluetooth RFCOMM
description: "En este artículo se proporciona información general de Bluetooth RFCOMM en aplicaciones para la Plataforma universal de Windows (UWP), junto con el código de ejemplo sobre cómo enviar o recibir un archivo."
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: a4d7b0c9e51f3d118c5ed9ac83af2cc6d502d6b3

---
# Bluetooth RFCOMM

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** API importantes **

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.Rfcomm**](https://msdn.microsoft.com/library/windows/apps/Dn263529)

En este artículo se proporciona información general de Bluetooth RFCOMM en aplicaciones para la Plataforma universal de Windows (UWP), junto con el código de ejemplo sobre cómo enviar o recibir un archivo.

## Información general

Las API del espacio de nombres [**Windows.Devices.Bluetooth.Rfcomm**](https://msdn.microsoft.com/library/windows/apps/Dn263529) se basan en los patrones existentes para Windows.Devices, incluidos [**enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459) e [**instantiation**](https://msdn.microsoft.com/library/windows/apps/BR225654). La lectura y la escritura de datos están diseñadas para aprovechar los [**patrones de flujo de datos establecidos**](https://msdn.microsoft.com/library/windows/apps/BR208119) y los objetos en [**Windows.Storage.Streams**](https://msdn.microsoft.com/library/windows/apps/BR241791). Los atributos del Protocolo de detección de servicios (DSP) tienen un valor y un tipo esperado. Sin embargo, algunos dispositivos comunes tienen implementaciones incorrectas de los atributos SDP, en los que el valor no es del tipo esperado. Además, muchos usos de RFCOMM no requieren atributos SDP adicionales. Por estas razones, esta API permite el acceso a los datos SDP sin analizar, desde los cuales los desarrolladores pueden obtener la información que necesitan.

Las API RFCOMM usan el concepto de identificadores de servicio. Aunque un identificador de servicio es simplemente un GUID de 128bit, también suelen especificarse como un entero de 16 o 32bits. La API RFCOMM ofrece un contenedor para que los identificadores de servicio puedan especificarse y consumirse como GUID de 128bits y como enteros de 32bit, pero no ofrece enteros de 16bits. Esto no es un problema para la API, ya que los lenguajes se convierten de forma automática a un entero de 32bits y el identificador todavía puede generarse correctamente.

Las aplicaciones pueden realizar operaciones del dispositivo de varios pasos en una tarea en segundo plano para así poder ejecutarse hasta completarse, incluso si la aplicación pasa a segundo plano y se suspende. Esto permite un mantenimiento del dispositivo más fiable, como los cambios en el firmware o la configuración permanente, así como la sincronización de contenidos, sin que el usuario tenga que interrumpir lo que hacía y ver una barra de progreso. Usa la clase [**DeviceServicingTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn297315) para el mantenimiento del dispositivo y la clase [**DeviceUseTrigger**](https://msdn.microsoft.com/library/windows/apps/Dn297337) para la sincronización de contenidos. Ten en cuenta que estas tareas en segundo plano limitan el tiempo que la aplicación puede ejecutarse en segundo plano, y no tienen por objetivo permitir el funcionamiento indefinidamente ni la sincronización infinita.

Para obtener un ejemplo de código completo que detalle la operación de RFCOMM, consulta [**Bluetooth RFCOMM chat sample (Muestra de chat de Bluetooth RFCOMM)**](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BluetoothRfcommChat) en Github.  
## Enviar un archivo como cliente

Al enviar archivos, el escenario de aplicación básico consiste en conectarse a un dispositivo emparejado según el dispositivo deseado. Esto implica los pasos siguientes:

-   Usa las funciones de **RfcommDeviceService.GetDeviceSelector\*** para ayudar a generar una consulta AQS que pueda usarse para las instancias de dispositivos emparejados enumeradas del servicio deseado.
-   Selecciona un dispositivo enumerado, crea una clase [**RfcommDeviceService**](https://msdn.microsoft.com/library/windows/apps/Dn263463) y lee los atributos SDP según sea necesario (usando [**established data helpers**](https://msdn.microsoft.com/library/windows/apps/BR208119) para analizar los datos del atributo).
-   Crea un socket y usa las propiedades [**RfcommDeviceService.ConnectionHostName**](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.rfcomm.rfcommdeviceservice.connectionhostname.aspx) y [**RfcommDeviceService.ConnectionServiceName**](https://msdn.microsoft.com/library/windows/apps/windows.devices.bluetooth.rfcomm.rfcommdeviceservice.connectionservicename.aspx) en el método [**StreamSocket.ConnectAsync**](https://msdn.microsoft.com/library/windows/apps/Hh701504) para realizar el mantenimiento del dispositivo remoto con los parámetros adecuados.
-   Sigue los patrones de flujo de datos establecidos para leer grupos de datos desde el archivo y enviarlos a la propiedad [**StreamSocket.OutputStream**](https://msdn.microsoft.com/library/windows/apps/BR226920) del socket al dispositivo.

```csharp
Windows.Devices.Bluetooth.RfcommDeviceService _service;
Windows.Networking.Sockets.StreamSocket _socket;

async void Initialize()
{
    // Enumerate devices with the object push service
    auto services =
        await Windows.Devices.Enumeration.DeviceInformation.FindAllAsync(
            RfcommDeviceService.GetDeviceSelector(
                RfcommServiceId.ObexObjectPush));

    if (services.Count > 0) 
    {
        // Initialize the target Bluetooth BR device
        auto service = await RfcommDeviceService.FromIdAsync(services[0].Id);

        // Check that the service meets this App's minimum requirement
        if (SupportsProtection(service) && IsCompatibleVersion(service))
        {
            _service = service;

            // Create a socket and connect to the target
            _socket = new StreamSocket();
            await _socket.ConnectAsync(
                _service.ConnectionHostName,
                _service.ConnectionServiceName,
                SocketProtectionLevel
                    .BluetoothEncryptionAllowNullAuthentication);

            // The socket is connected. At this point the App can wait for
            // the user to take some action, e.g. click a button to send a
            // file to the device, which could invoke the Picker and then
            // send the picked file. The transfer itself would use the
            // Sockets API and not the Rfcomm API, and so is omitted here for
            // brevity.
        }
    }
}

// This App requires a connection that is encrypted but does not care about
// whether its authenticated.
bool SupportsProtection(RfcommDeviceService service)
{
    switch (service.ProtectionLevel)
    {
    case SocketProtectionLevel.PlainSocket:
        if ((service.MaximumProtectionLevel == SocketProtectionLevel
                .BluetoothEncryptionWithAuthentication)
            || (service.MaximumProtectionLevel == SocketProtectionLevel
                .BluetoothEncryptionAllowNullAuthentication)
        {
            // The connection can be upgraded when opening the socket so the
            // App may offer UI here to notify the user that Windows may
            // prompt for a PIN exchange.
            return true;
        }
        else
        {
            // The connection cannot be upgraded so an App may offer UI here
            // to explain why a connection won't be made.
            return false;
        }
    case SocketProtectionLevel.BluetoothEncryptionWithAuthentication:
        return true;
    case SocketProtectionLevel.BluetoothEncryptionAllowNullAuthentication:
        return true;
    }
    return false;
}

// This App relies on CRC32 checking available in version 2.0 of the service.
const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint MINIMUM_SERVICE_VERSION = 200;
bool IsCompatibleVersion(RfcommDeviceService service)
{
    auto attributes = await service.GetSdpRawAttributesAsync(
        BluetothCacheMode.Uncached);
    auto attribute = attributes[SERVICE_VERSION_ATTRIBUTE_ID];
    auto reader = DataReader.FromBuffer(attribute);

    // The first byte contains the attribute' s type
    byte attributeType = reader.ReadByte();
    if (attributeType == SERVICE_VERSION_ATTRIBUTE_TYPE)
    {
        // The remainder is the data
        uint version = reader.Uint32();
        return version >= MINIMUM_SERVICE_VERSION;
    }
}
```

```cpp
Windows::Devices::Bluetooth::RfcommDeviceService^ _service;
Windows::Networking::Sockets::StreamSocket^ _socket;

void Initialize()
{
    // Enumerate devices with the object push service
    create_task(
        Windows::Devices::Enumeration::DeviceInformation::FindAllAsync(
            RfcommDeviceService::GetDeviceSelector(
                RfcommServiceId::ObexObjectPush)))
    .then([](DeviceInformationCollection^ services)
    {
        if (services->Size > 0) 
        {
            // Initialize the target Bluetooth BR device
            create_task(RfcommDeviceService::FromIdAsync(services[0]->Id))
            .then([](RfcommDeviceService^ service)
            {
                // Check that the service meets this App's minimum
                // requirement
                if (SupportsProtection(service)
                    && IsCompatibleVersion(service))
                {
                    _service = service;

                    // Create a socket and connect to the target
                    _socket = ref new StreamSocket();
                    create_task(_socket->ConnectAsync(
                        _service->ConnectionHostName,
                        _service->ConnectionServiceName,
                        SocketProtectionLevel
                            ::BluetoothEncryptionAllowNullAuthentication)
                    .then([](void)
                    {
                        // The socket is connected. At this point the App can
                        // wait for the user to take some action, e.g. click
                        // a button to send a file to the device, which could
                        // invoke the Picker and then send the picked file.
                        // The transfer itself would use the Sockets API and
                        // not the Rfcomm API, and so is omitted here for
                        //brevity.
                    });
                }
            });
        }
    });
}

// This App requires a connection that is encrypted but does not care about
// whether its authenticated.
bool SupportsProtection(RfcommDeviceService^ service)
{
    switch (service->ProtectionLevel)
    {
    case SocketProtectionLevel->PlainSocket:
        if ((service->MaximumProtectionLevel == SocketProtectionLevel
                ::BluetoothEncryptionWithAuthentication)
            || (service->MaximumProtectionLevel == SocketProtectionLevel
                ::BluetoothEncryptionAllowNullAuthentication)
        {
            // The connection can be upgraded when opening the socket so the
            // App may offer UI here to notify the user that Windows may
            // prompt for a PIN exchange.
            return true;
        }
        else
        {
            // The connection cannot be upgraded so an App may offer UI here
            // to explain why a connection won't be made.
            return false;
        }
    case SocketProtectionLevel::BluetoothEncryptionWithAuthentication:
        return true;
    case SocketProtectionLevel::BluetoothEncryptionAllowNullAuthentication:
        return true;
    }
    return false;
}

// This App relies on CRC32 checking available in version 2.0 of the service.
const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint MINIMUM_SERVICE_VERSION = 200;
bool IsCompatibleVersion(RfcommDeviceService^ service)
{
    auto attributes = await service->GetSdpRawAttributesAsync(
        BluetoothCacheMode::Uncached);
    auto attribute = attributes[SERVICE_VERSION_ATTRIBUTE_ID];
    auto reader = DataReader.FromBuffer(attribute);

    // The first byte contains the attribute' s type
    byte attributeType = reader->ReadByte();
    if (attributeType == SERVICE_VERSION_ATTRIBUTE_TYPE)
    {
        // The remainder is the data
        uint version = reader->Uint32();
        return version >= MINIMUM_SERVICE_VERSION;
    }
}
```

## Recibir un archivo como servidor

Otro escenario de aplicación RFCOMM común es hospedar un servicio en el equipo y exponerlo para otros dispositivos.

-   Crea una clase [**RfcommServiceProvider**](https://msdn.microsoft.com/library/windows/apps/Dn263511) para anunciar el servicio deseado.
-   Establece los atributos de SDP según sea necesario (con [**established data helpers**](https://msdn.microsoft.com/library/windows/apps/BR208119) para generar los datos del atributo) y empieza a anunciar los registros de SDP para que otros dispositivos los recuperen.
-   Para conectarse a un dispositivo cliente, crea un dispositivo de escucha de socket para empezar a escuchar las solicitudes de conexiones entrantes.
-   Cuando se recibe una conexión, almacena el socket conectado para un procesamiento posterior.
-   Sigue los patrones de flujo de datos establecidos para leer grupos de datos desde InputStream del socket y guardarlos en un archivo.

Para mantener un servicio de RFCOMM en segundo plano, usa el objeto [**RfcommConnectionTrigger**](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.rfcommconnectiontrigger.aspx). La tarea en segundo plano se desencadena cuando se establece la conexión al servicio. El desarrollador recibe un identificador para el socket en la tarea en segundo plano. La tarea en segundo plano es de larga ejecución y se mantiene mientras el socket está en uso.    

```csharp
Windows.Devices.Bluetooth.RfcommServiceProvider _provider;

async void Initialize()
{
    // Initialize the provider for the hosted RFCOMM service
    _provider = await Windows.Devices.Bluetooth.
        RfcommServiceProvider.CreateAsync(RfcommServiceId.ObexObjectPush);

    // Create a listener for this service and start listening
    StreamSocketListener listener = new StreamSocketListener();
    listener.ConnectionReceived += OnConnectionReceived;
    await listener.BindServiceNameAsync(
        _provider.ServiceId.AsString(),
        SocketProtectionLevel
            .BluetoothEncryptionAllowNullAuthentication);

    // Set the SDP attributes and start advertising
    InitializeServiceSdpAttributes(_provider);
    _provider.StartAdvertising();
}

const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint SERVICE_VERSION = 200;
void InitializeServiceSdpAttributes(RfcommServiceProvider provider)
{
    auto writer = new Windows.Storage.Streams.DataWriter();

    // First write the attribute type
    writer.WriteByte(SERVICE_VERSION_ATTRIBUTE_TYPE)
    // Then write the data
    writer.WriteUint32(SERVICE_VERSION);
    
    auto data = writer.DetachBuffer();
    provider.SdpRawAttributes.Add(SERVICE_VERSION_ATTRIBUTE_ID, data);
}

void OnConnectionReceived(
    StreamSocketListener listener,
    StreamSocketListenerConnectionReceivedEventArgs args)
{
    // Stop advertising/listening so that we're only serving one client
    _provider.StopAdvertising();
    await listener.Close();
    _socket = args.Socket;

    // The client socket is connected. At this point the App can wait for
    // the user to take some action, e.g. click a button to receive a file
    // from the device, which could invoke the Picker and then save the
    // received file to the picked location. The transfer itself would use
    // the Sockets API and not the Rfcomm API, and so is omitted here for
    // brevity.
}
```

```cpp
Windows::Devices::Bluetooth::RfcommServiceProvider^ _provider;

void Initialize()
{
    // Initialize the provider for the hosted RFCOMM service
    create_task(Windows::Devices::Bluetooth.
        RfcommServiceProvider::CreateAsync(
            RfcommServiceId::ObexObjectPush))
    .then([](RfcommServiceProvider^ provider) -> task<void> {
        _provider = provider;

        // Create a listener for this service and start listening
        auto listener = ref new StreamSocketListener();
        listener->ConnectionReceived += ref new TypedEventHandler<
                StreamSocketListener^,
                StreamSocketListenerConnectionReceivedEventArgs^>
           (&OnConnectionReceived);
        return create_task(listener->BindServiceNameAsync(
            _provider->ServiceId->AsString(),
            SocketProtectionLevel
                ::BluetoothEncryptionAllowNullAuthentication));
    }).then([listener](void) {
        // Set the SDP attributes and start advertising
        InitializeServiceSdpAttributes(_provider);
        _provider->StartAdvertising();
    });
}

const uint SERVICE_VERSION_ATTRIBUTE_ID = 0x0300;
const byte SERVICE_VERSION_ATTRIBUTE_TYPE = 0x0A;   // UINT32
const uint SERVICE_VERSION = 200;
void InitializeServiceSdpAttributes(RfcommServiceProvider^ provider)
{
    auto writer = ref new Windows::Storage::Streams::DataWriter();

    // First write the attribute type
    writer->WriteByte(SERVICE_VERSION_ATTRIBUTE_TYPE)
    // Then write the data
    writer->WriteUint32(SERVICE_VERSION);
    
    auto data = writer->DetachBuffer();
    provider->SdpRawAttributes->Add(SERVICE_VERSION_ATTRIBUTE_ID, data);
}

void OnConnectionReceived(
    StreamSocketListener^ listener,
    StreamSocketListenerConnectionReceivedEventArgs^ args)
{
    // Stop advertising/listening so that we're only serving one client
    _provider->StopAdvertising();
    create_task(listener->Close())
    .then([args](void) {
        _socket = args->Socket;

        // The client socket is connected. At this point the App can wait for
        // the user to take some action, e.g. click a button to receive a
        // file from the device, which could invoke the Picker and then save
        // the received file to the picked location. The transfer itself
        // would use the Sockets API and not the Rfcomm API, and so is
        // omitted here for brevity.
    });
}
```




<!--HONumber=Jul16_HO2-->


