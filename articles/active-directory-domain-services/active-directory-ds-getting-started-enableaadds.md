---
title: 'Azure AD Domain Services: Aktivieren von Azure AD Domain Services | Microsoft Docs'
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
ms.date: 10/19/2016
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 64bb5f7e78a6e48faf3487da780597b25891a2fa


---
# <a name="enable-azure-ad-domain-services"></a>Aktivieren von Azure AD Domain Services
## <a name="task-3-enable-azure-ad-domain-services"></a>Aufgabe 3: Aktivieren der Azure AD-Domänendienste
In dieser Aufgabe aktivieren Sie Azure AD Domain Services für Ihr Verzeichnis. Führen Sie die folgenden Konfigurationsschritte aus, um Azure AD Domain Services für Ihr Verzeichnis zu aktivieren.

1. Navigieren Sie zum **klassischen Azure-Portal** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
2. Wählen Sie den Knoten **Active Directory** im linken Bereich aus.
3. Wählen Sie den Azure AD-Mandanten (Verzeichnis) aus, für den Sie Azure AD-Domänendienste aktivieren möchten.
   
    ![Auswählen des Azure AD-Verzeichnisses](./media/active-directory-domain-services-getting-started/select-aad-directory.png)
4. Klicken Sie auf die Registerkarte **Configure** .
   
    ![Registerkarte "Konfigurieren" des Verzeichnisses](./media/active-directory-domain-services-getting-started/configure-tab.png)
5. Scrollen Sie hinunter zum Abschnitt **Domänendienste**.
   
    ![Konfigurationsabschnitt von Domänendienste](./media/active-directory-domain-services-getting-started/domain-services-configuration.png)
6. Schalten Sie die Option **Domänendienste für dieses Verzeichnis aktivieren** auf **JA**. Auf der Seite werden einige weitere Konfigurationsoptionen für Azure AD Domain Services angezeigt.
   
    ![Aktivieren von Domänendienste](./media/active-directory-domain-services-getting-started/enable-domain-services.png)
   
   > [!NOTE]
   > Wenn Sie Azure AD Domain Services für Ihren Mandanten aktivieren, generiert und speichert Azure AD die Hashwerte für die Kerberos- und NTLM-Anmeldeinformationen, die zum Authentifizieren von Benutzern benötigt werden.
   > 
   > 
7. Geben Sie einen Wert für **DNS-Domänenname der Domänendienste**an.
   
   * Standardmäßig ist der Standarddomänenname des Verzeichnisses (mit dem Domänensuffix **.onmicrosoft.com** ) ausgewählt.
   * Die Liste enthält alle Domänen, die für Ihr Azure AD-Verzeichnis konfiguriert wurden, einschließlich überprüfter und nicht überprüfter Domänen, die Sie auf der Registerkarte „Domänen“ konfigurieren.
   * Darüber hinaus können Sie auch einen benutzerdefinierten Domänennamen eingeben. In diesem Beispiel haben wir den benutzerdefinierten Domänennamen „contoso100.com“ eingegeben.
     
     > [!WARNING]
     > Vergewissern Sie sich, dass das Domänenpräfix des angegebenen Domänennamens (beispielsweise „contoso100“ im Domänennamen „contoso100.com“) weniger als 15 Zeichen umfasst. Das Erstellen von Azure Active Directory-Domänendienste-Domänen mit einem Domänenpräfix von mehr als 15 Zeichen ist nicht möglich.
     > 
     > 
8. Stellen Sie sicher, dass der DNS-Domänenname, den Sie für die verwaltete Domäne ausgewählt haben, nicht bereits im virtuellen Netzwerk vorhanden ist. Überprüfen Sie insbesondere, ob Folgendes zutrifft:
   
   * Im virtuellen Netzwerk ist bereits eine Domäne mit dem gleichen DNS-Domänennamen vorhanden.
   * Zwischen dem ausgewählten virtuellen Netzwerk und Ihrem lokalen Netzwerk besteht eine VPN-Verbindung, und in Ihrem lokalen Netzwerk ist bereits eine Domäne mit dem gleichen DNS-Domänennamen vorhanden.
   * Im virtuellen Netzwerk ist bereits ein Clouddienst mit diesem Namen vorhanden.
