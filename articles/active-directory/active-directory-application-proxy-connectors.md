---
title: Azure AD-App-Proxyconnectors im klassischen Portal | Microsoft-Dokumentation
description: "Erläutert das Erstellen und Verwalten von Connectorgruppen im Azure AD-Anwendungsproxy."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: harshja
ms.assetid: b283796a-9679-4c79-b703-802bb850f65d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/23/2017
ms.author: kgremban
ms.translationtype: Human Translation
ms.sourcegitcommit: 54b5b8d0040dc30651a98b3f0d02f5374bf2f873
ms.openlocfilehash: d37f37f77a32f388fac4271af2a5ebda21c014eb
ms.contentlocale: de-de
ms.lasthandoff: 04/28/2017


---
# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Veröffentlichen von Anwendungen in getrennten Netzwerken und an getrennten Standorten mithilfe von Connectorgruppen
> [!div class="op_single_selector"]
> * [Azure-Portal](active-directory-application-proxy-connectors-azure-portal.md)
> * [Klassisches Azure-Portal](active-directory-application-proxy-connectors.md)
>
>

Connectorgruppen sind unter anderem in folgenden Szenarien nützlich:

* Standorte mit mehreren verbundenen Rechenzentren. In diesem Fall soll jeweils möglichst viel Datenverkehr innerhalb des Rechenzentrums verlaufen, da Verbindungen zwischen Rechenzentren für gewöhnlich teuer und langsam sind. Sie können in jedem Rechenzentrum Connectors bereitstellen, die jeweils nur die lokalen Anwendungen bedienen. Mit diesem Vorgehen werden bei völliger Transparenz für den Benutzer die Verbindungen zwischen den Rechenzentren minimiert.
* Verwalten von Anwendungen, die in isolierten Netzwerken installiert, jedoch nicht Teil des Hauptnetzwerks des Unternehmens sind. Connectorgruppen können verwendet werden, um dedizierte Connectors für isolierte Netzwerke zu installieren und außerdem die Anwendungen vom Netzwerk zu isolieren.
* Für IaaS-Anwendungen mit Cloudzugriff bieten Connectorgruppen einen gemeinsamen Dienst zum Absichern des Zugriffs auf alle Apps. Connectorgruppen erzeugen keine zusätzliche Abhängigkeit vom Unternehmensnetzwerk und beeinträchtigen nicht das App-Erlebnis. Connectors können in allen Cloudrechenzentren installiert werden und bedienen nur Anwendungen, die sich in diesem Netzwerk befinden. Es können mehrere Connectors installiert werden, um eine hohe Verfügbarkeit zu gewährleisten.
* Unterstützung für Umgebungen mit mehreren Gesamtstrukturen, in denen spezifische Connectors in einzelnen Gesamtstrukturen bereitgestellt werden können, um dort bestimmte Anwendungen zu bedienen.
* Connectorgruppen können bei Websites als Teil von Notfallkonzepten eingesetzt werden, um ein Failover zu erkennen oder als Datensicherung für die Hauptwebsite zu dienen.
* Connectorgruppen können auch verwendet werden, um mehrere Unternehmen von einem einzigen Mandanten aus zu bedienen.

## <a name="prerequisite-create-your-connectors"></a>Voraussetzung: Erstellen der Connectors
Um Ihre Connectors zu gruppieren [installieren Sie mehrere Connectors](active-directory-application-proxy-enable.md), und benennen und gruppieren Sie sie. Im letzten Schritt müssen die Connectors dann bestimmten Apps zugewiesen werden.

## <a name="step-1-create-connector-groups"></a>Schritt 1: Erstellen von Connectorgruppen
Sie können beliebig viele Connectorgruppen erstellen. Das Erstellen von Connectorgruppen erfolgt im klassischen Azure-Portal.

1. Wählen Sie Ihr Verzeichnis aus, und klicken Sie auf **Konfigurieren**.  
    ![Screenshot zum Konfigurieren des Anwendungsproxys – Klicken auf „Connectorgruppen verwalten“](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)
2. Klicken Sie dann unter „Anwendungsproxy“ auf **Connectorgruppen verwalten**, und erstellen Sie eine neue Connectorgruppe, indem Sie die Gruppe benennen.  
    ![Screenshot zu Connectorgruppen für Anwendungsproxys – neue Gruppe benennen](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)

## <a name="step-2-assign-connectors-to-your-groups"></a>Schritt 2: Zuordnen von Connectors zu Ihren Gruppen
Nachdem die Connectorgruppen erstellt wurden, werden die Connectors in die entsprechende Gruppe verschoben.

1. Klicken Sie unter **Anwendungsproxy** auf **Connectors verwalten**.
2. Wählen Sie unter **Gruppe**die Gruppe für jeden Connector aus. Beachten Sie, dass es bis zu 10 Minuten dauern kann, bis die Connectors in der neuen Gruppe aktiv werden.  
    ![Screenshot zu Anwendungsproxyconnectors – Gruppe aus dem Dropdown-Menü auswählen](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)

## <a name="step-3-assign-applications-to-your-connector-groups"></a>Schritt 3: Zuweisen von Anwendungen zu Ihren Connectorgruppen
Der letzte Schritt besteht darin, für jede Anwendung die Connectorgruppe festzulegen, von der sie bedient wird.

1. Wählen Sie im klassischen Azure-Portal in Ihrem Verzeichnis die Anwendung aus, die Sie der Gruppe zuweisen möchten, und klicken Sie auf **Konfigurieren**.
2. Wählen Sie unter **Connectorgruppe**die Gruppe aus, die die Anwendung verwenden soll. Diese Änderung wird sofort übernommen.  
    ![Screenshot zu Anwendungsproxy-Connectorgruppen – Gruppe aus dem Dropdown-Menü auswählen](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)

## <a name="see-also"></a>Siehe auch
* [Aktivieren des Anwendungsproxys](active-directory-application-proxy-enable.md)
* [Aktivieren der einmaligen Anmeldung](active-directory-application-proxy-sso-using-kcd.md)
* [Aktivieren des bedingten Zugriffs](active-directory-application-proxy-conditional-access.md)
* [Problembehandlung von Anwendungsproxys](active-directory-application-proxy-troubleshoot.md)

Aktuelle Neuigkeiten und Updates finden Sie im [Blog zum Anwendungsproxy](http://blogs.technet.com/b/applicationproxyblog/)

