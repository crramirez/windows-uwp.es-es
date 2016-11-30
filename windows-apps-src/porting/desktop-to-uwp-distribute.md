---
author: awkoren
Description: "Distribuir la aplicación para UWP convertida con el puente de escritorio a UWP"
Search.Product: eADQiWindows 10XVcnh
title: "Distribuir la aplicación para UWP convertida con el puente de escritorio a UWP"
translationtype: Human Translation
ms.sourcegitcommit: fe96945759739e9260d0cdfc501e3e59fb915b1e
ms.openlocfilehash: fbe8a77e3d5735b14088efb0ff6aa01be8d4badf

---

# Distribuir aplicaciones convertidas con el puente de escritorio

Hay tres métodos para implementar la aplicación convertida: la Tienda Windows, la instalación de prueba y el registro de archivos dinámico.  

## Tienda Windows

La Tienda Windows es la forma más cómoda para que los clientes obtengan tu aplicación. Para empezar, rellena el formulario en [Bring your existing apps and games to the Windows Store with the Desktop Bridge](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge) (Llevar las aplicaciones y los juegos existentes a la Tienda Windows con el puente de escritorio). Microsoft se pondrá en contacto contigo para iniciar el proceso de incorporación. 

Ten en cuenta que debes ser el desarrollador o el editor de la aplicación o el juego para incorporarlos a la TiendaWindows. Como tal, asegúrate de que tu nombre y dirección de correo coincidan con el sitio web que envías como la dirección URL a continuación, para que podemos validar que eres el desarrollador o el editor.

## Instalación de prueba

La instalación de prueba proporciona una forma fácil para implementar en varios equipos. Es especialmente útil en escenarios empresariales o de línea de negocio (LOB) en los que quieres un mayor control sobre la experiencia de distribución, pero no quieres participar en el certificado de la Tienda.

Antes de implementar la aplicación mediante transferencia local, tienes que iniciar sesión con un certificado. Para obtener información sobre cómo crear un certificado, consulta [Firmar el paquete .appx](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter#deploy-your-converted-appx). 

Para importar un certificado que creaste anteriormente, sigue estos pasos. Puedes importar el certificado directamente con CERTUTIL o puedes instalarlo desde un paquete AppX que hayas firmado, tal como lo hará el cliente. 

Para instalar el certificado mediante CERTUTIL, ejecuta el siguiente comando desde un símbolo del sistema de administrador:

```cmd
Certutil -addStore TrustedPeople <testcert.cer>
```

Para importar el certificado desde el paquete AppX como lo haría un cliente:

1.  En el Explorador de archivos, haz clic en un paquete AppX que hayas firmado con un certificado de prueba y elige **Propiedades** en el menú contextual.
2.  Haz clic o pulsa la pestaña **Firmas digitales**.
3.  Haz clic o pulsa en el certificado y elige **Detalles**.
4.  Haz clic o pulsa **Ver certificado**.
5.  Haz clic o pulsa **Instalar certificado**.
6.  En el grupo **Ubicación del almacén**, selecciona **Equipo local**.
7.  Haz clic o pulsa **Siguiente** y **Aceptar** para confirmar el cuadro de diálogo de UAC.
8.  En la siguiente pantalla del Asistente para importación de certificados, cambia la opción seleccionada a **Colocar todos los certificados en el siguiente almacén**.
9.  Haz clic o pulsa **Examinar**. En la ventana Seleccionar almacén de certificados, desplázate hacia abajo y selecciona **Personas de confianza** y, por último, haz clic o pulsa **Aceptar**.
10. Haz clic o pulsa **Siguiente**. Aparece una nueva pantalla. Haz clic o pulsa **Finalizar**.
11. Debería aparecer un cuadro de diálogo de confirmación. Si es así, haz clic en **Aceptar**. Si aparece otro cuadro de diálogo en el que se indica que hay un problema con el certificado, tendrás que solucionarlo.

Nota: Para que Windows confíe en el certificado, el certificado debe estar ubicado en el nodo **Certificados (equipo local) > Entidades de certificación raíz de confianza > Certificados** o en el nodo **Certificados (equipo local) > Personas de confianza > Certificados**. Solo los certificados de estas dos ubicaciones pueden validar certificados de confianza en el contexto del equipo local. De lo contrario, aparece un mensaje de error similar a la siguiente cadena:

```CMD
"Add-AppxPackage : Deployment failed with HRESULT: 0x800B0109, A certificate chain processed,
but terminated in a rootcertificate which is not trusted by the trust provider.
(Exception from HRESULT: 0x800B0109) error 0x800B0109: The root certificate of the signature
in the app package must be trusted."
```

Ahora que se ha establecido la confianza del certificado, existen dos formas de instalar el paquete: a través de PowerShell o, simplemente, al hacer doble clic en el archivo del paquete AppX para instalarlo.  Para instalar a través de PowerShell, ejecuta el cmdlet siguiente:

```powershell
Add-AppxPackage <MyApp>.appx
```

### Registro de archivos dinámico

El registro de archivos dinámico resulta útil para la depuración cuando los archivos se colocan en el disco en una ubicación a la que puedes acceder y que puedes actualizar con facilidad. Además, este tipo de registro no requiere firma ni certificado.  

Para implementar la aplicación durante el desarrollo, ejecuta el siguiente cmdlet de PowerShell: 

```Add-AppxPackage –Register AppxManifest.xml```

Para actualizar los archivos .exe o .dll de la aplicación, simplemente reemplaza los archivos existentes en el paquete con los nuevos, aumenta el número de versión en AppxManifest.xml y, a continuación, vuelve a ejecutar el comando anterior.

Ten en cuenta esto: 

* Cualquier unidad en la que instales la aplicación convertida debe tener el formato NTFS.
* Una aplicación convertida se ejecuta siempre como el usuario interactivo.


<!--HONumber=Nov16_HO1-->


