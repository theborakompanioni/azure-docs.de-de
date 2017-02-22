---
title: "Azure AD Connect Sync: Operative Aufgaben und Überlegungen | Microsoft Docs"
description: "In diesem Thema werden die operativen Aufgaben für Azure AD Connect Sync und die Vorbereitung dieser Komponente für den Betrieb beschrieben."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: b29c1790-37a3-470f-ab69-3cee824d220d
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/08/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: f9523ce62fb24b280c8d5869bf394ab279c48f23
ms.openlocfilehash: 10b8221c873e3510189e7d82e034669f0466cd84


---
# <a name="azure-ad-connect-sync-operational-tasks-and-consideration"></a>Azure AD Connect Sync: Operative Aufgaben und Überlegungen
In diesem Thema werden die operativen Aufgaben für Azure AD Connect Sync beschrieben.

## <a name="staging-mode"></a>Stagingmodus
Der Stagingmodus kann für verschiedene Szenarien genutzt werden, darunter:

* Hohe Verfügbarkeit.
* Testen und Bereitstellen neuer Konfigurationsänderungen.
* Einführen eines neuen Servers und Außerbetriebnahme des alten Servers.

Mit einem Server im Stagingmodus können Sie Änderungen an der Konfiguration vornehmen und eine Vorschau der Änderungen anzeigen, bevor Sie den Server aktiv schalten. Ein Server im Stagingmodus ermöglicht es Ihnen außerdem, einen vollständigen Import und eine vollständige Synchronisierung durchzuführen und so sicherzustellen, dass alle Änderungen wie erwartet durchgeführt werden, bevor Sie die Änderungen in Ihrer Produktionsumgebung implementieren.

Bei der Installation können Sie auswählen, dass der Server in den **Stagingmodus**versetzt werden soll. Dadurch wird der Server für Import und Synchronisierung aktiviert, es werden jedoch keine Exportvorgänge ausgeführt. Ein Server im Stagingmodus führt keine Kennwortsynchronisierung und kein Kennwortrückschreiben durch – selbst dann nicht, wenn Sie diese Features bei der Installation ausgewählt haben. Wenn Sie den Stagingmodus deaktivieren, beginnt der Server mit dem Exportieren und aktiviert die Kennwortsynchronisierung und das Kennwortrückschreiben.

Sie können einen Export weiterhin mithilfe des Synchronisierungsdienst-Managers erzwingen.

Ein Server im Stagingmodus empfängt weiterhin Änderungen von Active Directory und Azure AD. Er verfügt stets über eine Kopie der aktuellen Änderungen und kann sehr schnell die Funktion eines anderen Servers übernehmen. Wenn Sie Konfigurationsänderungen an Ihrem primären Server vornehmen, müssen Sie die gleichen Änderungen auch für den Server im Stagingmodus durchführen.

Wenn Sie sich bisher nur mit älteren Synchronisierungstechnologien auskennen, müssen Sie wissen, dass Änderungen für den Stagingmodus gelten, seit dieser über eine eigene SQL-Datenbank verfügt. Dank dieser Architektur kann sich der Server im Stagingmodus in einem anderen Datencenter befinden.

### <a name="verify-the-configuration-of-a-server"></a>Überprüfen der Konfiguration eines Servers
Führen Sie zum Anwenden dieser Methode die folgenden Schritte aus:

