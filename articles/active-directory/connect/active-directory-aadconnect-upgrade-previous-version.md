---
title: "Azure AD Connect: Aktualisieren von einer früheren Version | Microsoft Docs"
description: "Erläutert die verschiedenen Methoden für das Upgrade auf die aktuelle Version von Azure Active Directory Connect, einschließlich des direkten Upgrades und der Swing-Migration."
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
ms.sourcegitcommit: 1f7ec5d53512dcfbff17269802c8889eae0ad744
ms.openlocfilehash: 5dd69a0b9357a601070765817a814dae3e7e5c05


---
# <a name="azure-ad-connect-upgrade-from-a-previous-version-to-the-latest"></a>Azure AD Connect: Aktualisieren von einer früheren Version auf die aktuelle Version
In diesem Thema werden die verschiedenen Methoden beschrieben, die Sie verwenden können, um Ihre Azure AD Connect-Installation auf die aktuelle Version zu aktualisieren. Es wird empfohlen, immer die aktuelle Version von Azure AD Connect zu nutzen. Die unter [Swing-Migration](#swing-migration) beschriebenen Schritte gelten auch für wesentliche Änderungen an der Konfiguration.

Wenn Sie von DirSync aktualisieren möchten, lesen Sie stattdessen [Aktualisieren des Windows Azure Active Directory-Synchronisierungstools (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md).

Es gibt verschiedene Strategien für das Upgrade von Azure AD Connect.

| Methode | Beschreibung |
| --- | --- |
| [Automatisches Upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) |Für Kunden mit einer Expressinstallation ist dies die einfachste Methode. |
| [Direktes Upgrade](#in-place-upgrade) |Wenn Sie über einen einzelnen Server verfügen, aktualisieren Sie die Installation direkt auf dem Server. |
| [Swing-Migration](#swing-migration) |Wenn Sie über zwei Server verfügen, können Sie einen der Server mit der neuen Version oder Konfiguration vorbereiten und den aktiven Server ändern, wenn Sie bereit sind. |

Die erforderlichen Berechtigungen für das Upgrade finden Sie [hier](active-directory-aadconnect-accounts-permissions.md#upgrade).

> [!NOTE]
> Nachdem Sie Ihren neuen Azure AD Connect-Server aktiviert haben, um mit dem Synchronisieren von Änderungen mit Azure AD zu beginnen, dürfen Sie kein Rollback zur Verwendung von DirSync oder Azure AD Sync mehr durchführen. Ein Downgrade von Azure AD Connect auf Legacyclients, z.B. DirSync und Azure AD Sync, wird nicht unterstützt und kann zu Problemen führen, z.B. zu Datenverlust in Azure AD. 
> 
> 

## <a name="in-place-upgrade"></a>Direktes Upgrade
Ein direktes Upgrade funktioniert für das Aktualisieren von Azure AD Sync oder Azure AD Connect. Es funktioniert nicht für DirSync oder für eine Lösung mit FIM und Azure AD-Connector.

Dies ist die bevorzugte Methode, wenn Sie über einen einzelnen Server und weniger als ca. 100.000 Objekte verfügen. Nach dem Upgrade werden ein vollständiger Import und eine vollständige Synchronisierung durchgeführt, wenn die standardmäßigen Synchronisierungsregeln geändert wurden. Dadurch wird sichergestellt, dass die neue Konfiguration auf alle vorhandenen Objekte im System angewendet wird. Dies kann einige Stunden. Die Dauer ist abhängig von der Anzahl der Objekte, die vom Synchronisierungsmodul berücksichtigt werden. Die normale geplante Deltasynchronisierung (standardmäßig alle 30 Minuten) wird angehalten, aber die Kennwortsynchronisierung wird fortgesetzt. Sie sollten das direkte Upgrade am Wochenende durchführen. Wenn Sie an der Standardkonfiguration der neuen Azure AD Connect-Version keine Änderungen durchgeführt haben, wird stattdessen ein normaler Deltaimport/eine Deltasynchronisierung durchgeführt.  
![Direktes Upgrade](./media/active-directory-aadconnect-upgrade-previous-version/inplaceupgrade.png)

Wenn Sie standardmäßige Synchronisierungsregeln geändert haben, werden diese beim Upgrade wieder auf die Standardkonfiguration zurückgesetzt. Um sicherzustellen, dass Ihre Konfiguration zwischen Upgrades erhalten bleibt, achten Sie darauf, dass die Änderungen vorgenommen werden, wie in [Bewährte Methoden zum Ändern der Standardkonfiguration](active-directory-aadconnectsync-best-practices-changing-default-configuration.md)beschrieben.

## <a name="swing-migration"></a>Swing-Migration
Wenn Sie über eine komplexe Bereitstellung oder sehr viele Objekte verfügen, kann möglicherweise ein direktes Upgrade auf einem Live-System nicht ausgeführt werden. Dies könnte für einige Kunden mehrere Tage dauern, und während dieser Zeit werden keine Deltaänderungen verarbeitet. Diese Methode wird auch angewendet, wenn Sie wesentliche Änderungen an Ihrer Konfiguration planen und diese vor der Übertragung in die Cloud testen möchten.

Die empfohlene Methode für diese Szenarien ist die Swing-Migration. Dafür benötigen Sie (mindestens) zwei Server: einen aktiven Server und einen Stagingserver. Der aktive Server (die durchgezogenen blauen Linien in der Abbildung unten) ist für die aktive Produktionslast verantwortlich. Der Stagingserver (die gestrichelten lilafarbenen Linien in der Abbildung unten) wird mit der neuen Version oder Konfiguration vorbereitet. Wenn er vollständig bereit ist, wird dieser Server zum aktiven Server gemacht. Der vorherige aktive Server, auf dem noch die alte Version oder Konfiguration installiert ist, wird jetzt zum Stagingserver und wird aktualisiert.

Die beiden Server können unterschiedliche Versionen verwenden. Sie können zum Beispiel auf dem aktiven Server, den Sie außer Betrieb nehmen möchten, Azure AD Sync und auf dem neuen Stagingserver Azure AD Connect verwenden. Wenn Sie für die Entwicklung einer neuen Konfiguration die Swing-Migration verwenden, ist es ratsam, auf beiden Servern dieselben Versionen auszuführen.  
![Stagingserver](./media/active-directory-aadconnect-upgrade-previous-version/stagingserver1.png)

Hinweis: Es wurde festgestellt, dass einige Kunden für dieses Szenario drei oder vier Server verwenden möchten. Wenn der Stagingserver aktualisiert wird, steht Ihnen in dieser Zeit kein Sicherungsserver für eine [Notfallwiederherstellung](active-directory-aadconnectsync-operations.md#disaster-recovery) zur Verfügung. Mit drei oder vier Servern kann ein Satz aus primärem/Standbyserver mit der neuen Version vorbereitet werden. Dadurch ist sichergestellt, dass immer ein Stagingserver zur Verfügung steht.

Diese Schritte funktionieren auch bei einem Wechsel von Azure AD Sync oder einer Lösung mit FIM und Azure AD-Connector. Diese Schritte gelten nicht für DirSync. Die entsprechende Methode einer Swing-Migration (auch parallele Bereitstellung genannt) mit den Schritten für DirSync finden Sie unter [Aktualisieren des Azure Active Directory-Synchronisierungstools (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md).

### <a name="swing-migration-steps"></a>Schritte bei der Swing-Migration
1. Wenn Sie Azure AD Connect auf beiden Servern verwenden und lediglich eine Änderung an der Konfiguration vornehmen möchten, stellen Sie sicher, dass auf dem aktiven und dem Stagingserver die gleiche Version verwendet wird. So sind die Unterschiede später leichter zu erkennen. Wenn Sie ein Upgrade von Azure AD Sync durchführen, weisen diese Server verschiedene Versionen auf. Wenn Sie das Upgrade von einer älteren Version von Azure AD Connect durchführen, ist es ratsam, mit den beiden Servern zu beginnen, auf denen dieselbe Version installiert ist. Dies ist jedoch nicht zwingend erforderlich.
2. Wenn Sie benutzerdefinierte Konfigurationen vorgenommen haben, die auf dem Stagingserver nicht vorhanden sind, führen Sie die Schritte unter [Verschieben der benutzerdefinierten Konfiguration vom aktiven Server auf den Stagingserver](#move-custom-configuration-from-active-to-staging-server)aus.
3. Wenn Sie ein Upgrade von einer früheren Version von Azure AD Connect durchführen, aktualisieren Sie den Stagingserver auf die neueste Version. Wenn Sie eine Verschiebung von Azure AD Sync durchführen, installieren Sie Azure AD Connect auf Ihrem Stagingserver.
4. Führen Sie mit dem Synchronisierungsmodul einen vollständigen Import und eine vollständige Synchronisierung auf dem Stagingserver aus.
5. Stellen Sie mithilfe der Schritte unter **Überprüfen** in [Überprüfen der Konfiguration eines Servers](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server) sicher, dass die neue Konfiguration nicht zu unerwarteten Änderungen führt. Wenn etwas nicht wie erwartet ist, korrigieren Sie es. Führen Sie dann den Import und die Synchronisierung erneut aus, und überprüfen Sie alles noch einmal, bis die Daten in Ordnung sind. Diese Schritte finden Sie im verknüpften Thema.
6. Machen Sie den Stagingserver zum aktiven Server. Dies ist der letzte Schritt in **Überprüfen der Konfiguration eines Servers** : [Wechseln des aktiven Servers](active-directory-aadconnectsync-operations.md#verify-the-configuration-of-a-server).
7. Wenn Sie ein Upgrade von Azure AD Connect durchführen, aktualisieren Sie den jetzt im Stagingmodus befindlichen Server auf die aktuelle Version. Führen Sie die gleichen Schritte wie zuvor aus, um die Daten und die Konfiguration zu aktualisieren. Wenn Sie ein Upgrade von Azure AD Sync durchgeführt haben, können Sie den alten Server jetzt ausschalten und außer Betrieb nehmen.

### <a name="move-custom-configuration-from-active-to-staging-server"></a>Verschieben der benutzerdefinierten Konfiguration vom aktiven Server auf den Stagingserver
Wenn Sie Änderungen an der Konfiguration des aktiven Servers vorgenommen haben, müssen Sie sicherstellen, dass die gleichen Änderungen auf dem Stagingserver angewendet werden.

Die benutzerdefinierten Synchronisierungsregeln, die Sie erstellt haben, können mit PowerShell verschoben werden. Andere Änderungen müssen in gleicher Weise auf beiden Systemen angewendet und können nicht migriert werden.

Folgendes muss auf beiden Servern auf die gleiche Weise konfiguriert sein:

* Verbindung mit den gleichen Gesamtstrukturen.
* Filter von Domänen und Organisationseinheiten.
* Die gleichen optionalen Funktionen wie Kennwortsynchronisierung und Kennwortrückschreiben.

**Verschieben von Synchronisierungsregeln**  
Gehen Sie folgendermaßen vor, um eine benutzerdefinierte Synchronisierungsregel zu verschieben:

1. Öffnen Sie den **Synchronisierungsregel-Editor** auf dem aktiven Server.
2. Wählen Sie die benutzerdefinierte Regel aus. Klicken Sie auf **Exportieren**. Ein Editor-Fenster wird geöffnet. Speichern Sie die temporäre Datei mit der Erweiterung „PS1“. Damit ist es ein PowerShell-Skript. Kopieren Sie die PS1-Datei auf den Stagingserver.  
   ![Exportieren von Synchronisierungsregeln](./media/active-directory-aadconnect-upgrade-previous-version/exportrule.png)
3. Die Connector-GUID auf dem Stagingserver weicht ab und muss geändert werden. Um die GUID abzurufen, starten Sie den **Synchronisierungsregel-Editor**. Wählen Sie eine der standardmäßigen Regeln aus, die das gleiche verbundene System darstellen, und klicken Sie auf **Exportieren**. Ersetzen Sie die GUID in der PS1-Datei durch die GUID des Stagingservers.
4. Führen Sie die PS1-Datei an einer PowerShell-Eingabeaufforderung aus. Dadurch wird die benutzerdefinierte Synchronisierungsregel auf dem Stagingserver erstellt.
5. Wenn Sie über mehrere benutzerdefinierte Regeln verfügen, wiederholen Sie die Schritte für alle benutzerdefinierten Regeln.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).




<!--HONumber=Jan17_HO4-->


