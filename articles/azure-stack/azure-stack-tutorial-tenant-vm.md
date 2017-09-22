---
title: "Verfügbarmachen von virtuellen Computern für Ihre Azure Stack-Benutzer | Microsoft-Dokumentation"
description: "Enthält ein Tutorial zum Verfügbarmachen von virtuellen Computern in Azure Stack."
services: azure-stack
documentationcenter: 
author: vhorne
manager: 
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/22/2017
ms.author: victorh
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: d2f38bc1c0b97e408f619f3ea2f704725e3bb460
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="make-virtual-machines-available-to-your-azure-stack-users"></a>Verfügbarmachen von virtuellen Computern für Ihre Azure Stack-Benutzer
Als Azure Stack-Cloudadministrator können Sie Angebote erstellen, die von Ihren Benutzern (ggf. auch als Mandanten bezeichnet) abonniert werden können. Mit ihrem Abonnement können Benutzer dann Azure Stack-Dienste nutzen.

In diesem Artikel wird veranschaulicht, wie Sie ein Angebot erstellen und anschließend testen. Für den Test melden Sie sich im Portal als Benutzer an, abonnieren das Angebot und erstellen mit dem Abonnement dann einen virtuellen Computer.

Sie lernen Folgendes:

> [!div class="checklist"]
> * Erstellen von Angeboten
> * Hinzufügen eines Image
> * Testen des Angebots


Dienste werden in Azure Stack mithilfe von Abonnements, Angeboten und Plänen für Benutzer bereitgestellt. Die Benutzer können mehrere Angebote abonnieren. Angebote können einen oder mehrere Pläne enthalten und die Pläne wiederum einen oder mehrere Dienste.

![Abonnements, Angebote und Pläne](media/azure-stack-key-features/image4.png)

Weitere Informationen finden Sie unter [Wichtige Features und Konzepte in Azure Stack](azure-stack-key-features.md).

## <a name="create-an-offer"></a>Erstellen von Angeboten

Sie können jetzt die Einrichtung für Ihre Benutzer durchführen. Wenn Sie den Prozess starten, werden Sie nacheinander zum Erstellen des Angebots, eines Plans und von Kontingenten aufgefordert.

3. **Erstellen von Angeboten**

   Angebote sind Gruppen mit einem oder mehreren Plänen, die Anbieter Benutzern zum Erwerben oder Abonnieren anbieten.

   a. Führen Sie die [Anmeldung](azure-stack-connect-azure-stack.md) am Portal als Cloudadministrator durch, und klicken Sie anschließend auf **Neu** > **Mandantenangebote + Pläne** > **Angebot**.
   ![Neues Angebot](media/azure-stack-tutorial-tenant-vm/image01.png)

   b. Geben Sie im Abschnitt **Neues Angebot** die Werte für **Anzeigename** und **Ressourcenname** ein, und wählen Sie anschließend eine neue oder vorhandene **Ressourcengruppe** aus. Der Anzeigename ist der verständliche Anzeigename des Angebots. Nur der Cloudbetreiber kann den Ressourcennamen sehen. Es handelt sich um den Namen, mit dem Administratoren das Angebot als Azure-Ressourcen-Manager-Ressource bearbeiten.

   ![Anzeigename](media/azure-stack-tutorial-tenant-vm/image02.png)

   c. Klicken Sie auf **Basispläne** und im Abschnitt **Plan** auf **Hinzufügen**, um dem Angebot einen neuen Plan hinzuzufügen.

   ![Hinzufügen eines Plans](media/azure-stack-tutorial-tenant-vm/image03.png)

   d. Geben Sie im Abschnitt **Neuer Plan** Werte für die Optionen **Anzeigename** und **Ressourcenname** ein. Der Anzeigename ist der verständliche Anzeigename des Plans, der für Benutzer angezeigt wird. Nur der Cloudbetreiber kann den Ressourcennamen sehen. Dieser Name wird von Cloudbetreibern verwendet, um mit dem Plan als Azure Resource Manager-Ressource zu arbeiten.

   ![Anzeigename des Plans](media/azure-stack-tutorial-tenant-vm/image04.png)

   e. Klicken Sie auf **Dienste**, wählen Sie **Microsoft.Compute**, **Microsoft.Network** und **Microsoft.Storage**, und klicken Sie dann auf **Auswählen**.

   ![Dienste des Plans](media/azure-stack-tutorial-tenant-vm/image05.png)

   f. Klicken Sie auf **Kontingente**, und wählen Sie dann den ersten Dienst aus, für den Sie ein Kontingent erstellen möchten. Führen Sie für ein IaaS-Kontingent diese Schritte für die Compute-, Netzwerk- und Speicherdienste aus.

   In diesem Beispiel erstellen Sie zuerst ein Kontingent für den Computedienst. Wählen Sie in der Liste mit den Namespaces den Namespace **Microsoft.Compute** aus, und klicken Sie anschließend auf **Neues Kontingent erstellen**.
   
   ![Erstellen eines neuen Kontingents](media/azure-stack-tutorial-tenant-vm/image06.png)

   g. Geben Sie im Abschnitt **Kontingent erstellen** einen Namen für das Kontingent ein, und legen Sie die gewünschten Parameter für das Kontingent fest. Klicken Sie anschließend auf **OK**.

   ![Namen des Kontingents](media/azure-stack-tutorial-tenant-vm/image07.png)

   h. Wählen Sie für **Microsoft.Compute** nun das von Ihnen erstellte Kontingent aus.

   ![Auswählen des Kontingents](media/azure-stack-tutorial-tenant-vm/image08.png)

   Wiederholen Sie diese Schritte für den Netzwerk- und Speicherdienst, und klicken Sie dann im Abschnitt **Kontingente** auf **OK**.

   i. Klicken Sie im Abschnitt **Neuer Plan** auf **OK**.

   j. Wählen Sie im Abschnitt **Plan** den neuen Plan aus, und klicken Sie auf **Auswählen**.

   k. Klicken Sie im Abschnitt **Neues Angebot** auf **Erstellen**. Sie erhalten eine Benachrichtigung, wenn das Angebot erstellt wurde.

   l. Klicken Sie im Dashboardmenü auf **Angebote** und dann auf das von Ihnen erstellte Angebot.

   m. Klicken Sie auf **Status ändern** und dann auf **Öffentlich**.

   ![Status „Öffentlich“](media/azure-stack-tutorial-tenant-vm/image09.png)

