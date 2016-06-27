<properties 
	pageTitle="Problembehandlung für Analytics, das leistungsfähige Suchtool von Application Insights | Microsoft Azure" 
	description="Probleme mit Analytics in Application Insights? Beginnen Sie hier" 
	services="application-insights" 
    documentationCenter=""
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="06/07/2016" 
	ms.author="awills"/>


# Problembehandlung für Analytics in Application Insights


Probleme mit [Analytics in Application Insights](app-insights-analytics.md)? Beginnen Sie hier Analytics ist das leistungsfähige Suchtool von Visual Studio Application Insights.



## Grenzen

* Derzeit sind Abfrageergebnisse auf eine Woche alte Daten beschränkt.
* Getestete Browser: die neuesten Versionen von Chrome, Edge und Internet Explorer.

## „Unerwarteter Fehler“

![Bildschirm „Unerwarteter Fehler“](./media/app-insights-analytics-troubleshooting/010.png)

Während der Portallaufzeit ist ein interner Fehler aufgetreten – Ausnahmefehler.

* Löschen Sie den Browsercache. 

## 403 ... Versuchen Sie, neu zu laden

![403 \... Versuchen Sie, neu zu laden](./media/app-insights-analytics-troubleshooting/020.png)

Es ist ein authentifizierungsbezogener Fehler aufgetreten (während der Authentifizierung oder während der Generierung des Zugriffstokens). Das Portal kann möglicherweise nicht wiederhergestellt werden, ohne die Browsereinstellungen zu ändern.

* Überprüfen Sie, ob Drittanbietercookies im Browser aktiviert sind. 

    Informationen zum Deaktivieren von Drittanbietercookies finden Sie [hier](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers). Beachten Sie jedoch, dass die Cookies in diesem Fall **aktiviert** werden müssen.

## 403 ... Überprüfen Sie die Sicherheitszone

![403 \... Überprüfen Sie die Sicherheitszone](./media/app-insights-analytics-troubleshooting/030.png)

Es ist ein authentifizierungsbezogener Fehler aufgetreten (während der Authentifizierung oder während der Generierung des Zugriffstokens). Das Portal kann möglicherweise nicht wiederhergestellt werden, ohne die Browsereinstellungen zu ändern.

1. Überprüfen Sie, ob Drittanbietercookies im Browser aktiviert sind. 

    Informationen zum Deaktivieren von Drittanbietercookies finden Sie [hier](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers). Beachten Sie jedoch, dass die Cookies in diesem Fall **aktiviert** werden müssen.

2. Haben Sie versucht, das Analytics-Portal über einen Favoriten, ein Lesezeichen oder einen gespeicherten Link zu öffnen? Sind Sie mit anderen Anmeldeinformationen angemeldet als denen, die Sie beim Speichern des Links verwendet haben?

 2. Versuchen Sie ein InPrivate- bzw. Inkognito-Browserfenster zu verwenden (nachdem Sie alle solchen möglicherweise vorhandenen Fenster geschlossen haben). Sie müssen Ihre Anmeldeinformationen bereitstellen.

 2. Öffnen Sie ein anderes (normales) Browserfenster, und wechseln Sie zu [Azure](https://portal.azure.com). Melden Sie sich ab. Öffnen Sie Ihren Link, und melden Sie sich mit den richtigen Anmeldeinformationen an.

2. Dieser Fehler kann auch bei Edge und Internet Explorer auftreten, wenn die Einstellungen für vertrauenswürdige Zonen nicht unterstützt werden.

	Überprüfen Sie, ob sich das [Analytics-Portal](https://analytics.applicationinsights.io) und das [Azure Active Directory-Portal](https://portal.azure.com) in der gleichen Sicherheitszone befinden:

 * Öffnen Sie in Internet Explorer **Internetoptionen**, **Sicherheit**, **Vertrauenswürdige Sites**, **Sites**:

    ![Dialogfeld „Internetoptionen“, Hinzufügen einer Website zu den vertrauenswürdigen Websites](./media/app-insights-analytics-troubleshooting/033.png)

    Wenn sich eine der folgenden URLs in der Liste der Websites befindet, stellen Sie sicher, dass auch die anderen URLs in dieser Liste aufgeführt sind:

    * https://login.microsoftonline.com
    * https://login.windows.net
    * https://analytics.applicationinsights.io 


## 404 ... Ressource nicht gefunden

![404 \... Ressource nicht gefunden](./media/app-insights-analytics-troubleshooting/040.png)

Die Anwendungsressource wurde aus Application Insights gelöscht und ist nicht mehr verfügbar. Dies kann passieren, wenn Sie die URL auf der Analytics-Seite gespeichert haben.


## 403 ... Keine Autorisierung

![403 \... nicht autorisiert](./media/app-insights-analytics-troubleshooting/050.png)

Sie verfügen nicht über die Berechtigung, diese Anwendung in Analytics zu öffnen.

* Haben Sie den Link von einer anderen Person erhalten? Bitten Sie diese Person, sicherzustellen, dass Sie zu [den Lesern oder Mitwirkenden für diese Ressourcengruppe](app-insights-resources-roles-access-control.md) gehören.
* Haben Sie den Link mit anderen Anmeldeinformationen gespeichert? Öffnen Sie das [Azure-Portal](https://portal.azure.com), und melden Sie sich ab. Versuchen Sie dann erneut, den Link zu öffnen, und verwenden Sie dabei die richtigen Anmeldeinformationen.

## 403 ... HTML5 Storage

Unser Portal verwendet die HTML 5-Mechanismen localStorage und sessionStorage.

* Chrome: Einstellungen, Datenschutz, Inhaltseinstellungen.
* Internet Explorer: Internetoptionen, Erweitert (Registerkarte), Sicherheit, DOM-Storage aktivieren


![403 \... Versuchen Sie, HTML5 Storage zu aktivieren](./media/app-insights-analytics-troubleshooting/060.png)

## 404 ... Abonnement nicht gefunden


![404 \... Abonnement nicht gefunden](./media/app-insights-analytics-troubleshooting/070.png)

Die URL ist ungültig.

* Öffnen Sie die App-Ressource im [Application Insights-Portal](https://portal.azure.com). Verwenden Sie dann die Schaltfläche „Analytics“.

## 404 ... Seite ist nicht vorhanden

![404 \... Seite ist nicht vorhanden](./media/app-insights-analytics-troubleshooting/080.png)

Die URL ist ungültig.

* Öffnen Sie die App-Ressource im [Application Insights-Portal](https://portal.azure.com). Verwenden Sie dann die Schaltfläche „Analytics“.

## Wenn nichts funktioniert...    

Öffnen Sie ein [Supportticket](app-insights-get-dev-support.md).
 
[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

<!---HONumber=AcomDC_0615_2016-->