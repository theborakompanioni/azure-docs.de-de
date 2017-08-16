---
title: "Häufig gestellte Fragen zu Azure App Service-Web-App unter Linux | Microsoft-Dokumentation"
description: "Häufig gestellte Fragen zu Azure App Service-Web-App unter Linux."
keywords: "Azure App Service, Web-App, häufig gestellte Fragen, Linux, OSS"
services: app-service
documentationCenter: 
authors: ahmedelnably
manager: erikre
editor: 
ms.assetid: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: aelnably;wesmc
ms.translationtype: HT
ms.sourcegitcommit: 1dbb1d5aae55a4c926b9d8632b416a740a375684
ms.openlocfilehash: ff4f4ecd12bc26fcc44a20a193d73f952ed56f1a
ms.contentlocale: de-de
ms.lasthandoff: 08/07/2017

---

# <a name="azure-app-service-web-app-on-linux-faq"></a>Häufig gestellte Fragen zu Azure App Service-Web-App unter Linux

[!INCLUDE [app-service-linux-preview](../../includes/app-service-linux-preview.md)]


Mit der Veröffentlichung von Web-App unter Linux erweitern wir unsere Plattform um Funktionen und Verbesserungen. Im Folgenden finden Sie einige häufig gestellte Fragen (FAQ) von unseren Kunden aus den letzten Monaten.
Wenn Sie eine Frage haben, kommentieren Sie den Artikel, und wir werden so bald wie möglich antworten.

## <a name="built-in-images"></a>Integrierte Images

**F:** Ich möchte die integrierten Docker-Container nutzen, die von der Plattform bereitgestellt werden. Wo finde ich die Dateien?