## <a name="add-an-image"></a>Hinzufügen eines Image

Bevor Sie virtuelle Computer bereitstellen können, müssen Sie dem Azure Stack-Marketplace ein Image hinzufügen. Sie können über den Azure Marketplace das Image Ihrer Wahl, z.B. auch Linux-Images, hinzufügen.

Wenn Sie ein verbundenes Szenario nutzen und Ihre Azure Stack-Instanz bei Azure registriert haben, können Sie das Windows Server 2016-VM-Image vom Azure Marketplace herunterladen, indem Sie die Schritte im Thema [Herunterladen von Marketplace-Elementen von Azure in Azure Stack](azure-stack-download-azure-marketplace-item.md) ausführen.

Informationen zum Hinzufügen von unterschiedlichen Elementen zum Marketplace finden Sie unter [Der Azure Stack-Marketplace – Übersicht](azure-stack-marketplace.md).

## <a name="test-the-offer"></a>Testen des Angebots

Nachdem Sie ein Angebot erstellt haben, können Sie es testen. Melden Sie sich als Benutzer an, und abonnieren Sie das Angebot. Fügen Sie anschließend einen virtuellen Computer hinzu.

1. **Abonnieren von Angeboten**

   Sie können sich jetzt als Benutzer am Portal anmelden, um ein Angebot zu abonnieren.

   a. Melden Sie sich auf dem Azure Stack Deployment Kit-Computer unter `https://portal.local.azurestack.external` als Benutzer an, und klicken Sie auf **Get a Subscription** (Abonnement erwerben).

   ![Erwerben eines Abonnements](media/azure-stack-subscribe-plan-provision-vm/image01.png)

   b. Geben Sie im Feld **Anzeigename** einen Namen für Ihr Abonnement ein, klicken Sie auf **Angebot**, klicken Sie im Abschnitt **Angebot wählen** auf ein Angebot, und klicken Sie dann auf **Erstellen**.

   ![Erstellen von Angeboten](media/azure-stack-subscribe-plan-provision-vm/image02.png)

   c. Klicken Sie zum Anzeigen des von Ihnen erstellten Abonnements auf **Weitere Dienste** und dann auf **Abonnements** und Ihr neues Abonnement.  

   Aktualisieren Sie nach dem Abonnieren eines Angebots das Portal, um anzuzeigen, welche Dienste zum neuen Abonnement gehören.

2. **Bereitstellen von virtuellen Computern**

   Sie können sich jetzt als Benutzer am Portal anmelden, um mit dem Abonnement einen virtuellen Computer bereitzustellen. 

   a. Melden Sie sich auf dem Azure Stack Deployment Kit-Computer als Benutzer unter `https://portal.local.azurestack.external` an, und klicken Sie dann auf **Neu** > **Compute** > **Windows Server 2016 Datacenter Eval**.  

   b. Geben Sie im Abschnitt **Grundlagen** einen **Namen**, **Benutzernamen** und ein **Kennwort** ein. Wählen Sie unter **VM-Datenträgertyp** die Option **HDD**. Wählen Sie ein **Abonnement**aus. Erstellen Sie eine **Ressourcengruppe**, oder wählen Sie eine vorhandene aus, und klicken Sie anschließend auf **OK**.  

   c. Klicken Sie im Abschnitt **Größe auswählen** auf **A1 Basic** und dann auf **Auswählen**.  

   d. Klicken Sie im Abschnitt **Einstellungen** auf **Virtuelles Netzwerk**. Klicken Sie im Abschnitt **Virtuelles Netzwerk auswählen** auf **Neu erstellen**. Übernehmen Sie im Abschnitt **Virtuelles Netzwerk erstellen** alle Standardeinstellungen, und klicken Sie auf **OK**. Klicken Sie im Abschnitt **Einstellungen** auf **OK**.

   ![Virtuelles Netzwerk erstellen](media/azure-stack-provision-vm/image04.png)

   e. Klicken Sie im Abschnitt **Zusammenfassung** auf **OK**, um den virtuellen Computer zu erstellen.  

   f. Klicken Sie zum Anzeigen Ihres neuen virtuellen Computers auf **Alle Ressourcen**, und suchen Sie dann nach dem virtuellen Computer. Klicken Sie auf seinen Namen.

    ![Alle Ressourcen](media/azure-stack-provision-vm/image06.png)

In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Erstellen von Angeboten
> * Hinzufügen eines Image
> * Testen des Angebots

> [!div class="nextstepaction"]
> [Verfügbarmachen von Web-, API- und mobilen Apps für Ihre Azure Stack-Benutzer](azure-stack-tutorial-app-service.md)
