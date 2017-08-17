---
title: Erstellen des ersten Jenkins-Masters auf einer Linux-VM (Ubuntu) in Azure
description: "Verwenden Sie die Lösungsvorlage zum Bereitstellen von Jenkins."
services: app-service\web
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: multiple
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: Jenkins
ms.translationtype: HT
ms.sourcegitcommit: 80fd9ee9b9de5c7547b9f840ac78a60d52153a5a
ms.openlocfilehash: 06d6d305eb9711768dc62a04726359e6280d1b69
ms.contentlocale: de-de
ms.lasthandoff: 08/14/2017

---

# <a name="create-your-first-jenkins-master-on-a-linux-ubuntu-vm-on-azure"></a>Erstellen des ersten Jenkins-Masters auf einer Linux-VM (Ubuntu) in Azure

Dieser Schnellstart veranschaulicht die Installation der neuesten stabilen Jenkins-Version auf einem virtuellen Linux-Computer (Ubuntu 14.04 LTS) zusammen mit den Tools und Plug-Ins, die für die Arbeit mit Azure konfiguriert sind. Zu diesen Tools zählen:
<ul>
<li>Git für die Quellcodeverwaltung</li>
<li>Azure-Anmeldeinformationen-Plug-In für sichere Verbindungen</li>
<li>Azure VM-Agents-Plug-In für elastische Builds, Tests und Continuous Integration</li>
<li>Azure Storage-Plug-In für das Speichern von Artefakten</li>
<li>Azure-Befehlszeilenschnittstelle zum Bereitstellen von Apps mithilfe von Skripts</li>
</ul>

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Erstellen eines kostenlosen Azure-Kontos
> * Erstellen eines Jenkins-Masters auf einer Azure-VM mit einer Lösungsvorlage 
> * Durchführen der Erstkonfiguration für Jenkins
> * Installieren der vorgeschlagenen Plug-Ins

Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) erstellen, bevor Sie beginnen.

## <a name="create-the-vm-in-azure-by-deploying-the-solution-template-for-jenkins"></a>Erstellen des virtuellen Computers in Azure durch Bereitstellen der Lösungsvorlage für Jenkins