**A:** Sie finden alle Docker-Dateien auf [GitHub](https://github.com/azure-app-service). Sie finden alle Docker-Container auf [Docker Hub](https://hub.docker.com/u/appsvc/).

**F:** Welche Werte sind beim Konfigurieren des Laufzeitstapels im Abschnitt „Startdatei“ anzugeben?

**A:** Für Node.js geben Sie die PM2-Konfigurationsdatei oder Ihre Skriptdatei an. Für .NET Core geben Sie den Namen der kompilierten DLL-Datei an. Für Ruby können Sie das Ruby-Skript angeben, mit dem Ihre App initialisiert werden soll.

## <a name="management"></a>Verwaltung

**F:** Was geschieht, wenn ich auf die Schaltfläche „Neu starten“ im Azure-Portal klicke?

**A:** Dies entspricht dem Docker-Neustart.

**F:** Kann ich über Secure Shell (SSH) eine Verbindung mit dem virtuellen Computer (VM) des App-Containers herstellen?

**A:** Ja, dies ist über die SCM-Website möglich. Weitere Informationen finden Sie im Artikel [SSH support for Azure Web App on Linux](./app-service-linux-ssh-support.md) (SSH-Unterstützung für Azure-Web-App unter Linux).

## <a name="continuous-integrationdeployment"></a>Continuous Integration/Bereitstellung

**F:** Meine Web-App verwendet nach dem Aktualisieren des Images auf DockerHub noch ein altes Docker-Containerimage. Unterstützen Sie Continuous Integration/Bereitstellung von benutzerdefinierten Containern?

**A:** Informationen zum Einrichten von Continuous Integration/Deployment für Azure Container Registry- oder DockerHub-Images finden Sie im Artikel [Continuous Deployment mit Azure-Web-App unter Linux](./app-service-linux-ci-cd.md). Bei privaten Registrierungen können Sie den Container aktualisieren, indem Sie Ihre Web-App beenden und dann wieder starten. Sie können auch eine Dummyeinstellung in der Anwendung ändern oder hinzufügen, um die Aktualisierung Ihres Containers zu erzwingen.

**Q:** Werden Stagingumgebungen unterstützt?

**A:** Ja.

**F:** Kann ich **Web Deploy** zur Bereitstellung meiner Web-App verwenden?

**A:** Ja, Sie müssen die App-Einstellung `WEBSITE_WEBDEPLOY_USE_SCM` auf `false` festlegen.

## <a name="language-support"></a>Sprachunterstützung

**F:** Unterstützen Sie nicht kompilierte .NET Core-Apps?

**A:** Ja.

**F:** Unterstützen Sie Composer als Abhängigkeits-Manager für PHP-Apps?

**A:** Ja. Während einer Git-Bereitstellung sollte Kudu erkennen, dass Sie eine PHP-Anwendung bereitstellen (durch das Vorhandensein einer „composer.json“-Datei), und eine Composer-Installation auslösen.

## <a name="custom-containers"></a>Benutzerdefinierte Container

**F:** Ich verwende meinen eigenen benutzerdefinierten Container. Meine App befindet sich im Verzeichnis `\home\`, aber ich kann meine Dateien nicht finden, wenn ich mit der [SCM](https://github.com/projectkudu/kudu)-Website oder mit einem FTP-Client den Inhalt durchsuche. Wo sind meine Dateien?

**A:** Wir stellen eine SMB-Freigabe im Verzeichnis `\home\` bereit. Dadurch werden alle vorhandenen Inhalte überschrieben.

**F:** Welches Format hat die Server-URL der privaten Registrierung?

**A:** Sie müssen die vollständige Registrierungs-URL samt `http://` oder `https://` eingeben.

**F:** Welches Format hat der Bildname in der Option zur privaten Registrierung?

**A:** Sie müssen den vollständigen Bildnamen samt URL der privaten Registrierung hinzufügen (z.B. myacr.azurecr.io/dotnet:latest)

**F:** Ich möchte mehr als einen Port in meinem benutzerdefinierten Containerimage verfügbar machen. Ist das möglich?

**A:** Dies wird derzeit nicht unterstützt.

**F:** Kann ich meinen eigenen Speicher verwenden?

**A:** Dies wird derzeit nicht unterstützt.

**F:** Ich kann über die SCM-Website das Dateisystem meines benutzerdefinierten Containers nicht durchsuchen und keine Prozesse ausführen. Warum?

**A:** Die SCM-Website wird in einem separaten Container ausgeführt. Sie können das Dateisystem oder aktive Prozesse des App-Containers nicht überprüfen.

**F:** Mein benutzerdefinierter Container lauscht auf einen anderen Port als Port 80. Wie kann ich meine App so konfigurieren, dass die Anforderungen an diesen Port weitergeleitet werden?

**A:** Ports werden automatisch erkannt. Zudem können Sie eine Anwendungseinstellung mit dem Namen **WEBSITES_PORT** mit dem Wert der erwarteten Portnummer angeben. Bei der Plattform wurde in der Vergangenheit die App-Einstellung `PORT` verwendet. Es ist geplant, die Verwendung dieser App-Einstellung als veraltet zu markieren und ausschließlich `WEBSITES_PORT` zu verwenden.

**F:** Muss ich HTTPS in meinem benutzerdefinierten Container implementieren?

**A:** Die Plattform handhabt die HTTPS-Beendigung an den freigegebenen Front-Ends.

## <a name="pricing-and-sla"></a>Preise und SLA

**F:** Welche Preise gelten für die öffentliche Vorschau?

**A:** Ihnen wird die Hälfte der Stunden, die Ihre App ausgeführt wird, zu den normalen Azure App Service-Preisen berechnet. Sie erhalten also effektiv einen Rabatt von 50 % auf den normalen Azure App Service-Preis.

## <a name="other"></a>Sonstige

**F:** Welche Zeichen werden in den Namen von Anwendungseinstellungen unterstützt?

**A:** Sie können nur A-Z, a-z, 0-9 und Unterstriche für Anwendungseinstellungen verwenden.

**F:** Wo kann ich neue Funktionen beantragen?

**A:** Sie können Ihre Idee im [Web-Apps-Feedbackforum](https://aka.ms/webapps-uservoice) einreichen. Fügen Sie im Titel Ihrer Idee „[Linux]“ hinzu.

## <a name="next-steps"></a>Nächste Schritte
* [Was ist Azure-Web-App unter Linux?](app-service-linux-intro.md)
* [Erstellen von Web-Apps in Azure-Web-App unter Linux](app-service-linux-how-to-create-web-app.md)
* [SSH-Unterstützung für Azure-Web-App unter Linux](./app-service-linux-ssh-support.md)
* [Einrichten von Stagingumgebungen in Azure App Service](./web-sites-staged-publishing.md)
* [Continuous Deployment mit Azure-Web-App unter Linux](./app-service-linux-ci-cd.md)

