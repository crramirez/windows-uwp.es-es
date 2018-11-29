---
title: Problemas conocidos de UWP en el Programa para desarrolladores de Xbox
description: Se indican los problemas conocidos con la UWP en el Programa para desarrolladores de Xbox.
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: a7b82570-1f99-4bc3-ac78-412f6360e936
ms.localizationpriority: medium
ms.openlocfilehash: 55068ef3f0a0a0d01c61746bde02ddb7aa4ef885
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2018
ms.locfileid: "7991673"
---
# <a name="known-issues-with-uwp-on-xbox-developer-program"></a>Problemas conocidos de UWP en el Programa para desarrolladores de Xbox

En este tema se describen los problemas conocidos de UWP en el Programa para desarrolladores de Xbox One. Para obtener más información acerca de este programa, consulta [UWP en Xbox](index.md). 

\[Si llegaste aquí desde un vínculo en un tema de referencia de API y buscas información sobre la API de la familia de dispositivos universales, consulta [UWP features that aren't yet supported on Xbox](http://go.microsoft.com/fwlink/?LinkID=760755) (Características de la UWP que aún no se admiten en Xbox).\]

La siguiente lista destaca algunos problemas conocidos que puedes encontrarte, aunque esta no es una lista exhaustiva. 

**Queremos recibir tus comentarios**, así que notifica todos los problemas que encuentres en el foro de [desarrollo de aplicaciones para la Plataforma universal de Windows](https://social.msdn.microsoft.com/forums/windowsapps/home?forum=wpdevelop). 

Si sigues teniendo problemas, lee la información de este tema, consulta las [preguntas más frecuentes](frequently-asked-questions.md) y usa los foros para pedir ayuda.

 
## <a name="deploying-from-vs-fails-with-parental-controls-turned-on"></a>La implementación desde VS sufre un error si el control parental está activado

Se producirá un error al iniciar la aplicación desde VS si la consola tiene activado el control parental en la configuración.

Para solucionar este problema, deshabilita temporalmente el control parental o:
1. Implementa tu aplicación en la consola con el control parental desactivado.
2. Activa el control parental.
3. Inicia la aplicación desde la consola.
4. Escribe un PIN o una contraseña para permitir que la aplicación se inicie.
5. La aplicación se iniciará.
6. Cierra la aplicación.
7. Inicia desde VS con F5 y la aplicación se iniciará sin pedir confirmación.

En este punto, el permiso es _permanente_ hasta que se cierra la sesión del usuario, aunque se desinstale y vuelva a instalar la aplicación.
 
Existe otro tipo de excepción que solo está disponible para cuentas de menores. Una cuenta de un menor requiere que uno de los progenitores inicie sesión para conceder permiso, pero, cuando lo hace, el progenitor tiene la opción de elegir **Siempre** y permitir que el menor inicie la aplicación. Esa excepción se almacena en la nube y se conservará, incluso si el menor cierra la sesión y vuelve a iniciarla.

## <a name="storagefilecopyasync-fails-to-copy-encrypted-files-to-unencrypted-destination"></a>Se produce un error de StorageFile.CopyAsync al copiar archivos cifrados en un destino sin cifrar 

Cuando StorageFile.CopyAsync se usa para copiar un archivo cifrado en un destino que no está cifrado, se producirá un error en la llamada con la excepción siguiente:

```
System.UnauthorizedAccessException: Access is denied. (Excep_FromHResult 0x80070005)
```

Esto puede afectar a los desarrolladores de Xbox que quieran copiar en otra ubicación los archivos que se implementan como parte de su paquete de la aplicación. El motivo es que el contenido del paquete está cifrado en una consola Xbox en el modo comercial, pero no en el modo de desarrollo. Como resultado, es posible que parezca que la aplicación funciona según lo esperado durante el desarrollo y pruebas, pero presente un error cuando se publique y luego instale en una consola Xbox en modo comercial.
 

## <a name="blocked-networking-ports-on-xbox-one"></a>Puertos de red bloqueados en Xbox One

Las aplicaciones para la Plataforma universal de Windows (UWP) en dispositivos Xbox One tienen restringido el enlace a los puertos del intervalo [57344, 65535], inclusive. Aunque el enlace a estos puertos puede parecer correcto en tiempo de ejecución, el tráfico de red puede eliminarse de forma silenciosa antes de llegar a la aplicación. La aplicación se debe enlazar al puerto 0 siempre que sea posible, lo que permite al sistema seleccionar el puerto local. Si necesitas usar un puerto específico, el número de puerto debe estar en el intervalo [1025, 49151], y debes comprobar y evitar conflictos con el registro IANA. Para obtener más información, consulta [Service Name and Transport Protocol Port Number Registry](http://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml) (Nombre de servicio y registro de número de puerto del protocolo de transporte).

## <a name="uwp-api-coverage"></a>Cobertura de las APIs de UWP

En Xbox no se admiten todas las API de UWP. Para ver la lista de las API que sabemos que no funcionan, consulta [UWP features that aren't yet supported on Xbox](http://go.microsoft.com/fwlink/p/?LinkId=760755) (Características UWP que aún no se admiten en Xbox). Si encuentras problemas con otras API, notifícalo en los foros. 


## <a name="navigating-to-wdp-causes-a-certificate-warning"></a>Desplazarse a WDP genera una advertencia de certificado

Recibirás una advertencia acerca del certificado proporcionado, similar a la siguiente captura de pantalla, porque el certificado de seguridad firmado por la consola Xbox One no se considera un editor de confianza conocido. Para acceder a Windows Device Portal, haz clic en **Continuar a este sitio web**.

![Advertencia de certificado de seguridad de sitio web](images/security_cert_warning.jpg)


## <a name="knownfoldersmediaserverdevices-caveat-on-xbox"></a>Advertencia de KnownFolders.MediaServerDevices en Xbox

En el escritorio, los servidores multimedia se "emparejan" con el equipo y el servicio de asociación de dispositivos realiza un seguimiento constante de los servidores que están en línea, de modo que una consulta del sistema de archivos inicial puede devolver inmediatamente una lista de los servidores emparejados que están actualmente en línea.

En Xbox, no hay ninguna interfaz de usuario para agregar o quitar servidores, por lo que la consulta del sistema de archivos inicial siempre volverá vacía. Debes crear una consulta, suscribirte al evento ContentsChanged y actualizar la consulta cada vez que recibas una notificación. Se generarán los servidores y la mayoría aparecerán en 3 segundos.

Código de ejemplo sencillo:

```
namespace TestDNLA {

    public sealed partial class MainPage : Page {
        public MainPage() {
            this.InitializeComponent();
        }

        private async void FindFiles_Click(object sender, RoutedEventArgs e) {
            try {
                StorageFolder library = KnownFolders.MediaServerDevices;
                var folderQuery = library.CreateFolderQuery();
                folderQuery.ContentsChanged += FolderQuery_ContentsChanged;
                IReadOnlyList<StorageFolder> rootFolders = await folderQuery.GetFoldersAsync();
                if (rootFolders.Count == 0) {
                    Debug.WriteLine("No Folders found");
                } else {
                    Debug.WriteLine("Folders found");
                }
            } catch (Exception ex) {
                Debug.WriteLine("Error: " + ex.Message);
            } finally {
                Debug.WriteLine("Done");
            }
        }

        private async void FolderQuery_ContentsChanged(Windows.Storage.Search.IStorageQueryResultBase sender, object args) {
            Debug.WriteLine("Folder added " + sender.Folder.Name);
            IReadOnlyList<StorageFolder> topLevelFolders = await sender.Folder.GetFoldersAsync();
            foreach (StorageFolder topLevelFolder in topLevelFolders) {
                Debug.WriteLine(topLevelFolder.Name);
            }
        }
    }
}
```

## <a name="see-also"></a>Consulta también
- [Preguntas más frecuentes](frequently-asked-questions.md)
- [UWP en Xbox One](index.md)
