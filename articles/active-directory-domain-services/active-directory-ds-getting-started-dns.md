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
ms.translationtype: Human Translation
ms.sourcegitcommit: fc27849f3309f8a780925e3ceec12f318971872c
ms.openlocfilehash: dacd689576dce65bbf1a975409ea7d7f2c3ada90
ms.contentlocale: de-de
ms.lasthandoff: 06/14/2017


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
> Virtuelle Computer im Netzwerk übernehmen die neuen DNS-Einstellungen erst bei einem Neustart. Wenn die DNS-Einstellungen sofort aktualisiert werden sollen, lösen Sie über das Verwaltungsportal, PowerShell oder CLI einen Neustart aus.
>
>

## <a name="next-steps"></a>Nächste Schritte
Aufgabe 5: [Aktivieren der Kennwortsynchronisierung für Azure Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)

