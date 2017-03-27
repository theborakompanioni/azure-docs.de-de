---
title: Neuerungen in Power BI Embedded
description: Abrufen aktueller Informationen zu Neuerungen in Power BI Embedded
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 2794ae98-b9a7-45df-b6e1-962a395b91fa
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 03/11/2017
ms.author: asaxton
translationtype: Human Translation
ms.sourcegitcommit: c1cd1450d5921cf51f720017b746ff9498e85537
ms.openlocfilehash: 07c53a5d6b1881a4c207a2aefed9fcede0fa069e
ms.lasthandoff: 03/14/2017

---
# <a name="whats-new-in-power-bi-embedded"></a>Neuerungen in Power BI Embedded

Updates für **Power BI Embedded** werden in regelmäßigen Abständen veröffentlicht. Nicht jede Version umfasst jedoch neue Funktionen für Benutzer, da sich einige Versionen auf Back-End-Dienstfunktionen konzentrieren. Wir stellen hier neue Funktionen für Benutzer vor. Überprüfen Sie diese Seite daher regelmäßig.

## <a name="march-2017"></a>17. März 2017

<iframe width="640" height="360" src="https://www.youtube.com/embed/ibuN4DzCl5c?showinfo=0" frameborder="0" allowfullscreen></iframe>

**Self-service-Funktionen**

* [Create a new report from a dataset in Power BI Embedded](power-bi-embedded-create-report-from-dataset.md) (Erstellen eines neuen Berichts aus einem Dataset in Power BI Embedded)
* [Save reports in Power BI Embedded](power-bi-embedded-save-reports.md) (Speichern von Berichten in Power BI Embedded)
* Einbetten eines Berichts im Lese-/Bearbeitungs-/Neuerstellungsmodus 
* [Toggle between view and edit mode for reports in Power BI Embedded](power-bi-embedded-toggle-mode.md) (Wechseln zwischen Ansichts- und Bearbeitungsmodus für Berichte in Power BI Embedded)

**Datenkonnektivität mit REST-APIs**

* [Post Datasets](https://msdn.microsoft.com/library/azure/mt778875.aspx) (Erstellen von Datasets)
* Übertragen von Daten 

**Verwaltungs-APIs**

* Klonen von Bericht und Dataset
* Binden des Berichts an ein anderes Dataset

**Beispiele**

* Aktualisiertes [Beispiel für die Einbettung von JavaScript-Berichten](https://microsoft.github.io/PowerBI-JavaScript/demo)

## <a name="december-2016"></a>Dezember 2016

* [Neues Einbettungsbeispiel in JavaScript](https://microsoft.github.io/PowerBI-JavaScript/demo/)

## <a name="october-2016"></a>Oktober 2016

* [Erweiterte Analysen mit Power BI Embedded und R](https://powerbi.microsoft.com/blog/r-in-pbie/)

## <a name="august-31st-2016"></a>31. August 2016
In dieser Version:

* Ein ganz neues JavaScript-SDK, das [erweiterte Filter und Seitennavigation](power-bi-embedded-interact-with-reports.md)unterstützt.
* Power BI Embedded wird jetzt im zentralen Rechenzentrum in Kanada unterstützt. Überprüfen Sie den [Status von Rechenzentren](https://azure.microsoft.com/status/).

## <a name="july-11th-2016"></a>11. Juli 2016
In dieser Version:

* **Gute Nachrichten!** Von Power BI Embedded gibt es nicht mehr nur eine Vorschau-, sondern auch eine GA-Version.  
* Alle REST-APIs wurden nun von **/beta** zu **/v1.0** migriert.
* .NET- und JavaScript-SDKs wurden für **v1.0**aktualisiert.
* Power BI-API-Aufrufe können nun direkt mithilfe von API-Schlüsseln authentifiziert werden. App-Tokens werden nur für die Einbettung benötigt. Infolgedessen sind Bereitstellungs- und Entwicklertoken in v1.0-APIs veraltet. Sie können jedoch in der Betaversion bis 30.12.2016 verwendet werden. Weitere Informationen finden Sie unter [Authentifizieren und Autorisieren mit Power BI Embedded](power-bi-embedded-app-token-flow.md).
* Unterstützung der Sicherheit auf Zeilenebene für App-Token und eingebettete Berichte. Weitere Informationen finden Sie unter [Sicherheit auf Zeilenebene in Power BI Embedded](power-bi-embedded-rls.md).
* Aktualisierte Beispielanwendung für alle Aufrufe von **v1.0** -APIs.
* Power BI Embedded-Unterstützung für Azure SDK, PowerShell und CLI.
* Benutzer können Visualisierungsdaten in eine **CSV-Datei**exportieren.
* Power BI Embedded wird nun in den gleichen Sprachen/Gebietsschemas unterstützt wie Microsoft Azure. Weitere Informationen finden Sie unter [Azure – Languages](http://social.technet.microsoft.com/wiki/contents/articles/4234.windows-azure-extent-of-localization.aspx) (Azure – Sprachen).


