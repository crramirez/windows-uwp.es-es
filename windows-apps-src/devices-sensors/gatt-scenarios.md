---
author: msatranjr
ms.assetid: 28B30708-FE08-4BE9-AE11-5429F963C330
title: Bluetooth GATT
description: "Este artículo proporciona información general sobre el perfil de atributo genérico (GATT) de Bluetooth para las aplicaciones para la Plataforma universal de Windows (UWP), junto con código de muestra para tres escenarios habituales de GATT"
ms.sourcegitcommit: 62e97bdb8feb78981244c54c76a00910a8442532
ms.openlocfilehash: 664f6ce7829c9e9a6674daa6cdc21e7561ff094b

---
# Perfil GATT de Bluetooth

\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

** API importantes **

-   [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413)
-   [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685)

Este artículo proporciona información general sobre el perfil de atributo genérico (GATT) de Bluetooth para las aplicaciones para la Plataforma universal de Windows (UWP), junto con código de muestra para tres escenarios habituales de GATT: recuperar datos de Bluetooth, controlar un dispositivo de termómetro Bluetooth LE y controlar la presentación de datos de dispositivos Bluetooth LE.

## Información general

Los desarrolladores pueden usar las API del espacio de nombres [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685) para tener acceso a los servicios, descriptores y características de Bluetooth LE. Los dispositivos Bluetooth LE exponen su funcionalidad a través de una colección de:

-   Servicios principales
-   Servicios incluidos
-   Características
-   Descriptores

Los servicios principales definen el contrato funcional del dispositivo LE y contienen una colección de características que definen el servicio. Dichas características, a su vez, contienen descriptores que las describen.

