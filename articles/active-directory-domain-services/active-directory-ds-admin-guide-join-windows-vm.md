---
title: "Azure Active Directory Domain Services: Einbinden eines virtuellen Windows Server-Computers in eine verwaltete Domäne | Microsoft Docs"
description: "Einbinden eines virtuellen Windows Server-Computers in die Azure AD-Domänendienste"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 74dbdb33-05db-4d47-badc-0d7bb6d0c8cb
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: ee34a7ebd48879448e126c1c9c46c751e477c406
ms.openlocfilehash: aad6bcd3eb704f090156d2ace80d2540a9543bd7
ms.lasthandoff: 12/28/2016


---
# <a name="join-a-windows-server-virtual-machine-to-a-managed-domain"></a>Einbinden eines virtuellen Windows Server-Computers in eine verwaltete Domäne
> [!div class="op_single_selector"]
> * [Klassisches Azure-Portal – Windows](active-directory-ds-admin-guide-join-windows-vm.md)
> * [PowerShell – Windows](active-directory-ds-admin-guide-join-windows-vm-classic-powershell.md)
>
>

<br>

Dieser Artikel veranschaulicht, wie Sie einen virtuellen Computer unter Windows Server 2012 R2 über das klassische Azure-Portal in eine durch Azure AD-Domänendienste verwaltete Domäne einbinden.

