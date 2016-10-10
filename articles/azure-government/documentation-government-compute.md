<properties
	pageTitle="Dokumentation zu Azure Government | Microsoft Azure"
	description="Dies bietet einen Vergleich der Features und Richtlinien zum Entwickeln von Anwendungen für Azure Government"
	services="Azure-Government"
	cloud="gov" 
	documentationCenter=""
	authors="ryansoc"
	manager="zakramer"
	editor=""/>

<tags
	ms.service="multiple"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="azure-government"
	ms.date="09/14/2016"
	ms.author="ryansoc"/>


#  Azure Government Compute

##  Virtual Machines

###  Überlegungen zu Daten

Die folgenden Informationen geben Aufschluss über die Abgrenzung von Azure Government für Azure Virtual Machines:

| Regulierte/kontrollierte Daten zulässig | Regulierte/kontrollierte Daten nicht zulässig |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Daten, die in einem virtuellen Computer eingegeben, gespeichert und verarbeitet werden, können Exportbestimmungen unterliegende Daten enthalten. Binärdateien, die in Azure Virtual Machines ausgeführt werden. Statische Authentifikatoren wie Kennwörter und Smartcard-PINs für den Zugriff auf Komponenten der Azure-Plattform. Private Schlüssel von Zertifikaten, die für die Verwaltung von Komponenten der Azure-Plattform verwendet werden. SQL-Verbindungszeichenfolgen. Andere Sicherheitsinformationen/geheime Schlüssel, wie etwa Zertifikate, Verschlüsselungsschlüssel, Hauptschlüssel und Speicherschlüssel, die in Azure-Diensten gespeichert sind. | Metadaten dürfen keine den Exportbestimmungen unterliegenden Daten enthalten. Zu diesen Metadaten gehören alle Konfigurationsdaten, die beim Erstellen und Verwalten der Azure Virtual Machine-Instanz eingegeben werden. Geben Sie keine regulierten/kontrollierten Daten in die folgenden Felder ein: Tenant role names (Namen von Mandantenrollen), Ressourcengruppen, Deployment names (Bereitstellungsnamen), Ressourcennamen, Resourcentags.  

Weitere Informationen finden Sie in der <a href=https://azure.microsoft.com/de-de/documentation/services/virtual-machines/windows/>öffentlichen Dokumentation zu Azure Virtual Machines</a>.

Weitere Informationen und Updates erhalten Sie, indem Sie den <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-Blog</a> abonnieren.

<!---HONumber=AcomDC_0928_2016-->