1. [Vorbereiten](#prepare)
2. [Importieren und Synchronisieren](#import-and-synchronize)
3. [Überprüfen](#verify)
4. [Wechseln des aktiven Servers](#switch-active-server)

#### <a name="prepare"></a>Vorbereiten
1. Installieren Sie Azure AD Connect, wählen Sie **Stagingmodus** aus, und deaktivieren Sie auf der letzten Seite des Installations-Assistenten die Option **Synchronisierung starten**. In diesem Modus kann das Synchronisierungsmodul manuell ausgeführt werden.
   ![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/readytoconfigure.png)
2. Melden Sie sich ab und wieder an, und wählen Sie im Startmenü die Option **Synchronisierungsdienst**aus.

#### <a name="import-and-synchronize"></a>Importieren und Synchronisieren
1. Wählen Sie **Connectors** aus, und wählen Sie dann den ersten Connector mit dem Typ **Active Directory Domain Services** aus. Klicken Sie auf **Ausführen**, wählen Sie **Vollständiger Import** aus, und klicken Sie auf **OK**. Führen Sie diese Schritte für alle Connectors dieses Typs aus.
2. Wählen Sie den Connector mit dem Typ **Azure Active Directory (Microsoft)**aus. Klicken Sie auf **Ausführen**, wählen Sie **Vollständiger Import** aus, und klicken Sie auf **OK**.
3. Stellen Sie sicher, dass die Registerkarte „Connectors“ noch ausgewählt ist. Klicken Sie für jeden Connector mit dem Typ **Active Directory Domain Services** auf **Ausführen**, wählen Sie **Deltasynchronisierung** aus, und klicken Sie auf **OK**.
4. Wählen Sie den Connector mit dem Typ **Azure Active Directory (Microsoft)**aus. Klicken Sie auf **Ausführen**, wählen Sie **Deltasynchronisierung** aus, und klicken Sie auf **OK**.

Sie haben einen Stagingexport der Änderungen an Azure AD und der lokalen Active Directory-Umgebung durchgeführt (wenn Sie eine Exchange-Hybridbereitstellung verwenden). In den nächsten Schritten können Sie überprüfen, was geändert wird, bevor Sie mit dem Export in die Verzeichnisse beginnen.

#### <a name="verify"></a>Überprüfen
1. Starten Sie eine Eingabeaufforderung, und wechseln Sie zu: `%ProgramFiles%\Microsoft Azure AD Sync\bin`
2. Führen Sie diesen Befehl aus: `csexport "Name of Connector" %temp%\export.xml /f:x`  
   Den Namen des Connectors finden Sie im Synchronisierungsdienst. Für Azure AD sieht der Name in etwa wie folgt aus: contoso.com – AAD.
3. Führen Sie diesen Befehl aus: `CSExportAnalyzer %temp%\export.xml > %temp%\export.csv`
4. Im Ordner „%temp%“ befindet sich nun eine Datei namens „export.csv“, die Sie sich in Microsoft Excel ansehen können. Diese Datei enthält alle Änderungen, die exportiert werden sollen.
5. Nehmen Sie erforderliche Änderungen an den Daten oder der Konfiguration vor, und führen Sie die oben genannten Schritte (Importieren, Synchronisieren, Überprüfen) erneut aus, bis Sie die Änderungen erhalten, die Sie exportieren möchten.

**Grundlegendes zur Datei "export.csv"**

Die meisten Bestandteile der Datei sind selbsterklärend. Nachfolgend werden zum Verständnis des Inhalts einige Abkürzungen erläutert:

* OMODT: Object Modification Type. Gibt an, ob es sich bei dem Vorgang auf Objektebene um einen Hinzufüge-, Aktualisierungs- oder Löschvorgang handelt.
* AMODT: Attribute Modification Type. Gibt an, ob es sich bei dem Vorgang auf Attributebene um einen Hinzufüge-, Aktualisierungs- oder Löschvorgang handelt.

Bei mehrwertigen Attributen wird nicht jede Änderung angezeigt. Nur die Anzahl von hinzugefügten und entfernten Werten wird angezeigt.

#### <a name="switch-active-server"></a>Wechseln des aktiven Servers
1. Schalten Sie den derzeit aktiven Server entweder aus (DirSync/FIM/Azure AD Sync), damit kein Export nach Azure AD stattfindet, oder versetzen Sie ihn in den Stagingmodus (Azure AD Connect).
2. Führen Sie auf dem Server im **Stagingmodus** den Installations-Assistenten aus, und deaktivieren Sie den **Stagingmodus**.
   ![ReadyToConfigure](./media/active-directory-aadconnectsync-operations/additionaltasks.png)

## <a name="disaster-recovery"></a>Notfallwiederherstellung
Teil des Implementierungsentwurfs ist die Planung für einen notfallbedingten Ausfall des Synchronisierungsservers. Es stehen mehrere Modelle zur Verfügung, und die Wahl des Modells ist von verschiedenen Faktoren abhängig:

* Wie hoch ist die Toleranz, wenn während der Ausfallzeit keine Objektänderungen in Azure AD durchgeführt werden können?
* Bei Verwendung der Kennwortsynchronisierung: Ist es für die Benutzer akzeptabel, dass sie in Azure AD ihr altes Kennwort verwenden müssen, wenn sie ihr lokales Kennwort ändern?
* Besteht eine Abhängigkeit von Echtzeitvorgängen, beispielsweise dem Kennwortrückschreiben?

Abhängig von Ihren Antworten auf diese Fragen und Ihrer Organisationsrichtlinie kann eine der folgenden Strategien implementiert werden:

* Führen Sie ggf. eine Neuerstellung durch.
* Richten Sie einen Reserveserver im Standbymodus ein, auch bezeichnet als **Stagingmodus**.
* Verwenden Sie virtuelle Computer.

Wenn Sie nicht die integrierte SQL Express-Datenbank verwenden, lesen Sie sich auch den Abschnitt [Hohe Verfügbarkeit von SQL](#sql-high-availability) durch.

### <a name="rebuild-when-needed"></a>Neuerstellung, falls erforderlich
Eine sinnvolle Strategie besteht darin, eine ggf. erforderliche Neuerstellung des Servers zu planen. In der Regel sind die Installation des Synchronisierungsmoduls und der anfängliche Import sowie die Synchronisierung innerhalb weniger Stunden abgeschlossen. Wenn kein Ersatzserver verfügbar ist, kann vorübergehend ein Domänencontroller zum Hosten des Synchronisierungsmoduls eingesetzt werden.

Das Synchronisierungsmodul speichert keine Statusinformationen zu den Objekten, deshalb kann die Datenbank mithilfe der Daten in Active Directory und Azure AD neu erstellt werden. Das Attribut **sourceAnchor** wird verwendet, um die lokalen Objekte und die Cloudobjekte miteinander zu verknüpfen. Wenn Sie den Server mit vorhandenen lokalen Objekten und Cloudobjekten neu erstellen, werden diese Objekte bei der Neuinstallation vom Synchronisierungsmodul erneut abgeglichen. Dokumentieren und speichern Sie die am Server vorgenommenen Änderungen, beispielsweise die Filter- und Synchronisierungsregeln. Diese benutzerdefinierten Konfigurationen müssen erneut angewendet werden, bevor Sie die Synchronisierung starten.

### <a name="have-a-spare-standby-server---staging-mode"></a>Stellen Sie einen Reservestandbyserver bereit (Stagingmodus)
Wenn Sie über eine komplexere Umgebung verfügen, wird empfohlen, mindestens einen Standbyserver einzurichten. Bei der Installation können Sie festlegen, dass der Server in den **Stagingmodus**versetzt werden soll.

Weitere Informationen finden Sie unter [Stagingmodus](#staging-mode).

### <a name="use-virtual-machines"></a>Einsatz virtueller Computer
Eine gängige und unterstützte Methode ist die Ausführung des Synchronisierungsmoduls auf einem virtuellen Computer. Wenn ein Problem mit dem Host vorliegt, kann das Image mit dem Synchronisierungsmodulserver zu einem anderen Server migriert werden.

### <a name="sql-high-availability"></a>Hohe Verfügbarkeit von SQL
Falls Sie nicht SQL Server Express (in Azure AD Connect enthalten) verwenden, sollten Sie auch eine hohe Verfügbarkeit für SQL Server in Erwägung ziehen. Die einzige unterstützte Hochverfügbarkeitslösung ist das SQL-Clustering. Nicht unterstützt werden Spiegelung und Always On-Lösungen.

## <a name="next-steps"></a>Nächste Schritte
**Übersichtsthemen**  

* [Azure AD Connect-Synchronisierung: Grundlagen und Anpassung der Synchronisierung](active-directory-aadconnectsync-whatis.md)  
* [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md)  




<!--HONumber=Feb17_HO1-->


