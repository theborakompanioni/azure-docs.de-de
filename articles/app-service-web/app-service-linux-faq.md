---
title: "Häufig gestellte Fragen zu Azure App Service-Web-Apps unter Linux | Microsoft-Dokumentation"
description: "Häufig gestellte Fragen zu Azure App Service-Web-Apps unter Linux."
keywords: "Azure App Service, Web-App, häufig gestellte Fragen, Linux, OSS"
services: app-service
documentationCenter: 
authors: aelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/14/2017
ms.author: aelnably
translationtype: Human Translation
ms.sourcegitcommit: 831ef097027721146531e8d699fe3f67417a57ea
ms.openlocfilehash: b88aa3d0ae89aec81c2b9144fb5de3210a0b8d1e
ms.lasthandoff: 02/18/2017


---

# <a name="azure-app-service-web-apps-on-linux-faq"></a>Häufig gestellte Fragen zu Azure App Service-Web-Apps unter Linux #

Mit der Veröffentlichung von Azure App Service unter Linux (derzeit als Vorschau verfügbar) erweitern wir unsere Plattform um Funktionen und Verbesserungen. Im Folgenden finden Sie eine Reihe häufig gestellter Fragen von unseren Kunden aus den letzten Monaten.
Wenn Sie eine Frage haben, kommentieren Sie den Artikel, und wir werden so bald wie möglich antworten.

## <a name="built-in-images"></a>Integrierte Images ##

**F:** Ich möchte die integrierten Docker-Container nutzen, die von der Plattform bereitgestellt werden. Wo finde ich die Dateien?

**A:** Alle Docker-Dateien finden Sie hier: https://github.com/azure-app-service. Alle Docker-Container finden Sie hier: https://hub.docker.com/u/appsvc/.

## <a name="management"></a>Verwaltung ##

**F:** Ich klicke im Portal auf die Schaltfläche für den Neustart, aber meine Web-App wird nicht neu gestartet. Warum ist das so?

**A:** Wir arbeiten daran, die Schaltfläche zum Zurücksetzen in der nahen Zukunft zu aktivieren. Momentan stehen Ihnen zwei Optionen zur Verfügung.
1. Fügen Sie der Anwendung eine Dummyeinstellung hinzu, oder ändern Sie eine solche Einstellung. Dadurch wird der Neustart der Web-App erzwungen. 
2. Beenden und starten Sie Ihre Web-App.

**F:** Kann ich über SSH eine Verbindung mit dem virtuellen Computer herstellen?

**A:** Nein, aber wir werden in einer zukünftigen Version eine Möglichkeit bereitstellen, über SSH eine Verbindung mit Ihrem App-Container herzustellen.

## <a name="continuous-integration--deployment"></a>Continuous Integration/Bereitstellung ##

**F:** Meine Web-App verwendet nach dem Aktualisieren des Images auf DockerHub noch eines altes Docker-Containerimage? Unterstützen Sie Continuous Integration/Bereitstellung von benutzerdefinierten Containern?

**A:** Sie können den Container aktualisieren, indem Sie entweder Ihre Web-App beenden und dann wieder starten oder indem Sie eine Dummyeinstellung in der Anwendung ändern oder hinzufügen, um die Aktualisierung Ihres Containers zu erzwingen. In einer zukünftigen Version werden wir ein CI/CD-Feature für benutzerdefinierte Container anbieten.

## <a name="language-support"></a>Sprachunterstützung ##

**F:** Unterstützen Sie nicht kompilierte .NET Core-Apps?

**A:** Nein, Sie müssen die kompilierte .NET Core-App mit allen Abhängigkeiten bereitstellen. Vollständige Funktionen für Bereitstellungen und Builds werden in einer zukünftigen Version verfügbar sein.

## <a name="built-in-images"></a>Integrierte Images ##

**F:** Welche Werte sind beim Konfigurieren des Laufzeitstapels im Abschnitt „Startdatei“ anzugeben?

**A:** Für Node.js können Sie die PM2-Konfigurationsdatei oder Ihre Skriptdatei angeben. Für .NET Core sollten Sie den Namen der kompilierten DLL-Datei angeben. Für Ruby können Sie ein Ruby-Skript angeben, mit dem Ihre App initialisiert werden soll.

## <a name="custom-containers"></a>Benutzerdefinierte Container ##

**F:** Ich verwende meinen eigenen benutzerdefinierten Container. Meine App befindet sich im Verzeichnis „\home\“, aber ich kann meine Dateien nicht finden, wenn ich mit der SCM-Website oder mit einem FTP-Client den Inhalt durchsuche. Wo sind meine Dateien?

**A:** Wir stellen eine SMB-Freigabe im Verzeichnis „\home\“ bereit und überschreiben daher alle vorhandenen Inhalte.

**F:** Ich möchte mehr als einen Port in meinem benutzerdefinierten Containerimage verfügbar machen. Ist das möglich?

**A:** Dies wird derzeit nicht unterstützt.

**F:** Kann ich meinen eigenen Speicher verwenden?

**A:** Dies wird derzeit nicht unterstützt.

**F:** Ich kann über die SCM-Website das Dateisystem meines benutzerdefinierten Containers nicht durchsuchen und keine Prozesse ausführen. Warum?

**A:** Die SCM-Website wird in einem separaten Container ausgeführt. Sie können das Dateisystem oder aktive Prozesse des App-Containers nicht überprüfen.

**F:** Meine benutzerdefinierten Container lauschen auf einen anderen Port als Port 80. Wie kann ich meine App so konfigurieren, dass die Anforderungen an diesen Port weitergeleitet werden?

**A:** Sie können eine Anwendungseinstellung mit dem Namen **PORT** und für diese den Wert der erwarteten Portnummer angeben.

## <a name="pricing-and-sla"></a>Preise und SLA ##

**F:** Welche Preise gelten für die öffentliche Vorschau?

**A:** Ihnen wird die Hälfte der Stunden, die Ihre App ausgeführt wird, zu den normalen Azure App Service-Preisen berechnet. Sie erhalten also effektiv einen Rabatt von 50 % auf den normalen Azure App Service-Preis.

## <a name="other"></a>Sonstige ##

**F:** Welche Zeichen werden in den Namen von Anwendungseinstellungen unterstützt?

**A:** Sie können nur A-Z, a-z, 0-9 und Unterstriche für Anwendungseinstellungen verwenden.

**F:** Wo kann ich neue Funktionen beantragen?

**A:** Sie können Ihre Idee hier einreichen: https://aka.ms/webapps-uservoice. Bitte fügen Sie im Titel Ihrer Idee [Linux] hinzu.

## <a name="next-steps"></a>Nächste Schritte
* [Was ist App Service unter Linux?](app-service-linux-intro.md)
* [Erstellen von Web-Apps in App Service unter Linux](app-service-linux-how-to-create-a-web-app.md)

