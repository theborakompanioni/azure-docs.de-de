---
title: 'Azure Active Directory Domain Services: Aktivieren von Azure Active Directory Domain Services | Microsoft-Dokumentation'
description: "Erste Schritte mit Azure Active Directory-Domänendiensten"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: c659da59-f4b5-4edd-b702-1727a8ccb36f
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/06/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: e5f1fe51d8931985fa55b2d8c0a3fd25bb93f20f
ms.lasthandoff: 04/12/2017


---
# <a name="enable-azure-active-directory-domain-services"></a>Aktivieren von Azure Active Directory Domain Services
## <a name="task-3-enable-azure-active-directory-domain-services"></a>Aufgabe 3: Aktivieren von Azure Active Directory Domain Services
In dieser Aufgabe aktivieren Sie Azure Active Directory Domain Services (Azure AD DS) für Ihr Verzeichnis:

1. Wechseln Sie zum [klassischen Azure-Portal](https://manage.windowsazure.com).
2. Wählen Sie im linken Bereich die Schaltfläche **Active Directory** aus.
3. Wählen Sie den Azure Active Directory-Mandanten (Verzeichnis) aus, für den Sie Azure AD DS aktivieren möchten.

    ![Auswählen des Azure AD-Verzeichnisses](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. Klicken Sie auf der Seite **preview directory** (Vorschauverzeichnis) auf die Registerkarte **Konfigurieren**.

    ![Registerkarte „Konfigurieren“ des Verzeichnisses](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. Legen Sie unter **Domänendienste** die Option **Domänendienste für dieses Verzeichnis aktivieren** auf **Ja** fest.  
    Auf der Seite werden zusätzliche Konfigurationsoptionen für Azure Active Directory Domain Services angezeigt.

    ![Aktivieren von Domänendienste](./media/active-directory-domain-services-getting-started/enable-domain-services.png)

   > [!NOTE]
   > Wenn Sie Azure Active Directory Domain Services für Ihren Mandanten aktivieren, generiert und speichert Azure AD die Kerberos- und NTLM-Anmeldeinformationshashes, die für die Benutzerauthentifizierung benötigt werden.
   >
   >
6. Geben Sie einen Wert für **DNS-Domänenname der Domänendienste**an.

   * Standardmäßig ist der Standarddomänenname des Verzeichnisses (mit dem Suffix **.onmicrosoft.com**) ausgewählt.

   * Die Liste enthält alle Domänen, die für Ihr Azure AD-Verzeichnis konfiguriert wurden, einschließlich überprüfter und nicht überprüfter Domänen, die Sie auf der Registerkarte **Domänen** konfigurieren.

   * Sie können auch einen benutzerdefinierten Domänennamen eingeben. In diesem Beispiel lautet der benutzerdefinierte Domänenname *contoso100.com*.

     > [!WARNING]
     > Das Präfix des angegebenen Domänennamens (beispielsweise *contoso100* im Domänennamen *contoso100.com*) darf maximal 15 Zeichen lang sein. Sie können keine Azure Active Directory Domain Services-Domäne mit einem Präfix erstellen, das mehr als 15 Zeichen lang ist.
     >
     >
7. Stellen Sie sicher, dass der DNS-Domänenname, den Sie für die verwaltete Domäne ausgewählt haben, nicht bereits im virtuellen Netzwerk vorhanden ist. Prüfen Sie insbesondere, ob Folgendes zutrifft:

   * Im virtuellen Netzwerk ist bereits eine Domäne mit dem gleichen DNS-Domänennamen vorhanden.

   * Zwischen dem ausgewählten virtuellen Netzwerk und Ihrem lokalen Netzwerk besteht eine VPN-Verbindung, und in Ihrem lokalen Netzwerk ist bereits eine Domäne mit dem gleichen DNS-Domänennamen vorhanden.

   * Im virtuellen Netzwerk ist bereits ein Clouddienst mit diesem Namen vorhanden.
8. Wählen Sie ein virtuelles Netzwerk aus, in dem Azure Active Directory Domain Services verfügbar sein soll. Wählen Sie in der Dropdownliste **Domänendienste mit diesem virtuellen Netzwerk verbinden** das erstellte virtuelle Netzwerk und das dedizierte Subnetz aus. Berücksichtigen Sie außerdem folgende Punkte:

   * Stellen Sie sicher, dass das angegebene virtuelle Netzwerk zu einer Azure-Region gehört, die von Azure Active Directory Domain Services unterstützt wird. Informationen dazu, in welchen Azure-Regionen Azure Active Directory Domain Services verfügbar sind, finden Sie unter [Azure-Regionen](https://azure.microsoft.com/regions/#services/).

   * Virtuelle Netzwerke, die zu einer Region gehören, in der Azure Active Directory Domain Services nicht unterstützt wird, werden in der Dropdownliste nicht angezeigt.

   * Verwenden Sie ein dediziertes Subnetz innerhalb des virtuellen Netzwerks für Azure Active Directory Domain Services. Wählen Sie *nicht* das Gatewaysubnetz aus. Weitere Informationen finden Sie unter [Überlegungen zum Netzwerkbetrieb](active-directory-ds-networking.md).

   * In der Dropdownliste werden auch keine virtuellen Netzwerke angezeigt, die mit Azure Resource Manager erstellt wurden. Resource Manager-basierte virtuelle Netzwerke werden derzeit nicht von Azure Active Directory Domain Services unterstützt.
9. Klicken Sie im Aufgabenbereich am unteren Rand der Seite auf **Speichern**, um Azure Active Directory Domain Services zu aktivieren. 
    * Während der Aktivierung von Azure Active Directory Domain Services für Ihr Verzeichnis wird auf der Seite der Status *Ausstehend* angezeigt.

        ![Fenster „Domänendienste aktivieren“](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)

        > [!NOTE]
        > Azure Active Directory Domain Services bietet eine hohe Verfügbarkeit für Ihre verwaltete Domäne. Nach der Aktivierung von Azure Active Directory Domain Services werden nacheinander die IP-Adressen angezeigt, unter denen Domänendienste im virtuellen Netzwerk verfügbar sind. Die zweite IP-Adresse wird kurz nach der ersten angezeigt, sobald der Dienst die hohe Verfügbarkeit für Ihre Domäne aktiviert. Wenn hohe Verfügbarkeit für Ihre Domäne konfiguriert und aktiv ist, sollten im Abschnitt **Domänendienste** der Registerkarte **Konfigurieren** zwei IP-Adressen angezeigt werden.
        >
        >
    * Nach etwa 20 bis 30 Minuten wird auf der Seite **Konfigurieren** im Feld **IP-Adresse** die erste IP-Adresse angezeigt, unter der Domänendienste in Ihrem virtuellen Netzwerk verfügbar sind.

        ![Domain Services-Fenster mit der ersten bereitgestellten IP-Adresse](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)
    * Wenn die hohe Verfügbarkeit für Ihre Domäne aktiv ist, werden auf der Seite zwei IP-Adressen angezeigt. Die verwaltete Domäne ist im ausgewählten virtuellen Netzwerk über diese beiden IP-Adressen verfügbar. 
    
10. Notieren Sie sich die beiden IP-Adressen, um die DNS-Einstellungen für Ihr virtuelles Netzwerk aktualisieren zu können. Mit diesem Schritt können virtuelle Computer im virtuellen Netzwerk eine Verbindung mit der Domäne herstellen, um Vorgänge wie etwa einen Domänenbeitritt durchzuführen.

    ![Domain Services-Fenster mit beiden bereitgestellten IP-Adressen](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [!NOTE]
> Die Dauer der Synchronisierung mit Ihrer verwalteten Domäne ist abhängig von der Größe Ihres Azure AD-Mandanten (also beispielsweise von der Benutzer- oder Gruppenanzahl). Diese Synchronisation erfolgt im Hintergrund. Bei umfangreichen Mandanten mit Zehntausenden von Objekten kann es bis zu zwei Tage dauern, bis alle Benutzer, Gruppenmitgliedschaften und Anmeldeinformationen synchronisiert wurden.
>
>

## <a name="next-steps"></a>Nächste Schritte
Aufgabe 4: [Aktualisieren der DNS-Einstellungen für das virtuelle Azure-Netzwerk](active-directory-ds-getting-started-dns.md)

