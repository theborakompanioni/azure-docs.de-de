---
title: "Warnungsüberprüfung in Azure Security Center | Microsoft-Dokumentation"
description: "In diesem Dokument erfahren Sie, wie Sie die Sicherheitswarnungen in Azure Security Center überprüfen."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: f8f17a55-e672-4d86-8ba9-6c3ce2e71a57
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2017
ms.author: yurid
ms.translationtype: HT
ms.sourcegitcommit: a9cfd6052b58fe7a800f1b58113aec47a74095e3
ms.openlocfilehash: 121b5d8f023a9b663d0e7af26dce8f81db27672c
ms.contentlocale: de-de
ms.lasthandoff: 08/12/2017

---
# <a name="alerts-validation-in-azure-security-center"></a>Warnungsüberprüfung in Azure Security Center
In diesem Dokument erfahren Sie, wie Sie überprüfen, ob Ihr System ordnungsgemäß für Azure Security Center-Warnungen konfiguriert ist.

## <a name="what-are-security-alerts"></a>Was sind Sicherheitswarnungen?
Security Center erfasst, analysiert und integriert automatisch Protokolldaten Ihrer Azure-Ressourcen, aus dem Netzwerk und von verbundenen Partnerlösungen (wie etwa Firewalls und Lösungen zum Schutz von Endpunkten), um Bedrohungen zu erkennen und Sie darauf aufmerksam zu machen. Weitere Informationen zu Sicherheitswarnungen finden Sie unter [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Weitere Informationen zu den unterschiedlichen Arten von Warnungen finden Sie unter [Verstehen der Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type).

## <a name="alert-validation"></a>Warnungsüberprüfung
Nach der Installation des Security Center-Agents auf Ihrem Computer führen Sie auf dem Computer, auf dem sich die angegriffene Ressource für die Warnung befinden soll, die folgenden Schritte aus:

1. Kopieren Sie eine ausführbare Datei (beispielsweise „calc.exe“) auf den Desktop des Computers oder in ein beliebiges anderes Verzeichnis.
2. Benennen Sie diese Datei in **ASC_AlertTest_662jfi039N.exe** um.
3. Öffnen Sie die Eingabeaufforderung, und führen Sie die Datei mit einem (selbst ausgedachten) Argument aus. Beispiel: *ASC_AlertTest_662jfi039N.exe -foo*
4. Warten Sie fünf bis zehn Minuten, und öffnen Sie die Security Center-Warnungen. Dort sollte eine Warnung wie die folgende vorliegen:

    ![Warnungsüberprüfung](./media/security-center-alert-validation/security-center-alert-validation-fig1.png)

Vergewissern Sie sich bei der Prüfung dieser Warnung, dass „Arguments Auditing Enabled“ (Argumentüberprüfung aktiviert) auf „true“ festgelegt ist. Sollte die Option auf „false“ festgelegt sein, müssen Sie die Überprüfung von Befehlszeilenargumenten aktivieren. Die Option können Sie mithilfe der folgenden Befehlszeile aktivieren:

*reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\policies\system\Audit" /f /v "ProcessCreationIncludeCmdLine_Enabled"*


## <a name="see-also"></a>Weitere Informationen
In diesem Artikel wurden Sie in den Prozess der Warnungsüberprüfung eingeführt. Nachdem Sie sich mit dieser Überprüfung vertraut gemacht haben, können Sie mit den folgenden Artikeln fortfahren:

* [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-managing-and-responding-alerts). Hier erfahren Sie, wie Sie Warnungen verwalten und auf Sicherheitsvorfälle in Security Center reagieren.
* [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md). Hier erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
* [Verstehen der Sicherheitswarnungen in Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-alerts-type). Hier finden Sie Informationen zu den unterschiedlichen Arten von Sicherheitswarnungen.
* [Azure Security Center – Handbuch zur Problembehandlung](https://docs.microsoft.com/azure/security-center/security-center-troubleshooting-guide). Hier erfahren Sie, wie Sie allgemeine Probleme in Security Center behandeln. 
* [Azure Security Center – häufig gestellte Fragen](security-center-faq.md)festgelegt ist. Enthält häufig gestellte Fragen zur Verwendung des Diensts.
* [Azure Security-Blog](http://blogs.msdn.com/b/azuresecurity/). Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.


