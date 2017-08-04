---
title: "Verwenden Sie Azure-VM-Agents für die Continuous Integration mit Jenkins."
description: Azure-VM-Agents als untergeordnete Jenkins-Server
services: multiple
documentationcenter: 
author: mlearned
manager: douge
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 6/7/2017
ms.author: mlearned
ms.custom: Jenkins
ms.translationtype: Human Translation
ms.sourcegitcommit: 1e6f2b9de47d1ce84c4043f5f6e73d462e0c1271
ms.openlocfilehash: 5f2df414b4d0e8798b7ed6d90d0ea0fb79d42fc2
ms.contentlocale: de-de
ms.lasthandoff: 06/21/2017

---
# <a name="use-azure-vm-agents-for-continuous-integration-with-jenkins"></a>Verwenden Sie Azure-VM-Agents für die Continuous Integration mit Jenkins.

Dieser Schnellstart veranschaulicht die Verwendung des Azure-VM-Agents-Plug-Ins für Jenkins zum Erstellen eines bedarfsgesteuerten Linux-Agents (Ubuntu) in Azure.

## <a name="prerequisites"></a>Voraussetzungen

So führen Sie diesen Schnellstart durch

* Wenn Sie noch nicht über einen Jenkins-Master verfügen, beginnen Sie mit der [Lösungsvorlage](install-jenkins-solution-template.md). 
* Lesen Sie [Erstellen eines Azure-Dienstprinzipals mit Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/create-an-azure-service-principal-azure-cli?toc=%2fazure%2fazure-resource-manager%2ftoc.json), wenn Sie noch nicht über einen Azure-Dienstprinzipal verfügen.

## <a name="install-azure-vm-agents-plugin"></a>Installieren des Azure-VM-Agents-Plug-In

Wenn Sie die [Lösungsvorlage](install-jenkins-solution-template.md) verwendet haben, ist das Azure-VM-Agent-Plug-In bereits auf dem Jenkins-Master installiert.

Installieren Sie andernfalls das **Azure-VM-Agents**-Plug-In vom Jenkins-Dashboard.

## <a name="configure-the-plugin"></a>Konfigurieren des Plug-Ins

* Klicken Sie im Jenkins-Dashboard auf **Manage Jenkins** (Jenkins verwalten). Scrollen Sie auf der Seite nach unten, und suchen Sie den Abschnitt mit der Dropdownliste **Add new cloud** (Neue Cloud hinzufügen). Wählen Sie die Menüoption **Microsoft Azure-VM-Agents** aus.
* Wählen Sie in der Dropdownliste mit Azure-Anmeldeinformationen ein vorhandenes Konto aus.  Zum Hinzufügen eines neuen **Microsoft Azure-Dienstprinzipals** geben Sie die folgenden Werte ein: Abonnement-ID, Client-ID, geheimer Clientschlüssel und OAuth 2.0-Token-Endpunkt.

![Azure-Anmeldeinformationen](./media/jenkins-azure-vm-agents/service-principal.png)

* Klicken Sie auf **Verify configuration** (Konfiguration überprüfen), um sicherzustellen, dass die Profilkonfiguration richtig ist.
* Speichern Sie die Konfiguration, und fahren Sie mit dem nächsten Schritt fort.

## <a name="template-configuration"></a>Vorlagenkonfiguration

### <a name="general-configuration"></a>Allgemeine Konfiguration
Als Nächstes konfigurieren Sie eine Vorlage für das Definieren eines Azure-VM-Agents. 

