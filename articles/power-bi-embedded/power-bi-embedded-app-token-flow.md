<properties
   pageTitle="Ablauf der Authentifizierung mithilfe eines App-Tokens in Power BI Embedded"
   description="Power BI Embedded über App-Token zur Authentifizierung und Autorisierung"
   services="power-bi-embedded"
   documentationCenter=""
   authors="minewiskan"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="06/28/2016"
   ms.author="owend"/>

# Ablauf der Authentifizierung mithilfe eines App-Tokens in Power BI Embedded

Der Dienst **Power BI Embedded** nutzt **App-Token** zur Authentifizierung und Autorisierung anstelle der expliziten Endbenutzerauthentifizierung. Im **App-Token**-Modell verwaltet Ihre Anwendung die Authentifizierung und Autorisierung für Ihre Endbenutzer. Bei Bedarf erstellt und sendet Ihre App **App Token**, die unseren Dienst anweisen, den angeforderten Bericht zu rendern. In dieser Version muss Ihre App **Azure Active Directory** nicht für die Authentifizierung und Autorisierung der Benutzer verwenden, obwohl Sie dies tun können.

**So funktioniert der Ablauf des Tokenschlüssels der App**

1. Kopieren der API-Schlüssel zu Ihrer Anwendung Sie erhalten die Schlüssel im **Azure-Portal**.

    ![](media\powerbi-embedded-get-started-sample\azure-portal.png)

2. Das Token setzt einen Anspruch durch und hat eine Ablaufzeit.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-2.png)

3. Das Token wird mit einer API-Tastenkombination angemeldet.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-3.png)

4. Der Benutzer fordert die Ansicht des Berichts an.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-4.png)

5.	Das Token wird mit einer API-Tastenkombination validiert.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-5.png)

6.	Power BI Embedded sendet einen Bericht an den Benutzer.

    ![](media\powerbi-embedded-get-started-sample\power-bi-embedded-token-6.png)

Nachdem **Power BI Embedded** einen Bericht an den Benutzer geschickt hat, kann der Benutzer den Bericht in Ihrer benutzerdefinierten App sehen. Wenn Sie z.B. das [Beispiel „Analyzing Sales Data PBIX“](http://download.microsoft.com/download/1/4/E/14EDED28-6C58-4055-A65C-23B4DA81C4DE/Analyzing_Sales_Data.pbix) importiert haben, sieht die Beispiel-Web-App wie folgt aus:

![](media\powerbi-embedded-get-started-sample\sample-web-app.png)

## Siehe auch
- [Erste Schritte mit dem Beispiel zu Microsoft Power BI Embedded](power-bi-embedded-get-started-sample.md)
- [Was ist Microsoft Power BI Embedded](power-bi-embedded-what-is-power-bi-embedded.md)
- [Häufige Microsoft Power BI Embedded Preview-Szenarios](power-bi-embedded-scenarios.md)
- [Get started with Microsoft Power BI Embedded Preview](power-bi-embedded-get-started.md) (Erste Schritte mit Microsoft Power BI Embedded Preview)

<!---HONumber=AcomDC_0629_2016-->