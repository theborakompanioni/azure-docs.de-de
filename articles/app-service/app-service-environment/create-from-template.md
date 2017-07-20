---
title: Erstellen einer Azure App Service-Umgebung mit einer Resource Manager-Vorlage
description: Beschreibt das Erstellen einer externen oder ILB-Azure App Service-Umgebung mithilfe einer Resource Manager-Vorlage.
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 6eb7d43d-e820-4a47-818c-80ff7d3b6f8e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.translationtype: Human Translation
ms.sourcegitcommit: cb4d075d283059d613e3e9d8f0a6f9448310d96b
ms.openlocfilehash: 370ab7a32634f04b4776a2a9a84ecaacf11ea617
ms.contentlocale: de-de
ms.lasthandoff: 06/26/2017

---
# <a name="how-to-create-an-ase-using-azure-resource-manager-templates"></a>Erstellen einer ASE mit Azure Resource Manager-Vorlagen

## <a name="overview"></a>Übersicht
App Service-Umgebungen (App Service Environments, ASEs) können mit einem über das Internet zugänglichen Endpunkt oder einem Endpunkt auf einer internen Adresse im Azure Virtual Network (VNet) erstellt werden.  Beim Erstellen mit einem internen Endpunkt, wird dieser Endpunkt über eine Azure-Komponente, den sogenannten internen Lastenausgleich (internal load balancer, ILB), bereitgestellt.  Die ASE mit einem öffentlichen Endpunkt ist eine externe ASE, und die ASE auf einer internen IP-Adresse wird als ILB-ASE bezeichnet.  

Eine ASE kann mithilfe des Azure-Portals oder mit Azure Resource Manager-Vorlagen erstellt werden.  In diesem Artikel werden die Schritte und Syntaxelemente beschrieben, die zum Erstellen einer externen ASE oder einer ILB-ASE mit Azure Resource Manager-Vorlagen benötigt werden.  Weitere Informationen zum Erstellen einer ASE im Portal finden Sie unter [Create an external App Service Environment][MakeExternalASE] (Erstellen einer externen App Service-Umgebung) oder unter [Create and Use an Internal Load Balancer with an App Service Environment][MakeILBASE] (Erstellen und Verwenden eines internen Lastenausgleichs mit einer App Service-Umgebung).

Beim Erstellen einer ASE im Portal können Sie Ihr VNet entweder gleichzeitig erstellen, oder ein vorhandenes VNet auswählen, in das die Bereitstellung erfolgt.  Beim Erstellen aus einer Vorlage müssen Sie mit Folgendem beginnen: 
* Einem Resource Manager-VNet
* Einem Subnetz in diesem VNet.  Dabei empfiehlt sich ein /25-ASE-Subnetz mit 128 Adressen, damit eine spätere Erweiterung möglich ist.  Diese kann nach dem Erstellen der ASE nicht mehr geändert werden.
* Die Ressourcen-Id aus Ihrem VNet.  Sie können diese im Azure-Portal unter den Eigenschaften Ihres virtuellen Azure-Netzwerks abrufen.
* Das Abonnement, in das die Bereitstellung erfolgen soll
* Der Speicherort, in den die Bereitstellung erfolgen soll

Gehen Sie zum Automatisieren der ASE-Erstellung wie folgt vor:

