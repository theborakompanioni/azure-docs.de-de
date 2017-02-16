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
ms.date: 11/01/2016
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: 28b5da6098316f8fbe84966e0dac88f5b7d2cb1d
ms.openlocfilehash: e5983496cda061b28418dcf86c38fbe80c611503


---
# <a name="topologies-for-azure-ad-connect"></a>Topologien für Azure AD Connect
Das Ziel dieses Themas ist, verschiedene lokale und Azure AD-Topologien mit Azure AD Connect-Synchronisierung als Schlüsselintegrationslösung zu beschreiben. Es werden sowohl unterstützte als auch nicht unterstützte Konfigurationen beschrieben.

Legende für Bilder im Dokument:

| Beschreibung | Symbol |
| --- | --- |
| Lokale Active Directory-Gesamtstruktur |![AD](./media/active-directory-aadconnect-topologies/LegendAD1.png) |
| Active Directory mit gefiltertem Import |![AD](./media/active-directory-aadconnect-topologies/LegendAD2.png) |
| Azure AD Connect-Synchronisierungsserver |![Synchronisierung](./media/active-directory-aadconnect-topologies/LegendSync1.png) |
| Azure AD Connect-Synchronisierungsserver "Stagingmodus" |![Synchronisierung](./media/active-directory-aadconnect-topologies/LegendSync2.png) |
| GALSync mit FIM2010 oder MIM2016 |![Synchronisierung](./media/active-directory-aadconnect-topologies/LegendSync3.png) |
| Azure AD Connect-Synchronisierungsserver, detailliert |![Synchronisierung](./media/active-directory-aadconnect-topologies/LegendSync4.png) |
| Azure AD-Verzeichnis |![AAD](./media/active-directory-aadconnect-topologies/LegendAAD.png) |
| Nicht unterstütztes Szenario |![Nicht unterstützt](./media/active-directory-aadconnect-topologies/LegendUnsupported.png) |

## <a name="single-forest-single-azure-ad-tenant"></a>Einzelne Gesamtstruktur, einzelner Azure AD-Mandant
![Einzelne Gesamtstruktur – einzelner Mandant](./media/active-directory-aadconnect-topologies/SingleForestSingleDirectory.png)

Die häufigste Topologie ist eine einzelne lokale Gesamtstruktur mit einer oder mehreren Domänen und einem einzelnen Azure AD-Mandanten. Für die Azure AD-Authentifizierung wird die Kennwortsynchronisierung verwendet. Die Expressinstallation von Azure AD Connect unterstützt nur diese Topologie.

### <a name="single-forest-multiple-sync-servers-to-one-azure-ad-tenant"></a>Einzelne Gesamtstruktur, mehrere Synchronisierungsserver zu einem Azure AD-Mandanten
![Einzelne Gesamtstruktur – gefiltert (nicht unterstützt)](./media/active-directory-aadconnect-topologies/SingleForestFilteredUnsupported.png)

