---
ms.assetid: 26834A51-512B-485B-84C8-ABF713787588
title: Crear una aplicación de tarjeta NFC inteligente
description: Windows Phone 8.1 admitía las aplicaciones de emulación de tarjeta NFC con un elemento seguro basado en SIM, pero ese modelo requería que las aplicaciones de pago seguro estuvieran estrechamente unidas a los operadores de redes móviles (MNO).
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ed6d9e21f3fed4a5f1d02a3b45fa08917a96117f
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/30/2018
ms.locfileid: "8204744"
---
# <a name="create-an-nfc-smart-card-app"></a>Crear una aplicación de tarjeta NFC inteligente


**Importante**en este tema solo se aplica a Windows 10 Mobile.

Windows Phone 8.1 admitía las aplicaciones de emulación de tarjeta NFC con un elemento seguro basado en SIM, pero ese modelo requería que las aplicaciones de pago seguro estuvieran estrechamente unidas a los operadores de redes móviles (MNO). Esto limitaba la variedad de soluciones de pago posibles por otros comerciantes o desarrolladores que no estaban unidos a MNO. En Windows 10 Mobile, hemos introducido una nueva tecnología de emulación de tarjetas denominada emulación de tarjeta de Host (HCE). La tecnología HCE permite a tu aplicación comunicarse directamente con un lector de tarjetas NFC. En este tema se muestra cómo funciona la emulación de tarjeta de Host (HCE) en dispositivos Windows 10 Mobile y cómo se desarrolla una aplicación HCE para que los clientes pueden acceder a los servicios a través de su teléfono en lugar de una tarjeta física sin colaboración con un MNO.

## <a name="what-you-need-to-develop-an-hce-app"></a>Qué necesitas para desarrollar una aplicación HCE