9. Im nächsten Schritt wird ein virtuelles Netzwerk ausgewählt, in dem Azure AD-Domänendienste verfügbar sein soll. Wählen Sie in der Dropdownliste **Domänendienste mit diesem virtuellen Netzwerk verbinden** das erstellte virtuelle Netzwerk und das dedizierte Subnetz aus.
   
   * Stellen Sie sicher, dass das angegebene virtuelle Netzwerk zu einer Azure-Region gehört, die von Azure AD-Domänendienste unterstützt wird. Informationen zu den Azure-Regionen, in denen Azure AD Domain Services verfügbar ist, finden Sie unter [Azure-Dienste nach Region](https://azure.microsoft.com/regions/#services/) .
   * Virtuelle Netzwerke, die zu einer Region gehören, in der Azure AD Domain Services nicht unterstützt wird, werden in der Dropdownliste nicht angezeigt.
   * Verwenden Sie ein dediziertes Subnetz innerhalb des virtuellen Netzwerks für Azure AD-Domänendienste. Stellen Sie sicher, dass Sie nicht das Gateway-Subnetz auswählen. Weitere Informationen finden Sie unter [Überlegungen zum Netzwerkbetrieb](active-directory-ds-networking.md). 
   * Auch virtuelle Netzwerke, die mit Azure Resource Manager erstellt wurden, werden nicht in der Dropdownliste angezeigt. Resource Manager-basierte virtuelle Netzwerke werden derzeit nicht von Azure AD Domain Services unterstützt.
10. Klicken Sie im Aufgabenbereich am unteren Rand der Seite auf **Speichern** , um Azure AD Domain Services zu aktivieren.
11. Auf der Seite wird der Status „Ausstehend“ angezeigt, während Azure AD Domain Services für das Verzeichnis aktiviert wird.
    
    ![Aktivieren von Domänendienste – Status "Ausstehend"](./media/active-directory-domain-services-getting-started/enable-domain-services-pendingstate.png)
    
    > [!NOTE]
    > Azure AD-Domänendienste bietet eine hohe Verfügbarkeit für Ihre verwaltete Domäne. Nach der Aktivierung von Azure AD Domain Services werden nacheinander die IP-Adressen angezeigt, unter denen Domain Services im virtuellen Netzwerk verfügbar ist. Die zweite IP-Adresse wird angezeigt, sobald der Dienst die hohe Verfügbarkeit für Ihre Domäne aktiviert. Wenn hohe Verfügbarkeit für Ihre Domäne konfiguriert und aktiv ist, sollten im Abschnitt **Domänendienste** der Registerkarte **Konfigurieren** zwei IP-Adressen angezeigt werden.
    > 
    > 
12. Nach etwa 20 bis 30 Minuten wird im Feld **IP-Adresse** auf der Seite **Konfigurieren** die erste IP-Adresse angezeigt, unter der Domain Services in Ihrem virtuellen Netzwerk verfügbar ist.
    
    ![Domänendienste aktiviert – erste IP-Adresse bereitgestellt](./media/active-directory-domain-services-getting-started/domain-services-enabled-firstdc-available.png)
13. Wenn die hohe Verfügbarkeit für Ihre Domäne aktiv ist, werden auf der Seite zwei IP-Adressen angezeigt. Die verwaltete Domäne ist im ausgewählten virtuellen Netzwerk über diese beiden IP-Adressen verfügbar. Notieren Sie sich die IP-Adressen, um die DNS-Einstellungen für Ihr virtuelles Netzwerk aktualisieren zu können. Mit diesem Schritt können virtuelle Computer im virtuellen Netzwerk eine Verbindung mit der Domäne für Vorgänge wie Domänenbeitritt herstellen.
    
    ![Domänendienste aktiviert – beide IP-Adressen bereitgestellt](./media/active-directory-domain-services-getting-started/domain-services-enabled-bothdcs-available.png)

> [!NOTE]
> Je nach Größe Ihres Azure AD-Mandanten (Anzahl von Benutzern, Gruppen usw.) nimmt die Synchronisierung mit der verwalteten Domäne mehr oder weniger Zeit in Anspruch. Diese Synchronisation erfolgt im Hintergrund. Bei umfangreichen Mandanten mit Zehntausenden von Objekten kann es bis zu zwei Tage dauern, bis alle Benutzer, Gruppenmitgliedschaften und Anmeldeinformationen synchronisiert wurden.
> 
> 

<br>

## <a name="task-4---update-dns-settings-for-the-azure-virtual-network"></a>Aufgabe 4: Aktualisieren der DNS-Einstellungen für das virtuelle Azure-Netzwerk
Als nächste Konfigurationsaufgabe steht die [Aktualisierung der DNS-Einstellungen für das virtuelle Azure-Netzwerk](active-directory-ds-getting-started-dns.md)an.




<!--HONumber=Dec16_HO2-->


