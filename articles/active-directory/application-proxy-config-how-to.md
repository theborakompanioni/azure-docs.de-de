---
title: Konfigurieren einer Anwendungsproxyanwendung | Microsoft-Dokumentation
description: Informationen zum Erstellen und Konfigurieren einer Anwendungsproxyanwendung in wenigen einfachen Schritten
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 9abec188a5677942fd9eb9192c4067d2851a1969
ms.contentlocale: de-de
ms.lasthandoff: 04/17/2017

---

<a id="how-to-configure-an-application-proxy-application" class="xliff"></a>

# Konfigurieren einer Anwendungsproxyanwendung

In diesem Artikel wird das Konfigurieren einer Anwendungsproxyanwendung in Azure AD erläutert, um Ihre lokalen Anwendungen in der Cloud verfügbar zu machen.

<a id="recommended-documents" class="xliff"></a>

## Empfohlene Dokumente 

Informationen zu den Erstkonfigurationen und zur Erstellung einer Anwendungsproxyanwendung über das Administratorportal finden Sie unter [Veröffentlichen von Anwendungen mit Azure AD-Anwendungsproxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal).

Ausführliche Informationen zum Konfigurieren von Connectors finden Sie unter [Aktivieren des Anwendungsproxys über das Azure-Portal](active-directory-application-proxy-enable.md).

Informationen zum Hochladen von Zertifikaten und Verwenden von benutzerdefinierten Domänen finden Sie unter [Arbeiten mit benutzerdefinierten Domänen im Azure AD-Anwendungsproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains).

<a id="create-the-applicationsetting-the-urls" class="xliff"></a>

## Erstellen der Anwendung/Festlegen der URLs

Wenn Sie die Schritte in der Dokumentation [Veröffentlichen von Anwendungen mit Azure AD-Anwendungsproxy](https://docs.microsoft.com/azure/active-directory/application-proxy-publish-azure-portal) befolgen und beim Erstellen der Anwendung einen Fehler erhalten, finden Sie weitere Informationen und Vorschläge zur Behebung der Anwendung in den Fehlerdetails. Die meisten Fehlermeldungen enthalten eine empfohlene Problemlösung. Stellen Sie Folgendes sicher, um häufige Fehler zu vermeiden:

-   Sie sind ein Administrator mit der Berechtigung zum Erstellen einer Anwendungsproxyanwendung.

-   Die interne URL ist eindeutig.

-   Die externe URL ist eindeutig.

-   Die URLs beginnen mit „http“ oder „https“ und enden mit einem „/“.

-   Die URL muss ein Domänenname, keine IP-Adresse sein.

Die Fehlermeldung sollte beim Erstellen der Anwendung in der oberen rechten Ecke angezeigt werden. Sie können auch das Benachrichtigungssymbol auswählen, um die Fehlermeldungen anzuzeigen.

   ![Benachrichtigungsaufforderung](./media/application-proxy-config-how-to/error-message.png)

<a id="configure-connectorsconnector-groups" class="xliff"></a>

## Konfigurieren von Connectors/Connectorgruppen

Wenn Sie aufgrund einer Warnung zu Connectors oder Connectorgruppen Probleme beim Konfigurieren Ihrer Anwendung haben, finden Sie weitere Informationen zum Herunterladen von Connectors in den Anweisungen zum Aktivieren des Anwendungsproxys. Weitere Informationen zu Connectors finden Sie in der [Connectors-Dokumentation](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).

Wenn die Connectors inaktiv sind, bedeutet dies, dass sie den Dienst nicht erreichen können. Der Grund hierfür ist häufig, dass die erforderlichen Ports nicht geöffnet sind. Eine Liste der erforderlichen Ports finden Sie im Abschnitt „Voraussetzungen“ der Dokumentation zum Aktivieren des Anwendungsproxys.

<a id="upload-certificates-for-custom-domains" class="xliff"></a>

## Hochladen von Zertifikaten für benutzerdefinierte Domänen

Benutzerdefinierte Domänen bieten Ihnen die Möglichkeit, die Domäne Ihrer externen URLs anzugeben. Sie müssen das Zertifikat für diese Domäne hochladen, damit Sie benutzerdefinierte Domänen verwenden können. Informationen zu benutzerdefinierten Domänen und Zertifikaten finden Sie unter [Arbeiten mit benutzerdefinierten Domänen im Azure AD-Anwendungsproxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-custom-domains). 

Wenn beim Hochladen des Zertifikats Probleme auftreten, suchen Sie im Portal nach den Fehlermeldungen, um weitere Informationen zu den Problemen mit dem Zertifikat zu erhalten. Allgemeine Zertifikatprobleme umfassen Folgendes:

-   Abgelaufenes Zertifikat

-   Selbstsigniertes Zertifikat

-   Fehlender privater Schlüssel für Zertifikat

Die Fehlermeldung wird in der oberen rechten Ecke angezeigt, während Sie versuchen, das Zertifikat hochzuladen. Sie können auch das Benachrichtigungssymbol auswählen, um die Fehlermeldungen anzuzeigen.

   ![Benachrichtigungsaufforderung](./media/application-proxy-config-how-to/error-message2.png)

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
[Veröffentlichen von Anwendungen mit Azure AD-Anwendungsproxy](application-proxy-publish-azure-portal.md)

