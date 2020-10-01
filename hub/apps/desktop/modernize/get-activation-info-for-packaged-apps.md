---
description: Obtenga información sobre cómo obtener determinados tipos de información de activación de aplicaciones .NET empaquetadas y aplicaciones de C++ y Win32 nativas.
title: Obtener información de activación de aplicaciones empaquetadas
ms.date: 09/17/2020
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 5593b59a542f014e482c590b8d534804c836a1a7
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2020
ms.locfileid: "91216728"
---
# <a name="get-activation-info-for-packaged-apps"></a>Obtener información de activación de aplicaciones empaquetadas

A partir de Windows 10, versión 1809, las aplicaciones de escritorio empaquetadas pueden llamar al método [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) para recuperar determinados tipos de información de activación de la aplicación durante el inicio. Por ejemplo, puede llamar a este método para obtener información relacionada con la activación de la aplicación al abrir un archivo, al hacer clic en una notificación del sistema interactiva o mediante un protocolo. A partir de Windows 10, versión 2004, esta característica también se admite en las aplicaciones que usan [paquetes dispersos](./grant-identity-to-nonpackaged-apps.md).

> [!NOTE]
> Además de recuperar determinados tipos de información de activación mediante el método [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) como se describe en este artículo, también puede recuperar información de activación de las tareas en segundo plano mediante la definición de una clase COM. Para más información, consulte [Creación y registro de una tarea en segundo plano COM de WinMain](/windows/uwp/launch-resume/create-and-register-a-winmain-background-task).

## <a name="code-example"></a>Ejemplo de código

En el ejemplo de código siguiente se muestra cómo llamar al método [AppInstance.getactivatedeventargsGetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) desde la función **Main** en una aplicación de Windows Forms. Para cada tipo de activación que admita la aplicación, convierta el valor devuelto de `args` al tipo de argumentos del evento correspondiente. En este ejemplo de código, se supone que los métodos `Handlexxx` son el código del controlador de activación dedicado que se ha definido en otra parte.

```csharp
static void Main()
{
    Application.EnableVisualStyles();
    Application.SetCompatibleTextRenderingDefault(false);

    var args = AppInstance.GetActivatedEventArgs();
    switch (args.Kind)
    {
        case ActivationKind.Launch:
            HandleLaunch(args as LaunchActivatedEventArgs);
            break;
        case ActivationKind.ToastNotification:
            HandleToastNotification(args as ToastNotificationActivatedEventArgs);
            break;
        case ActivationKind.VoiceCommand:
            HandleVoiceCommand(args as VoiceCommandActivatedEventArgs);
            break;
        case ActivationKind.File:
            HandleFile(args as FileActivatedEventArgs);
            break;
        case ActivationKind.Protocol:
            HandleProtocol(args as ProtocolActivatedEventArgs);
            break;
        case ActivationKind.StartupTask:
            HandleStartupTask(args as StartupTaskActivatedEventArgs);
            break;
        default:
            HandleLaunch(null);
            break;
    }
```

## <a name="supported-activation-types"></a>Tipos de activación admitidos

Puede usar el método [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) para recuperar información de activación del conjunto admitido de objetos de argumentos de eventos que se enumeran en la tabla siguiente. Algunos de estos tipos de activación requieren el uso de una extensión de paquete en el manifiesto del paquete.

La información de activación de [ShareTargetActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.sharetargetactivatedeventargs) solo se admite en Windows 10, versión 2004 y versiones posteriores. Todos los demás tipos de información de activación se admiten en Windows 10, versión 1809 y versiones posteriores.

| Tipo de argumentos de eventos | Extensión de paquete | Documentos relacionados | 
|-------------------|-----------------|-----------------------|
| [ShareTargetActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.sharetargetactivatedeventargs) | [uap:ShareTarget](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-sharetarget) | [Convertir la aplicación de escritorio en un destino de recursos compartidos](./desktop-to-uwp-extend.md#making-your-desktop-application-a-share-target) |
| [ProtocolActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.protocolactivatedeventargs) | [uap:Protocol](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-protocol) | [Iniciar la aplicación mediante un protocolo](./desktop-to-uwp-extensions.md#start-your-application-by-using-a-protocol) |
| [ToastNotificationActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.toastnotificationactivatedeventarg) | desktop:ToastNotificationActivation | [Notificaciones del sistema desde aplicaciones de escritorio](/windows/uwp/design/shell/tiles-and-notifications/toast-desktop-apps). |
| [StartupTaskActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.startuptaskactivatedeventargs)  | desktop:StartupTask | [Iniciar un archivo ejecutable cuando los usuarios inicien sesión en Windows](./desktop-to-uwp-extensions.md#start-an-executable-file-when-users-log-into-windows) |
| [FileActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.fileactivatedeventargs) | [uap:FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation) | [Asociar la aplicación empaquetada con un conjunto de tipos de archivo](./desktop-to-uwp-extensions.md#associate-your-packaged-application-with-a-set-of-file-types) |
| [VoiceCommandActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.voicecommandactivatedeventargs) | Ninguno | [Controlar la activación y ejecutar los comandos de voz](/cortana/voice-commands/launch-a-foreground-app-with-voice-commands-in-cortana) |
| [LaunchActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs) | Ninguno |  |