* Klicken Sie auf **Add** (Hinzufügen), um eine Vorlage hinzuzufügen. 
* Geben Sie einen Namen für die neue Vorlage an. 
* Geben Sie als Bezeichnung „ubuntu“ ein. Diese Bezeichnung wird während der Auftragskonfiguration verwendet.
* Wählen Sie die gewünschte Region im Kombinationsfeld aus.
* Wählen Sie die gewünschte Größe für den virtuellen Computer aus.
* Geben Sie den Namen des Azure Storage-Kontos an, oder lassen Sie das Feld leer, um den vorgegebenen Namen „jenkinsarmst“ zu verwenden.
* Geben Sie die Vermerkdauer in Minuten an. Diese Einstellung legt die Anzahl der Minuten fest, die Jenkins warten kann, bevor ein Agent im Leerlauf automatisch gelöscht wird. Sie können auch 0 angeben, wenn Sie nicht möchten, dass Agents im Leerlauf automatisch gelöscht werden.

![Allgemeine Konfiguration](./media/jenkins-azure-vm-agents/general-config.png)

### <a name="image-configuration"></a>Imagekonfiguration

Um einen Agent für Linux (Ubuntu) zu erstellen, wählen Sie **Image reference** (Imageverweis) aus und verwenden die folgende Konfiguration als Beispiel. Im [Azure Marketplace](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute?subcategories=virtual-machine-images&page=1) finden Sie die neuesten unterstützten Azure-Images.

* Image Publisher (Imageherausgeber): Canonical
* Image Offer (Imageangebot): UbuntuServer
* Image Sku: 14.04.5-LTS
* Image version: latest (Imageversion: neueste)
* OS Type (Betriebssystemtyp): Linux
* Launch method (Startmethode): SSH
* Geben Sie Administratoranmeldeinformationen an.
* Geben Sie als VM-Initialisierungsskript Folgendes ein:
```
# Install Java
sudo apt-get -y update
sudo apt-get install -y openjdk-7-jdk
sudo apt-get -y update --fix-missing
sudo apt-get install -y openjdk-7-jdk
```
![Imagekonfiguration](./media/jenkins-azure-vm-agents/image-config.png)

* Klicken Sie auf **Verify Template** (Vorlage überprüfen), um die Konfiguration zu überprüfen.
* Klicken Sie auf **Speichern**.

## <a name="create-a-job-in-jenkins"></a>Erstellen eines Auftrags in Jenkins

* Klicken Sie im Jenkins-Dashboard auf **New Item**. 
* Geben Sie einen Namen ein, wählen Sie **Freestyle project** (Freies Projekt) aus, und klicken Sie auf **OK**.
* Wählen Sie auf der Registerkarte **General** (Allgemein) die Option „Restrict where this project can be run“ (Ausführungsort dieses Projekts beschränken) aus, und geben Sie in „Label Expression“ (Bezeichner) „ubuntu“ ein. In der Dropdownliste wird jetzt „ubuntu“ angezeigt.
* Klicken Sie auf **Speichern**.

![Einrichten eines Auftrags](./media/jenkins-azure-vm-agents/job-config.png)

## <a name="build-your-new-project"></a>Erstellen des neuen Projekts

* Wechseln Sie zurück zum Jenkins-Dashboard.
* Klicken Sie mit der rechten Maustaste auf den neuen Auftrag, den Sie erstellt haben, und klicken Sie dann auf **Build now** (Jetzt erstellen). Ein Build wird gestartet. 
* Nachdem der Build abgeschlossen ist, wechseln Sie zu **Console output** (Konsolenausgabe). Sie sehen, dass der Build remote in Azure ausgeführt wurde.

![Konsolenausgabe](./media/jenkins-azure-vm-agents/console-output.png)

## <a name="reference"></a>Referenz

* Azure Friday-Video: [Continuous Integration with Jenkins using Azure VM agents](https://channel9.msdn.com/Shows/Azure-Friday/Continuous-Integration-with-Jenkins-Using-Azure-VM-Agents) (Continuous Integration mit Jenkins mithilfe von Azure-VM-Agents)
* Supportinformationen und Konfigurationsoptionen: [Azure VM Agent Jenkins Plugin Wiki](https://wiki.jenkins-ci.org/display/JENKINS/Azure+VM+Agents+Plugin) (Wiki zum Azure-VM-Agent-Plug-In für Jenkins) 