## <a name="step-1-create-the-windows-server-virtual-machine"></a>Schritt 1: Erstellen des virtuellen Windows Server-Computers
Folgen Sie den Anweisungen im Tutorial [Erstellen eines virtuellen Computers unter Windows im klassischen Azure-Portal](../virtual-machines/virtual-machines-windows-classic-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Achten Sie darauf, dass dieser neu erstellte virtuelle Computer dem virtuellen Netzwerk hinzugefügt wird, in dem Sie auch die Azure AD-Domänendienste aktiviert haben. Der virtuelle Computer kann nicht über die Option „Schnellerfassung“ in ein virtuelles Netzwerk eingebunden werden. Sie müssen daher die Option „Aus Katalog“ verwenden, um den virtuellen Computer zu erstellen.

Führen Sie die folgenden Schritte durch, um einen virtuellen Windows-Computer zu erstellen, der in das virtuelle Netzwerk eingebunden wird, in dem Sie die Azure Active Directory-Domänendienste aktiviert haben:

1. Klicken Sie im klassischen Azure-Portal in der Befehlsleiste unten im Fenster auf **Neu**.
2. Klicken Sie unter**Compute** auf **Virtueller Computer** und anschließend auf **Aus Katalog**.
3. Im ersten Bildschirm können Sie aus der Liste verfügbarer Images für Ihren virtuellen Computer ein **Image auswählen** . Wählen Sie ein geeignetes Image aus.

    ![Image auswählen](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-image.png)
4. Im zweiten Bildschirm wählen Sie einen Computernamen, Größe sowie Benutzername und Passwort des Administrators aus. Andernfalls wählen Sie die benötigte Schicht und die Größe, um Ihre Anwendung oder Ihren Leistungsumfang auszuführen. Bei dem hier ausgewählten Benutzer handelt es sich um einen lokalen Administratorbenutzer des Computers. Geben Sie hier nicht die Anmeldeinformationen eines Domänenbenutzerkontos ein.

    ![Virtuellen Computer konfigurieren](./media/active-directory-domain-services-admin-guide/create-windows-vm-config.png)
5. Auf dem dritten Bildschirm können Sie die Ressourcen für Netzwerk, Speicher und Verfügbarkeit konfigurieren. Stellen Sie sicher, dass Sie in der Dropdownliste **Region/Affinitätsgruppe/Virtuelles Netzwerk** das virtuelle Netzwerk auswählen, in dem Sie die Azure AD-Domänendienste aktiviert haben. Geben Sie einen für den virtuellen Computer geeigneten **DNS-Namen des Clouddiensts** an.

    ![Virtuelles Netzwerk für den virtuellen Computer auswählen](./media/active-directory-domain-services-admin-guide/create-windows-vm-select-vnet.png)

   > [!WARNING]
   > Stellen Sie sicher, dass Sie den virtuellen Computer in das gleiche virtuelle Netzwerk einbinden, in dem Sie die Azure AD Domain Services aktiviert haben. Dadurch kann der virtuelle Computer die Domäne erkennen und Aufgaben wie den Beitritt zur Domäne ausführen. Wenn Sie den virtuellen Computer in einem anderen virtuellen Netzwerk erstellen, verbinden Sie dieses virtuelle Netzwerk mit dem virtuellen Netzwerk, in dem Sie die Azure AD Domain Services aktiviert haben.
   >
   >
6. Auf dem vierten Konfigurationsbildschirm können Sie den VM-Agent installieren und einige der verfügbaren Erweiterungen konfigurieren.

    ![Vorgehensweise](./media/active-directory-domain-services-admin-guide/create-windows-vm-done.png)
7. Nach der Erstellung des virtuellen Computers wird der neue virtuelle Computer im klassischen Portal unter dem Knoten **Virtuelle Computer** aufgelistet. Der virtuelle Computer und der Clouddienst werden automatisch gestartet. Das Verwaltungsportal zeigt ihren Status als **Wird ausgeführt** an.

    ![Virtueller Computer wird ausgeführt](./media/active-directory-domain-services-admin-guide/create-windows-vm-running.png)

## <a name="step-2-connect-to-the-windows-server-virtual-machine-using-the-local-administrator-account"></a>Schritt 2: Herstellen einer Verbindung mit dem virtuellen Windows Server-Computer über das lokale Administratorkonto.
Jetzt stellen wir eine Verbindung mit dem neu erstellten virtuellen Windows Server-Computer her, damit dieser der Domäne beitreten kann. Verwenden Sie die Anmeldeinformationen des lokalen Administrators, die Sie beim Erstellen des virtuellen Computers angegeben haben, um die Verbindung herzustellen.

Führen Sie die folgenden Schritte aus, um eine Verbindung mit dem virtuellen Computer herzustellen:

1. Wechseln Sie im klassischen Portal zum Knoten **Virtuelle Computer** . Wählen Sie den virtuellen Computer aus, den Sie in Schritt 1 erstellt haben, und klicken Sie auf der Befehlsleiste im unteren Fensterbereich auf **Verbinden** .

    ![Verbindung mit virtuellem Windows-Computer herstellen](./media/active-directory-domain-services-admin-guide/connect-windows-vm.png)
2. Sie werden aufgefordert, eine Datei mit der Erweiterung „.rdp“ zu öffnen oder zu speichern. Diese wird zum Herstellen der Verbindung mit dem virtuellen Computer verwendet. Warten Sie, bis der Download abgeschlossen ist, und klicken Sie dann auf die Datei, um sie zu öffnen.
3. Geben Sie an der Anmeldeaufforderung die **Anmeldeinformationen des lokalen Administrators**ein, die Sie beim Erstellen des virtuellen Computers angegeben haben. Wir haben in diesem Beispiel „localhost\mahesh“ verwendet.

Dadurch sollten Sie mithilfe der lokalen Administratoranmeldeinformationen beim neu erstellen virtuellen Windows-Computer angemeldet werden. Der nächste Schritt besteht darin, den virtuellen Computer in die Domäne einzubinden.

## <a name="step-3-join-the-windows-server-virtual-machine-to-the-aad-ds-managed-domain"></a>Schritt 3: Einbinden des virtuellen Windows Server-Computers in die durch die Azure AD-Domänendienste verwaltete Domäne
Führen Sie die folgenden Schritte aus, um den virtuellen Windows Server-Computer der durch die AAD-Domänendienste verwalteten Domäne hinzuzufügen.

1. Stellen Sie eine Verbindung mit dem Windows Server-Computer her, wie in Schritt 2 gezeigt. Öffnen Sie auf dem Startbildschirm den **Server-Manager**.
2. Klicken Sie im linken Bereich des Server-Manager-Fensters auf **Lokaler Server** .

    ![Server-Manager auf dem virtuellen Computer starten](./media/active-directory-domain-services-admin-guide/join-domain-server-manager.png)
3. Klicken Sie im Abschnitt **EIGENSCHAFTEN** auf **ARBEITSGRUPPE**. Klicken Sie auf der Seite **Systemeigenschaften** auf **Ändern**, um der Domäne beizutreten.

    ![Seite mit Systemeigenschaften](./media/active-directory-domain-services-admin-guide/join-domain-system-properties.png)
4. Geben Sie im Textfeld **Domäne** den Domänennamen Ihrer durch die Azure AD Domain Services verwalteten Domäne ein, und klicken Sie auf **OK**.

    ![Domäne für den Beitritt angeben](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-domain.png)
5. Sie werden aufgefordert, Ihre Anmeldeinformationen für den Domänenbeitritt einzugeben. Stellen Sie sicher, dass Sie die **Anmeldeinformationen eines Benutzers angeben, der zur Administratorengruppe für Azure AD-Domänencontroller gehört** . Nur Mitglieder dieser Gruppen verfügen über die Berechtigungen, Computer in die verwaltete Domäne einzubinden.

    ![Anmeldeinformationen für den Domänenbeitritt angeben](./media/active-directory-domain-services-admin-guide/join-domain-system-properties-specify-credentials.png)
6. Sie können Anmeldeinformationen auf eine der folgenden Arten eingeben:

   * UPN-Format: Hierbei handelt es sich um das UPN-Suffix für das Benutzerkonto, wie in Azure AD konfiguriert. Das UPN-Suffix des Benutzers „bob“ in diesem Beispiel lautet „bob@domainservicespreview.onmicrosoft.com“.
   * SAMAccountName-Format: Sie können den Kontonamen im Format „SAMAccountName“ angeben. Der Benutzer „bob“ würde in diesem Beispiel „CONTOSO100\bob“ eingeben.

     > [!NOTE]
     > **Wir empfehlen die Verwendung des UPN-Formats, um Anmeldeinformationen anzugeben.** Der Name „sAMAccountName“ wird möglicherweise automatisch generiert, wenn das UPN-Präfix eines Benutzer übermäßig lang ist (z.B. „Janwirklichlangerbenutzername“). Falls mehrere Benutzer in Ihrem Azure AD-Mandanten das gleiche UPN-Präfix besitzen (beispielsweise „bob“), wird ihr SAMAccountName-Format automatisch generiert. In diesen Fällen kann das UPN-Format zur Anmeldung bei der Domäne verwendet werden.
     >
     >
7. Nach dem erfolgreichen Domänenbeitritt wird folgende Willkommensmeldung angezeigt. Starten Sie den virtuellen Computer neu, um den Domänenbeitritt abzuschließen.

    ![Willkommen in der Domäne](./media/active-directory-domain-services-admin-guide/join-domain-done.png)

## <a name="troubleshooting-domain-join"></a>Problembehandlung beim Domänenbeitritt
### <a name="connectivity-issues"></a>Konnektivitätsprobleme
Wenn der virtuelle Computer die Domäne nicht findet, führen Sie folgende Schritte zur Problembehandlung aus:

* Stellen Sie sicher, dass der virtuelle Computer mit dem gleichen virtuellen Netzwerk verbunden ist, in dem Sie die Domänendienste aktiviert haben. Andernfalls kann der virtuelle Computer keine Verbindung mit der Domäne herstellen und ihr daher auch nicht beitreten.
* Wenn der virtuelle Computer mit einem anderen virtuellen Netzwerk verbunden ist, stellen Sie sicher, dass dieses virtuelle Netzwerk mit dem virtuellen Netzwerk verbunden ist, in dem Sie die Domänendienste aktiviert haben.
* Pingen Sie die Domäne unter Verwendung des Domänennamens der verwalteten Domäne (Beispiel: „ping contoso100.com“). Sollte das nicht möglich sein, pingen Sie die IP-Adressen für die Domäne, die auf der Seite angezeigt werden, auf der Sie die Azure AD-Domänendienste aktiviert haben (Beispiel: „ping 10.0.0.4“). Wenn Sie die IP-Adressen pingen können, aber nicht die Domäne, ist DNS möglicherweise falsch konfiguriert. Möglicherweise wurden die IP-Adressen der Domäne nicht als DNS-Server für das virtuelle Netzwerk konfiguriert.
* Leeren Sie den DNS-Resolvercache im virtuellen Computer („ipconfig /flushdns“).

Wenn ein Dialogfeld angezeigt wird, in dem Anmeldeinformationen für den Domänenbeitritt angefordert werden, bestehen keine Verbindungsprobleme.

### <a name="credentials-related-issues"></a>Probleme mit Anmeldeinformationen
Führen Sie folgende Schritte aus, wenn Probleme mit Anmeldeinformationen auftreten und ein Domänenbeitritt nicht möglich ist.

* Verwenden Sie das UPN-Format für die Angabe von Anmeldeinformationen. Der „SAMAccountName“ für Ihr Konto wird möglicherweise automatisch generiert, wenn mehrere Benutzer in Ihrem Mandanten das gleiche UPN-Präfix verwenden oder wenn Ihr UPN-Präfix übermäßig lang ist. Das Format „SAMAccountName“ für Ihr Konto ist möglicherweise anders als Sie erwarten bzw. unterscheidet sich von dem, was Sie in Ihrer lokalen Domäne verwenden.
* Verwenden Sie die Anmeldeinformationen eines Benutzerkontos, das der Gruppe „AAD DC Administratoren“ angehört, um der verwalteten Domäne Computer hinzuzufügen.
* Stellen Sie sicher, dass Sie die [Aktivierung der Kennwortsynchronisierung](active-directory-ds-getting-started-password-sync.md) in Übereinstimmung mit den Schritten durchgeführt haben, die im Leitfaden zu den ersten Schritten angegeben sind.
* Verwenden Sie den UPN des Benutzers bei der Anmeldung exakt so, wie er in Azure AD konfiguriert ist (Beispiel: 'bob@domainservicespreview.onmicrosoft.com')).
* Stellen Sie sicher, dass Sie lange genug warten, damit die Kennwortsynchronisierung abgeschlossen ist, wie im Leitfaden „Erste Schritte“ angegeben.

## <a name="related-content"></a>Verwandte Inhalte
* [Azure AD-Domänendienste – Leitfaden zu den ersten Schritten](active-directory-ds-getting-started.md)
* [Verwalten einer durch Azure AD-Domänendienste verwalteten Domäne](active-directory-ds-admin-guide-administer-domain.md)

