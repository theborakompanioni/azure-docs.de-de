---
title: "Azure AD Connect: Aktualisieren von einer früheren Version | Microsoft Docs"
description: "Erfahren Sie etwas über die verschiedenen Methoden für das Upgrade auf die aktuelle Version von Azure Active Directory Connect, einschließlich des direkten Upgrades und der Swing-Migration."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 31f084d8-2b89-478c-9079-76cf92e6618f
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Identity
ms.date: 02/08/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: b6ec60f9e15e459f70127448eb7d9c03e4b118e8
ms.openlocfilehash: 3bd1ca8e0bb9f17b76dda68a6cb2f9f64a5d05dd
ms.lasthandoff: 02/23/2017


---
# <a name="azure-ad-connect-upgrade-from-a-previous-version-to-the-latest"></a>Azure AD Connect: Aktualisieren von einer früheren Version auf die aktuelle Version
In diesem Thema werden die verschiedenen Methoden beschrieben, die Sie verwenden können, um Ihre Azure Active Directory (Azure AD) Connect-Installation auf die aktuelle Version zu aktualisieren. Es wird empfohlen, immer die aktuelle Version von Azure AD Connect zu nutzen. Sie können auch die unter [Swing-Migration](#swing-migration) beschriebenen Schritte ausführen, wenn Sie wesentliche Änderungen an der Konfiguration vornehmen.

Wenn Sie von DirSync aktualisieren möchten, lesen Sie stattdessen [Aktualisieren des Windows Azure Active Directory-Synchronisierungstools (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md).

Es gibt verschiedene Strategien für das Upgrade von Azure AD Connect.

| Methode | Beschreibung |
| --- | --- |
| [Automatisches Upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) |Für Kunden mit einer Expressinstallation ist dies die einfachste Methode. |
| [Direktes Upgrade](#in-place-upgrade) |Wenn Sie über einen einzelnen Server verfügen, können Sie die Installation direkt auf demselben Server aktualisieren. |
| [Swing-Migration](#swing-migration) |Wenn Sie über zwei Server verfügen, können Sie einen der Server mit der neuen Version oder Konfiguration vorbereiten und den aktiven Server ändern, wenn Sie bereit sind. |

Informationen zu den Berechtigungen finden Sie unter [Erforderliche Berechtigungen für ein Upgrade](active-directory-aadconnect-accounts-permissions.md#upgrade).

> [!NOTE]
> Nachdem Sie Ihren neuen Azure AD Connect-Server aktiviert haben, um mit dem Synchronisieren von Änderungen mit Azure AD zu beginnen, dürfen Sie kein Rollback zur Verwendung von DirSync oder Azure AD Sync mehr durchführen. Ein Downgrade von Azure AD Connect auf Legacyclients, z.B. DirSync und Azure AD Sync, wird nicht unterstützt und kann zu Problemen führen, z.B. zu Datenverlust in Azure AD.

## <a name="in-place-upgrade"></a>Direktes Upgrade
Ein direktes Upgrade funktioniert für das Aktualisieren von Azure AD Sync oder Azure AD Connect. Es funktioniert nicht für einen Umstieg von DirSync oder für eine Lösung mit Forefront Identity Manager (FIM) + Azure AD-Connector.

Dies ist die bevorzugte Methode, wenn Sie über einen einzelnen Server und weniger als ca. 100.000 Objekte verfügen. Nach dem Upgrade werden ein vollständiger Import und eine vollständige Synchronisierung durchgeführt, wenn die standardmäßigen Synchronisierungsregeln geändert wurden. Dadurch wird sichergestellt, dass die neue Konfiguration auf alle vorhandenen Objekte im System angewendet wird. Dies kann einige Stunden. Die Dauer ist abhängig von der Anzahl der Objekte, die vom Synchronisierungsmodul berücksichtigt werden. Der normale Deltasynchronisierungsscheduler (der standardmäßig alle 30 Minuten synchronisiert) wird angehalten, aber die Kennwortsynchronisierung wird fortgesetzt. Sie sollten das direkte Upgrade am Wochenende durchführen. Wenn Sie an der Standardkonfiguration der neuen Azure AD Connect-Version keine Änderungen durchgeführt haben, wird stattdessen ein normaler Deltaimport/eine Deltasynchronisierung durchgeführt.  
![Direktes Upgrade](./media/active-directory-aadconnect-upgrade-previous-version/inplaceupgrade.png)

Wenn Sie vordefinierte Synchronisierungsregeln geändert haben, werden diese beim Upgrade wieder auf die Standardkonfiguration zurückgesetzt. Um sicherzustellen, dass Ihre Konfiguration zwischen Upgrades erhalten bleibt, achten Sie darauf, dass die Änderungen wie in [Best Practices zum Ändern der Standardkonfiguration](active-directory-aadconnectsync-best-practices-changing-default-configuration.md) beschrieben vorgenommen werden.

## <a name="swing-migration"></a>Swing-Migration
Wenn Sie über eine komplexe Bereitstellung oder viele Objekte verfügen, kann möglicherweise ein direktes Upgrade auf einem Livesystem nicht ausgeführt werden. Dies könnte für einige Kunden mehrere Tage dauern, und während dieser Zeit werden keine Deltaänderungen verarbeitet. Sie können diese Methode auch anwenden, wenn Sie wesentliche Änderungen an Ihrer Konfiguration planen und diese vor der Übertragung in die Cloud testen möchten.

Die empfohlene Methode für diese Szenarien ist die Swing-Migration. Dafür benötigen Sie (mindestens) zwei Server: einen aktiven Server und einen Stagingserver. Der aktive Server (die durchgezogenen blauen Linien in der Abbildung unten) ist für die aktive Produktionslast verantwortlich. Der Stagingserver (mit violettem Strichlinien gezeigt) wird mit der neuen Version oder Konfiguration vorbereitet. Wenn er vollständig bereit ist, wird dieser Server zum aktiven Server gemacht. Der vorherige aktive Server, auf dem noch die alte Version oder Konfiguration installiert ist, wird jetzt zum Stagingserver und wird aktualisiert.

Die beiden Server können unterschiedliche Versionen verwenden. Sie können zum Beispiel auf dem aktiven Server, den Sie außer Betrieb nehmen möchten, Azure AD Sync und auf dem neuen Stagingserver Azure AD Connect verwenden. Wenn Sie für die Entwicklung einer neuen Konfiguration die Swing-Migration verwenden, ist es ratsam, auf beiden Servern dieselben Versionen auszuführen.  
![Stagingserver](./media/active-directory-aadconnect-upgrade-previous-version/stagingserver1.png)

> [!NOTE]
> Einige Kunden bevorzugen für dieses Szenario die Verwendung von drei oder vier Servern. Wenn der Stagingserver aktualisiert wird, steht Ihnen in dieser Zeit kein Sicherungsserver für eine [Notfallwiederherstellung](active-directory-aadconnectsync-operations.md#disaster-recovery) zur Verfügung. Mit drei oder vier Servern kann ein Satz aus primären/Standbyservern mit der neuen Version vorbereitet werden. Dadurch ist sichergestellt, dass immer ein Stagingserver zur Verfügung steht.

Diese Schritte funktionieren auch bei einem Wechsel von Azure AD Sync oder einer Lösung mit FIM und Azure AD-Connector. Diese Schritte gelten nicht für DirSync. Die entsprechende Methode einer Swing-Migration (auch parallele Bereitstellung genannt) mit den Schritten für DirSync finden Sie unter [Aktualisieren des Azure Active Directory-Synchronisierungstools (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md).

### <a name="use-a-swing-migration-to-upgrade"></a>Verwenden der Swing-Migration für das Upgraden
1. Wenn Sie Azure AD Connect auf beiden Servern verwenden und lediglich Konfigurationsänderungen vornehmen möchten, stellen Sie sicher, dass auf dem aktiven und dem Stagingserver die gleiche Version verwendet wird. So sind die Unterschiede später leichter zu erkennen. Wenn Sie ein Upgrade von Azure AD Sync durchführen, weisen diese Server unterschiedliche Versionen auf. Wenn Sie das Upgrade von einer älteren Version von Azure AD Connect durchführen, ist es ratsam, mit den beiden Servern zu beginnen, auf denen dieselbe Version installiert ist. Dies ist jedoch nicht zwingend erforderlich.
2. Wenn Sie eine benutzerdefinierte Konfiguration erstellt haben, die auf dem Stagingserver nicht vorhanden ist, führen Sie die Schritte unter [Verschieben einer benutzerdefinierten Konfiguration vom aktiven Server auf den Stagingserver](#move-custom-configuration-from-active-to-staging-server) aus.
3. Wenn Sie ein Upgrade von einer früheren Version von Azure AD Connect durchführen, aktualisieren Sie den Stagingserver auf die neueste Version. Wenn Sie eine Verschiebung von Azure AD Sync durchführen, installieren Sie Azure AD Connect auf Ihrem Stagingserver.
4. Führen Sie mit dem Synchronisierungsmodul einen vollständigen Import und eine vollständige Synchronisierung auf dem Stagingserver aus.
5. Stellen Sie mithilfe der Schritte unter „Überprüfen“ in [Überprüfen der Konfiguration eines Servers](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server) sicher, dass die neue Konfiguration nicht zu unerwarteten Änderungen führt. Wenn etwas nicht den Erwartungen entspricht, korrigieren Sie es, und führen Sie die Import- und Synchronisierungsschritte so lange anhand dieser Schritte erneut aus, bis die Daten stimmen.
6. Machen Sie den Stagingserver zum aktiven Server. Dies ist der letzte Schritt zum Wechseln des aktiven Servers in [Überprüfen der Konfiguration eines Servers](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server).
7. Wenn Sie ein Upgrade von Azure AD Connect durchführen, aktualisieren Sie den jetzt im Stagingmodus befindlichen Server auf die aktuelle Version. Führen Sie die gleichen Schritte wie zuvor aus, um die Daten und die Konfiguration zu aktualisieren. Wenn Sie ein Upgrade von Azure AD Sync durchgeführt haben, können Sie den alten Server jetzt ausschalten und außer Betrieb nehmen.

### <a name="move-a-custom-configuration-from-the-active-server-to-the-staging-server"></a>Verschieben einer benutzerdefinierten Konfiguration vom aktiven Server auf den Stagingserver
Wenn Sie Änderungen an der Konfiguration des aktiven Servers vorgenommen haben, müssen Sie sicherstellen, dass die gleichen Änderungen auf dem Stagingserver angewendet werden.

Sie können die benutzerdefinierten Synchronisierungsregeln verschieben, die Sie mithilfe von PowerShell erstellt haben. Sie müssen weitere Änderungen auf beiden Systemen auf die gleiche Weise anwenden – die Änderungen können nicht migriert werden.

Sie müssen die folgenden Konfigurationen auf beiden Servern auf die gleiche Weise vornehmen:

* Verbindung mit den gleichen Gesamtstrukturen
* Filter von Domänen und Organisationseinheiten
* Die gleichen optionalen Features wie Kennwortsynchronisierung und Kennwortrückschreiben

**Verschieben benutzerdefinierter Synchronisierungsregeln**  
Gehen Sie folgendermaßen vor, um benutzerdefinierte Synchronisierungsregeln zu verschieben:

1. Öffnen Sie den **Synchronisierungsregel-Editor** auf dem aktiven Server.
2. Wählen Sie eine benutzerdefinierte Regel aus. Klicken Sie auf **Exportieren**. Ein Editor-Fenster wird geöffnet. Speichern Sie die temporäre Datei mit der Erweiterung „PS1“. Damit ist es ein PowerShell-Skript. Kopieren Sie die PS1-Datei auf den Stagingserver.  
   ![Exportieren von Synchronisierungsregeln](./media/active-directory-aadconnect-upgrade-previous-version/exportrule.png)
3. Die Connector-GUID weicht auf dem Stagingserver ab und muss von Ihnen geändert werden. Um die GUID abzurufen, starten Sie den **Synchronisierungsregel-Editor**. Wählen Sie eine der vordefinierten Regeln aus, die das gleiche verbundene System darstellen, und klicken Sie auf **Exportieren**. Ersetzen Sie die GUID in der PS1-Datei durch die GUID des Stagingservers.
4. Führen Sie die PS1-Datei an einer PowerShell-Eingabeaufforderung aus. Dadurch wird die benutzerdefinierte Synchronisierungsregel auf dem Stagingserver erstellt.
5. Wiederholen Sie diesen Schritt für alle Ihre benutzerdefinierten Regeln.

## <a name="next-steps"></a>Nächste Schritte
Erfahren Sie mehr zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