Las API de Bluetooth GATT exponen objetos y funciones, en lugar del acceso al transporte sin procesar. En el nivel del controlador, los servicios primarios se enumeran como nodos de dispositivo secundarios que forman parte del dispositivo Bluetooth LE y que usan las API [**Windows.Devices.Enumeration**](https://msdn.microsoft.com/library/windows/apps/BR225459).

Las API del perfil GATT de Bluetooth también permiten a los desarrolladores trabajar con dispositivos Bluetooth LE, ofreciéndoles la posibilidad de realizar las siguientes tareas:

-   Realizar la detección de servicios / características / descriptores
-   Leer y escribir valores de características / descriptores
-   Registrar una devolución de llamada para el evento de característica ValueChanged

Las API de Bluetooth GATT simplifican el desarrollo al trabajar con propiedades comunes y al ofrecer valores predeterminados razonables para facilitar la administración y la configuración del dispositivo. Ofrecen a los desarrolladores un medio para tener acceso a la funcionalidad de un dispositivo Bluetooth LE desde una aplicación.

Para crear una implementación útil, el desarrollador debe tener conocimientos previos sobre los servicios y las características GATT que la aplicación pretende consumir, y debe procesar los valores de características específicos para que los datos binarios proporcionados por la API se conviertan en datos útiles antes de presentárselos al usuario. Las API de Bluetooth GATT exponen solo los primitivos básicos requeridos para comunicarse con un dispositivo Bluetooth LE. Para interpretar los datos, debe definirse un perfil de aplicación, ya sea mediante un perfil estándar de un SIG de Bluetooth o mediante un perfil personalizado implementado por un proveedor de dispositivos. Un perfil crea un contrato vinculante entre la aplicación y el dispositivo, que indica qué representan los datos intercambiados y cómo interpretarlos.

Para mayor comodidad, el SIG de Bluetooth ofrece una [lista de perfiles públicos](http://go.microsoft.com/fwlink/p/?LinkID=317977).

## Recuperar datos de Bluetooth

En este ejemplo, la aplicación consume medidas de temperatura de un dispositivo Bluetooth LE que implementa el servicio de termómetro médico de Bluetooth LE. La aplicación especifica que desea que se le notifique cuando una nueva medida de temperatura esté disponible. Mediante el registro de un controlador de eventos para el evento "Thermometer Characteristic Value Changed", la aplicación recibirá notificaciones de evento que hacen referencia al cambio del valor de la característica, mientras esta se ejecuta en segundo plano.

Ten en cuenta que cuando se suspende la aplicación, esta debe liberar todos los recursos del dispositivo, mientras que cuando se reanuda debe realizar la inicialización y la enumeración del dispositivo de nuevo. Si se desea una interacción del dispositivo en segundo plano, echa un vistazo a  [DeviceUseTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.deviceusetrigger.aspx) o [GattCharacteristicNotificationTrigger](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.background.gattcharacteristicnotificationtrigger.aspx). DeviceUseTrigger suele ser mejor en eventos con mayor frecuencia, mientras que GattCharacteristicNotificationTrigger es mejor para controlar eventos con poca frecuencia.  

```csharp
double convertTemperatureData(byte[] temperatureData)
{
    // Read temperature data in IEEE 11703 floating point format
    // temperatureData[0] contains flags about optional data - not used
    UInt32 mantissa = ((UInt32)temperatureData[3] << 16) |
        ((UInt32)temperatureData[2] << 8) |
        ((UInt32)temperatureData[1]);

    Int32 exponent = (Int32)temperatureData[4];

    return mantissa * Math.Pow(10.0, exponent);
}

async void Initialize()
{
    var themometerServices = await Windows.Devices.Enumeration
        .DeviceInformation.FindAllAsync(GattDeviceService
            .GetDeviceSelectorFromUuid(
                GattServiceUuids.HealthThermometer),
        null);

    GattDeviceService firstThermometerService = await
        GattDeviceService.FromIdAsync(themometerServices[0].Id);

    serviceNameTextBlock.Text = "Using service: " + 
        themometerServices[0].Name;

    GattCharacteristic thermometerCharacteristic =
        firstThermometerService.GetCharacteristics(
            GattCharacteristicUuids.TemperatureMeasurement)[0];

    thermometerCharacteristic.ValueChanged += temperatureMeasurementChanged;

    await thermometerCharacteristic
        .WriteClientCharacteristicConfigurationDescriptorAsync(
            GattClientCharacteristicConfigurationDescriptorValue.Notify);
}

void temperatureMeasurementChanged(
    GattCharacteristic sender,
    GattValueChangedEventArgs eventArgs)
{
    byte[] temperatureData = new byte[eventArgs.CharacteristicValue.Length];
    Windows.Storage.Streams.DataReader.FromBuffer(
        eventArgs.CharacteristicValue).ReadBytes(temperatureData);

    var temperatureValue = convertTemperatureData(temperatureData);

    temperatureTextBlock.Text = temperatureValue.ToString();
}
```

```cpp
double MainPage::ConvertTemperatureData(
    const Array<unsigned char>^ temperatureData)
{
    unsigned mantissa = ((unsigned)temperatureData[3] << 16) |
        ((unsigned)temperatureData[2] << 8) |
        ((unsigned)temperatureData[1]);

    int exponent = (int)temperatureData[4];

    return mantissa * pow(10.0, (double)exponent);
}

void MainPage::Initialize()
{
    create_task(DeviceInformation::FindAllAsync(
        GattDeviceService::GetDeviceSelectorFromUuid(
            GattServiceUuids::HealthThermometer), 
        nullptr)).then(
            [this] (DeviceInformationCollection^ thermometerServices) 
    {
        create_task(GattDeviceService::FromIdAsync(
            thermometerServices->GetAt(0)->Id))
            .then([this] (GattDeviceService^ firstThermometerService) 
        {
            GattCharacteristic^ thermometerCharacteristic = 
                firstThermometerService->GetCharacteristics(
                    GattCharacteristicUuids::TemperatureMeasurement)
                        ->GetAt(0);

            thermometerCharacteristic->ValueChanged += 
                ref new TypedEventHandler<
                    GattCharacteristic^, 
                    GattValueChangedEventArgs^>(
                        this, &MainPage::TemperatureMeasurementChanged);

            create_task(thermometerCharacteristic->
                WriteClientCharacteristicConfigurationDescriptorAsync(
                GattClientCharacteristicConfigurationDescriptorValue
                    ::Notify));
        });
    });
}


void MainPage::TemperatureMeasurementChanged(
    GattCharacteristic^ sender,
    GattValueChangedEventArgs^ eventArgs)
{
    auto temperatureData =  ref new Array<unsigned char>(
        eventArgs->CharacteristicValue->Length);
    DataReader::FromBuffer(eventArgs->CharacteristicValue)
        ->ReadBytes(temperatureData);

    double temperatureValue = ConvertTemperatureData(temperatureData);
    std::wstringstream str;
    str << temperatureValue;

    temperatureTextBlock->Text = ref new String(str.str().c_str());
}
```

## Controlar un dispositivo de termómetro Bluetooth LE

En este ejemplo, una aplicación para UWP actúa como controlador del dispositivo ficticio de termómetro Bluetooth LE. Asimismo, el dispositivo también declara una característica de formato que permite a los usuarios recuperar la lectura del valor en grados centígrados o Fahrenheit, además de las características estándar del perfil [**HealthThermometer**](https://msdn.microsoft.com/library/windows/apps/Dn297603). La aplicación usa transacciones de escritura de confianza para garantizar que los intervalos de medida y formato se establecen con un valor único.

```csharp
// Uuid of the "Format" Characteristic Value
Guid formatCharacteristicUuid = 
    new Guid("{00000000-0000-0000-0000-000000000010}");

// Constant representing a Fahrenheit scale temperature measurement
const byte FahrenheitReading = 1;
async void Initialize()
{
    var themometerServices = await Windows.Devices.Enumeration
        .DeviceInformation.FindAllAsync(GattDeviceService
            .GetDeviceSelectorFromUuid(
                GattServiceUuids.HealthThermometer),
        null);

    GattDeviceService thermometerService = await
        GattDeviceService.FromIdAsync(themometerServices[0].Id);

    serviceNameTextBlock.Text = "Using service: " + 
        themometerServices[0].Name;

    GattCharacteristic intervalCharacteristic = thermometerService
        .GetCharacteristics(GattCharacteristicUuids.MeasurementInterval)[0];

    GattCharacteristic formatCharacteristic = thermometerService
        .GetCharacteristics(formatCharacteristicUuid)[0];

    GattReliableWriteTransaction gattTransaction = 
        new GattReliableWriteTransaction();

    var writer = new Windows.Storage.Streams.DataWriter();

    // Get a temperature reading every 60 seconds
    writer.WriteUInt16(60);

    gattTransaction.WriteValue(
        intervalCharacteristic, 
        writer.DetachBuffer());

    // Get the reading on the Fahrenheit scale
    writer.WriteByte(FahrenheitReading);

    gattTransaction.WriteValue(
        formatCharacteristic, 
        writer.DetachBuffer());

    GattCommunicationStatus status = await gattTransaction.CommitAsync();

    if (GattCommunicationStatus.Unreachable == status)
    {
        statusTextBlock.Text = "Writing to your device failed !";
    }
}
```

```cpp
// Uuid of the "Format" Characteristic Value
Guid formatCharacteristicUuid(0x00000000, 0x0000, 0x0000, 0x00, 0x00, 
                                  0x00, 0x00, 0x00, 0x00, 0x00, 0x10);

// Constant representing a Fahrenheit scale temperature measurement
const unsigned char FAHRENHEIT_READING = 1;

void MainPage::Initialize()
{
    create_task(DeviceInformation::FindAllAsync(
        GattDeviceService::GetDeviceSelectorFromUuid(
            GattServiceUuids::HealthThermometer), 
        nullptr)).then(
            [this] (DeviceInformationCollection^ thermometerServices) 
    {
        create_task(GattDeviceService::FromIdAsync(
            thermometerServices->GetAt(0)->Id)).then([this] (
                GattDeviceService^ thermometerService) 
        {
            GattCharacteristic^ intervalCharacteristic = 
                thermometerService->GetCharacteristics(
                    GattCharacteristicUuids::MeasurementInterval)
                        ->GetAt(0);

            GattCharacteristic^ formatCharacteristic = 
                thermometerService->GetCharacteristics(
                    formatCharacteristicUuid)->GetAt(0);

            GattReliableWriteTransaction^ gattTransaction = 
                ref new GattReliableWriteTransaction();

            DataWriter^ writer = ref new DataWriter();

            // Get a temperature reading every 60 seconds
            writer->WriteUInt16(60);

            gattTransaction->WriteValue(
                intervalCharacteristic, 
                writer->DetachBuffer());

            writer->WriteByte(FAHRENHEIT_READING);

            gattTransaction->WriteValue(
                formatCharacteristic, 
                writer->DetachBuffer());

            create_task(gattTransaction->CommitAsync())
                .then([this] (GattCommunicationStatus status) 
            {
                if (GattCommunicationStatus::Unreachable == status) 
                { 
                    statusTextBlock->Text = 
                        ref new String(L"Writing to your device failed !");
                }
            });
        });
    });

```

## Controlar la presentación de datos de dispositivos Bluetooth LE

Un dispositivo Bluetooth LE puede exponer un servicio de batería que indique el nivel de batería actual al usuario. El servicio de batería incluye un descriptor [**PresentationFormats**](https://msdn.microsoft.com/library/windows/apps/Dn263742) opcional, que permite cierta flexibilidad a la hora de interpretar los datos del nivel de batería. Este escenario proporciona un ejemplo de aplicación que funciona con este tipo de dispositivo y usa la propiedad **PresentationFormats** para dar formato a un valor de característica antes de presentarlo al usuario.

```csharp
async void Initialize()
{
    var batteryServices = await Windows.Devices.Enumeration
        .DeviceInformation.FindAllAsync(GattDeviceService
            .GetDeviceSelectorFromUuid(GattServiceUuids.Battery),
        null);

    if (batteryServices.Count > 0)
    {
        // Use the first Battery service on the system
        GattDeviceService batteryService = await GattDeviceService
            .FromIdAsync(batteryServices[0].Id);

        // Use the first Characteristic of that Service
        GattCharacteristic batteryLevelCharacteristic =
            batteryService.GetCharacteristics(
                GattCharacteristicUuids.BatteryLevel)[0];

        batteryLevelCharacteristic.ValueChanged += batteryLevelChanged;
    }
    else
    {
        statusTextBlock.Text = "No Battery services found !";
    }
}

void batteryLevelChanged(
    GattCharacteristic sender,
    GattValueChangedEventArgs eventArgs)
{
    byte levelData = Windows.Storage.Streams.DataReader
        .FromBuffer(eventArgs.CharacteristicValue).ReadByte();

    double levelValue;

    if (sender.PresentationFormats.Count > 0)
    {
        levelValue = levelData * 
            Math.Pow(10.0, sender.PresentationFormats[0].Exponent);
    }
    else
    {
        levelValue = (double)levelData;
    }

    batteryLevelTextBlock.Text = levelValue.ToString();
}
```

```cpp
void MainPage::Initialize()
{
    create_task(DeviceInformation::FindAllAsync(
        GattDeviceService::GetDeviceSelectorFromUuid(
            GattServiceUuids::Battery), 
        nullptr)).then([this] (DeviceInformationCollection^ batteryServices) 
    {
        create_task(GattDeviceService::FromIdAsync(
            batteryServices->GetAt(0)->Id)).then([this] (
                GattDeviceService^ batteryService) 
        {
            GattCharacteristic^ batteryLevelCharacteristic = 
                batteryService->GetCharacteristics(
                    GattCharacteristicUuids::BatteryLevel)->GetAt(0);

            batteryLevelCharacteristic->ValueChanged += 
                ref new TypedEventHandler<
                    GattCharacteristic^, 
                    GattValueChangedEventArgs^>
                    (this, &MainPage::BatteryLevelChanged);

            create_task(batteryLevelCharacteristic
                ->WriteClientCharacteristicConfigurationDescriptorAsync(
                GattClientCharacteristicConfigurationDescriptorValue
                    ::Notify));
        });
    });
}

void MainPage::BatteryLevelChanged(
    GattCharacteristic^ sender,
    GattValueChangedEventArgs^ eventArgs)
{
    unsigned char batteryLevelData = DataReader::FromBuffer(
        eventArgs->CharacteristicValue)->ReadByte();

    // if this characteristic has a presentation format
    // use that information to format the value
    double batteryLevelValue;
    if (sender->PresentationFormats->Size > 0)
    {
        batteryLevelValue = batteryLevelData * 
            pow(10.0, sender->PresentationFormats->GetAt(0)->Exponent);
    }
    else
    {
        batteryLevelValue = batteryLevelData;
    }

    std::wstringstream str;
    str << batteryLevelValue;
    batteryLevelTextBlock->Text = 
        ref new String(str.str().c_str());
}
```






<!--HONumber=Jun16_HO4-->


