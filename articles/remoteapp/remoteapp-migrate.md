
<properties
    pageTitle="Migrieren von Benutzerdaten von Azure RemoteApp | Microsoft Azure"
    description="Erfahren Sie, wie Sie Ihre Benutzerdaten nach und aus Azure RemoteApp migrieren."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />



# Migrieren von Daten in und aus Azure RemoteApp

> [AZURE.IMPORTANT]
Azure RemoteApp wird eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148).

Sie können viele verschiedene Tools und Methoden verwenden, um [Benutzerdaten](remoteapp-upd.md) nach und aus Azure RemoteApp zu übertragen. Hier sind einige Methoden:

- Kopieren und Einfügen über eine gemeinsam genutzte Zwischenablage
- Kopieren von Dateien und Daten auf einen Dateiserver
- Kopieren von Dateien nach OneDrive for Business über einen Browser
- Kopieren von Dateien mithilfe einer Umleitung

>[AZURE.NOTE] 
Sie können die Synchronisierungs-Agents für OneDrive for Business oder Consumer nicht aktivieren – sie werden in Azure RemoteApp [nicht unterstützt](remoteapp-onedrive.md).

## Verwenden des Kopier- und Einfügevorgangs im Datei-Explorer

Kopieren und Einfügen über die Zwischenablage ist in RemoteApp-Bereitstellungen [standardmäßig](remoteapp-redirection.md) aktiviert. So können Benutzer Dateien von ihrem lokalen PC in ihre RemoteApp-Apps und umgekehrt kopieren. Häufig speichern Benutzer bei der ganz normalen Verwendung ihrer Apps in RemoteApp Dateien auf ihren Benutzerprofil-Datenträgern (User Profile Disks, UPDs). Diese Daten lassen sich ganz einfach aus RemoteApp kopieren:

1. [Veröffentlichen Sie den Datei-Explorer als App](remoteapp-publish.md) in einer RemoteApp-Sammlung. (Beachten Sie, dass es sich hier um eine administrative Aufgabe handelt.)
2. Weisen Sie Ihre Benutzer an, die von Ihnen veröffentlichte Datei-Explorer-App zu starten und diese zum Kopieren und Einfügen von Dateien aus ihren und in ihre UPDs zu verwenden.

## Hochladen von Dateien und Daten auf einen Dateiserver mithilfe des standardmäßigen Kopiervorgangs in einem Netzwerk

Organisationen verwenden häufig Dateiserver, um allgemeine Daten zu speichern. Wenn Name oder Speicherort des Servers bekannt sind, können Ihre Benutzer das lokale Netzwerk nach dem Server durchsuchen und ihre Dateien dorthin kopieren. Auch in diesem Fall empfiehlt es sich, den Datei-Explorer in RemoteApp zu veröffentlichen und für Ihre Benutzer freizugeben.

>[AZURE.NOTE] 
Der Dateiserver muss sich in dem routingfähigen Netzwerk befinden, in dem RemoteApp bereitgestellt wurde.

## Kopieren von Dateien nach OneDrive for Business
Sie können zwar den Synchronisierungs-Agent für OneDrive for Business nicht in RemoteApp aktivieren, aber Sie können Dateien von Ihrem UPD über einen Browser nach OneDrive for Business kopieren.

1. Veröffentlichen Sie den Datei-Explorer in RemoteApp, und teilen Sie Ihren Benutzern mit, dass sie über diese App auf die Dateien zugreifen können.
2. Dateien lassen sich am einfachsten in komprimiertem Format übertragen, Ihre Benutzer sollten also eine ZIP-Datei erstellen, die alle Dateien enthält, die nach OneDrive for Business kopiert werden sollen.
3. Teilen Sie Ihren Benutzern mit, dass sie das Office 365-Portal besuchen, dann zu OneDrive wechseln und die ZIP-Datei dort hochladen sollen.

## Kopieren von Dateien mithilfe einer Laufwerkumleitung

Wenn Sie die [Laufwerkumleitung](remoteapp-redirection.md) aktiviert haben, haben Sie bereits ein zugeordnetes Laufwerk für Ihre Benutzer erstellt. In diesem Fall können die Benutzer ihre Dateien im umgeleiteten Laufwerk zippen und dann auf ihrem lokalen PC speichern.

<!---HONumber=AcomDC_0817_2016-->