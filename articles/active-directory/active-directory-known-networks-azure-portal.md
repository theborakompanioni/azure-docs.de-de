---
title: Benannte Netzwerke in Azure Active Directory | Microsoft-Dokumentation
description: "Durch das Konfigurieren benannter Netzwerke können Sie vermeiden, dass IP-Adressen Ihrer Organisation falsch positive Ergebnisse für die Risikoereignisse „Anmeldungen aus mehreren geografischen Regionen“ und „Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten“ erzeugen."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/17/2017
ms.author: markvi
translationtype: Human Translation
ms.sourcegitcommit: 71116ade676677c5b6d14b07a038d043fba46032
ms.openlocfilehash: 02a1aae805aa36aa68f341197e44b19bc6236b7b


---
# <a name="named-networks-in-azure-active-directory"></a>Benannte Netzwerke in Azure Active Directory

> [!div class="op_single_selector"]
> * [Azure-Portal](active-directory-known-networks-azure-portal.md)
> * [Klassisches Azure-Portal](active-directory-known-networks.md)
> 
> 


Azure Active Directory erstellt einen Datensatz für jedes erkannte [Risikoereignis](active-directory-identity-protection-risk-events.md). Da die Informationen zu Risikoereignissen in den Azure Active Directory-Sicherheitsberichten verfügbar sind, erhalten Sie Einblicke in die Wahrscheinlichkeit für kompromittierte Benutzerkonten in Ihrer Umgebung.   

Möglicherweise erkennt Azure Active Directory falsch positive Ergebnisse der [Risikoereignistypen](active-directory-identity-protection-risk-events.md#risk-event-types) *Unmöglicher Ortswechsel zu atypischen Orten* und *Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten* für IP-Adressen, die sich tatsächlich im Besitz Ihrer Organisation befinden. 

Dies kann in folgenden Fällen geschehen: 

- Ein Benutzer in der Niederlassung in Boston hat sich per Remoteverbindung beim Rechenzentrum in San Francisco angemeldet. Dadurch wird das Risikoereignis *Anmeldungen aus mehreren geografischen Regionen* generiert. 

- Ein Benutzer Ihrer Organisation versucht mehrere Male, sich mit einem falschen Kennwort anzumelden. Dadurch wird das Risikoereignis *Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten* generiert. 

Damit in diesen Fällen keine irreführenden Risikoereignisse generiert werden, wird empfohlen, der Liste der öffentlichen IP-Adressen Ihres Unternehmens benannte IP-Adressbereiche hinzuzufügen.    

### <a name="to-add-your-organizations-public-ip-address-ranges-perform-the-following-steps"></a>Führen Sie zum Hinzufügen der öffentlichen IP-Adressbereiche Ihrer Organisation die folgenden Schritte aus:

1. Melden Sie sich beim Azure-Verwaltungsportal an.
 
2. Klicken Sie im linken Bereich auf **Active Directory**.

    ![Bekannte Netzwerke](./media/active-directory-known-networks-azure-portal/01.png)

3. Klicken Sie auf dem Blatt „Verzeichnis“ im Abschnitt **Verwalten** auf **Benannte Netzwerke**.

    ![Bekannte Netzwerke](./media/active-directory-known-networks-azure-portal/02.png)


4. Klicken Sie auf **Standort hinzufügen**.

    ![Bekannte Netzwerke](./media/active-directory-known-networks-azure-portal/03.png)

5. Führen Sie auf dem Blatt **Hinzufügen** folgende Aktionen aus:

    ![Bekannte Netzwerke](./media/active-directory-known-networks-azure-portal/04.png)

    a. Geben Sie im Textfeld **Name** einen Namen ein.

    b. Geben Sie im Textfeld **IP-Bereich** einen IP-Bereich ein. Der IP-Bereich muss im CIDR-Format eingegeben werden. Für eine Massenaktualisierung können Sie eine CSV-Datei mit den IP-Bereichen hochladen. Durch das Hochladen werden die IP-Bereiche in der Datei der Liste hinzugefügt, anstatt diese zu überschreiben.

    c. Klicken Sie auf **Erstellen**.


**Zusätzliche Ressourcen:**

* [Anzeigen Ihrer Zugriffs- und Nutzungsberichte](active-directory-view-access-usage-reports.md)
* [Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md)
* [Anmeldungen aus mehreren geografischen Regionen](active-directory-reporting-sign-ins-from-multiple-geographies.md)




<!--HONumber=Jan17_HO3-->


