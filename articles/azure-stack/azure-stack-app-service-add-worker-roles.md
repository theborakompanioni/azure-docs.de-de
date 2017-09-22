---
title: "Horizontales Hochskalieren von Workerrollen in App Services – Azure Stack | Microsoft-Dokumentation"
description: "Ausführliche Anleitung zum Skalieren von Azure Stack App Services"
services: azure-stack
documentationcenter: 
author: kathm
manager: slinehan
editor: anwestg
ms.assetid: 3cbe87bd-8ae2-47dc-a367-51e67ed4b3c0
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/6/2017
ms.author: kathm
ms.translationtype: HT
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: f844658c6ad2529fd385476be63095bdae7c88e5
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="app-service-on-azure-stack-adding-more-worker-roles"></a>App Service in Azure Stack: Hinzufügen von weiteren Workerrollen 

Dieses Dokument enthält eine Anleitung zum Skalieren der Workerrollen für App Service in Azure Stack. Es werden die Schritte zum Erstellen von zusätzlichen Workerrollen aufgeführt, um Anwendungen beliebiger Größe zu unterstützen.

> [!NOTE]
> Wenn Ihre Azure Stack POC-Umgebung nicht mehr als 96 GB RAM aufweist, können beim Hinzufügen von zusätzlicher Kapazität Schwierigkeiten auftreten.

Standardmäßig unterstützt App Service in Azure Stack kostenlose und gemeinsam genutzte Workerebenen. Um andere Workerebenen hinzufügen zu können, müssen Sie weitere Workerrollen hinzufügen.

Wenn Sie sich nicht sicher sind, welche Elemente mit der App Service-Standardversion bei der Azure Stack-Installation bereitgestellt wurden, helfen Ihnen die zusätzlichen Informationen unter [Übersicht über App Service in Azure Stack](azure-stack-app-service-overview.md) weiter.

