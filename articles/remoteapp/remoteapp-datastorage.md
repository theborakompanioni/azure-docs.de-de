---
title: "Speichern Sie vertrauliche Daten niemals in benutzerdefinierten Images für Azure RemoteApp. | Microsoft Docs"
description: "Erfahren Sie mehr über die Richtlinien zum Speichern von Daten in benutzerdefinierten Images in Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 5a19903b-15f9-49d9-9bc1-ae80f2671aa1
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 75d5415d33324d957617426e75909a6c6c58b1f9
ms.lasthandoff: 03/31/2017


---
# <a name="never-store-sensitive-data-on-custom-images"></a>Speichern Sie vertrauliche Daten niemals in benutzerdefinierten Images.
> [!IMPORTANT]
> Azure RemoteApp wird am 31. August 2017 eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

Wenn Sie Ihre eigene Anwendung in Azure RemoteApp hosten, besteht der erste Schritt darin, ein benutzerdefiniertes Image zu erstellen. Wir verwenden dieses benutzerdefinierte Image, um VM-Instanzen zu erstellen, die Ihre Apps für Ihre Benutzer bereitstellen. Das benutzerdefinierte Image sollte NUR Anwendungen enthalten, niemals vertrauliche Daten, die nicht verloren gehen dürfen – beispielsweise SQL-Datenbanken, Personaldateien oder spezielle Datendateien wie QuickBooks-Unternehmensdateien. Alle vertraulichen Daten sollten außerhalb von Azure RemoteApp auf einem Dateiserver, einer anderen Azure-VM oder in SQL Azure gespeichert werden. Das Image sollte nur die Anwendung hosten, die eine Verbindung mit der Datenquelle herstellt und die Daten bereitstellt. Weitere Informationen finden Sie unter [Anforderungen für Azure RemoteApp-Images](remoteapp-imagereqs.md) . 

Um zu verstehen, warum vertrauliche Daten nicht in Azure RemoteApp gespeichert werden sollten, müssen Sie wissen, wie RemoteApp funktioniert. Wenn eine Sammlung erstellt oder aktualisiert wird, werden im Hintergrund mehrere Klone oder Kopien des Image erstellt. All diese VM-Instanzen sind exakte Replikate des benutzerdefinierten Image. Wenn Benutzer Anwendungen starten, werden sie mit einer dieser Instanzen verbunden. Es ist nicht garantiert, dass immer die gleiche Instanz verwendet wird, und dies sollte auch keine Rolle spielen, da die Instanzen nicht persistent sind. Die VM-Instanzen, auf denen die Anwendungen gehostet werden, sind nicht persistent und können gelöscht werden, z.B. während der Aktualisierung einer Sammlung. 

Sobald eine Sammlung bereitgestellt wurde und Benutzer damit beginnen, Verbindungen mit den VMs herzustellen, sind die Benutzerdaten persistent und geschützt, da sie in einem separaten Speicher innerhalb einer virtuellen Festplatte (Virtual Hard Disk, VHD) gespeichert werden. Diese VHD wird als [Benutzerprofil-Datenträger (User Profile Disk, UPD)](remoteapp-upd.md) bezeichnet. Bei dem separaten Speicher handelt es sich um das Benutzerprofil unter „C:\users\<Benutzerprofil>“. Wenn eine Anwendung gestartet wird, wird der UPD vom Betriebssystem wie ein lokales Benutzerprofil bereitgestellt und behandelt. Erfahren Sie mehr darüber, [wie Azure RemoteApp Benutzerdaten und -einstellungen speichert](remoteapp-upd.md).

Beispiele für Daten, die nicht im Image gespeichert werden sollten:

* Freigegebene Daten für den Benutzerzugriff
* SQL-Datenbank oder QuickBooks-Datenbank
* Jegliche Daten in D:\

Beispiele für Daten, die sich im Standardprofil befinden können, das in den UPD jedes Benutzers kopiert wird:

* Konfigurationsdateien für die Benutzer
* Skripts, die Benutzer in ihrem UPD benötigen

Die wichtigsten Punkte:

* Speichern Sie in einem benutzerdefinierten Image keine vertraulichen Daten, die nicht verloren gehen dürfen.
* Vertrauliche Daten sollten immer auf einem separaten Dateiserver, einer separaten Azure-VM oder in der Cloud gespeichert werden – auf jeden Fall außerhalb der VM-Instanzen, die Ihre Anwendungen in Azure RemoteApp hosten. 
* Benutzerdaten werden auf dem Benutzerprofil-Datenträger gespeichert und beibehalten.


