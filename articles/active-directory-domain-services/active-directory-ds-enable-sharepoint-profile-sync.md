---
title: "Azure Active Directory Domain Services: Aktivieren der Unterstützung für den SharePoint-Benutzerprofildienst | Microsoft-Dokumentation"
description: "Konfigurieren von in Azure Active Directory Domain Services verwalteten Domänen für die Unterstützung der Profilsynchronisierung für SharePoint Server"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 938a5fbc-2dd1-4759-bcce-628a6e19ab9d
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/28/2017
ms.author: maheshu
translationtype: Human Translation
ms.sourcegitcommit: 2ef700f9a27d27997d53143260603bda822aa104
ms.openlocfilehash: b84853da989d18414a31cf5b178c6506abc56bbe


---

# <a name="configure-a-managed-domain-to-support-profile-synchronization-for-sharepoint-server"></a>Konfigurieren einer verwalteten Domäne für die Unterstützung der Profilsynchronisierung für SharePoint Server
SharePoint Server umfasst einen Benutzerprofildienst, der für die Benutzerprofilsynchronisierung verwendet wird. Zum Einrichten des Benutzerprofildiensts müssen in einer Active Directory-Domäne entsprechende Berechtigungen erteilt werden. Weitere Informationen finden Sie unter [Erteilen von Berechtigungen für Active Directory Domain Services für die Profilsynchronisierung in SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx).

In diesem Artikel wird erläutert, wie in Azure AD Domain Services verwaltete Domänen für die Bereitstellung des Benutzerprofil-Synchronisierungsdiensts von SharePoint Server konfiguriert werden können.

## <a name="the-aad-dc-service-accounts-group"></a>Die Gruppe „AAD-DC-Dienstkonten“
Eine Sicherheitsgruppe namens **AAD-DC-Dienstkonten** steht innerhalb der Organisationseinheit „Benutzer“ in Ihrer verwalteten Domäne zur Verfügung. Diese Gruppe wird im MMC-Snap-In **Active Directory-Benutzer und -Computer** für Ihre verwaltete Domäne angezeigt.

![Die Sicherheitsgruppe „AAD-DC-Dienstkonten“](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts.png)

An Mitglieder dieser Sicherheitsgruppe werden die folgenden Berechtigungen delegiert:
- Die Berechtigung „Verzeichnisänderungen replizieren“ für den Stamm-DSE der verwalteten Domäne.
- Die Berechtigung „Verzeichnisänderungen replizieren“ im Konfigurationsnamenskontext (cn=Konfigurationscontainer) der verwalteten Domäne.

Diese Sicherheitsgruppe ist außerdem ein Mitglied der integrierten Gruppe **Prä-Windows 2000 kompatibler Zugriff**.

![Die Sicherheitsgruppe „AAD-DC-Dienstkonten“](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-properties.png)


## <a name="enable-your-managed-domain-to-support-sharepoint-server-user-profile-sync"></a>Aktivieren der verwalteten Domäne für die Unterstützung der Benutzerprofilsynchronisierung in SharePoint Server
Sie können den Dienstkontobenutzer für die SharePoint-Benutzerprofilsynchronisierung der Gruppe **AAD-DC-Dienstkonten** hinzufügen. Dadurch erhält das Synchronisierungskonto die entsprechenden Berechtigungen zum Replizieren von Änderungen in das Verzeichnis. Dieser Konfigurationsschritt ermöglicht eine ordnungsgemäße Ausführung der SharePoint Server-Benutzerprofilsynchronisierung.

![AAD-DC-Dienstkonten – Mitglieder hinzufügen](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member.png)

![AAD-DC-Dienstkonten – Mitglieder hinzufügen](./media/active-directory-domain-services-admin-guide/aad-dc-service-accounts-add-member2.png)

## <a name="related-content"></a>Verwandte Inhalte
* [Technische Referenz – Erteilen von AD DS-Berechtigungen für die Profilsynchronisierung in SharePoint Server 2013](https://technet.microsoft.com/library/hh296982.aspx)



<!--HONumber=Jan17_HO5-->