Para desarrollar una aplicación de emulación de tarjeta basada en HCE para Windows 10 Mobile, tendrás que obtener la configuración del entorno de desarrollo. Puedes obtener configurar instalando Studio2015 Visual de Microsoft, que incluye las herramientas de desarrollo de Windows y el emulador de Windows 10 Mobile con compatibilidad con la emulación de NFC. Para más información sobre cómo obtener la configuración, consulta [Preparación](https://msdn.microsoft.com/library/windows/apps/Dn726766)

Opcionalmente, si quieres probar con un dispositivo real de Windows 10 Mobile en lugar de en el emulador de Windows 10 Mobile incluido, también deberás los siguientes elementos.

-   Un dispositivo de Windows 10 Mobile con compatibilidad con HCE NFC. Actualmente, los modelos Lumia 730, 830, 640 y 640 XL tienen el hardware para admitir aplicaciones HCE NFC.
-   Un terminal de lector que admita protocolos ISO/IEC 14443-4 y ISO/IEC 7816-4

Windows 10 Mobile implementa un servicio HCE que proporciona las siguientes funcionalidades.

-   Las aplicaciones pueden registrar los identificadores de applet (AID) de las tarjetas que deseen emular.
-   La resolución de conflictos y el enrutamiento de la pares de respuesta y comandos de unidad de datos de protocolo de aplicación (APDU) a una de las aplicaciones registradas en función de la preferencia de usuario y la selección de tarjeta externa del lector.
-   Control de eventos y notificaciones para las aplicaciones como resultado de las acciones del usuario.

Windows 10 admite la emulación de tarjetas inteligentes que se basan en ISO-DEP (ISO-IEC 14443-4) y se comunica mediante APDU según se define en la norma ISO-IEC 7816-4 especificación. Windows 10 admite ISO/IEC 14443-4 tecnología de tipo A para aplicaciones HCE. Las tecnologías de tipo B, tipo F y no ISO-DEP (p. ej., MIFARE) se enrutan a la tarjeta SIM de manera predeterminada.

Solo los dispositivos Windows 10 Mobile están habilitados con la característica de emulación de tarjeta. La emulación de tarjeta basada en HCE y SIM no está disponible en otras versiones de Windows 10.

La arquitectura para la compatibilidad con la emulación de tarjetas basadas en HCE y SIM se muestra en el siguiente diagrama.

![Arquitectura para HCE y emulación de tarjeta SIM](./images/nfc-architecture.png)

## <a name="app-selection-and-aid-routing"></a>Selección de la aplicación y enrutamiento de AID

Para desarrollar una aplicación HCE, debes saber cómo los dispositivos Windows 10 Mobile enrutan Aid a una aplicación específica porque los usuarios pueden instalar varias aplicaciones HCE diferentes. Cada aplicación puede registrar varias tarjetas HCE y SIM. Las aplicaciones de Windows Phone 8.1 heredadas que se basan en SIM seguirán funcionando en Windows 10 Mobile siempre y cuando el usuario elige la opción de "Tarjeta SIM" como su tarjeta de pago predeterminada en el menú de configuración de NFC. Esto se establece de forma predeterminada al activar el dispositivo por primera vez.

Cuando el usuario pulsa en un Terminal desde su dispositivo de Windows 10 Mobile, los datos se enrutan automáticamente a la aplicación correcta instalada en el dispositivo. Este enrutamiento se basa en los identificadores de applet (AID), que son identificadores de 5-16 bytes. Durante la pulsación, el terminal externo transmitirá un comando APDU SELECT para especificar al AID que desea que los se enruten los siguientes comandos de APDU. Los comandos SELECT siguientes cambiarán de nuevo el enrutamiento. En función de los AID registrados por aplicaciones y configuración de usuario, el tráfico APDU se enruta a una aplicación específica, que enviará una respuesta APDU. Ten en cuenta que un terminal puede querer comunicarse con varias aplicaciones diferentes durante la misma pulsación. Por tanto, debes asegurarte de que la tarea en segundo plano de la aplicación salga lo antes posible cuando se desactiva para dejar espacio para que otra tarea en segundo plano responda a APDU. Trataremos las tareas en segundo plano más adelante en este tema.

Las aplicaciones HCE deben registrarse con AID particulares que pueden controlar, así que recibirás APDU para un AID. Las aplicaciones declaran AID usando grupos de AID. Un grupo de AID es conceptualmente equivalente a una tarjeta física individual. Por ejemplo, una tarjeta de crédito se declara con un grupo de AID y una segunda tarjeta de crédito de un banco diferente se declara con un segundo grupo de AID diferente, incluso aunque ambos puedan tener el mismo AID.

## <a name="conflict-resolution-for-payment-aid-groups"></a>Resolución de conflictos para grupos de AID de pago

Cuando una aplicación registra tarjetas físicas (grupos de AID), puede declarar la categoría de grupo de AID como "Payment" u "Other". Aunque puede haber varios grupos de AID de pago registrados en cualquier momento, solo uno de estos grupos de AID de pago puede estar habilitado para tocar y pagar cada vez, el cual selecciona el usuario. Este comportamiento existe porque el usuario espera tener el control de elegir conscientemente usar una tarjeta de pago único, crédito o débito para no pagar con una tarjeta diferente no intencionada al pulsar en un terminal desde su dispositivo.

Sin embargo, se pueden habilitar varios grupos de AID registrados como "Otros" al mismo tiempo sin interacción del usuario. Este comportamiento existe porque se espera que otros tipos de tarjetas, como tarjetas de fidelización, cupones o de tránsito, funcionen sin ningún esfuerzo ni sin preguntar cada vez que pulsan en su teléfono.

Todos los grupos de AID que estén registrados como "Payment" aparecen en la lista de tarjetas de la página de configuración de NFC, donde el usuario puede seleccionar sus tarjetas de pago predeterminadas. Cuando se selecciona una tarjeta de pago predeterminada, la aplicación que registra este grupo de AID de pago se convierte en la aplicación de pago predeterminada. Las aplicaciones de pago predeterminadas pueden habilitar o deshabilitar cualquiera de los grupos de Ayuda sin interacción del usuario. Si el usuario rechaza la petición de aplicación de pago predeterminada, la aplicación de pago predeterminada actual (si existe) permanece como predeterminada. En la siguiente captura de pantalla, se muestra la página de Configuración de NFC.

![Captura de pantalla de la página de configuración de NFC](./images/nfc-settings.png)

Con la captura de pantalla de ejemplo anterior, si el usuario cambia su tarjeta de pago predeterminada a otra tarjeta que no esté registrada por "HCE Application 1", el sistema crea un mensaje de confirmación para solicitar el consentimiento del usuario. Sin embargo, si el usuario cambia su tarjeta de pago predeterminada a otra tarjeta registrada por "HCE Application 1", el sistema no crea una solicitud de confirmación para el usuario, puesto que "HCE Application 1" ya es la aplicación de pago predeterminada.

## <a name="conflict-resolution-for-non-payment-aid-groups"></a>Resolución de conflictos para grupos de AID no de pago

Las tarjetas no de pago clasificadas como "Other" no aparecen en la página de Configuración de NFC.

La aplicación puede crear, registrar y habilitar grupos de AID no de pago de la misma manera que los grupos de AID de pago. La principal diferencia es que, para los grupos de AID no de pago, la categoría de emulación está establecida en "Other" en lugar de en "Payment". Después de registrar el grupo de ayuda con el sistema, debes habilitar el grupo de AID para que reciba tráfico de NFC. Cuando intentes habilitar un grupo de AID no de pago para recibir tráfico, no se solicitará confirmación al usuario a menos que haya un conflicto con uno de los AID ya registrados en el sistema por una aplicación diferente. Si hay un conflicto, se pedirá al usuario información sobre qué tarjeta y qué aplicación asociada se deshabilitará si el usuario elige habilitar el grupo de AID recién registrado.

**Coexistencia con aplicaciones NFC basadas en SIM**

En Windows 10 Mobile, el sistema configura la tabla de enrutamiento de controlador de NFC que se usa para tomar decisiones de enrutamiento en la capa del controlador. La tabla contiene información de los siguientes elementos de distribución.

-   Rutas de AID individuales.
-   Ruta basada en protocolo (ISO-DEP).
-   Enrutamiento basado en tecnología (NFC-A/B/F).

Cuando un lector externo envía un comando "SELECT AID", el controlador de NFC comprueba primero las rutas de AID en la tabla de enrutamiento en busca de una coincidencia. Si no hay ninguna coincidencia, usará la ruta basada en protocolos como la ruta predeterminada para el tráfico ISO-DEP (14443-4-A). Para cualquier otro tráfico no ISO-DEP, usará el enrutamiento basado en la tecnología.

Windows 10 Mobile proporciona una opción de menú "Tarjeta SIM" en la página de configuración de NFC para seguir usando aplicaciones de basadas en SIM de Windows Phone 8.1 heredadas, que no registran sus Aid con el sistema. Si el usuario selecciona "Tarjeta SIM" como su tarjeta de pago predeterminada, la ruta de ISO-DEP se establece en UICC; para todas las demás opciones del menú desplegable, la ruta de ISO-DEP es al host.

La ruta de ISO-DEP se establece en "Tarjeta SIM" para dispositivos que un SE habilitó la tarjeta SIM cuando el dispositivo se arranca por primera vez con Windows 10 Mobile. Cuando el usuario instala una aplicación habilitada para HCE y esa aplicación permite registros de grupo AID de HCE, la ruta de ISO-DEP apuntará al host. Las nuevas aplicaciones basadas en SIM necesitan registrar los AID en la tarjeta SIM para que las rutas AID específicas se rellenen en la tabla de enrutamiento del controlador.

## <a name="creating-an-hce-based-app"></a>Crear una aplicación basada en HCE

La aplicación HCE tiene dos partes.

-   La aplicación principal en primer plano para la interacción del usuario.
-   Una tarea en segundo plano que activa el sistema para procesar APDU para un AID determinado.

Debido a los requisitos de rendimiento extremadamente ajustados para cargar la tarea en segundo plano en respuesta a una pulsación de NFC, te recomendamos que todas tus tareas en segundo plano se implementen en código nativo C++/CX (incluidas las dependencias, las referencias o las bibliotecas de las que depende) en lugar de código C# o código administrado. Aunque el código C# y el código administrado normalmente funcionan correctamente, hay sobrecargas, como la carga de .NET CLR, que pueden evitarse escribiendo en C++/CX.
## <a name="create-and-register-your-background-task"></a>Crear y registrar una tarea en segundo plano

Debes crear una tarea en segundo plano en la aplicación HCE para procesar y responder a APDU enrutados por el sistema. La primera vez que se inicia la aplicación, el primer plano registra una tarea en segundo plano de HCE que implementa la interfaz [**IBackgroundTaskRegistration**](https://msdn.microsoft.com/library/windows/apps/BR224803) como se muestra en el código siguiente.

```csharp
var taskBuilder = new BackgroundTaskBuilder();
taskBuilder.Name = bgTaskName;
taskBuilder.TaskEntryPoint = taskEntryPoint;
taskBuilder.SetTrigger(new SmartCardTrigger(SmartCardTriggerType.EmulatorHostApplicationActivated));
bgTask = taskBuilder.Register();
```

Ten en cuenta que el desencadenador de tareas se establece en [**SmartCardTriggerType**](https://msdn.microsoft.com/library/windows/apps/Dn608017). **EmulatorHostApplicationActivated**. Esto significa que cada vez que el sistema operativo que resuelve la aplicación recibe una APDU del comando SELECT AID, se iniciará la tarea en segundo plano.

## <a name="receive-and-respond-to-apdus"></a>Recibir y responder a APDU

Cuando hay una APDU destinada a la aplicación, el sistema iniciará la tarea en segundo plano. La tarea en segundo plano recibe la APDU pasada a través de la propiedad [**CommandApdu**](https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.smartcardemulatorapdureceivedeventargs.commandapdu.aspx) del objeto [**SmartCardEmulatorApduReceivedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Dn894640) y responde a la APDU mediante el método [**TryRespondAsync**](https://msdn.microsoft.com/library/windows/apps/mt634299.aspx) del mismo objeto. Considera la posibilidad de mantener la tarea en segundo plano para operaciones ligeras por motivos de rendimiento. Por ejemplo, responde a las APDU inmediatamente y sal de la tarea en segundo plano cuando se complete todo el procesamiento. Debido a la naturaleza de las transacciones NFC, los usuarios tienden a mantener su dispositivo con el lector solo un período muy breve de tiempo. La tarea en segundo plano seguirá recibiendo tráfico del lector hasta que se desactive la conexión, en cuyo caso se recibirá un objeto [**SmartCardEmulatorConnectionDeactivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Dn894644). Se puede desactivar la conexión por los siguientes motivos, como se indica en la propiedad [**SmartCardEmulatorConnectionDeactivatedEventArgs.Reason**](https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.smartcardemulatorconnectiondeactivatedeventargs.reason).

-   Si se desactiva la conexión con el valor **ConnectionLost**, significa que el usuario ha alejado el dispositivo del lector. Si la aplicación necesita que el usuario pulse en el terminal más tiempo, puede que tengas que pedirle comentarios. Debes finalizar la tarea en segundo plano rápidamente (completando el aplazamiento) para asegurarte de que si pulsa otra vez, no tenga que esperar a que se salga de la tarea en segundo plano anterior.
-   Si se desactiva la conexión con el valor **ConnectionRedirected**, significa que el terminal envió un nuevo comando SELECT AID de APDU dirigido a un AID diferente. En este caso, la aplicación debe salir de la tarea en segundo plano inmediatamente (completando el aplazamiento) para permitir que otra tarea en segundo plano se ejecute.

También se debe registrar la tarea en segundo plano para el [**evento Canceled**](https://msdn.microsoft.com/library/windows/apps/BR224798) en la [**interfaz IBackgroundTaskInstance**](https://msdn.microsoft.com/library/windows/apps/BR224797) y, de igual forma, salir rápidamente de la tarea en segundo plano (completando el aplazamiento) porque el sistema desencadena este evento cuando termina con la tarea en segundo plano. A continuación se proporciona el código que muestra una tarea en segundo plano de una aplicación HCE.

```csharp
void BgTask::Run(
    IBackgroundTaskInstance^ taskInstance)
{
    m_triggerDetails = static_cast<SmartCardTriggerDetails^>(taskInstance->TriggerDetails);
    if (m_triggerDetails == nullptr)
    {
        // May be not a smart card event that triggered us
        return;
    }

    m_emulator = m_triggerDetails->Emulator;
    m_taskInstance = taskInstance;

    switch (m_triggerDetails->TriggerType)
    {
    case SmartCardTriggerType::EmulatorHostApplicationActivated:
        HandleHceActivation();
        break;

    case SmartCardTriggerType::EmulatorAppletIdGroupRegistrationChanged:
        HandleRegistrationChange();
        break;

    default:
        break;
    }
}

void BgTask::HandleHceActivation()
{
 try
 {
        auto lock = m_srwLock.LockShared();
        // Take a deferral to keep this background task alive even after this "Run" method returns
        // You must complete this deferal immediately after you have done processing the current transaction
        m_deferral = m_taskInstance->GetDeferral();

        DebugLog(L"*** HCE Activation Background Task Started ***");

        // Set up a handler for if the background task is cancelled, we must immediately complete our deferral
        m_taskInstance->Canceled += ref new Windows::ApplicationModel::Background::BackgroundTaskCanceledEventHandler(
            [this](
            IBackgroundTaskInstance^ sender,
            BackgroundTaskCancellationReason reason)
        {
            DebugLog(L"Cancelled");
            DebugLog(reason.ToString()->Data());
            EndTask();
        });

        if (Windows::Phone::System::SystemProtection::ScreenLocked)
        {
            auto denyIfLocked = Windows::Storage::ApplicationData::Current->RoamingSettings->Values->Lookup("DenyIfPhoneLocked");
            if (denyIfLocked != nullptr && (bool)denyIfLocked == true)
            {
                // The phone is locked, and our current user setting is to deny transactions while locked so let the user know
                // Denied
                DoLaunch(Denied, L"Phone was locked at the time of tap");

                // We still need to respond to APDUs in a timely manner, even though we will just return failure
                m_fDenyTransactions = true;
            }
        }
        else
        {
            m_fDenyTransactions = false;
        }

        m_emulator->ApduReceived += ref new TypedEventHandler<SmartCardEmulator^, SmartCardEmulatorApduReceivedEventArgs^>(
            this, &BgTask::ApduReceived);

        m_emulator->ConnectionDeactivated += ref new TypedEventHandler<SmartCardEmulator^, SmartCardEmulatorConnectionDeactivatedEventArgs^>(
                [this](
                SmartCardEmulator^ emulator,
                SmartCardEmulatorConnectionDeactivatedEventArgs^ eventArgs)
            {
                DebugLog(L"Connection deactivated");
                EndTask();
            });

  m_emulator->Start();
        DebugLog(L"Emulator started");
 }
 catch (Exception^ e)
 {
        DebugLog(("Exception in Run: " + e->ToString())->Data());
        EndTask();
 }
}
```
## <a name="create-and-register-aid-groups"></a>Crear y registrar grupos AID

Durante el primer inicio de la aplicación, una vez aprovisionada la tarjeta, crearás y registrarás grupos de AID con el sistema. El sistema determina la aplicación con la que le gustaría comunicar un lector externo y a la que le gustaría enrutar ADPU según corresponda en función de la configuración de usuario y de AID registrados.

La mayor parte de las tarjetas de pago se registran para el mismo AID (que es PPSE AID) junto con los AID específicos de la tarjeta de red de pago adicional. Cada grupo de AID representa una tarjeta y, cuando el usuario habilita la tarjeta, se habilitan todos los AID del grupo. De forma similar, cuando el usuario desactiva la tarjeta, se deshabilitan todos los AID del grupo.

Para registrar un grupo de AID, necesitas crear un objeto [**SmartCardAppletIdGroup**](https://msdn.microsoft.com/library/windows/apps/Dn910955) y establecer sus propiedades para reflejar que se trata de una tarjeta de pago basada en HCE. El nombre para mostrar debe ser descriptivo para el usuario, ya que se mostrará en el menú de Configuración de NFC, además de los avisos al usuario. Para las tarjetas de pago HCE, la propiedad [**SmartCardEmulationCategory**](https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationcategory.aspx) debería establecerse en **Payment** y la propiedad [**SmartCardEmulationType**](https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationtype) se debería establecer en **Host**.

```csharp
public static byte[] AID_PPSE =
        {
            // File name "2PAY.SYS.DDF01" (14 bytes)
            (byte)'2', (byte)'P', (byte)'A', (byte)'Y',
            (byte)'.', (byte)'S', (byte)'Y', (byte)'S',
            (byte)'.', (byte)'D', (byte)'D', (byte)'F', (byte)'0', (byte)'1'
        };

var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName",
                                new List<IBuffer> {AID_PPSE.AsBuffer()},
                                SmartCardEmulationCategory.Payment,
                                SmartCardEmulationType.Host);
```

Para las tarjetas no de pago HCE, la propiedad [**SmartCardEmulationCategory**](https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationcategory.aspx) debería establecerse en **Other** y la propiedad [**SmartCardEmulationType**](https://msdn.microsoft.com/library/windows/apps/windows.devices.smartcards.smartcardappletidgroup.smartcardemulationtype) se debería establecer en **Host**.

```csharp
public static byte[] AID_OTHER =
        {
            (byte)'1', (byte)'2', (byte)'3', (byte)'4',
            (byte)'5', (byte)'6', (byte)'7', (byte)'8',
            (byte)'O', (byte)'T', (byte)'H', (byte)'E', (byte)'R'
        };

var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName",
                                new List<IBuffer> {AID_OTHER.AsBuffer()},
                                SmartCardEmulationCategory.Other,
                                SmartCardEmulationType.Host);
```

Puedes incluir hasta 9 AID (con una longitud de 5-16 bytes cada uno) por cada grupo de AID.

Usa el método [**RegisterAppletIdGroupAsync**](https://msdn.microsoft.com/library/windows/apps/Dn894656) para registrar el grupo de AID con el sistema, que devolverá un objeto [**SmartCardAppletIdGroupRegistration**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration). De manera predeterminada, la propiedad [**ActivationPolicy**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) del objeto de registro se establece en **Disabled**. Esto significa que aunque los AID se registren con el sistema, aún no se han habilitado y no recibirán tráfico.

```csharp
reg = await SmartCardEmulator.RegisterAppletIdGroupAsync(appletIdGroup);
```

Puedes habilitar las tarjetas registradas (grupos de AID) mediante el método [**RequestActivationPolicyChangeAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) de la clase [**SmartCardAppletIdGroupRegistration**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) como se muestra a continuación. Como solo se puede habilitar una tarjeta de pago única a la vez en el sistema, si establece el elemento [**ActivationPolicy**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) de un grupo de AID de pago en **Enabled**, se obtiene el mismo resultado que si se establece la tarjeta de pago predeterminada. Se pedirá al usuario que permita esta tarjeta como una tarjeta de pago predeterminada, independientemente de si hay una tarjeta de pago predeterminada ya seleccionada o no. Esta declaración no corresponde si la aplicación ya es la aplicación de pago predeterminada y está cambiando simplemente entre sus propios grupos de AID. Puedes registrar hasta 10 grupos de AID por aplicación.

```csharp
reg.RequestActivationPolicyChangeAsync(AppletIdGroupActivationPolicy.Enabled);
```

Puedes consultar grupos de AID de la aplicación registrados con el sistema operativo y comprobar su directiva de activación con el método [**GetAppletIdGroupRegistrationsAsync**](https://msdn.microsoft.com/library/windows/apps/Dn894654).

Se notificará a los usuarios cuando se cambie la directiva de activación de una tarjeta de pago de **Disabled** a **Enabled**, solo si la aplicación no es ya la aplicación de pago predeterminada. Solo se notificará a los usuarios cuando se cambie la directiva de activación de una tarjeta no de pago de **Disabled** a **Enabled** si hay un conflicto de AID.

```csharp
var registrations = await SmartCardEmulator.GetAppletIdGroupRegistrationsAsync();
    foreach (var registration in registrations)
    {
registration.RequestActivationPolicyChangeAsync (AppletIdGroupActivationPolicy.Enabled);
    }
```

**Notificación de eventos al cambiar la directiva de activación**

En la tarea en segundo plano, puedes registrar que se reciban eventos cuando la directiva de activación de uno de los registros del grupo de AID cambie fuera de la aplicación. Por ejemplo, el usuario puede cambiar la aplicación de pago predeterminada mediante el menú de configuración NFC de una de las tarjetas a otra tarjeta hospedada por otra aplicación. Si la aplicación necesita que se le notifique este cambio para la configuración interna, como la actualización de los iconos dinámicos, puedes recibir notificaciones de eventos de este cambio y realizar una acción en la aplicación en consecuencia.

```csharp
var taskBuilder = new BackgroundTaskBuilder();
taskBuilder.Name = bgTaskName;
taskBuilder.TaskEntryPoint = taskEntryPoint;
taskBuilder.SetTrigger(new SmartCardTrigger(SmartCardTriggerType.EmulatorAppletIdGroupRegistrationChanged));
bgTask = taskBuilder.Register();
```

## <a name="foreground-override-behavior"></a>Comportamiento de reemplazo de primer plano

Puedes cambiar el elemento [**ActivationPolicy**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) de cualquiera de los registros de grupos de AID a **ForegroundOverride** mientras la aplicación está en primer plano sin pedir confirmación al usuario. Cuando el usuario pulsa en un terminal desde su dispositivo mientras la aplicación está en primer plano, el tráfico se enruta a la aplicación incluso si el usuario no ha elegido ninguna de las tarjetas de pago como su tarjeta de pago predeterminada. Cuando se cambia la directiva de activación de la tarjeta a **ForegroundOverride**, este cambio es solo temporal hasta que la aplicación deje de estar en primer plano y no afectará a la tarjeta de pago predeterminada establecida por el usuario. Puedes cambiar el elemento **ActivationPolicy** de tus tarjetas de pago o no de pago desde la aplicación en primer plano de la siguiente manera. Ten en cuenta que solo es posible llamar al método [**RequestActivationPolicyChangeAsync**](https://docs.microsoft.com/en-us/uwp/api/windows.devices.smartcards.smartcardappletidgroupregistration) desde una aplicación en primer plano y no se puede llamar desde una tarea en segundo plano.

```csharp
reg.RequestActivationPolicyChangeAsync(AppletIdGroupActivationPolicy.ForegroundOverride);
```

Además, puedes registrar un grupo de AID que conste de un solo AID de longitud 0, lo que hará que el sistema enrute todos los APDU independientemente del AID, incluyendo los comandos APDU enviados antes de que se reciba un comando SELECT AID. Sin embargo, un grupo de AID de este tipo solo funciona mientras la aplicación está en primer plano, porque solo puede establecerse en **ForegroundOverride** y no se puede habilitar de forma permanente. Además, este mecanismo funciona para los valores **Host** y **UICC** de la enumeración [**SmartCardEmulationType**](https://msdn.microsoft.com/library/windows/apps/Dn894639) para enrutar todo el tráfico a la tarea en segundo plano de HCE o a la tarjeta SIM.

```csharp
public static byte[] AID_Foreground =
        {};

var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName",
                                new List<IBuffer> {AID_Foreground.AsBuffer()},
                                SmartCardEmulationCategory.Other,
                                SmartCardEmulationType.Host);
reg = await SmartCardEmulator.RegisterAppletIdGroupAsync(appletIdGroup);
reg.RequestActivationPolicyChangeAsync(AppletIdGroupActivationPolicy.ForegroundOverride);
```

## <a name="check-for-nfc-and-hce-support"></a>Comprobar la compatibilidad de NFC y HCE

La aplicación debe comprobar si un dispositivo tiene hardware NFC, admite la característica de emulación de la tarjeta y admite la emulación de tarjeta de host antes de ofrecer estas características al usuario.

La característica de emulación de tarjeta inteligente NFC solo está habilitada en Windows 10 Mobile, por lo tanto, si se intentan usar las API del emulador de tarjeta inteligente en otra versión de Windows 10, se producirán errores. Puedes comprobar la compatibilidad con la API de tarjetas inteligentes en el siguiente fragmento de código.

```csharp
Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.Devices.SmartCards.SmartCardEmulator");
```

Además puedes comprobar si el dispositivo tiene hardware NFC con capacidad para alguna forma de emulación de la tarjeta; para ello, comprueba si el método [**SmartCardEmulator.GetDefaultAsync**](https://msdn.microsoft.com/library/windows/apps/Dn608008) devuelve un valor null. Si es así, el dispositivo no admite ninguna emulación de tarjeta NFC.

```csharp
var smartcardemulator = await SmartCardEmulator.GetDefaultAsync();<
```

El soporte para enrutamiento UICC basado en HCE y AID solo está disponible en dispositivos iniciados recientemente, como los Lumia 730, 830, 640 y 640 XL. Los nuevos dispositivos compatibles con NFC que ejecutan Windows 10 Mobile y versiones posteriores deberían admitir HCE. Tu aplicación puede comprobar la compatibilidad con HCE de la siguiente manera.

```csharp
Smartcardemulator.IsHostCardEmulationSupported();
```

## <a name="lock-screen-and-screen-off-behavior"></a>Comportamiento de la pantalla de bloqueo y de la pantalla apagada

Windows 10 Mobile tiene la configuración de emulación de tarjeta de nivel de dispositivo, que se puede establecer el operador de telefonía móvil o el fabricante del dispositivo. De manera predeterminada, la alternancia de "pulsar para pagar "está deshabilitada, y la "directiva de habilitación del nivel del dispositivo" está definida en "Siempre", a menos que el OEM o el operador móvil omitan estos valores.

La aplicación puede consultar el valor de [**EnablementPolicy**](https://msdn.microsoft.com/library/windows/apps/Dn608006) en el nivel del dispositivo y realizar una acción para cada caso según el comportamiento deseado de la aplicación en cada estado.

```csharp
SmartCardEmulator emulator = await SmartCardEmulator.GetDefaultAsync();

switch (emulator.EnablementPolicy)
{
case Never:
// you can take the user to the NFC settings to turn "tap and pay" on
await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings-nfctransactions:"));
break;

 case Always:
return "Card emulation always on";

 case ScreenOn:
 return "Card emulation on only when screen is on";

 case ScreenUnlocked:
 return "Card emulation on only when screen unlocked";
}
```

La tarea en segundo plano de su aplicación se iniciará incluso si el teléfono está apagado o si la pantalla está apagada solo si el lector externo selecciona un AID que se resuelva en la aplicación. Puedes responder a los comandos del lector en la tarea en segundo plano, pero si necesitas la entrada del usuario, o si deseas mostrar un mensaje al usuario, puedes iniciar la aplicación en primer plano con algunos argumentos. La tarea en segundo plano puede iniciar la aplicación en primer plano con el siguiente comportamiento.

-   En la pantalla de bloqueo de dispositivo (el usuario verá la aplicación en primer plano únicamente después de que se desbloquee el dispositivo)
-   Por encima de la pantalla de bloqueo de dispositivo (después de que el usuario cierre la aplicación, el dispositivo sigue en estado bloqueado)

```csharp
        if (Windows::Phone::System::SystemProtection::ScreenLocked)
        {
            // Launch above the lock with some arguments
            var result = await eventDetails.TryLaunchSelfAsync("app-specific arguments", SmartCardLaunchBehavior.AboveLock);
        }
```

## <a name="aid-registration-and-other-updates-for-sim-based-apps"></a>Registro de AID y otras actualizaciones para las aplicaciones en basadas en SIM

Las aplicaciones de emulación de tarjeta que usan la SIM como el elemento seguro pueden registrarse con el servicio de Windows para declarar los AID compatibles con la tarjeta SIM. Este registro es muy similar a un registro de aplicación basado en HCE. La única diferencia es el valor [**SmartCardEmulationType**](https://msdn.microsoft.com/library/windows/apps/Dn894639), que se debe establecer como Uicc para aplicaciones basadas en SIM. Como resultado del registro de tarjetas de pago, también se rellena el nombre para mostrar de la tarjeta en el menú de configuración de NFC.

```csharp
var appletIdGroup = new SmartCardAppletIdGroup(
                        "Example DisplayName",
                                new List<IBuffer> {AID_PPSE.AsBuffer()},
                                SmartCardEmulationCategory.Payment,
                                SmartCardEmulationType.Uicc);
```

** Importantes ** se ha quitado la compatibilidad de interceptar SMS binaria heredada en Windows Phone 8.1 y se ha reemplazado con una nueva compatibilidad más amplia de SMS en Windows 10 Mobile, pero las aplicaciones de Windows Phone 8.1 heredadas en la que deben actualizarse para usar Windows 10 Mobile SMS nueva API.
