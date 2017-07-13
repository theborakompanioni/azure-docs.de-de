---
title: "Azure AD Connect: Unterstützte Topologien | Microsoft Docs"
description: "In diesem Thema werden unterstützte und nicht unterstützte Topologien für Azure AD Connect behandelt."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 1034c000-59f2-4fc8-8137-2416fa5e4bfe
ms.service: active-directory
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: identity
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.translationtype: Human Translation
ms.sourcegitcommit: 1da3bc5454111bac5e958bf26d33cf61a5d4213c
ms.openlocfilehash: ecff6e2e5be05499896ad23675682db184c634af
ms.contentlocale: de-de
ms.lasthandoff: 02/17/2017

---
<a id="topologies-for-azure-ad-connect" class="xliff"></a>

# Topologien für Azure AD Connect
In diesem Artikel werden verschiedene lokale und Azure Active Directory (Azure AD)-Topologien beschrieben, die Azure AD Connect-Synchronisierung als Schlüsselintegrationslösung verwenden. In diesem Artikel werden sowohl unterstützte als auch nicht unterstützte Konfigurationen beschrieben.

Legende für Bilder im Artikel:

| Beschreibung | Symbol |
| --- | --- |
| Lokale Active Directory-Gesamtstruktur |![Lokale Active Directory-Gesamtstruktur](./media/active-directory-aadconnect-topologies/LegendAD1.png) |
| Lokales Active Directory mit gefiltertem Import |![Active Directory mit gefiltertem Import](./media/active-directory-aadconnect-topologies/LegendAD2.png) |
| Azure AD Connect-Synchronisierungsserver |![Azure AD Connect-Synchronisierungsserver](./media/active-directory-aadconnect-topologies/LegendSync1.png) |
| Azure AD Connect-Synchronisierungsserver „Stagingmodus“ |![Azure AD Connect-Synchronisierungsserver „Stagingmodus“](./media/active-directory-aadconnect-topologies/LegendSync2.png) |
| GALSync mit Forefront Identity Manager (FIM) 2010 oder Microsoft Identity Manager (MIM) 2016 |![GALSync mit FIM 2010 oder MIM 2016](./media/active-directory-aadconnect-topologies/LegendSync3.png) |
| Azure AD Connect-Synchronisierungsserver, detailliert |![Azure AD Connect-Synchronisierungsserver, detailliert](./media/active-directory-aadconnect-topologies/LegendSync4.png) |
| Azure AD |![Azure Active Directory](./media/active-directory-aadconnect-topologies/LegendAAD.png) |
| Nicht unterstütztes Szenario |![Nicht unterstütztes Szenario](./media/active-directory-aadconnect-topologies/LegendUnsupported.png) |

<a id="single-forest-single-azure-ad-tenant" class="xliff"></a>

## Einzelne Gesamtstruktur, einzelner Azure AD-Mandant
![Topologie für einzelne Gesamtstruktur und einzelnen Mandanten](./media/active-directory-aadconnect-topologies/SingleForestSingleDirectory.png)

Die häufigste Topologie ist eine einzelne lokale Gesamtstruktur mit einer oder mehreren Domänen und einem einzelnen Azure AD-Mandanten. Für die Azure AD-Authentifizierung wird die Kennwortsynchronisierung verwendet. Die Expressinstallation von Azure AD Connect unterstützt nur diese Topologie.

<a id="single-forest-multiple-sync-servers-to-one-azure-ad-tenant" class="xliff"></a>

### Einzelne Gesamtstruktur, mehrere Synchronisierungsserver zu einem Azure AD-Mandanten
![Nicht unterstützte gefilterte-Topologie für eine einzelne Gesamtstruktur](./media/active-directory-aadconnect-topologies/SingleForestFilteredUnsupported.png)

