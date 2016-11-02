<properties
    pageTitle="Verwenden von Outlook in Azure RemoteApp | Microsoft Azure" 
    description="Informationen zum Konfigurieren und Verwenden von Outlook in Azure RemoteApp | Microsoft Azure"
    services="remoteapp"
    documentationCenter=""
    authors="pavithir"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="using-microsoft-outlook-in-azure-remoteapp"></a>Verwenden Microsoft Outlook in Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp wird eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp unterstützt Microsoft Outlook O365. Erfahren Sie mehr über das Verwenden von [Office in Azure RemoteApp](remoteapp-officesubscription.md). Für die Verwendung von Outlook in Azure RemoteApp werden einige  Einstellungen empfohlen.

## <a name="cached-mode"></a>Cache-Modus
Der Cache-Modus ist eine empfohlene Konfiguration für die Verwendung von Outlook in Azure RemoteApp. Wenn Sie für ein Outlook 2013-Konto den Exchange-Cache-Modus konfigurieren, arbeitet Outlook 2013 mit einer lokalen Kopie des Microsoft Exchange-Postfachs des Benutzers, die mit dem Offline Offlineadressbuch (OAB) in einer Offline-Datendatei (OST-Datei) auf dem Computer des Benutzers gespeichert ist. Das zwischengespeicherte Postfach und OAB werden in regelmäßigen Abständen vom O365-Dienst aktualisiert. Erfahren Sie mehr über [die Unterschiede zwischen Cache- und Onlinemodus](https://technet.microsoft.com/library/jj683103.aspx).

Der Benutzer kann bei der Kontoeinrichtung zwischen **Exchange-Cachemodus** oder **Onlinemodus** wählen oder die Kontoeinstellungen ändern. Sie können auch mit dem Office-Anpassungstool (OAT) oder der Gruppenrichtlinie einen der Modi bereitstellen.  

Informationen dazu finden Sie unter [Schrittweise Anweisungen zum Aktivieren des Cache-Modus](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc).

## <a name="search"></a>Suche
In Azure RemoteApp ist die Suche in Outlook eingeschränkt. Azure RemoteApp verwendet VM-Pools, um Benutzersitzungen zu ermöglichen. Die Suchindizierung hängt von der VM-ID ab, die sich je nach virtueller Maschine unterscheidet. So ist es möglich, dass ein Benutzer bei jeder Anmeldung bei der Azure RemoteApp an eine neue virtuelle Maschine weitergeleitet wird. Bei Aktivierung der lokalen Suche würde bei jeder Änderung der VM-ID (wenn der Benutzer an eine andere virtuelle Maschine geleitet wird) der Indexer ausgeführt werden. Je nach Größe der OST-Datei könnte die Ausführung des Indexers sehr lange dauern und die erforderlichen Ressourcen für andere Apps belegen. Die Suche wäre nicht nur langsam, sondern ergäbe möglicherweise keine Ergebnisse. Mit einem Kontoprofil im Onlinemodus können Sie dieses Problem umgehen, die Gesamtleistung wäre jedoch aufgrund des fehlenden lokalen Cache beeinträchtigt (unter dem oben genannten Link finden Sie weitere Informationen zum Unterschied zwischen dem Cache- und dem Onlinemodel). Leider kann die indizierte/lokale Suche nicht deaktiviert werden, und die Online-Suche kann in Outlook 2013 nicht standardmäßig aktiviert werden.



<!--HONumber=Oct16_HO2-->