1. Erstellen Sie die ASE aus einer Vorlage.  Wenn Sie eine externe ASE erstellen, müssen Sie nur diesen einen Schritt ausführen.  Wenn Sie eine ILB-ASE erstellen, sind einige weitere Schritte erforderlich.
2. Nach dem Erstellen Ihrer ILB-ASE wird ein SSL-Zertifikat hochgeladen, das Ihrer ILB-ASE-Domäne entspricht.
3. Das hochgeladene SSL-Zertifikat wird der ILB-ASE als SSL-Standardzertifikat zugewiesen.  Dieses SSL-Zertifikat wird für SSL-Datenverkehr zu Apps in der ILB-ASE verwendet, wenn die Apps mit der allgemeinen Stammdomäne adressiert werden, die der ASE zugewiesen ist (z.B. https://someapp.mycustomrootcomain.com).


## <a name="creating-the-ase"></a>Erstellen der ASE
Eine Azure Resource Manager-Beispielvorlage, die die ASE erstellt, und die zugeordnete Parameterdatei sind [hier][quickstartasev2create] bei GitHub verfügbar.

Wenn Sie eine ILB-ASE erstellen möchten, müssen Sie die [hier][quickstartilbasecreate] verfügbaren Resource Manager-Vorlagen verwenden.  Diese sind für diesen Anwendungsfall bestimmt.  Die meisten Parameter in der Datei *azuredeploy.parameters.json* gelten für die Erstellung beider ILB-ASEs sowie für externe ASEs.  In der Liste unten sind die Parameter angegeben, die für die Erstellung einer ILB-ASE eine besondere Bedeutung haben bzw. eindeutig sind:

* *interalLoadBalancingMode*: Sollte in den meisten Fällen auf „3“ festgelegt werden. Dies bedeutet, dass sowohl der HTTP/HTTPS-Datenverkehr über die Ports 80/443 als auch die Steuer-/Datenkanalports, auf die der FTP-Dienst in der ASE lauscht, an eine per ILB zugeordnete Adresse des virtuellen Netzwerks gebunden sind.  Falls diese Eigenschaft stattdessen auf 2 festgelegt ist, sind nur die auf den FTP-Dienst bezogenen Ports (Steuer- und Datenkanäle) an eine ILB-Adresse gebunden. Der HTTP/HTTPS-Datenverkehr verbleibt auf der öffentlichen VIP.
* *dnsSuffix*: Dieser Parameter definiert die Standardstammdomäne, die der ASE zugewiesen wird.  In der öffentlichen Variante von Azure App Service lautet die Standardstammdomäne für alle Web-Apps *azurewebsites.net*.  Da eine ILB-ASE intern im virtuellen Netzwerk eines Kunden vorliegt, ergibt es keinen Sinn, die Standardstammdomäne des öffentlichen Diensts zu verwenden.  Stattdessen sollte eine ILB-ASE über eine Standardstammdomäne verfügen, für die die Verwendung im internen virtuellen Netzwerk eines Unternehmens sinnvoll ist.  Die fiktive Contoso Corporation kann beispielsweise die Standardstammdomäne *internal-contoso.com* für Apps verwenden, für die beabsichtigt ist, dass sie nur im virtuellen Netzwerk von Contoso auflösbar und zugänglich sind. 
* *ipSslAddressCount*: Für diesen Parameter wird in der Datei *azuredeploy.json* automatisch der Wert 0 verwendet, da ILB-ASEs nur über eine einzelne ILB-Adresse verfügen.  Für eine ILB-ASE sind keine expliziten IP-SSL-Adressen vorhanden, sodass der IP-SSL-Adresspool für eine ILB-ASE auf 0 festgelegt werden muss. Andernfalls tritt ein Bereitstellungsfehler auf. 

Nachdem die Datei *azuredeploy.parameters.json* ausgefüllt wurde, kann die ASE mit dem folgenden PowerShell-Codeausschnitt erstellt werden.  Ändern Sie die Dateipfade (PATH) in den Speicherort, an dem sich die Azure Resource Manager-Vorlagendateien auf Ihrem Computer befinden.  Geben Sie auch Ihre eigenen Werte für den Azure Resource Manager-Bereitstellungsnamen und Ressourcengruppennamen an.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Nachdem die Azure Resource Manager-Vorlage übermittelt wurde, dauert es rund eine Stunde, bis die ASE erstellt wird.  Nach Abschluss der Erstellung wird die ASE auf der Benutzeroberfläche des Portals in der Liste mit den App Service-Umgebungen für das Abonnement angezeigt, über das die Bereitstellung ausgelöst wurde.

## <a name="uploading-and-configuring-the-default-ssl-certificate"></a>Hochladen und Konfigurieren des SSL-Standardzertifikats
Nach der Erstellung der ILB-ASE sollte ein SSL-Zertifikat der ASE als SSL-Standardzertifikat zugeordnet werden, das zum Herstellen von SSL-Verbindungen mit Apps verwendet wird.  Für das Beispiel mit der fiktiven Contoso Corporation gilt: Wenn das DNS-Standardsuffix der ASE *internal-contoso.com* lautet, ist für eine Verbindung mit *https://beliebige-app.internal-contoso.com* ein SSL-Zertifikat erforderlich, das für **.internal-contoso.com* gültig ist. 

Es gibt viele Möglichkeiten, ein gültiges SSL-Zertifikat zu beschaffen, z.B. interne Zertifizierungsstellen, Erwerb eines Zertifikats von einem externen Aussteller und Verwendung eines selbstsignierten Zertifikats.  Unabhängig von der Quelle des SSL-Zertifikats müssen die folgenden Zertifikatattribute richtig konfiguriert werden:

* *Antragsteller*: Dieses Attribut muss auf **.ihre-stammdomäne-hier.com* festgelegt werden.
* *Alternativer Antragstellername*: Dieses Attribut muss sowohl **.ihre-stammdomäne-hier.com* als auch **.scm.ihre-stammdomäne-hier.com* enthalten.  Der Grund für den zweiten Eintrag ist, dass für SSL-Verbindungen zur SCM/Kudu-Website, die jeder App zugeordnet ist, eine Adresse im Format *your-app-name.scm.your-root-domain-here.com*verwendet wird.

Wenn ein gültiges SSL-Zertifikat vorhanden ist, sind zwei weitere Vorbereitungsschritte erforderlich.  Das SSL-Zertifikat muss in eine PFX-Datei konvertiert bzw. in diesem Format gespeichert werden.  Beachten Sie, dass die PFX-Datei alle Zwischen- und Stammzertifikate enthalten und mit einem Kennwort geschützt werden muss.

Anschließend muss die sich ergebende PFX-Datei in eine Base64-Zeichenfolge konvertiert werden, da das SSL-Zertifikat mit einer Azure Resource Manager-Vorlage hochgeladen wird.  Da Azure Resource Manager-Vorlagen Textdateien sind, muss die PFX-Datei in eine Base64-Zeichenfolge konvertiert werden, damit sie als Parameter der Vorlage einbezogen werden kann.

Der folgende PowerShell-Codeausschnitt veranschaulicht ein Beispiel für das Generieren eines selbstsignierten Zertifikats, das Exportieren des Zertifikats als PFX-Datei, das Konvertieren der PFX-Datei in eine Base64-codierte Zeichenfolge und das anschließende Speichern der Base64-codierten Zeichenfolge in eine separate Datei.  Der PowerShell-Code für die Base64-Codierung wurde aus dem [Blog zu PowerShell-Skripts][examplebase64encoding] übernommen und angepasst.

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"

    $certThumbprint = "cert:\localMachine\my\" + $certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText

    $fileName = "exportedcert.pfx"
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password     

    $fileContentBytes = get-content -encoding byte $fileName
    $fileContentEncoded = [System.Convert]::ToBase64String($fileContentBytes)
    $fileContentEncoded | set-content ($fileName + ".b64")

Nachdem das SSL-Zertifikat erfolgreich generiert und in eine Base64-codierte Zeichenfolge konvertiert wurde, kann die Azure Resource Manager-Beispielvorlage auf GitHub zum [Konfigurieren des SSL-Standardzertifikats][quickstartconfiguressl] verwendet werden.

Die Parameter in der Datei *azuredeploy.parameters.json* sind nachfolgend aufgeführt:

* *appServiceEnvironmentName*: Der Name der ILB-ASE, die konfiguriert wird.
* *existingAseLocation*: Die Textzeichenfolge mit der Azure-Region, in der die ILB-ASE bereitgestellt wurde.  Beispiel: „USA, Süden-Mitte“.
* *pfxBlobString*: Die Base64-codierte Zeichenfolgendarstellung der PFX-Datei.  Bei Verwendung des weiter oben angegebenen Codeausschnitts kopieren Sie die in „exportedcert.pfx.b64“ enthaltene Zeichenfolge und fügen sie als Wert des Attributs *pfxBlobString* ein.
* *password*: Das Kennwort, das zum Schützen der PFX-Datei verwendet wird.
* *certificateThumbprint*: Der Fingerabdruck des Zertifikats.  Wenn Sie diesen Wert aus PowerShell abrufen (z.B.*$certificate.Thumbprint* aus dem Codeausschnitt weiter oben), können Sie den Wert unverändert nutzen.  Falls Sie den Wert aus dem Windows-Zertifikatdialogfeld kopieren, müssen Sie die überflüssigen Leerzeichen entfernen.  *certificateThumbprint* sollte etwa wie folgt aussehen: AF3143EB61D43F6727842115BB7F17BBCECAECAE.
* *certificateName*: Ein benutzerfreundlicher Zeichenfolgenbezeichner zum Identifizieren des Zertifikats, den Sie selbst wählen können.  Der Name wird als Teil des eindeutigen Azure Resource Manager-Bezeichners für die Entität *Microsoft.Web/certificates* verwendet, die das SSL-Zertifikat darstellt.  Der Name **muss** auf folgendes Suffix enden: \_yourASENameHere_InternalLoadBalancingASE.  Dieses Suffix ist ein Indikator für das Portal, dass das Zertifikat zum Sichern einer für den internen Lastenausgleich geeigneten App Service-Umgebung genutzt wird.

Ein gekürztes Beispiel für *azuredeploy.parameters.json* sehen Sie hier:

    {
         "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json",
         "contentVersion": "1.0.0.0",
         "parameters": {
              "appServiceEnvironmentName": {
                   "value": "yourASENameHere"
              },
              "existingAseLocation": {
                   "value": "East US 2"
              },
              "pfxBlobString": {
                   "value": "MIIKcAIBAz...snip...snip...pkCAgfQ"
              },
              "password": {
                   "value": "PASSWORDGOESHERE"
              },
              "certificateThumbprint": {
                   "value": "AF3143EB61D43F6727842115BB7F17BBCECAECAE"
              },
              "certificateName": {
                   "value": "DefaultCertificateFor_yourASENameHere_InternalLoadBalancingASE"
              }
         }
    }

Nachdem die Daten in die Datei *azuredeploy.parameters.json* eingefügt wurden, kann das SSL-Standardzertifikat mit dem folgenden PowerShell-Codeausschnitt konfiguriert werden.  Ändern Sie die Dateipfade (PATH) in den Speicherort, an dem sich die Azure Resource Manager-Vorlagendateien auf Ihrem Computer befinden.  Geben Sie auch Ihre eigenen Werte für den Azure Resource Manager-Bereitstellungsnamen und Ressourcengruppennamen an.

    $templatePath="PATH\azuredeploy.json"
    $parameterPath="PATH\azuredeploy.parameters.json"

    New-AzureRmResourceGroupDeployment -Name "CHANGEME" -ResourceGroupName "YOUR-RG-NAME-HERE" -TemplateFile $templatePath -TemplateParameterFile $parameterPath

Nachdem die Azure Resource Manager-Vorlage übermittelt wurde, dauert die Anwendung der Änderung ca. 40 Minuten pro ASE-Front-End.  Bei einer ASE mit Standardgröße, für die zwei Front-Ends genutzt werden, dauert es beispielsweise ungefähr eine Stunde und 20 Minuten, bis der Vorgang für die Vorlage abgeschlossen ist.  Während der Ausführung der Vorlage kann die ASE nicht skaliert werden.  

Nachdem die Vorlage abgeschlossen wurde, kann auf die Apps in der ILB-ASE per HTTPS zugegriffen werden, und die Verbindungen werden mit dem SSL-Standardzertifikat geschützt.  Das SSL-Standardzertifikat wird verwendet, wenn Apps in der ILB-ASE mit einer Kombination aus dem Anwendungsnamen und dem Standardhostnamen adressiert werden.  Für *https://mycustomapp.internal-contoso.com* wird beispielsweise das SSL-Standardzertifikat für **.internal-contoso.com* verwendet.

Wie bei Apps, die unter dem öffentlichen mehrinstanzenfähigen Dienst ausgeführt werden, können Entwickler für einzelne Apps auch benutzerdefinierte Hostnamen und dann eindeutige SNI SSL-Zertifikatbindungen konfigurieren.  

## <a name="asev1"></a>ASEv1 ##
Es gibt zwei Versionen der App Service-Umgebung: ASEv1 und ASEv2. Schwerpunkt der vorherigen Informationen war ASEv2. In diesem Abschnitt werden die Unterschiede zwischen ASEv1 und ASEv2 aufgezeigt.

In ASEv1 müssen Sie alle Ressourcen manuell verwalten. Dies schließt Front-Ends, Worker und IP-Adressen ein, die für IP-basiertes SSL verwendet werden. Bevor Sie Ihren App Service-Plan hochskalieren können, müssen Sie zunächst den Workerpool skalieren, den Sie zum Hosten verwenden möchten.

ASEv1 verwendet ein anderes Preismodell als ASEv2. In ASEv1 müssen Sie jeden zugewiesenen Prozessorkern bezahlen. Dazu gehören Prozessorkerne, die für Front-Ends oder Worker verwendet werden, die keine Workloads hosten. In ASEv1 beträgt die maximale Standardgröße einer App Service-Umgebung insgesamt 55 Hosts. Dazu gehören Worker und Front-Ends. ASEv1 hat den einen Vorteil, dass die Bereitstellung in einem klassischen virtuellen Netzwerk sowie als virtuelles Resource Manager-Netzwerk möglich ist. Weitere Informationen zu ASEv1 finden Sie unter [Einführung in die App Service-Umgebung v1][ASEv1Intro].

Wenn Sie eine ASEv1 mithilfe einer Resource Manager-Vorlage erstellen möchten, beginnen Sie mit dem [Erstellen einer ILB-ASE v1 mit einer Resource Manager-Vorlage][ILBASEv1Template].


<!--Links-->
[quickstartilbasecreate]: http://azure.microsoft.com/documentation/templates/201-web-app-asev2-ilb-create
[quickstartasev2create]: http://azure.microsoft.com/documentation/templates/201-web-app-asev2-create
[quickstartconfiguressl]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl
[quickstartwebapponasev2create]: http://azure.microsoft.com/documentation/templates/201-web-app-asp-app-on-asev2-create
[examplebase64encoding]: http://powershellscripts.blogspot.com/2007/02/base64-encode-file.html 
[configuringDefaultSSLCertificate]: https://azure.microsoft.com/documentation/templates/201-web-app-ase-ilb-configure-default-ssl/
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[ASEReadme]: ./readme.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: ../../app-service-web/app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: ../../app-service-web/app-service-app-service-environment-intro.md
[webapps]: ../../app-service-web/app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[APIapps]: ../../app-service-api/app-service-api-apps-why-best-platform.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../../app-service-web/web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../../app-service-web/web-sites-deploy.md
[ASEWAF]: ../../app-service-web/app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[ILBASEv1Template]: ../../app-service-web/app-service-app-service-environment-create-ilb-ase-resourcemanager.md

