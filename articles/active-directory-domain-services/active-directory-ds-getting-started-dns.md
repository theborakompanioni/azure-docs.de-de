---
title: "Azure Active Directory Domain Services: Aktualisieren der DNS-Einstellungen für das virtuelle Azure-Netzwerk | Microsoft-Dokumentation"
description: Erste Schritte mit Azure Active Directory Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: d4f3e82c-6807-4690-b298-4eabad2b7927
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/06/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 785d3a8920d48e11e80048665e9866f16c514cf7
ms.openlocfilehash: abb27292d4b5533fe6f3d66d6921fea8c82f18dd
ms.lasthandoff: 04/12/2017


---
# <a name="update-dns-settings-for-the-azure-virtual-network"></a>Aktualisieren der DNS-Einstellungen für das virtuelle Azure-Netzwerk
## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Aufgabe 4: Aktualisieren der DNS-Einstellungen für das virtuelle Azure-Netzwerk
In den vorangegangenen Konfigurationsaufgaben haben Sie Azure Active Directory Domain Services für Ihr Verzeichnis erfolgreich aktiviert. Als Nächstes stellen Sie sicher, dass Computer im virtuellen Netzwerk eine Verbindung mit diesen Diensten herstellen und diese nutzen können. In diesem Artikel aktualisieren Sie die DNS-Servereinstellungen für Ihr virtuelles Netzwerk so, dass sie auf die beiden IP-Adressen verweisen, unter denen Azure Active Directory Domain Services im virtuellen Netzwerk verfügbar ist.

> [!NOTE]
> Notieren Sie sich nach dem Aktivieren von Azure Active Directory Domain Services für das Verzeichnis die dazugehörigen IP-Adressen, die auf der Registerkarte **Konfigurieren** Ihres Verzeichnisses angezeigt werden.
>
>

Gehen Sie wie folgt vor, um die DNS-Servereinstellung für das virtuelle Netzwerk zu aktualisieren, in dem Sie Azure Active Directory Domain Services aktiviert haben:

1. Wechseln Sie zum [klassischen Azure-Portal](https://manage.windowsazure.com).
2. Klicken Sie im linken Bereich auf **Netzwerke**.  
    Das Fenster **Netzwerke** wird geöffnet.

    ![Fenster „Virtuelle Netzwerke“](./media/active-directory-domain-services-getting-started/virtual-network-select.png)
3. Wählen Sie auf der Registerkarte **Virtuelle Netzwerke** das virtuelle Netzwerk aus, in dem Sie Azure Active Directory Domain Services aktiviert haben, um dessen Eigenschaften anzuzeigen.
4. Klicken Sie auf die Registerkarte **Configure** .

    ![Fenster „Virtuelle Netzwerke“](./media/active-directory-domain-services-getting-started/virtual-network-configure-tab.png)
5. Geben Sie im Abschnitt **DNS-Server** beide IP-Adressen ein, die auf der Registerkarte **Konfigurieren** Ihres Verzeichnisses im Abschnitt **Domänendienste** angezeigt wurden.
6. Klicken Sie im Aufgabenbereich am unteren Rand der Seite auf **Speichern**, um die DNS-Servereinstellungen für dieses virtuelle Netzwerk zu speichern.

   ![Aktualisieren der DNS-Servereinstellungen für das virtuelle Netzwerk](./media/active-directory-domain-services-getting-started/update-dns.png)

> [!NOTE]
> Nach der Aktualisierung der DNS-Servereinstellungen für das virtuelle Netzwerk kann es eine Weile dauern, bis virtuelle Computer im Netzwerk die aktualisierte DNS-Konfiguration erhalten. Wenn für einen virtuellen Computer keine Verbindung mit der Domäne hergestellt werden kann, können Sie den DNS-Cache („ipconfig /flushdns“) auf dem virtuellen Computer leeren. Dieser Befehl erzwingt eine Aktualisierung der DNS-Einstellungen auf dem virtuellen Computer.
>
>

## <a name="next-steps"></a>Nächste Schritte
Aufgabe 5: [Aktivieren der Kennwortsynchronisierung für Azure Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)

