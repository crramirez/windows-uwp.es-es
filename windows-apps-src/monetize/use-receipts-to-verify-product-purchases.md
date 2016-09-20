---
author: mcleanbyron
ms.assetid: E322DFFE-8EEC-499D-87BC-EDA5CFC27551
description: "Cada transacción de la Tienda Windows que tiene como resultado una compra correcta del producto también puede devolver un recibo de la transacción."
title: Usar recibos para comprobar compras de productos
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: b1322b74bf1038f05cd1cba275e432e279ed362d

---

# Usar recibos para comprobar compras de productos


\[ Actualizado para aplicaciones para UWP en Windows 10. Para leer más artículos sobre Windows 8.x, consulta el [archivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**API importantes**

-   [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765)
-   [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766)

Cada transacción de la Tienda Windows que tiene como resultado una compra correcta del producto también puede devolver un recibo de la transacción. Este recibos proporcionan información sobre el producto enumerado y el coste abonado por el cliente.

El acceso a esta información resulta útil en aquellos casos en los que la aplicación debe comprobar que un usuario compró tu aplicación o ha realizado compras de productos desde la aplicación en la TiendaWindows. Por ejemplo, imagina un juego que ofrece contenido descargado. Si el usuario que compró el contenido quiere jugar en otro dispositivo, debes comprobar que ese usuario ya sea propietario del contenido. A continuación se muestra cómo hacerlo.

## Solicitar un recibo


El espacio de nombres de **Windows.ApplicationModel.Store** permite obtener los recibos de dos maneras: mediante los métodos [**CurrentApp.RequestProductPurchaseAsync | requestProductPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/dn263381) o [**CurrentApp.RequestAppPurchaseAsync | requestAppPurchaseAsync**](https://msdn.microsoft.com/library/windows/apps/hh967813) con el parámetro *includeReceipt*, o llamando al método [**CurrentApp.GetAppReceiptAsync | getAppReceiptAsync**](https://msdn.microsoft.com/library/windows/apps/hh967811). Un recibo de aplicación tiene la siguiente apariencia.

```XML
<Receipt Version="1.0" ReceiptDate="2012-08-30T23:10:05Z" CertificateId="b809e47cd0110a4db043b3f73e83acd917fe1336" ReceiptDeviceId="4e362949-acc3-fe3a-e71b-89893eb4f528">
    <AppReceipt Id="8ffa256d-eca8-712a-7cf8-cbf5522df24b" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" PurchaseDate="2012-06-04T23:07:24Z" LicenseType="Full" />
    <ProductReceipt Id="6bbf4366-6fb2-8be8-7947-92fd5f683530" ProductId="Product1" PurchaseDate="2012-08-30T23:08:52Z" ExpirationDate="2012-09-02T23:08:49Z" ProductType="Durable" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" />
    <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
        <SignedInfo>
            <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
            <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
            <Reference URI="">
                <Transforms>
                    <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                </Transforms>
                <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <DigestValue>cdiU06eD8X/w1aGCHeaGCG9w/kWZ8I099rw4mmPpvdU=</DigestValue>
            </Reference>
        </SignedInfo>
        <SignatureValue>SjRIxS/2r2P6ZdgaR9bwUSa6ZItYYFpKLJZrnAa3zkMylbiWjh9oZGGng2p6/gtBHC2dSTZlLbqnysJjl7mQp/A3wKaIkzjyRXv3kxoVaSV0pkqiPt04cIfFTP0JZkE5QD/vYxiWjeyGp1dThEM2RV811sRWvmEs/hHhVxb32e8xCLtpALYx3a9lW51zRJJN0eNdPAvNoiCJlnogAoTToUQLHs72I1dECnSbeNPXiG7klpy5boKKMCZfnVXXkneWvVFtAA1h2sB7ll40LEHO4oYN6VzD+uKd76QOgGmsu9iGVyRvvmMtahvtL1/pxoxsTRedhKq6zrzCfT8qfh3C1w==</SignatureValue>
    </Signature>
</Receipt>
```

Un recibo de producto tiene la siguiente apariencia.

```XML
<Receipt Version="1.0" ReceiptDate="2012-08-30T23:08:52Z" CertificateId="b809e47cd0110a4db043b3f73e83acd917fe1336" ReceiptDeviceId="4e362949-acc3-fe3a-e71b-89893eb4f528">
    <ProductReceipt Id="6bbf4366-6fb2-8be8-7947-92fd5f683530" ProductId="Product1" PurchaseDate="2012-08-30T23:08:52Z" ExpirationDate="2012-09-02T23:08:49Z" ProductType="Durable" AppId="55428GreenlakeApps.CurrentAppSimulatorEventTest_z7q3q7z11crfr" />
    <Signature xmlns="http://www.w3.org/2000/09/xmldsig#">
        <SignedInfo>
            <CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
            <SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256" />
            <Reference URI="">
                <Transforms>
                    <Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
                </Transforms>
                <DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256" />
                <DigestValue>Uvi8jkTYd3HtpMmAMpOm94fLeqmcQ2KCrV1XmSuY1xI=</DigestValue>
            </Reference>
        </SignedInfo>
        <SignatureValue>TT5fDET1X9nBk9/yKEJAjVASKjall3gw8u9N5Uizx4/Le9RtJtv+E9XSMjrOXK/TDicidIPLBjTbcZylYZdGPkMvAIc3/1mdLMZYJc+EXG9IsE9L74LmJ0OqGH5WjGK/UexAXxVBWDtBbDI2JLOaBevYsyy+4hLOcTXDSUA4tXwPa2Bi+BRoUTdYE2mFW7ytOJNEs3jTiHrCK6JRvTyU9lGkNDMNx9loIr+mRks+BSf70KxPtE9XCpCvXyWa/Q1JaIyZI7llCH45Dn4SKFn6L/JBw8G8xSTrZ3sBYBKOnUDbSCfc8ucQX97EyivSPURvTyImmjpsXDm2LBaEgAMADg==</SignatureValue>
    </Signature>
</Receipt>
```

Puedes usar cualquiera de estos ejemplos de recibo para probar tu código de validación.

## Validar un recibo


Después de obtener un recibo, necesitas que tu sistema back-end (un servicio web o algo similar) lo valide. A continuación te mostramos un ejemplo de ese proceso de validación en .NET Framework.

```CSharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Security.Cryptography;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IO;
using System.Security.Cryptography.Xml;
using System.Net;

namespace ReceiptVerificationSample
{
        public sealed class RSAPKCS1SHA256SignatureDescription : SignatureDescription
        {
            public RSAPKCS1SHA256SignatureDescription()
            {
                base.KeyAlgorithm = typeof(RSACryptoServiceProvider).FullName;
                base.DigestAlgorithm = typeof(SHA256Managed).FullName;
                base.FormatterAlgorithm = typeof(RSAPKCS1SignatureFormatter).FullName;
                base.DeformatterAlgorithm = typeof(RSAPKCS1SignatureDeformatter).FullName;
            }

            public override AsymmetricSignatureDeformatter CreateDeformatter(AsymmetricAlgorithm key)
            {
                if (key == null)
                {
                    throw new ArgumentNullException("key");
                }

                RSAPKCS1SignatureDeformatter deformatter = new RSAPKCS1SignatureDeformatter(key);
                deformatter.SetHashAlgorithm("SHA256");
                return deformatter;
            }

            public override AsymmetricSignatureFormatter CreateFormatter(AsymmetricAlgorithm key)
            {
                if (key == null)
                {
                    throw new ArgumentNullException("key");
                }

                RSAPKCS1SignatureFormatter formatter = new RSAPKCS1SignatureFormatter(key);
                formatter.SetHashAlgorithm("SHA256");
                return formatter;
            }

        }

        class Program
        {

            // Utility function to read the bytes from an HTTP response
            private static int ReadResponseBytes(byte[] responseBuffer, Stream resStream)
            {
                int count = 0;

                int numBytesRead = 0;
                int numBytesToRead = responseBuffer.Length;

                do
                {
                    count = resStream.Read(responseBuffer, numBytesRead, numBytesToRead);

                    numBytesRead += count;
                    numBytesToRead -= count;

                } while (count > 0);

                return numBytesRead;
            }

            public static X509Certificate2 RetrieveCertificate(string certificateId)
            {
                const int MaxCertificateSize = 10000;

                // We are attempting to retrieve the following url. The getAppReceiptAsync website at 
                // http://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.currentapp.getappreceiptasync.aspx
                // lists the following format for the certificate url.
                String certificateUrl = String.Format("https://go.microsoft.com/fwlink/?LinkId=246509&cid={0}", certificateId);

                // Make an HTTP GET request for the certificate
                HttpWebRequest request = (HttpWebRequest)HttpWebRequest.Create(certificateUrl);
                request.Method = "GET";

                HttpWebResponse response = (HttpWebResponse)request.GetResponse();

                // Retrieve the certificate out of the response stream
                byte[] responseBuffer = new byte[MaxCertificateSize];
                Stream resStream = response.GetResponseStream();
                int bytesRead = ReadResponseBytes(responseBuffer, resStream);

                if (bytesRead < 1)
                {
                    //TODO: Handle error here
                }

                return new X509Certificate2(responseBuffer);
            }

            static bool ValidateXml(XmlDocument receipt, X509Certificate2 certificate)
            {
                // Create the signed XML object.
                SignedXml sxml = new SignedXml(receipt);

                // Get the XML Signature node and load it into the signed XML object.
                XmlNode dsig = receipt.GetElementsByTagName("Signature", SignedXml.XmlDsigNamespaceUrl)[0];
                if (dsig == null)
                {
                    // If signature is not found return false
                    System.Console.WriteLine("Signature not found.");
                    return false;
                }

                sxml.LoadXml((XmlElement)dsig);

                // Check the signature
                bool isValid = sxml.CheckSignature(certificate, true);

                return isValid;
            }

            static void Main(string[] args)
            {
                // .NET does not support SHA256-RSA2048 signature verification by default, so register this algorithm for verification
                CryptoConfig.AddAlgorithm(typeof(RSAPKCS1SHA256SignatureDescription), "http://www.w3.org/2001/04/xmldsig-more#rsa-sha256");

                // Load the receipt that needs to be verified as an XML document
                XmlDocument xmlDoc = new XmlDocument();
                xmlDoc.Load("..\\..\\receipt.xml");

                // The certificateId attribute is present in the document root, retrieve it
                XmlNode node = xmlDoc.DocumentElement;
                string certificateId = node.Attributes["CertificateId"].Value;

                // Retrieve the certificate from the official site.
                // NOTE: For sake of performance, you would want to cache this certificate locally.
                //       Otherwise, every single call will incur the delay of certificate retrieval.
                X509Certificate2 verificationCertificate = RetrieveCertificate(certificateId);

                try
                {
                    // Validate the receipt with the certificate retrieved earlier
                    bool isValid = ValidateXml(xmlDoc, verificationCertificate);
                    System.Console.WriteLine("Certificate valid: " + isValid);
                }
                catch (Exception ex)
                {
                    System.Console.WriteLine(ex.ToString());
                }
            }
        }
}
```

 

 






<!--HONumber=Jun16_HO4-->