Mehrere Azure AD Connect-Synchronisierungsserver können nicht mit dem gleichen Azure AD-Mandanten verbunden sein, [Stagingserver](#staging-server) ausgenommen. Dies wird auch dann nicht unterstützt, wenn die Server für die Synchronisierung sich gegenseitig ausschließender Objektsätze konfiguriert sind. Diese Möglichkeit haben Sie möglicherweise in Betracht gezogen, wenn Sie nicht alle Domänen in der Gesamtstruktur über einen einzelnen Server erreichen können oder die Last auf mehrere Server verteilen möchten.

## <a name="multiple-forests-single-azure-ad-tenant"></a>Mehrere Gesamtstrukturen, einzelner Azure AD-Mandant
![Mehrere Gesamtstrukturen – einzelner Mandant](./media/active-directory-aadconnect-topologies/MultiForestSingleDirectory.png)

Viele Organisationen verfügen über Umgebungen mit mehreren lokalen Active Directory-Gesamtstrukturen. Für die Verwendung mehrerer lokaler Active Directory-Gesamtstrukturen gibt es verschiedene Gründe. Typische Beispiele sind Designs mit Kontoressourcengesamtstrukturen und Gesamtstrukturen nach einer Unternehmensfusion oder -übernahme.

Wenn Sie über mehrere Gesamtstrukturen verfügen, müssen alle Gesamtstrukturen von einem einzelnen Azure AD Connect-Synchronisierungsserver aus erreichbar sein. Der Server muss keiner Domäne angehören Wenn alle Gesamtstrukturen erreichbar sein müssen, kann der Server in einer Netzwerk-DMZ platziert werden.

Der Installations-Assistent von Azure AD Connect bietet mehrere Optionen zur Konsolidierung von Benutzern in mehreren Gesamtstrukturen. Das Ziel besteht darin, dass ein Benutzer lediglich einmal in Azure AD vorhanden ist. Es gibt einige allgemeine Topologien, die Sie im benutzerdefinierten Installationspfad im Installations-Assistenten konfigurieren können. Wählen Sie auf der Seite **Eindeutige Identifizierung der Benutzer**die entsprechende Option für Ihre Topologie aus. Die Konsolidierung wird nur für Benutzer konfiguriert. Duplizierte Gruppen werden mit der Standardkonfiguration nicht konsolidiert.

Allgemeine Topologien werden im nächsten Abschnitt erläutert: [Separate Topologien](#multiple-forests-separate-topologies), [Vollständig vermaschtes Netz](#multiple-forests-full-mesh-with-optional-galsync) und [Kontoressource](#multiple-forests-account-resource-forest).

Die Standardkonfiguration der Azure AD Connect-Synchronisierung basiert auf folgenden Annahmen:

1. Benutzer haben nur ein einziges aktiviertes Konto, und die Gesamtstruktur, in der sich dieses Konto befindet, wird verwendet, um den Benutzer zu authentifizieren. Diese Annahme gilt sowohl für die Kennwortsynchronisierung als auch für den Verbund. „userPrincipalName“ und „sourceAnchor/immutableID“ stammen aus dieser Gesamtstruktur.
2. Benutzer haben nur ein einziges Postfach.
3. Die Gesamtstruktur, die das Postfach für einen Benutzer hostet, hat die beste Datenqualität für Attribute, die in der globalen Exchange-Adressliste (Global Address List, GAL) sichtbar sind. Wenn kein Postfach für den Benutzer vorhanden ist, kann jede Gesamtstruktur verwendet werden, um diese Attributwerte beizutragen.
4. Wenn Sie über ein verknüpftes Postfach verfügen, wird auch ein anderes Konto in einer anderen Gesamtstruktur für die Anmeldung verwendet.

Sollte Ihre Umgebung diese Voraussetzungen nicht erfüllen, passiert Folgendes:

* Wenn Sie über mehrere aktive Konten oder Postfächer verfügen, wählt das Synchronisierungsmodul eines davon aus und ignoriert die anderen.
* Verknüpfte Postfächer ohne anderes aktives Konto werden nicht in Azure AD exportiert. Das Benutzerkonto wird nicht als Mitglied einer Gruppe dargestellt. Ein verknüpftes Postfach in DirSync würde stets als normales Postfach angezeigt. Diese Änderung ist eine absichtlich herbeigeführte Verhaltensänderung, um Szenarien mit mehreren Gesamtstrukturen besser unterstützen zu können.

Ausführlichere Informationen finden Sie unter [Grundlegendes zur Standardkonfiguration](active-directory-aadconnectsync-understanding-default-configuration.md).

### <a name="multiple-forests-multiple-sync-servers-to-one-azure-ad-tenant"></a>Mehrere Gesamtstrukturen, mehrere Synchronisierungsserver zu einem Azure AD-Mandanten
![Mehrere Gesamtstrukturen – Mehrfachsynchronisierung (nicht unterstützt)](./media/active-directory-aadconnect-topologies/MultiForestMultiSyncUnsupported.png)

Es können nicht mehrere Azure AD Connect-Synchronisierungsserver mit einem einzelnen Azure AD-Mandanten verbunden sein. Von dieser Regel sind lediglich [Stagingserver](#staging-server)ausgenommen.

### <a name="multiple-forests--separate-topologies"></a>Mehrere Gesamtstrukturen – separate Topologien
**Benutzer werden nur einmal für alle Verzeichnisse dargestellt.**

![Mehrere Gesamtstrukturen – Benutzer (einmalig)](./media/active-directory-aadconnect-topologies/MultiForestUsersOnce.png)

![Mehrere Gesamtstrukturen – separate Topologien](./media/active-directory-aadconnect-topologies/MultiForestSeperateTopologies.png)

In dieser Umgebung werden alle lokalen Gesamtstrukturen als separate Entitäten behandelt, und kein Benutzer ist in einer anderen Gesamtstruktur vorhanden.
Jede Gesamtstruktur verfügt über eine eigene Exchange-Organisation, und es gibt keine GALSync zwischen den Gesamtstrukturen. Diese Topologie kann beispielsweise nach einer Fusion/Übernahme oder in einer Organisation vorliegen, in der die einzelnen Geschäftseinheiten isoliert voneinander operieren. Diese Gesamtstrukturen befinden sich in der gleichen Organisation in Azure AD und werden mit einer einheitlichen GAL angezeigt.
In dieser Abbildung wird jedes Objekt in jeder Gesamtstruktur einmal im Metaverse dargestellt und im Azure AD-Mandanten aggregiert.

### <a name="multiple-forests--match-users"></a>Mehrere Gesamtstrukturen – Benutzer abgleichen
**Benutzeridentitäten sind in mehreren Verzeichnissen vorhanden.**

Diese Szenarien haben alle eins gemeinsam: Die Verteiler- und Sicherheitsgruppen können eine Kombination aus Benutzern, Kontakten und fremden Sicherheitsprinzipalen (Foreign Security Principals, FSPs) enthalten.

FSPs werden in ADDS verwendet, um Mitglieder aus anderen Gesamtstrukturen in einer Sicherheitsgruppe darzustellen. Alle FSPs werden zu dem eigentlichen Objekt in Azure AD aufgelöst.

### <a name="multiple-forests--full-mesh-with-optional-galsync"></a>Mehrere Gesamtstrukturen – vollständiges Netz mit optionaler GALSync
**Benutzeridentitäten sind in mehreren Verzeichnissen vorhanden. Abgleich über: Mail-Attribut**

![Mehrere Gesamtstrukturen – Benutzer (E-Mail)](./media/active-directory-aadconnect-topologies/MultiForestUsersMail.png)

![Mehrere Gesamtstrukturen – vollständiges Netz](./media/active-directory-aadconnect-topologies/MultiForestFullMesh.png)

Mit einer vollständig vernetzten Topologie können Benutzer und Ressourcen in jeder Gesamtstruktur ermittelt werden, und in der Regel bestehen bidirektionale Vertrauensstellungen zwischen den Gesamtstrukturen.

Wenn Exchange in mehreren Gesamtstrukturen vorhanden ist, kann optional eine lokale GALSync-Lösung vorhanden sein. Dadurch würde jeder Benutzer in allen anderen Gesamtstrukturen als Kontakt dargestellt. GALSync wird häufig mithilfe von Forefront Identity Manager 2010 oder Microsoft Identity Manager 2016 implementiert. Azure AD Connect kann nicht für lokale GALSync verwendet werden.

In diesem Szenario werden Identitätsobjekte über das E-Mail-Attribut verknüpft. Ein Benutzer mit einem Postfach in einer Gesamtstruktur wird mit den Kontakten in den anderen Gesamtstrukturen verknüpft.

### <a name="multiple-forests--account-resource-forest"></a>Mehrere Gesamtstrukturen – Kontoressourcengesamtstruktur
**Benutzeridentitäten sind in mehreren Verzeichnissen vorhanden. Abgleich über: ObjectSID- und msExchMasterAccountSID-Attribute**

![Mehrere Gesamtstrukturen – Benutzer (ObjectSID)](./media/active-directory-aadconnect-topologies/MultiForestUsersObjectSID.png)

![Mehrere Gesamtstrukturen – AccountResource](./media/active-directory-aadconnect-topologies/MultiForestAccountResource.png)

In einer Kontoressourcengesamtstruktur-Topologie befinden sich eine oder mehrere Kontogesamtstrukturen mit aktiven Benutzerkonten. Außerdem verfügen Sie über mindestens eine Ressourcengesamtstruktur mit deaktivierten Konten.

In diesem Szenario vertraut mindestens eine **Ressourcengesamtstruktur** allen **Kontogesamtstrukturen**. Diese Ressourcengesamtstruktur verfügt in der Regel über ein erweitertes AD-Schema mit Exchange und Lync. Alle Dienste von Exchange und Lync sowie andere freigegebene Dienste befinden sich in dieser Gesamtstruktur. Benutzer haben ein deaktiviertes Benutzerkonto in dieser Gesamtstruktur, und das Postfach ist mit der Kontengesamtstruktur verknüpft.

## <a name="office-365-and-topology-considerations"></a>Office 365 und Überlegungen zur Netzwerktopologie
Für einige Office 365-Workloads gelten bestimmte Einschränkungen für unterstützte Topologien. Wenn Sie eine dieser Optionen verwenden möchten, lesen Sie das Thema zu unterstützten Topologien für die Workload.

| Workload |
| --- | --- |
| Exchange Online |
| Skype for Business |

## <a name="staging-server"></a>Stagingserver
![Stagingserver](./media/active-directory-aadconnect-topologies/MultiForestStaging.png)

Azure AD Connect unterstützt die Installation eines zweiten Servers im **Stagingmodus**. Ein Server in diesem Modus liest Daten aus allen verbundenen Verzeichnissen, schreibt jedoch nicht in die verbundenen Verzeichnisse. Er verwendet den normalen Synchronisierungszyklus und verfügt daher über eine aktualisierte Kopie der Identitätsdaten. Bei einem notfallbedingten Ausfall des primären Servers kann ein Failover auf den Stagingserver durchgeführt werden. Hierzu wird der Azure AD Connect-Assistent verwendet. Dieser zweite Server kann sich vorzugsweise in einem anderen Datencenter befinden, da keine Infrastruktur mit dem primären Server gemeinsam genutzt wird. Jede am primären Server vorgenommene Konfigurationsänderung muss manuell an den zweiten Server kopiert werden.

Ein Stagingserver kann auch verwendet werden, um eine neue benutzerdefinierte Konfiguration und deren Auswirkungen auf die Daten zu testen. Sie können eine Vorschau der Änderungen anzeigen und die Konfiguration anpassen. Wenn Sie mit der neuen Konfiguration zufrieden sind, können Sie den Stagingserver zum aktiven Server machen und den alten aktiven Server in den Stagingmodus versetzen.

Mit dieser Methode können Sie auch den aktiven Synchronisierungsserver ersetzen. Bereiten Sie den neuen Server vor, und versetzen Sie ihn in den Stagingmodus. Vergewissern Sie sich, dass er sich in einem fehlerfreien Zustand befindet, deaktivieren Sie den Stagingmodus, um den Server zu aktivieren, und fahren Sie den derzeit aktiven Server herunter.

Sie können mehrere Stagingserver verwenden, wenn Sie mehrere Sicherungen in verschiedenen Datencentern benötigen.

## <a name="multiple-azure-ad-tenants"></a>Mehrere Azure AD-Mandanten
Microsoft empfiehlt die Verwendung eines einzelnen Mandanten in Azure AD für eine Organisation.
Lesen Sie vor der Planung mehrerer Azure AD-Mandanten die folgenden Themen zu gängigen Szenarien, in denen Sie einen einzelnen Mandanten verwenden können.

| Thema |
| --- | --- |
| Delegieren mithilfe administrativer Einheiten |

![Mehrere Gesamtstrukturen – mehrere Mandanten](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectory.png)

Es besteht eine 1:1-Beziehung zwischen einem Azure AD Connect-Synchronisierungsserver und einem Azure AD-Mandanten. Für jeden Azure AD-Mandanten benötigen Sie eine Azure AD Connect-Synchronisierungsserverinstallation. Azure AD-Mandanteninstanzen sind als isolierte Instanzen konzipiert, und Benutzer in einer Instanz können Benutzer in anderen Mandanten nicht sehen. Falls diese Trennung erwünscht ist, handelt es sich um eine unterstützte Konfiguration. Andernfalls sollten Sie das Azure AD-Modell für Einzelmandanten verwenden.

### <a name="each-object-only-once-in-an-azure-ad-tenant"></a>Jedes Objekt nur einmal in einem Azure AD-Mandanten
![Einzelne Gesamtstruktur – gefiltert](./media/active-directory-aadconnect-topologies/SingleForestFiltered.png)

In dieser Topologie ist ein Azure AD Connect-Synchronisierungsserver mit jedem Azure AD-Mandanten verbunden. Die Azure AD Connect-Synchronisierungsserver müssen für das Filtern konfiguriert werden, sodass sie im Betrieb über jeweils exklusive Gruppen von Objekten verfügen. So können Sie beispielsweise den Gültigkeitsbereich jedes Servers auf eine bestimmte Domäne oder OE festlegen. Eine DNS-Domäne kann nur in einem einzelnen Azure AD-Mandanten registriert werden. Die UPNs der Benutzer im lokalen AD müssen außerdem separate Namespaces verwenden. Im obigen Bild sind z. B. drei separate UPN-Suffixe im lokalen AD registriert: contoso.com, fabrikam.com und wingtiptoys.com. Die Benutzer in jeder lokalen AD-Domäne verwenden einen anderen Namespace.

GALsync findet zwischen den Azure AD-Mandanteninstanzen nicht statt. Das Adressbuch in Exchange Online und Skype for Business zeigt nur Benutzer im gleichen Mandanten an.

Bei dieser Topologie liegen die folgenden Einschränkungen bei ansonsten unterstützten Szenarien vor:

* Nur einer der Azure AD-Mandanten kann die Exchange-Hybridbereitstellung mit der lokalen Active Directory-Instanz aktivieren.
* Windows 10-Geräte können nur mit einem Azure AD-Mandanten verknüpft werden.

Die Anforderung für wechselseitig exklusive Gruppen von Objekten gilt auch für das Zurückschreiben. Bei dieser Topologie werden einige Zurückschreibfeatures nicht unterstützt, da sie eine lokale Einzelkonfiguration voraussetzen.

* Gruppenrückschreiben mit Standardkonfiguration
* Geräterückschreiben

### <a name="each-object-multiple-times-in-an-azure-ad-tenant"></a>Jedes Objekt mehrmals in einem Azure AD-Mandanten
![Einzelne Gesamtstruktur – mehrere Mandanten (nicht unterstützt)](./media/active-directory-aadconnect-topologies/SingleForestMultiDirectoryUnsupported.png) ![Einzelne Gesamtstruktur – mehrere Connectors (nicht unterstützt)](./media/active-directory-aadconnect-topologies/SingleForestMultiConnectorsUnsupported.png)

* Die Synchronisierung des gleichen Benutzers mit mehreren Azure AD-Mandanten wird nicht unterstützt.
* Es wird nicht unterstützt, die Konfiguration so anzupassen, dass Benutzer in einer Azure AD-Instanz als Kontakte in einem anderen Azure AD-Mandanten angezeigt werden.
* Auch die Änderung der Azure AD Connect-Synchronisierung, sodass Verbindungen mit mehreren Azure AD-Mandanten hergestellt werden, wird nicht unterstützt.

### <a name="galsync-by-using-writeback"></a>GALsync mithilfe von Rückschreiben
![MultiForestMultiDirectoryGALSync1Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync1Unsupported.png) ![MultiForestMultiDirectoryGALSync2Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync2Unsupported.png)

Azure AD-Mandanten sind per Design isoliert.

* Es wird nicht unterstützt, die Konfiguration der Azure AD Connect-Synchronisierung so anzupassen, dass Daten aus einem anderen Azure AD-Mandanten gelesen werden.
* Benutzer können nicht mithilfe der Azure AD Connect-Synchronisierung als Kontakte in ein anderes lokales AD exportiert werden.

### <a name="galsync-with-on-premises-sync-server"></a>GALsync mit lokalem Server
![MultiForestMultiDirectoryGALSync](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync.png)

FIM2010/MIM2016 kann lokal für die GALsync-basierte Synchronisierung von Benutzern zwischen zwei Exchange-Organisationen verwendet werden. Die Benutzer in der einen Organisation werden in der anderen Organisation als fremde Benutzer/Kontakte angezeigt. Diese anderen lokalen AD-Instanzen können dann mit ihren eigenen Azure AD-Mandanten synchronisiert werden.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Installieren von Azure AD-Connect für diese Szenarios finden Sie unter [Benutzerdefinierte Installation von Azure AD Connect](active-directory-aadconnect-get-started-custom.md).

Weitere Informationen zur Konfiguration der [Azure AD Connect-Synchronisierung](active-directory-aadconnectsync-whatis.md)

Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).




<!--HONumber=Dec16_HO3-->