Es gibt zwei Möglichkeiten, für App Service in Azure Stack zusätzliche Kapazität hinzuzufügen:
1.  [Direktes Hinzufügen von zusätzlichen Workern über den Administrator des App Service-Ressourcenanbieters](#Add-additional-workers-directly-from-within-the-App-Service-Resource-Provider-Admin)
2.  [Manuelles Erstellen von zusätzlichen VMs und Hinzufügen zum App Service-Ressourcenanbieter](#Create-additional-VMs-manually-and-add-them-to-the-App-Service-Resource-Provider)

## <a name="add-additional-workers-directly-within-the-app-service-resource-provider-admin"></a>Direktes Hinzufügen von zusätzlichen Workern über den Administrator des App Service-Ressourcenanbieters

1.  Melden Sie sich am Azure Stack-Portal als Dienstadministrator an.
2.  Navigieren Sie zu **Resource Providers** (Ressourcenanbieter), und wählen Sie **App Service Resource Provider Admin** (Administrator von App Service-Ressourcenanbieter) aus. ![Azure Stack-Ressourcenanbieter][1]
3.  Klicken Sie auf **Rollen**.  Eine Übersicht mit allen bereitgestellten App Service-Rollen wird angezeigt.
4.  Klicken Sie auf die Option **Neue Rolleninstanz** ![Hinzufügen einer neuen Rolleninstanz][2]
5.  Gehen Sie auf dem Blatt **Neue Rolleninstanz** wie folgt vor:
    1. Geben Sie an, wie viele zusätzliche **Rolleninstanzen** Sie hinzufügen möchten.  In der Vorschauversion beträgt der Höchstwert 10.
    2. Wählen Sie den **Rollentyp** aus.  In dieser Vorschauversion ist diese Option auf Webworker beschränkt.
    3. Wählen Sie die **Workerebene** aus, auf der Sie diesen Worker bereitstellen möchten. Die Standardoptionen lauten „Klein“, „Mittel“, „Groß“ und „Shared“ (Gemeinsam genutzt).  Wenn Sie Ihre eigenen Workerebenen erstellt haben, können Sie diese ebenfalls auswählen.
    4. Klicken Sie auf **OK**, um die zusätzlichen Worker bereitzustellen.
6.  App Service in Azure Stack fügt jetzt die zusätzlichen VMs hinzu, konfiguriert sie, installiert die gesamte erforderliche Software und kennzeichnet sie als bereit, wenn dieser Prozess abgeschlossen ist.  Dieser Prozess kann ca. 80 Minuten dauern.
7.  Sie können den Bereitschaftsstatus der neuen Worker auf dem Blatt **Rollen** überwachen.

>[!NOTE]
>  In dieser Vorschauversion ist der integrierte Flow „Neue Rolleninstanz“ auf Workerrollen und die ausschließliche Bereitstellung von VMs der Größe A1 beschränkt.  Wir werden diese Funktion in einer zukünftigen Version erweitern.

## <a name="manually-adding-additional-capacity-to-app-service-on-azure-stack"></a>Manuelles Hinzufügen von zusätzlicher Kapazität zu App Service in Azure Stack

Die folgenden Schritte sind erforderlich, um zusätzliche Rollen hinzuzufügen:

1. [Erstellen eines neuen virtuellen Computers](#step-1-create-a-new-vm-to-support-the-new-instance-size)
2. [Konfigurieren des virtuellen Computers](#step-2-configure-the-virtual-machine)
3. [Konfigurieren der Webworkerrolle im Azure Stack-Portal](#step-3-configure-the-web-worker-role-in-the-azure-stack-portal)
4. [Konfigurieren von App Service-Plänen](#step-4-configure-app-service-plans)

## <a name="step-1-create-a-new-vm-to-support-the-new-instance-size"></a>Schritt 1: Erstellen einer neuen VM zum Unterstützen der neuen Instanzgröße
Erstellen Sie einen virtuellen Computer, wie in [diesem Artikel](azure-stack-provision-vm.md) beschrieben. Achten Sie darauf, dass Sie Folgendes auswählen:

* Benutzername und Kennwort: Geben Sie den gleichen Benutzernamen und das dazugehörige Kennwort wie bei der Installation von App Service in Azure Stack an.
* Abonnement: Verwenden Sie das Standard-Anbieterabonnement.
* Ressourcengruppe: Wählen Sie die Option **AppService-LOCAL**.

> [!NOTE]
> Speichern Sie die virtuellen Computer für Workerrollen in derselben Ressourcengruppe, in der auch App Service in Azure Stack bereitgestellt wurde. (Dies wird für diese Version empfohlen.)
> 
> 

## <a name="step-2-configure-the-virtual-machine"></a>Schritt 2: Konfigurieren des virtuellen Computers
Nach Abschluss der Bereitstellung ist die folgende Konfiguration erforderlich, um die Webworkerrolle zu unterstützen:

1. Navigieren Sie im Portal zur Ressourcengruppe **AppService-LOCAL**, und wählen Sie den neuen Computer aus, den Sie in Schritt 1 erstellt haben.
2. Klicken Sie auf dem VM-Blatt auf „Verbinden“, um das Remotedesktopprofil herunterzuladen.  Öffnen Sie das Profil, um eine Remotedesktopsitzung für Ihre VM zu öffnen.
3. Melden Sie sich an der VM an, indem Sie den Benutzernamen mit dem dazugehörigen Kennwort verwenden, den Sie in Schritt 1 angegeben haben.
4. Öffnen Sie PowerShell, indem Sie auf die Schaltfläche **Start** klicken und „PowerShell“ eingeben. Klicken Sie mit der rechten Maustaste auf **PowerShell.exe**, und wählen Sie die Option **Als Administrator ausführen**, um PowerShell im Administratormodus zu öffnen.
5. Kopieren Sie die folgenden Befehle (einzeln), und fügen Sie sie in das PowerShell-Fenster ein. Drücken Sie anschließend die EINGABETASTE:
   
   ```netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=Yes```
   ```netsh advfirewall firewall set rule group="Windows Management Instrumentation (WMI)" new enable=yes```
   ```reg add HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\Policies\\system /v LocalAccountTokenFilterPolicy /t REG\_DWORD /d 1 /f```
   
6. Schließen Sie Ihre Remotedesktopsitzung.
7. Starten Sie den virtuellen Computer über das Portal.

> [!NOTE]
> Dies sind Mindestanforderungen für App Service in Azure Stack. Dies sind die Standardeinstellungen des Windows 2012 R2-Image, das in Azure Stack enthalten ist. Die Anleitung wurde zu Referenzzwecken und für Personen angegeben, die ein anderes Image verwenden.
> 
> 

## <a name="step-3-configure-the-worker-role-in-the-azure-stack-portal"></a>Schritt 3: Konfigurieren der Workerrolle im Azure Stack-Portal
1. Öffnen Sie das Portal als Dienstadministrator auf **ClientVM**.
2. Navigieren Sie zu **Ressourcenanbieter** &gt; **App Service Resource Provider Admin** (Administrator von App Service-Ressourcenanbieter).![Administrator von App Service-Ressourcenanbieter][3]
3. Klicken Sie auf dem Blatt „Einstellungen“ auf **Rollen**.![Rollen des App Service-Ressourcenanbieters][4]
4. Klicken Sie auf **Rolleninstanz hinzufügen**.
5. Geben Sie im Textfeld für **Servername** die **IP-Adresse** des zuvor erstellten Servers ein (in Abschnitt 1).
6. Wählen Sie den **Rollentyp** aus, den Sie hinzufügen möchten: Controller, Verwaltungsserver, Front-End, Webworker, Herausgeber oder Dateiserver.  Wählen Sie für diese Instanz die Option „Webworker“.
7. Klicken Sie auf die **Ebene**, auf der Sie die neue Instanz bereitstellen möchten („Klein“, „Mittel“, „Groß“ oder „Shared“ (Gemeinsam genutzt)).  Wenn Sie Ihre eigenen Workerebenen erstellt haben, können Sie diese ebenfalls auswählen.
8. Klicken Sie auf **OK**.
9. Wechseln Sie zurück zur Ansicht **Rollen**.
10. Klicken Sie auf die Zeile mit der Kombination aus Rollentyp und Workerebene, der Sie Ihre VM zugewiesen haben.
11. Suchen Sie nach dem Servernamen, den Sie gerade hinzugefügt haben. Achten Sie auf die Spalte mit dem Status, und warten Sie, bis der Status „Bereit“ lautet, bevor Sie mit dem nächsten Schritt fortfahren. Dies kann ca. 80 Minuten dauern. ![App Service-Ressourcenanbieter-Rolle: Bereit][5]

## <a name="step-4-configure-app-service-plans"></a>Schritt 4: Konfigurieren von App Service-Plänen

1. Melden Sie sich auf der ClientVM am Portal an.
2. Navigieren Sie zu **Neu** &gt; **Web und mobil**.
3. Wählen Sie den Anwendungstyp aus, den Sie bereitstellen möchten.
4. Geben Sie die Informationen für die Anwendung an, und wählen Sie dann die Option **App Service-Plan/Standort**.
    1. Klicken Sie auf **Neu erstellen**.
    2. Erstellen Sie Ihren Plan, und wählen Sie den entsprechenden Tarif für den Plan aus.

> [!NOTE]
> Sie können auf diesem Blatt mehrere Pläne erstellen. Stellen Sie vor dem Bereitstellen aber sicher, dass Sie den richtigen Plan ausgewählt haben.
> 
> 

Unten sehen Sie ein Beispiel für die verschiedenen Tarife, die standardmäßig verfügbar sind.  Sie sehen, dass die Option zum Auswählen des entsprechenden Tarifs nicht verfügbar ist, wenn für eine bestimmte Workerebene keine Worker vorhanden sind.![App Service in Azure Stack – Standardtarife][6]

<!--Image references-->
[1]: ./media/azure-stack-app-service-add-worker-roles/azure-stack-resource-providers.png
[2]: ./media/azure-stack-app-service-add-worker-roles/app-service-new-role-instance.png
[3]: ./media/azure-stack-app-service-add-worker-roles/app-service-resource-provider-admin.png
[4]: ./media/azure-stack-app-service-add-worker-roles/app-service-resource-provider-roles.png
[5]: ./media/azure-stack-app-service-add-worker-roles/app-service-resource-provider-role-ready.png
[6]: ./media/azure-stack-app-service-add-worker-roles/app-service-resource-provider-pricing-tier.png

