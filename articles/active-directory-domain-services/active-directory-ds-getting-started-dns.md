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
ms.date: 09/20/2017
ms.author: maheshu
ms.translationtype: Human Translation
ms.sourcegitcommit: 1500c02fa1e6876b47e3896c40c7f3356f8f1eed
ms.openlocfilehash: c704ee189072ce8ed196d1ef0a23edd528a10025
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017

---
# <a name="enable-azure-active-directory-domain-services-preview"></a>Aktivieren von Azure Active Directory Domain Services (Vorschauversion)

## <a name="task-4-update-dns-settings-for-the-azure-virtual-network"></a>Aufgabe 4: Aktualisieren der DNS-Einstellungen für das virtuelle Azure-Netzwerk
In den vorangegangenen Konfigurationsaufgaben haben Sie Azure Active Directory Domain Services für Ihr Verzeichnis erfolgreich aktiviert. Als Nächstes stellen Sie sicher, dass Computer im virtuellen Netzwerk eine Verbindung mit diesen Diensten herstellen und diese nutzen können. In diesem Artikel aktualisieren Sie die DNS-Servereinstellungen für Ihr virtuelles Netzwerk so, dass sie auf die beiden IP-Adressen verweisen, unter denen Azure Active Directory Domain Services im virtuellen Netzwerk verfügbar ist.

Führen Sie die folgenden Schritte aus, um die DNS-Servereinstellung für das virtuelle Netzwerk zu aktualisieren, in dem Sie Azure Active Directory Domain Services aktiviert haben:

1. Die Registerkarte **Übersicht** enthält Liste mit **erforderlichen Konfigurationsschritten**, die ausgeführt werden müssen, nachdem die verwaltete Domäne vollständig bereitgestellt wurde. Der erste Konfigurationsschritt ist das **Aktualisieren von DNS-Servereinstellungen für Ihr virtuelles Netzwerk**.

    ![Domänendienste – Registerkarte „Übersicht“ nach der vollständigen Bereitstellung](./media/getting-started/domain-services-provisioned-overview.png)

2. Wenn Ihre Domäne vollständig bereitgestellt wurde, werden in dieser Kachel zwei IP-Adressen angezeigt. Jede dieser IP-Adressen stellt einen Domänencontroller für die verwaltete Domäne dar.

3. Zum Kopieren der ersten IP-Adresse in die Zwischenablage klicken Sie auf die Schaltfläche „Kopieren“ daneben. Klicken Sie dann auf die Schaltfläche **Konfigurieren von DNS-Servern**.

4. Fügen Sie die erste IP-Adresse in das Textfeld **DNS-Server hinzufügen** auf dem Blatt **DNS-Server** ein. Führen Sie einen horizontalen Bildlauf nach links durch, um die zweite IP-Adresse zu kopieren, und fügen Sie sie in das Textfeld **DNS-Server hinzufügen** ein.

    ![Domänendienste – DNS aktualisieren](./media/getting-started/domain-services-update-dns.png)

5. Klicken Sie anschließend auf **Speichern**, um die DNS-Server für das virtuelle Netzwerk zu aktualisieren.

> [!NOTE]
> Virtuelle Computer im Netzwerk übernehmen die neuen DNS-Einstellungen erst nach einem Neustart. Wenn die DNS-Einstellungen sofort aktualisiert werden sollen, lösen Sie über das Verwaltungsportal, PowerShell oder CLI einen Neustart aus.
>
>

## <a name="next-step"></a>Nächster Schritt
[Aufgabe 5: Aktivieren der Kennwortsynchronisierung für Azure Active Directory Domain Services](active-directory-ds-getting-started-password-sync.md)