Das Verbinden mehrerer Azure AD Connect-Synchronisierungsserver mit dem gleichen Azure AD-Mandanten wird nicht unterstützt, [Stagingserver](#staging-server) ausgenommen. Dies wird auch dann nicht unterstützt, wenn diese Server für die Synchronisierung sich gegenseitig ausschließender Objektsätze konfiguriert sind. Diese Topologie haben Sie möglicherweise in Betracht gezogen, wenn Sie nicht alle Domänen in der Gesamtstruktur über einen einzelnen Server erreichen können oder die Last auf mehrere Server verteilen möchten.

<a id="multiple-forests-single-azure-ad-tenant" class="xliff"></a>

## Mehrere Gesamtstrukturen, einzelner Azure AD-Mandant
![Topologie für mehrere Gesamtstrukturen und einen einzelnen Mandanten](./media/active-directory-aadconnect-topologies/MultiForestSingleDirectory.png)

Viele Organisationen verfügen über Umgebungen mit mehreren lokalen Active Directory-Gesamtstrukturen. Für die Verwendung mehrerer lokaler Active Directory-Gesamtstrukturen gibt es verschiedene Gründe. Typische Beispiele sind Entwürfe mit Kontoressourcengesamtstrukturen und Gesamtstrukturen nach einer Unternehmensfusion oder -übernahme.

Wenn Sie über mehrere Gesamtstrukturen verfügen, müssen alle Gesamtstrukturen von einem einzelnen Azure AD Connect-Synchronisierungsserver aus erreichbar sein. Der Server muss keiner Domäne angehören. Wenn alle Gesamtstrukturen erreichbar sein müssen, kann der Server in einem Umkreisnetzwerk (auch bekannt als DMZ, demilitarisierte Zone und überwachtes Subnetz) platziert werden.

Der Installationsassistent von Azure AD Connect bietet mehrere Optionen zur Konsolidierung von Benutzern in mehreren Gesamtstrukturen. Das Ziel besteht darin, dass ein Benutzer lediglich einmal in Azure AD vorhanden ist. Es gibt einige allgemeine Topologien, die Sie im benutzerdefinierten Installationspfad im Installations-Assistenten konfigurieren können. Wählen Sie auf der Seite **Eindeutige Identifizierung der Benutzer** die entsprechende Option für Ihre Topologie aus. Die Konsolidierung wird nur für Benutzer konfiguriert. Duplizierte Gruppen werden mit der Standardkonfiguration nicht konsolidiert.

Allgemeine Topologien werden in den Abschnitten [Separate Topologien](#multiple-forests-separate-topologies), [Vollständig vermaschtes Netz](#multiple-forests-full-mesh-with-optional-galsync) und [Kontoressource](#multiple-forests-account-resource-forest) erläutert.

Die Standardkonfiguration der Azure AD Connect-Synchronisierung basiert auf folgenden Annahmen:

* Jeder Benutzer hat nur ein einziges aktiviertes Konto, und die Gesamtstruktur, in der sich dieses Konto befindet, wird verwendet, um den Benutzer zu authentifizieren. Diese Annahme gilt sowohl für die Kennwortsynchronisierung als auch für den Verbund. „userPrincipalName“ und „sourceAnchor/immutableID“ stammen aus dieser Gesamtstruktur.
* Jeder Benutzer hat nur ein Postfach.
* Die Gesamtstruktur, die das Postfach für einen Benutzer hostet, hat die beste Datenqualität für Attribute, die in der globalen Exchange-Adressliste (Global Address List, GAL) sichtbar sind. Wenn kein Postfach für den Benutzer vorhanden ist, kann jede Gesamtstruktur verwendet werden, um diese Attributwerte beizutragen.
* Wenn Sie über ein verknüpftes Postfach verfügen, wird auch ein Konto in einer anderen Gesamtstruktur für die Anmeldung verwendet.

Wenn Ihre Umgebung diesen Annahmen nicht entspricht, passiert Folgendes:

* Wenn Sie über mehrere aktive Konten oder Postfächer verfügen, wählt das Synchronisierungsmodul eines davon aus und ignoriert die anderen.
* Verknüpfte Postfächer ohne anderes aktives Konto werden nicht in Azure AD exportiert. Das Benutzerkonto wird nicht als Mitglied einer Gruppe dargestellt. Ein verknüpftes Postfach in DirSync wird stets als normales Postfach angezeigt. Diese Änderung ist eine absichtlich herbeigeführte Verhaltensänderung, um Szenarien mit mehreren Gesamtstrukturen besser unterstützen zu können.

Ausführlichere Informationen finden Sie unter [Grundlegendes zur Standardkonfiguration](active-directory-aadconnectsync-understanding-default-configuration.md).

<a id="multiple-forests-multiple-sync-servers-to-one-azure-ad-tenant" class="xliff"></a>

### Mehrere Gesamtstrukturen, mehrere Synchronisierungsserver zu einem Azure AD-Mandanten
![Nicht unterstützte Topologie für mehrere Gesamtstrukturen und mehrere Synchronisierungsserver](./media/active-directory-aadconnect-topologies/MultiForestMultiSyncUnsupported.png)

Das Verbinden mehrerer Azure AD Connect-Synchronisierungsserver mit einem einzelnen Azure AD-Mandanten wird nicht unterstützt. Von dieser Regel sind lediglich [Stagingserver](#staging-server)ausgenommen.

<a id="multiple-forests-separate-topologies" class="xliff"></a>

### Mehrere Gesamtstrukturen, separate Topologien
![Option: Benutzer werden nur einmal für alle Verzeichnisse dargestellt](./media/active-directory-aadconnect-topologies/MultiForestUsersOnce.png)

![Abbildung mehrerer Gesamtstrukturen und separater Topologien](./media/active-directory-aadconnect-topologies/MultiForestSeperateTopologies.png)

In dieser Umgebung werden alle lokalen Gesamtstrukturen als getrennte Entitäten behandelt. Kein Benutzer ist in einer anderen Gesamtstruktur vorhanden. Jede Gesamtstruktur verfügt über eine eigene Exchange-Organisation, und es gibt keine GALSync zwischen den Gesamtstrukturen. Diese Topologie kann beispielsweise nach einer Fusion/Übernahme oder in einer Organisation vorliegen, in der jede Geschäftseinheit unabhängig agiert. Diese Gesamtstrukturen befinden sich in der gleichen Organisation in Azure AD und werden mit einer einheitlichen GAL angezeigt. In der vorherigen Abbildung wird jedes Objekt in jeder Gesamtstruktur einmal im Metaverse dargestellt und im Azure AD-Mandanten aggregiert.

<a id="multiple-forests-match-users" class="xliff"></a>

### Mehrere Gesamtstrukturen: Benutzer abgleichen
Diese Szenarien haben alle eins gemeinsam: Die Verteiler- und Sicherheitsgruppen können eine Kombination aus Benutzern, Kontakten und fremden Sicherheitsprinzipalen (Foreign Security Principals, FSPs) enthalten. FSPs werden in Active Directory Domain Services (AD DS) verwendet, um Mitglieder aus anderen Gesamtstrukturen in einer Sicherheitsgruppe darzustellen. Alle FSPs werden zu dem eigentlichen Objekt in Azure AD aufgelöst.

<a id="multiple-forests-full-mesh-with-optional-galsync" class="xliff"></a>

### Mehrere Gesamtstrukturen: Vollständiges Netz mit optionaler GALSync
![Option: Verwendung des E-Mail-Attributs für den Abgleich, wenn Benutzeridentitäten in mehreren Verzeichnissen vorhanden sind](./media/active-directory-aadconnect-topologies/MultiForestUsersMail.png)

![Vollständig vernetzte Topologie für mehrere Gesamtstrukturen](./media/active-directory-aadconnect-topologies/MultiForestFullMesh.png)

Mit einer vollständig vernetzten Topologie können sich Benutzer und Ressourcen in jeder Gesamtstruktur befinden. Häufig bestehen bidirektionale Vertrauensstellungen zwischen den Gesamtstrukturen.

Wenn Exchange in mehreren Gesamtstrukturen vorhanden ist, kann (optional) eine lokale GALSync-Lösung vorhanden sein. Dadurch wird jeder Benutzer in allen anderen Gesamtstrukturen als Kontakt dargestellt. GALSync wird in der Regel über FIM 2010 oder MIM 2016 implementiert. Azure AD Connect kann nicht für lokale GALSync verwendet werden.

In diesem Szenario werden Identitätsobjekte über das E-Mail-Attribut verknüpft. Ein Benutzer mit einem Postfach in einer Gesamtstruktur wird mit den Kontakten in den anderen Gesamtstrukturen verknüpft.

<a id="multiple-forests-account-resource-forest" class="xliff"></a>

### Mehrere Gesamtstrukturen: Kontoressourcengesamtstruktur
![Option Verwenden von ObjectSID- und msExchMasterAccountSID-Attribute für den Abgleich, wenn Identitäten in mehreren Verzeichnissen vorhanden sind](./media/active-directory-aadconnect-topologies/MultiForestUsersObjectSID.png)

![Topologie mit Kontoressourcengesamtstruktur für mehrere Gesamtstrukturen](./media/active-directory-aadconnect-topologies/MultiForestAccountResource.png)

In einer Topologie mit Kontoressourcengesamtstruktur verfügen Sie über eine oder mehrere *Kontogesamtstrukturen* mit aktiven Benutzerkonten. Außerdem verfügen Sie über mindestens eine *Ressourcengesamtstruktur* mit deaktivierten Konten.

In diesem Szenario vertraut (mindestens) eine Ressourcengesamtstruktur allen Kontogesamtstrukturen. Diese Ressourcengesamtstruktur verfügt in der Regel über ein erweitertes Active Directory-Schema mit Exchange und Lync. Alle Dienste von Exchange und Lync sowie andere freigegebene Dienste befinden sich in dieser Gesamtstruktur. Benutzer haben ein deaktiviertes Benutzerkonto in dieser Gesamtstruktur, und das Postfach ist mit der Kontengesamtstruktur verknüpft.

<a id="office-365-and-topology-considerations" class="xliff"></a>

## Office 365 und Überlegungen zur Netzwerktopologie
Für einige Office 365-Workloads gelten bestimmte Einschränkungen für unterstützte Topologien:

| Workload | Einschränkungen |
--------- | ---------
| Exchange Online | Wenn mehrere lokale Exchange-Organisationen vorhanden sind (d.h. Exchange für mehrere Gesamtstrukturen bereitgestellt wurde), müssen Sie Exchange 2013 SP1 oder höher verwenden. Weitere Informationen finden Sie unter [Hybrid-Bereitstellungen mit mehreren Active Directory-Gesamtstrukturen](https://technet.microsoft.com/library/jj873754.aspx). |
| Skype for Business | Wenn Sie mehrere lokale Gesamtstrukturen verwenden, wird nur die Topologie mit Kontoressourcengesamtstruktur unterstützt. Weitere Informationen finden Sie unter [Anforderungen für die Umgebung für Skype for Business Server 2015](https://technet.microsoft.com/library/dn933910.aspx). |


<a id="staging-server" class="xliff"></a>

## Stagingserver
![Stagingserver in einer Topologie](./media/active-directory-aadconnect-topologies/MultiForestStaging.png)

Azure AD Connect unterstützt die Installation eines zweiten Servers im *Stagingmodus*. Ein Server in diesem Modus liest Daten aus allen verbundenen Verzeichnissen, schreibt jedoch nicht in die verbundenen Verzeichnisse. Er verwendet den normalen Synchronisierungszyklus und verfügt daher über eine aktualisierte Kopie der Identitätsdaten.

Bei einem notfallbedingten Ausfall des primären Servers kann ein Failover auf den Stagingserver durchgeführt werden. Hierzu wird der Azure AD Connect-Assistent verwendet. Dieser zweite Server kann sich in einem anderen Rechenzentrum befinden, da keine Infrastruktur mit dem primären Server gemeinsam genutzt wird. Jede am primären Server vorgenommene Konfigurationsänderung muss manuell an den zweiten Server kopiert werden.

Sie können einen Stagingserver auch verwenden, um eine neue benutzerdefinierte Konfiguration und deren Auswirkungen auf die Daten zu testen. Sie können eine Vorschau der Änderungen anzeigen und die Konfiguration anpassen. Wenn Sie mit der neuen Konfiguration zufrieden sind, können Sie den Stagingserver zum aktiven Server machen und den alten aktiven Server in den Stagingmodus versetzen.

Mit dieser Methode können Sie auch den aktiven Synchronisierungsserver ersetzen. Bereiten Sie den neuen Server vor, und versetzen Sie ihn in den Stagingmodus. Vergewissern Sie sich, dass er sich in einem fehlerfreien Zustand befindet, deaktivieren Sie den Stagingmodus, um den Server zu aktivieren, und fahren Sie den derzeit aktiven Server herunter.

Sie können mehrere Stagingserver verwenden, wenn Sie mehrere Sicherungen in verschiedenen Rechenzentren benötigen.

<a id="multiple-azure-ad-tenants" class="xliff"></a>

## Mehrere Azure AD-Mandanten
Wir empfehlen einen einzelnen Mandanten in Azure AD für eine Organisation.
Wenn Sie mehrere Azure AD-Mandanten verwenden möchten, erhalten Sie im Artikel [Verwaltung administrativer Einheiten in Azure AD](../active-directory-administrative-units-management.md) weitere Informationen. Es umfasst häufige Szenarien, in denen Sie einen einzelnen Mandanten verwenden können.

![Topologie für mehrere Gesamtstrukturen und mehrere Mandanten](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectory.png)

Zwischen einem Azure AD Connect-Synchronisierungsserver und einem Azure AD-Mandanten besteht eine 1:1-Beziehung. Für jeden Azure AD-Mandanten benötigen Sie eine Azure AD Connect-Synchronisierungsserverinstallation. Die Azure AD-Mandanteninstanzen werden nach Entwurf isoliert. So werden Benutzern in einem Mandanten Benutzer im anderen Mandanten nicht angezeigt. Wünschen Sie diese Trennung, ist dies eine unterstützte Konfiguration. Andernfalls sollten Sie das Azure AD-Modell für Einzelmandanten verwenden.

<a id="each-object-only-once-in-an-azure-ad-tenant" class="xliff"></a>

### Jedes Objekt nur einmal in einem Azure AD-Mandanten
![Gefilterte-Topologie für eine einzelne Gesamtstruktur](./media/active-directory-aadconnect-topologies/SingleForestFiltered.png)

In dieser Topologie ist ein Azure AD Connect-Synchronisierungsserver mit jedem Azure AD-Mandanten verbunden. Die Azure AD Connect-Synchronisierungsserver müssen für das Filtern konfiguriert werden, sodass sie im Betrieb über jeweils exklusive Gruppen von Objekten verfügen. So können Sie beispielsweise den Gültigkeitsbereich jedes Servers auf eine bestimmte Domäne oder Organisationseinheit festlegen.

Eine DNS-Domäne kann nur in einem einzelnen Azure AD-Mandanten registriert werden. Die UPNs der Benutzer im lokalen Active Directory müssen außerdem separate Namespaces verwenden. In der vorherigen Abbildung sind drei separate UPN-Suffixe in der lokalen Active Directory-Instanz registriert: contoso.com, fabrikam.com und wingtiptoys.com. Die Benutzer in jeder lokalen Active Directory-Domäne verwenden einen anderen Namespace.

GALsync findet zwischen den Azure AD-Mandanteninstanzen nicht statt. Das Adressbuch in Exchange Online und Skype for Business zeigt nur Benutzer im gleichen Mandanten an.

Bei dieser Topologie liegen die folgenden Einschränkungen bei ansonsten unterstützten Szenarien vor:

* Nur einer der Azure AD-Mandanten kann die Exchange-Hybridbereitstellung mit der lokalen Active Directory-Instanz aktivieren.
* Windows 10-Geräte können nur mit einem Azure AD-Mandanten verknüpft werden.
* Die SSO-Option für die Kennwortsynchronisierung und Pass-Through-Authentifizierung kann nur mit einem Azure AD-Mandanten verwendet werden.

Die Anforderung für wechselseitig exklusive Gruppen von Objekten gilt auch für das Zurückschreiben. Bei dieser Topologie werden einige Zurückschreibfeatures nicht unterstützt, da sie eine lokale Einzelkonfiguration voraussetzen. Zu den Features zählen:

* Gruppenrückschreiben mit Standardkonfiguration
* Geräterückschreiben

<a id="each-object-multiple-times-in-an-azure-ad-tenant" class="xliff"></a>

### Jedes Objekt mehrmals in einem Azure AD-Mandanten
![Nicht unterstützte Topologie für eine einzelne Gesamtstruktur und mehrere Mandanten](./media/active-directory-aadconnect-topologies/SingleForestMultiDirectoryUnsupported.png) ![Nicht unterstützte Topologie für eine einzelne Gesamtstruktur und mehrere Connectors](./media/active-directory-aadconnect-topologies/SingleForestMultiConnectorsUnsupported.png)

Diese Aufgaben werden nicht unterstützt:

* Synchronisieren des gleichen Benutzers mit mehreren Azure AD-Mandanten
* Anpassen der Konfiguration, sodass Benutzer in einem Azure AD-Mandanten als Kontakte in einem anderen Azure AD-Mandanten angezeigt werden
* Ändern der Azure AD Connect-Synchronisierung, sodass Verbindungen mit mehreren Azure AD-Mandanten hergestellt werden

<a id="galsync-by-using-writeback" class="xliff"></a>

### GALSync mithilfe von Rückschreiben
![Nicht unterstützte Topologie für mehrere Gesamtstrukturen und mehrere Verzeichnisse, Schwerpunkt von GALSync auf Azure AD](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync1Unsupported.png) ![Nicht unterstützte Topologie für mehrere Gesamtstrukturen und mehrere Verzeichnisse, Schwerpunkt von GALSync auf lokales Azure Active Directory](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync2Unsupported.png)

Azure AD-Mandanten sind absichtlich isoliert. Diese Aufgaben werden nicht unterstützt:

* Anpassen der Konfiguration der Azure AD Connect-Synchronisierung, sodass Daten aus einem anderen Azure AD-Mandanten gelesen werden.
* Exportieren von Benutzern als Kontakte in ein anderes lokales AD mithilfe der Azure AD Connect-Synchronisierung

<a id="galsync-with-on-premises-sync-server" class="xliff"></a>

### GALSync mit lokalem Synchronisierungsserver
![GALSync in einer Topologie für mehrere Gesamtstrukturen und mehrere Verzeichnisse](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync.png)

Sie können FIM 2010 oder MIM 2016 lokal verwenden, um Benutzer (über GALSync) zwischen zwei Exchange-Organisationen zu synchronisieren. Die Benutzer in der einen Organisation werden in der anderen Organisation als fremde Benutzer/Kontakte angezeigt. Diese anderen lokalen Active Directory-Instanzen können dann mit ihren eigenen Azure AD-Mandanten synchronisiert werden.

<a id="next-steps" class="xliff"></a>

## Nächste Schritte
Weitere Informationen zum Installieren von Azure AD-Connect für diese Szenarios finden Sie unter [Benutzerdefinierte Installation von Azure AD Connect](active-directory-aadconnect-get-started-custom.md).

Weitere Informationen zur Konfiguration der [Azure AD Connect-Synchronisierung](active-directory-aadconnectsync-whatis.md) .

Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

