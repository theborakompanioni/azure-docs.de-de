---
title: Abrufen von Daten per Azure AD Reporting-API mit Zertifikaten | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie die Azure AD Reporting-API mit Zertifikatanmeldeinformationen verwenden, um Daten ohne Benutzereingriff aus Verzeichnissen abzurufen.
services: active-directory
documentationcenter: 
author: ramical
writer: v-lorisc
manager: kannar
ms.assetid: 
ms.service: active-directory
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/08/2017
ms.author: ramical
ms.translationtype: Human Translation
ms.sourcegitcommit: 07635b0eb4650f0c30898ea1600697dacb33477c
ms.openlocfilehash: c1345dcda6e52267a8037ffd7207e6bc3b0d3b31
ms.contentlocale: de-de
ms.lasthandoff: 03/28/2017

---
# <a name="get-data-using-the-azure-ad-reporting-api-with-certificates"></a>Abrufen von Daten per Azure AD Reporting-API mit Zertifikaten
In diesem Artikel wird beschrieben, wie Sie die Azure AD Reporting-API mit Zertifikatanmeldeinformationen verwenden, um Daten ohne Benutzereingriff aus Verzeichnissen abzurufen. 

## <a name="use-the-azure-ad-reporting-api"></a>Verwenden der Azure AD Reporting-API 
Für die Azure AD Reporting-API müssen die folgenden Schritte ausgeführt werden:
 *  Installieren der erforderlichen Komponenten
 *  Festlegen des Zertifikats in Ihrer App
 *  Abrufen eines Zugriffstokens
 *  Verwenden des Zugriffstokens zum Aufrufen der Graph-API

Informationen zum Quellcode finden Sie unter [Leverage Report API Module](https://github.com/AzureAD/azure-activedirectory-powershell/tree/gh-pages/Modules/AzureADUtils) (Nutzen des Reporting-API-Moduls). 

### <a name="install-prerequisites"></a>Installieren der erforderlichen Komponenten
Azure AD PowerShell V2 und das AzureADUtils-Modul müssen installiert sein.

1. Laden Sie Azure AD Powershell V2 herunter, und führen Sie die Installation durch, indem Sie die Anleitung unter [Azure Active Directory PowerShell](https://github.com/Azure/azure-docs-powershell-azuread/blob/master/Azure AD Cmdlets/AzureAD/index.md) befolgen.
2. Laden Sie das Azure AD Utils-Modul unter [AzureAD/azure-activedirectory-powershell](https://github.com/AzureAD/azure-activedirectory-powershell/blob/gh-pages/Modules/AzureADUtils/AzureADUtils.psm1) herunter. 
  Dieses Modul enthält mehrere Hilfsprogramm-Cmdlets, z.B.:
   * Aktuelle Version von ADAL mit Verwendung von Nuget
   * Zugriffstoken für Benutzer, Anwendungsschlüssel und Zertifikate mit Verwendung von ADAL
   * Graph-API zur Verarbeitung von auf Seiten aufgeteilten Ergebnissen

**Gehen Sie wie folgt vor, um das Azure AD Utils-Modul zu installieren:**

1. Erstellen Sie ein Verzeichnis, um das Hilfsprogrammmodul zu speichern (z.B. „c:\azureAD“), und laden Sie das Modul von GitHub herunter.
2. Öffnen Sie eine PowerShell-Sitzung, und navigieren Sie zum gerade erstellten Verzeichnis. 
3. Importieren Sie das Modul, und installieren Sie es unter dem PowerShell-Modulpfad, indem Sie das Install-AzureADUtilsModule-Cmdlet verwenden. 

Die Sitzung sollte etwa wie folgt aussehen:

  ![Windows PowerShell](./media/active-directory-report-api-with-certificates/windows-powershell.png)

### <a name="set-the-certificate-in-your-app"></a>Festlegen des Zertifikats in Ihrer App
1. Wenn Sie bereits über eine App verfügen, können Sie die zugehörige Objekt-ID über das Azure-Portal abrufen. 

  ![Azure-Portal](./media/active-directory-report-api-with-certificates/azure-portal.png)

2. Öffnen Sie eine PowerShell-Sitzung, und stellen Sie mit dem Connect-AzureAD-Cmdlet eine Verbindung mit Azure AD her.

  ![Azure-Portal](./media/active-directory-report-api-with-certificates/connect-azuaread-cmdlet.png)

3. Verwenden Sie das New-AzureADApplicationCertificateCredential-Cmdlet von AzureADUtils, um Zertifikatanmeldeinformationen hinzuzufügen. 

>[!Note]
>Sie müssen die Objekt-ID der Anwendung, die Sie zuvor erfasst haben, und das Zertifikatobjekt angeben (über das Cert:-Laufwerk).
>


  ![Azure-Portal](./media/active-directory-report-api-with-certificates/add-certificate-credential.png)
  
### <a name="get-an-access-token"></a>Abrufen eines Zugriffstokens

Verwenden Sie zum Abrufen eines Zugriffstokens das Get-AzureADGraphAPIAccessTokenFromCert-Cmdlet von AzureADUtils. 

>[!NOTE]
>Sie müssen anstelle der Objekt-ID, die Sie im letzten Abschnitt verwendet haben, die Anwendungs-ID verwenden.
>

 ![Azure-Portal](./media/active-directory-report-api-with-certificates/application-id.png)

### <a name="use-the-access-token-to-call-the-graph-api"></a>Verwenden des Zugriffstokens zum Aufrufen der Graph-API

Nun können Sie das Skript erstellen. Unten ist ein Beispiel für die Verwendung des Invoke-AzureADGraphAPIQuery-Cmdlets von AzureADUtils angegeben. Mit diesem Cmdlet werden mehrseitige Ergebnisse verarbeitet, die dann an die PowerShell-Pipeline gesendet werden. 

 ![Azure-Portal](./media/active-directory-report-api-with-certificates/script-completed.png)

Nun können Sie den Export in eine CSV-Datei durchführen und die Daten in einem SIEM-System speichern. Außerdem können Sie Ihr Skript mit einer geplanten Aufgabe umschließen, mit der Azure AD-Daten regelmäßig aus Ihrem Mandanten abgerufen werden, ohne dass Anwendungsschlüssel im Quellcode gespeichert werden müssen. 

## <a name="next-steps"></a>Nächste Schritte
[Grundlagen der Identitätsverwaltung in Azure](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals-identity)<br>




