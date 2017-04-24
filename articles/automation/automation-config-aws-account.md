---
title: Konfigurieren der Authentifizierung mit Amazon Web Services | Microsoft Docs
description: "In diesem Artikel wird beschrieben, wie Sie AWS-Anmeldeinformationen für Runbooks in Azure Automation zum Verwalten von AWS-Ressourcen erstellen und überprüfen."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: tysonn
keywords: AWS-Authentifizierung, AWS konfigurieren
ms.assetid: b6dde4bb-26ac-4876-9aa9-e586bed30d6b
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/14/2017
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 81e5e5d56a7e6149409e11aca2e5fdf28d6a7134
ms.lasthandoff: 04/15/2017


---
# <a name="authenticate-runbooks-with-amazon-web-services"></a>Authentifizieren von Runbooks mit Amazon Web Services
Die Automatisierung von häufigen Aufgaben mit Ressourcen in Amazon Web Services (AWS) kann mit Automation-Runbooks in Azure erreicht werden.  Sie können viele Aufgaben in AWS mit Automation-Runbooks automatisieren, wie dies auch mit Ressourcen in Azure möglich ist.  Sie benötigen lediglich zwei Dinge:

* Ein AWS-Abonnement und einen Satz Anmeldeinformationen.  Genauer gesagt: Ihren AWS-Zugriffsschlüssel und den geheimen Schlüssel.  Weitere Informationen finden Sie im Artikel [Using AWS Credentials](http://docs.aws.amazon.com/powershell/latest/userguide/specifying-your-aws-credentials.html)(Verwenden von AWS-Anmeldeinformationen).
* Ein Azure-Abonnement und ein Automation-Konto.  Weitere Informationen zum Einrichten eines Azure Automation-Kontos finden Sie im Artikel [Konfigurieren eines ausführenden Kontos in Azure](automation-sec-configure-azure-runas-account.md).  

Zum Authentifizieren mit AWS müssen Sie einen Satz mit AWS-Anmeldeinformationen angeben, um Ihre über Azure Automation ausgeführten Anmeldeinformationen zu authentifizieren. Falls Sie bereits ein Automation-Konto erstellt haben und für die Authentifizierung mit AWS verwenden möchten, können Sie die Schritte im folgenden Abschnitt ausführen.  Wenn Sie ein dediziertes Konto für Runbooks zur Verwendung mit AWS-Ressourcen bestimmen möchten, müssen Sie zuerst ein neues [Automation-Konto](automation-offering-get-started.md) erstellen (überspringen Sie die Option zum Erstellen eines Dienstprinzipals) und dann die unten angegebenen Schritte ausführen.

## <a name="configure-automation-account"></a>Konfigurieren des Automation-Kontos
Damit Azure Automation mit AWS kommunizieren kann, müssen Sie zuerst Ihre AWS-Anmeldeinformationen abrufen und als Ressourcen in Azure Automation speichern.  Führen Sie die folgenden Schritte aus, die im AWS-Dokument [Managing Access Keys for your AWS Account](http://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html) (Verwalten von Zugriffsschlüsseln für Ihr AWS-Konto) enthalten sind, um einen Zugriffsschlüssel zu erstellen und die **Zugriffsschlüssel-ID** und den **geheimen Zugriffsschlüssel** zu kopieren (optional können Sie auch die Schlüsseldatei herunterladen und an einem sicheren Ort speichern).

Nachdem Sie Ihre AWS-Sicherheitsschlüssel erstellt und kopiert haben, müssen Sie ein Anmeldeinformationsobjekt mit einem Azure Automation-Konto erstellen, um sie sicher zu speichern und in den Runbooks darauf verweisen zu können.  Führen Sie die Schritte im Abschnitt zum **Erstellen eines neuen Anmeldeinformationsobjekts** im Artikel [Anmeldeinformationsobjekte in Azure Automation](automation-credentials.md#to-create-a-new-credential-asset-with-the-azure-portal) aus, und geben Sie die folgenden Informationen ein:

1. Geben Sie im Feld **Name** den Wert **AWScred** oder einen anderen geeigneten Wert gemäß Ihren Benennungsvorgaben ein.  
2. Geben Sie im Feld **Benutzername** Ihre **Zugriffs-ID** und im Feld **Kennwort** bzw. **Kennwort bestätigen** den **geheimen Zugriffsschlüssel** ein.   

## <a name="next-steps"></a>Nächste Schritte
* Sehen Sie sich den Artikel [Automatisieren der Bereitstellung einer VM in Amazon Web Services](automation-scenario-aws-deployment.md) an, um zu erfahren, wie Sie Runbooks zum Automatisieren von Aufgaben in AWS erstellen.