Mit Azure-Schnellstartvorlagen können Sie schnell und zuverlässig komplexe Technologien in Azure bereitstellen.  Mit dem Azure Resource Manager können Sie Ihre Anwendungen mit einer [deklarativen Vorlage](https://azure.microsoft.com/en-us/resources/templates/?term=jenkins) bereitstellen. In einer Vorlage können Sie mehrere Dienste zusammen mit ihren Abhängigkeiten bereitstellen. Sie verwenden die gleiche Vorlage, um Ihre Anwendung während jeder Phase des Anwendungslebenszyklus wiederholt bereitzustellen.

Unter [Pläne + Preise](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azure-oss.jenkins?tab=Overview) finden Sie Informationen zu dieser Vorlage und den Tarifoptionen.

Navigieren Sie zum [Marketplace-Image für Jenkins](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/azure-oss.jenkins?tab=Overview), und klicken Sie auf **JETZT ANFORDERN**.  

Klicken Sie im Azure-Portal auf **Erstellen**.  Diese Vorlage erfordert die Verwendung von Resource Manager, daher ist die Dropdownliste „Modell“ für diese Vorlage deaktiviert.
   
![Azure-Portal, Dialogfeld](./media/install-jenkins-solution-template/ap-create.png)

Auf der Registerkarte **Grundeinstellungen konfigurieren**:

![Grundeinstellungen konfigurieren](./media/install-jenkins-solution-template/ap-basic.png)

* Geben Sie einen Namen für die Jenkins-Instanz an.
* Wählen Sie einen Datenträgertyp für den virtuellen Computer aus.  Wählen Sie für Produktionsworkloads für eine bessere Leistung eine größere VM und SSD aus.  Weitere Informationen zu Azure-Datenträgertypen finden Sie [hier](https://docs.microsoft.com/en-us/azure/storage/storage-premium-storage).
* Benutzername: muss die Längenanforderungen erfüllen und darf keine reservierten Wörter oder nicht unterstützten Zeichen enthalten. Namen wie „Admin“ sind nicht zulässig.  Weitere Informationen zu den Anforderungen an Benutzernamen und Kennwörter finden Sie [hier](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/faq).
* Authentifizierungstyp: Erstellen Sie eine Instanz, die durch ein Kennwort oder einen [öffentlichen SSH-Schlüssel](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/ssh-from-windows) geschützt ist. Wenn Sie ein Kennwort verwenden, muss dieses drei der folgenden Anforderungen erfüllen: ein Kleinbuchstabe, ein Großbuchstabe, eine Ziffer und ein Sonderzeichen.
* Behalten Sie für den Jenkins-Releasetyp **LTS** bei.
* Wählen Sie ein Abonnement aus.
* Erstellen Sie eine Ressourcengruppe, oder verwenden Sie eine bereits vorhandene leere Ressourcengruppe. 
* Wählen Sie einen Standort aus.

Auf der Registerkarte **Weitere Optionen konfigurieren**:

![Einrichten weiterer Optionen](./media/install-jenkins-solution-template/ap-addtional.png)

* Geben Sie eine Domänennamenbezeichnung zur eindeutigen Identifizierung des Jenkins-Masters ein.

Klicken Sie auf **OK**, um mit dem nächsten Schritt fortzufahren. 

Wenn die Validierung erfolgreich war, klicken Sie auf **OK**, um die Vorlage und die Parameter herunterzuladen. 

Wählen Sie als Nächstes **Kaufen** aus, um alle Ressourcen bereitzustellen.

## <a name="setup-ssh-port-forwarding"></a>Einrichten der SSH-Portweiterleitung

Standardmäßig verwendet die Jenkins-Instanz das http-Protokoll und lauscht an Port 8080. Die Authentifizierung von Benutzern sollte jedoch nicht über unsichere Protokolle erfolgen.
    
Richten Sie eine Portweiterleitung ein, um die Jenkins-Benutzeroberfläche auf dem lokalen Computer anzuzeigen.

### <a name="if-you-are-using-windows"></a>Bei Verwendung von Windows:

Installieren Sie PuTTY, und führen Sie diesen Befehl aus, wenn Sie zum Schützen von Jenkins ein Kennwort verwenden:
```
putty.exe -ssh -L 8080:localhost:8080 <username>@<Domain name label>.<location>.cloudapp.azure.com
```
* Geben Sie zum Anmelden das Kennwort ein.

![Geben Sie zum Anmelden das Kennwort ein.](./media/install-jenkins-solution-template/jenkins-pwd.png)

Wenn Sie SSH verwenden, führen Sie diesen Befehl aus:
```
putty -i <private key file including path> -L 8080:localhost:8080 <username>@<Domain name label>.<location>.cloudapp.azure.com
```

### <a name="if-you-are-using-linux-or-mac"></a>Wenn Sie Linux oder Mac verwenden:

Wenn Sie ein Kennwort verwenden, um Ihren Jenkins-Master zu schützen, führen Sie diesen Befehl aus:
```
ssh -L 8080:localhost:8080 <username>@<Domain name label>.<location>.cloudapp.azure.com
```
* Geben Sie zum Anmelden das Kennwort ein.

Wenn Sie SSH verwenden, führen Sie diesen Befehl aus:
```
ssh -i <private key file including path> -L 8080:localhost:8080 <username>@<Domain name label>.<location>.cloudapp.azure.com
```

## <a name="connect-to-jenkins"></a>Herstellen einer Verbindung mit Jenkins
Nachdem Sie Ihren Tunnel gestartet haben, navigieren Sie auf dem lokalen Computer zu http://localhost:8080/.

Entsperren Sie das Jenkins-Dashboard beim ersten Mal mit dem anfänglichen Administratorkennwort.

![Entsperren von Jenkins](./media/install-jenkins-solution-template/jenkins-unlock.png)

Zum Erhalt eines Tokens melden Sie sich per SSH auf dem virtuellen Computer an, und führen Sie `sudo cat /var/lib/jenkins/secrets/initialAdminPassword` aus.

![Entsperren von Jenkins](./media/install-jenkins-solution-template/jenkins-ssh.png)

Sie werden aufgefordert, die vorgeschlagenen Plug-Ins zu installieren.

Als Nächstes erstellen Sie einen Administratorbenutzer für Ihren Jenkins-Master.

Ihre Jenkins-Instanz ist jetzt einsatzbereit! Eine schreibgeschützte Ansicht finden Sie unter http://\<öffentlicher DNS-Name der soeben erstellten Instanz\>.

![Jenkins ist bereit!](./media/install-jenkins-solution-template/jenkins-welcome.png)

## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial haben Sie Folgendes durchgeführt:

> [!div class="checklist"]
> * Sie haben einen Jenkins-Master mit der Lösungsvorlage erstellt.
> * Sie haben die Erstkonfiguration von Jenkins durchgeführt.
> * Sie haben Plug-Ins installiert.

Unter diesem Link erfahren Sie, wie Sie Azure-VM-Agents für die Continuous Integration mit Jenkins verwenden.

> [!div class="nextstepaction"]
> [Azure-VMs als Jenkins-Agents](jenkins-azure-vm-agents.md)

