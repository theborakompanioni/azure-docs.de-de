---
title: "Benutzerdefinierte Domänen im Azure AD-Anwendungsproxy | Microsoft-Dokumentation"
description: "Verwalten Sie benutzerdefinierte Domänen im Azure AD-Anwendungsproxy, sodass die URL für die App immer die gleiche ist – unabhängig davon, wo Ihre Benutzer darauf zugreifen."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: harshja
ms.assetid: 2fe9f895-f641-4362-8b27-7a5d08f8600f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/13/2017
ms.author: kgremban
ms.translationtype: Human Translation
ms.sourcegitcommit: c308183ffe6a01f4d4bf6f5817945629cbcedc92
ms.openlocfilehash: 3004e694a8ea8cadf622ffeacf00f2b1ff9e550a
ms.contentlocale: de-de
ms.lasthandoff: 05/17/2017


---
# <a name="working-with-custom-domains-in-azure-ad-application-proxy"></a>Arbeiten mit benutzerdefinierten Domänen im Azure AD-Anwendungsproxy
Durch das Verwenden von Standarddomänen können Sie dieselbe URL als interne und externe URL für den Zugriff auf eine Anwendung festlegen, sodass Ihre Benutzer sich nur eine URL für den Zugriff merken müssen – unabhängig davon, von wo sie auf die Anwendung zugreifen. Außerdem können Sie im Zugriffsbereich eine zentrale Verknüpfung für die Anwendung erstellen. Wenn Sie die von Azure AD-Anwendungsproxy bereitgestellte Standarddomäne verwenden, ist keine weitere Konfiguration zum Aktivieren Ihrer Domäne erforderlich. Wenn Sie eine benutzerdefinierte Domäne verwenden, müssen Sie verschiedene Punkte berücksichtigen, damit der Anwendungsproxy Ihre Domäne erkennt und die zugehörigen Zertifikate überprüft.

## <a name="select-your-custom-domain"></a>Auswählen einer benutzerdefinierten Domäne
1. Veröffentlichen Sie Ihre Anwendung, indem Sie die Anweisungen im Abschnitt [Veröffentlichen von Anwendungen mit dem Anwendungsproxy](active-directory-application-proxy-publish.md)befolgen.
2. Sobald die Anwendung in der Liste der Anwendungen angezeigt wird, wählen Sie sie aus, und klicken Sie auf **Konfigurieren**.
3. Geben Sie unter **Externe URL**Ihre benutzerdefinierte Domäne ein.
4. Wenn Sie eine externe HTTPS-URL verwenden, werden Sie aufgefordert, ein Zertifikat hochzuladen, damit Azure die Gültigkeit der Anwendungs-URL überprüfen kann. Sie haben auch die Möglichkeit, ein Platzhalterzertifikat hochzuladen, das der externen URL der Anwendung entspricht. Diese Domäne muss in der Liste der [von Azure verifizierten Domänen](https://msdn.microsoft.com/library/azure/jj151788.aspx)enthalten sein. Azure muss über ein Zertifikat für die Domänen-URL der Anwendung oder ein Platzhalterzertifikat verfügen, das der externen URL der Anwendung entspricht.
5. Fügen Sie einen DNS-Eintrag hinzu, der die interne URL an die Anwendung weiterleitet. Dank dieses Eintrags können Sie die gleiche URL für den internen und externen Zugriff auf die Anwendung sowie eine einzige Verknüpfung in der Anwendungsliste der Benutzer verwenden.

## <a name="frequently-asked-questions"></a>Häufig gestellte Fragen
**F: Kann ich ein bereits hochgeladenes Zertifikat auswählen, ohne es erneut hochzuladen?**  
A: Bereits hochgeladene Zertifikate werden automatisch an eine Anwendung gebunden, und es stimmt genau ein Zertifikat mit dem Hostnamen der Anwendung überein.  

**F: Wie füge ich ein Zertifikat hinzu, und in welchem Format sollte das exportierte Zertifikat hochgeladen werden?**  
A: Das Zertifikat sollte von der Seite für die Anwendungskonfiguration hochgeladen werden. Beim Zertifikat sollte es sich um eine PFX-Datei handeln.  

**F: Können ECC-Zertifikate verwendet werden?**  
A: Es besteht keine explizite Einschränkung für Signaturmethoden.  

**F: Können SAN-Zertifikate verwendet werden?**  
A: Ja.  

**F: Können Platzhalterzertifikate verwendet werden?**  
A: Ja.  

**F: Können für jede Anwendung unterschiedliche Zertifikate verwendet werden?**  
Ja, es sei denn, zwei Anwendungen verwenden denselben externen Host.  

**F: Wenn ich eine neue Domäne registriere, kann ich diese Domäne verwenden?**  
A: Ja, die Liste mit den Domänen wird aus der Liste des Mandanten mit den überprüften Domänen eingefügt.  

**F: Was passiert, wenn ein Zertifikat abläuft?**  
A: Auf der Seite „Anwendungskonfiguration“ wird im Abschnitt „Zertifikate“ eine Warnung angezeigt. Wenn ein Benutzer versucht, auf die Anwendung zuzugreifen, wird eine Sicherheitswarnung angezeigt.  

**F: Wie kann ich vorgehen, wenn ich ein Zertifikat für eine bestimmte App ersetzen möchte?**  
A: Laden Sie ein neues Zertifikat über die Seite für die Anwendungskonfiguration hoch.  

**F: Kann ich ein Zertifikat löschen und ersetzen?**  
A: Wenn Sie ein neues Zertifikat hochladen, wird das alte Zertifikat automatisch gelöscht, sofern es nicht von einer anderen Anwendung verwendet wird.  

**F: Was passiert, wenn ein Zertifikat gesperrt wird?**  
A: Sperrprüfungen werden für Zertifikate nicht durchgeführt. Wenn ein Benutzer versucht, auf die Anwendung zuzugreifen, wird je nach verwendetem Browser unter Umständen eine Sicherheitswarnung angezeigt.  

**F: Kann ich ein selbstsigniertes Zertifikat verwenden?**  
A: Ja, selbstsignierte Zertifikate sind zulässig. Wenn Sie eine private Zertifizierungsstelle verwenden, sollte der Verteilungspunkt für die Zertifikatssperrliste (CDP) für das Zertifikat öffentlich sein.  

**F: Gibt es eine Ansicht, über die alle Zertifikate für einen Mandanten angezeigt werden können?**  
A: Diese Funktion wird in der aktuellen Version nicht unterstützt.  

## <a name="next-steps"></a>Nächste Schritte
* [Aktivieren des einmaligen Anmeldens](active-directory-application-proxy-sso-using-kcd.md) bei Ihren veröffentlichten Apps mithilfe der Azure AD-Authentifizierung.
* [Aktivieren des bedingten Zugriffs](active-directory-application-proxy-conditional-access.md) auf Ihre veröffentlichten Apps.
* [Hinzufügen eines benutzerdefinierten Domänennamens zu Azure Active Directory](active-directory-add-domain.md)



