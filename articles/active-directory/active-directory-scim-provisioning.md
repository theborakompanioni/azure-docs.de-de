---
title: "Verwenden von SCIM (System for Cross-domain Identity Management) für die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory für Anwendungen | Microsoft-Dokumentation"
description: "Über Azure Active Directory können Benutzer und Gruppen automatisch für alle Anwendungen oder Identitätsspeicher bereitgestellt werden, denen ein Webdienst mit einer Schnittstelle vorgelagert ist, wie sie in der SCIM-Protokollspezifikation definiert ist."
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: femila
editor: 
ms.assetid: 4d86f3dc-e2d3-4bde-81a3-4a0e092551c0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.custom: aaddev;it-pro;oldportal
ms.translationtype: HT
ms.sourcegitcommit: 137671152878e6e1ee5ba398dd5267feefc435b7
ms.openlocfilehash: 91978cee88d55c99bcb63c63cdaf01581ae84668
ms.contentlocale: de-de
ms.lasthandoff: 07/28/2017

---
# <a name="using-system-for-cross-domain-identity-management-to-automatically-provision-users-and-groups-from-azure-active-directory-to-applications"></a>Verwenden von SCIM (System for Cross-domain Identity Management) für die automatische Bereitstellung von Benutzern und Gruppen aus Azure Active Directory für Anwendungen

## <a name="overview"></a>Übersicht
Über Azure Active Directory (Azure AD) können Benutzer und Gruppen automatisch für alle Anwendungen oder Identitätsspeicher bereitgestellt werden, denen ein Webdienst mit der in der [SCIM 2.0-Protokollspezifikation](https://tools.ietf.org/html/draft-ietf-scim-api-19) definierten Schnittstelle vorgelagert ist. Azure Active Directory kann Anforderungen zum Erstellen, Ändern oder Löschen zugewiesener Benutzer und Gruppen an den Webdienst senden. Der Webdienst kann dann diese Anforderungen in Vorgänge für den Zielidentitätsspeicher übersetzen. 

> [!IMPORTANT]
> Microsoft empfiehlt, für die Verwaltung von Azure AD anstelle des in diesem Artikel erwähnten klassischen Azure-Portals das [Azure AD Admin Center](https://aad.portal.azure.com) zu verwenden. 



![][0]
*Abbildung 1: Bereitstellung aus Azure Active Directory in einem Identitätsspeicher per Webdienst*

Diese Funktion kann zusammen mit der Funktion „Bring your own app“ in Azure AD verwendet werden, um das einmalige Anmelden und die automatische Benutzerbereitstellung für Anwendungen zu ermöglichen, die einen SCIM-Webdienst bereitstellen oder denen ein solcher Dienst vorgelagert ist.

In Azure Active Directory gibt es zwei Anwendungsfälle für die Verwendung von SCIM:

* **Bereitstellen von Benutzern und Gruppen für Anwendungen, die SCIM unterstützen:** Anwendungen, die SCIM 2.0 unterstützen und OAuth-Bearertoken zum Authentifizieren verwenden, können ohne Konfiguration mit Azure AD verwendet werden.
* **Erstellen einer eigenen Bereitstellungslösung für Anwendungen, die eine andere API-basierte Bereitstellung unterstützen:** Für Anwendungen ohne SCIM können Sie einen SCIM-Endpunkt erstellen, der zwischen dem SCIM-Endpunkt von Azure AD und der API übersetzt, die die betreffende Anwendung zum Bereitstellen von Benutzern unterstützt. Um die Entwicklung eines SCIM-Endpunkts zu erleichtern, stellen wir Ihnen CLI-Bibliotheken (Common Language Infrastructure) sowie Codebeispiele zur Verfügung, die das Bereitstellen eines SCIM-Endpunkts und das Übersetzen von SCIM-Nachrichten veranschaulichen.  

## <a name="provisioning-users-and-groups-to-applications-that-support-scim"></a>Bereitstellen von Benutzern und Gruppen für Anwendungen, die SCIM unterstützen
Azure AD kann so konfiguriert werden, dass zugewiesene Benutzer und Gruppen automatisch für Anwendungen bereitgestellt werden, die einen [SCIM 2-Webdienst](https://tools.ietf.org/html/draft-ietf-scim-api-19) implementieren und OAuth-Bearertoken für die Authentifizierung akzeptieren. Im Rahmen der SCIM 2.0-Spezifikation müssen Anwendungen die folgenden Anforderungen erfüllen:

* Unterstützt das Erstellen von Benutzern und/oder Gruppen gemäß Abschnitt 3.3 des SCIM-Protokolls  
* Unterstützt das Ändern von Benutzern und/oder Gruppen mit Patch-Anforderungen gemäß Abschnitt 3.5.2 des SCIM-Protokolls  
* Unterstützt das Abrufen einer bekannten Ressource gemäß Abschnitt 3.4.1 des SCIM-Protokolls  
* Unterstützt das Abfragen von Benutzern und/oder Gruppen gemäß Abschnitt 3.4.2 des SCIM-Protokolls  In der Standardeinstellung werden Benutzer nach „externalId“ und Gruppen nach „displayName“ abgefragt.  
* Unterstützt das Abfragen von Benutzern nach ID und nach Manager gemäß Abschnitt 3.4.2 des SCIM-Protokolls  
* Unterstützt das Abfragen von Gruppen nach ID und nach Mitglied gemäß Abschnitt 3.4.2 des SCIM-Protokolls  
* Akzeptiert OAuth-Bearertoken für die Autorisierung gemäß Abschnitt 2.1 des SCIM-Protokolls

Überprüfen Sie beim Anbieter Ihrer Anwendung oder in der Dokumentation zu Ihrer Anwendung, ob diese Anforderungen voll erfüllt werden.

### <a name="getting-started"></a>Erste Schritte
Anwendungen, die das SCIM-Profil wie in diesem Artikel beschrieben erfüllen, können über das Feature „Nicht-Kataloganwendung“ im Azure AD-Anwendungskatalog mit Azure Active Directory verbunden werden. Nach dem Herstellen der Verbindung führt Azure AD alle 20 Minuten einen Synchronisierungsprozess aus, bei dem der SCIM-Endpunkt der Anwendung auf zugewiesene Benutzer und Gruppen abgefragt wird. Entsprechend den Details der Zuweisung werden Benutzer und Gruppen dann erstellt oder geändert.

**So verbinden Sie eine Anwendung, die SCIM unterstützt:**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
2. Navigieren Sie zu „Azure Active Directory > Unternehmensanwendungen“, und wählen Sie **Neue Anwendung > Alle > Nicht-Kataloganwendung**.
3. Geben Sie einen Namen für Ihre Anwendung ein, und klicken Sie auf das Symbol **Hinzufügen**, um ein App-Objekt zu erstellen.
    
  ![][1]
  *Abbildung 2: Azure AD-Anwendungskatalog*
    
4. Klicken Sie auf dem angezeigten Bildschirm in der linken Spalte auf die Registerkarte **Bereitstellung**.
5. Wählen Sie im Menü **Bereitstellungsmodus** die Option **Automatisch** aus.
    
  ![][2]
  *Abbildung 3: Konfigurieren der Bereitstellung im Azure-Portal*
    
6. Geben Sie im Feld **Mandanten-URL** die URL des SCIM-Endpunkts der Anwendung ein. Beispiel: https://api.contoso.com/scim/v2/
7. Wenn der SCIM-Endpunkt ein OAuth-Bearertoken benötigt, das von einem anderen Aussteller als Azure AD stammt, kopieren Sie das erforderliche OAuth-Bearertoken in das optionale Feld **Geheimes Token**. Wird dieses Feld leer gelassen, dann fügt Azure AD in jede Anforderung ein von Azure AD ausgestelltes OAuth-Bearertoken ein. Apps, die Azure AD als Identitätsanbieter verwenden, können dieses von Azure AD ausgestellte Token überprüfen.
8. Klicken Sie auf **Verbindung testen**, damit Azure Active Directory versucht, eine Verbindung mit dem SCIM-Endpunkt herzustellen. Wenn die Versuche fehlschlagen, werden Fehlerinformationen angezeigt.  
9. Wenn eine Verbindung mit der Anwendung hergestellt wird, klicken Sie auf **Speichern**, um die Administratoranmeldeinformationen zu speichern.
10. Im Abschnitt **Zuordnungen** stehen zwei Sätze von Attributzuordnungen zur Verfügung: eine für Benutzerobjekte und eine für Gruppenobjekte. Wählen Sie beide nacheinander aus, um die Attribute zu überprüfen, die von Azure Active Directory mit Ihrer App synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzer und Gruppen in Ihrer App für Updatevorgänge verwendet werden. Wählen Sie die Schaltfläche „Speichern“, um alle Änderungen zu übernehmen.

    >[!NOTE]
    >Sie können die Synchronisierung von Gruppenobjekten optional deaktivieren. Deaktivieren Sie dazu die Zuordnung „Gruppen“. 

11. Im Feld **Bereich** unter **Einstellungen** wird festgelegt, welche Benutzer und Gruppen synchronisiert werden. Wenn „Sync only assigned users and groups“ (Nur zugewiesene Benutzer und Gruppen synchronisieren) (empfohlen) ausgewählt wird, werden nur Benutzer und Gruppen synchronisiert, die auf der Registerkarte **Benutzer und Gruppen** zugewiesen sind.
12. Ändern Sie nach Abschluss der Konfiguration den **Bereitstellungsstatus** in **Ein**.
13. Klicken Sie auf **Speichern**, um den Azure AD-Bereitstellungsdienst zu starten. 
14. Wenn nur zugewiesene Benutzer und Gruppen synchronisiert werden (empfohlen), klicken Sie auf die Registerkarte **Benutzer und Gruppen**, und weisen Sie die Benutzer und/oder Gruppen zu, die synchronisiert werden sollen.

Nachdem die Erstsynchronisierung gestartet wurde, können Sie auf der Registerkarte **Überwachungsprotokolle** den Status überwachen. Hier werden alle vom Bereitstellungsdienst für Ihre App ausgeführten Aktionen angezeigt. Weitere Informationen zum Lesen von Azure AD-Bereitstellungsprotokollen finden Sie unter [Tutorial: Meldung zur automatischen Benutzerkontobereitstellung](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).

>[!NOTE]
>Die Erstsynchronisierung dauert länger als nachfolgende Synchronisierungen, die ungefähr alle 20 Minuten erfolgen, solange der Dienst ausgeführt wird. 


## <a name="building-your-own-provisioning-solution-for-any-application"></a>Erstellen einer eigenen Bereitstellungslösung für beliebige Anwendungen
Wenn Sie einen SCIM-Webdienst mit Schnittstelle zu Azure Active Directory erstellen, können Sie das einmalige Anmelden und die automatische Benutzerbereitstellung für nahezu alle Anwendungen ermöglichen, die über eine REST- oder SOAP-API zur Benutzerbereitstellung verfügen.

So funktioniert es:

1. Azure AD stellt eine Common Language Infrastructure-Bibliothek mit dem Namen [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/)bereit. Systemintegratoren und Entwickler können diese Bibliothek verwenden, um einen SCIM-basierten Webdienstendpunkt zu erstellen und bereitzustellen, über den Azure AD mit dem Identitätsspeicher einer beliebigen Anwendung verbunden werden kann.
2. Zuordnungen werden in den Webdienst implementiert, um das standardisierte Benutzerschema dem erforderlichen Benutzerschema und Protokoll für die Anwendung zuzuordnen.
3. Die Endpunkt-URL wird in Azure AD als Teil einer benutzerdefinierten Anwendung im Anwendungskatalog registriert.
4. Benutzer und Gruppen werden dieser Anwendung in Azure AD zugewiesen. Bei der Zuweisung werden sie in eine Warteschlange eingereiht, um mit der Zielanwendung synchronisiert zu werden. Der Synchronisierungsvorgang für die Warteschlange wird alle 20 Minuten ausgeführt.

### <a name="code-samples"></a>Codebeispiele
Zum Vereinfachen dieses Prozesses wird ein Satz mit [Codebeispielen](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) bereitgestellt, in denen ein SCIM-Webdienstendpunkt erstellt und die automatische Bereitstellung demonstriert wird. In einem Beispiel wird von einem Anbieter eine Datei mit Zeilen kommagetrennter Werte verwendet, die für Benutzer und Gruppen stehen.  Im anderen Beispiel wird ein Anbieter verwendet, der auf dem Amazon Web Services-Dienst für Identitäts- und Zugriffsverwaltung basiert.  

**Voraussetzungen**

* Visual Studio 2013 oder höher
* [Azure SDK für .NET](https://azure.microsoft.com/downloads/)
* Windows-Computer, der die Verwendung von ASP.NET Framework 4.5 als SCIM-Endpunkt unterstützt. Auf diesen Computer muss aus der Cloud zugegriffen werden können.
* [Azure-Abonnement mit Testversion oder lizenzierter Version von Azure AD Premium](https://azure.microsoft.com/services/active-directory/)
* Für das Amazon AWS-Beispiel sind Bibliotheken aus dem [AWS Toolkit für Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html)erforderlich. Weitere Informationen finden Sie in der INFODATEI des Beispiels.

### <a name="getting-started"></a>Erste Schritte
Die einfachste Möglichkeit zum Implementieren eines SCIM-Endpunkts, der Bereitstellungsanforderungen von Azure AD akzeptiert, ist das Erstellen und Bereitstellen des Codebeispiels, bei dem die bereitgestellten Benutzer in eine Datei mit kommagetrennten Werten (CSV) ausgegeben werden.

**So erstellen Sie einen SCIM-Beispielendpunkt**

1. Laden Sie das Paket mit dem Codebeispiel unter [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)
2. Entzippen Sie das Paket, und speichern Sie die Daten auf Ihrem Windows-Computer, z.B. unter „C:\AzureAD-BYOA-Provisioning-Samples“.
3. Starten Sie in diesem Ordner die FileProvisioningAgent-Projektmappe in Visual Studio.
4. Wählen Sie **Extras > Bibliothekspaket-Manager > Paket-Manager-Konsole**, und führen Sie die folgenden Befehle für das FileProvisioningAgent-Projekt aus, um die Projektmappenverweise aufzulösen:
  ```` 
   Install-Package Microsoft.SystemForCrossDomainIdentityManagement
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   Install-Package Microsoft.Owin.Diagnostics
   Install-Package Microsoft.Owin.Host.SystemWeb
  ````
5. Erstellen Sie das FileProvisioningAgent-Projekt.
6. Starten Sie die Anwendung „Eingabeaufforderung“ in Windows (als Administrator), und verwenden Sie den Befehl **cd**, um zum Ordner **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** zu wechseln.
7. Führen Sie den folgenden Befehl aus, und ersetzen Sie dabei „<ip-address>“ durch die IP-Adresse oder den Domänennamen des Windows-Computers:
  ````   
   FileAgnt.exe http://<ip-address>:9000 TargetFile.csv
  ````
8. Wählen Sie in Windows unter **Windows-Einstellungen > Netzwerk- und Interneteinstellungen** die Option **Windows-Firewall > Erweiterte Einstellungen** aus, und erstellen Sie eine **Eingangsregel**, mit der der eingehende Zugriff auf Port 9000 zugelassen wird.
9. Falls sich der Windows-Computer hinter einem Router befindet, muss der Router konfiguriert werden, um die Netzwerkadressübersetzung zwischen seinem Port 9000, der gegenüber dem Internet offen ist, und Port 9000 auf dem Windows-Computer durchzuführen. Dies ist erforderlich, damit Azure AD auf diesen Endpunkt in der Cloud zugreifen kann.

**So registrieren Sie den SCIM-Beispielendpunkt in Azure AD**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. 
2. Navigieren Sie zu „Azure Active Directory > Unternehmensanwendungen“, und wählen Sie **Neue Anwendung > Alle > Nicht-Kataloganwendung**.
3. Geben Sie einen Namen für Ihre Anwendung ein, und klicken Sie auf das Symbol **Hinzufügen**, um ein App-Objekt zu erstellen. Mit dem erstellten Anwendungsobjekt soll die Ziel-App dargestellt werden, für die Sie einmaliges Anmelden bereitstellen und implementieren möchten, und nicht nur der SCIM-Endpunkt.
4. Klicken Sie auf dem angezeigten Bildschirm in der linken Spalte auf die Registerkarte **Bereitstellung**.
5. Wählen Sie im Menü **Bereitstellungsmodus** die Option **Automatisch** aus.
    
  ![][2]
  *Abbildung 4: Konfigurieren der Bereitstellung im Azure-Portal*
    
6. Geben Sie im Feld **Mandanten-URL** die über das Internet zugängliche URL und den Port Ihres SCIM-Endpunkts ein. Dies kann beispielsweise http://testmachine.contoso.com:9000 oder http://<IP-Adresse>:9000/ sein, wobei <IP-Adresse> für die über das Internet zugängliche IP-Adresse steht.  
7. Wenn der SCIM-Endpunkt ein OAuth-Bearertoken benötigt, das von einem anderen Aussteller als Azure AD stammt, kopieren Sie das erforderliche OAuth-Bearertoken in das optionale Feld **Geheimes Token**. Wird dieses Feld leer gelassen, dann fügt Azure AD in jede Anforderung ein von Azure AD ausgestelltes OAuth-Bearertoken ein. Apps, die Azure AD als Identitätsanbieter verwenden, können dieses von Azure AD ausgestellte Token überprüfen.
8. Klicken Sie auf **Verbindung testen**, damit Azure Active Directory versucht, eine Verbindung mit dem SCIM-Endpunkt herzustellen. Wenn die Versuche fehlschlagen, werden Fehlerinformationen angezeigt.  
9. Wenn eine Verbindung mit der Anwendung hergestellt wird, klicken Sie auf **Speichern**, um die Administratoranmeldeinformationen zu speichern.
10. Im Abschnitt **Zuordnungen** stehen zwei Sätze von Attributzuordnungen zur Verfügung: eine für Benutzerobjekte und eine für Gruppenobjekte. Wählen Sie beide nacheinander aus, um die Attribute zu überprüfen, die von Azure Active Directory mit Ihrer App synchronisiert werden. Beachten Sie, dass die als **übereinstimmende** Eigenschaften ausgewählten Attribute für den Abgleich der Benutzer und Gruppen in Ihrer App für Updatevorgänge verwendet werden. Wählen Sie die Schaltfläche „Speichern“, um alle Änderungen zu übernehmen.
11. Im Feld **Bereich** unter **Einstellungen** wird festgelegt, welche Benutzer und Gruppen synchronisiert werden. Wenn „Sync only assigned users and groups“ (Nur zugewiesene Benutzer und Gruppen synchronisieren) (empfohlen) ausgewählt wird, werden nur Benutzer und Gruppen synchronisiert, die auf der Registerkarte **Benutzer und Gruppen** zugewiesen sind.
12. Ändern Sie nach Abschluss der Konfiguration den **Bereitstellungsstatus** in **Ein**.
13. Klicken Sie auf **Speichern**, um den Azure AD-Bereitstellungsdienst zu starten. 
14. Wenn nur zugewiesene Benutzer und Gruppen synchronisiert werden (empfohlen), klicken Sie auf die Registerkarte **Benutzer und Gruppen**, und weisen Sie die Benutzer und/oder Gruppen zu, die synchronisiert werden sollen.

Nachdem die Erstsynchronisierung gestartet wurde, können Sie auf der Registerkarte **Überwachungsprotokolle** den Status überwachen. Hier werden alle vom Bereitstellungsdienst für Ihre App ausgeführten Aktionen angezeigt. Weitere Informationen zum Lesen von Azure AD-Bereitstellungsprotokollen finden Sie unter [Tutorial: Meldung zur automatischen Benutzerkontobereitstellung](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).

Der letzte Schritt bei der Überprüfung des Beispiels besteht darin, die Datei „TargetFile.csv“ im Ordner „\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug“ auf Ihrem Windows-Computer zu öffnen. Wenn der Bereitstellungsprozess ausgeführt wird, werden in dieser Datei die Details aller zugewiesenen und bereitgestellten Benutzer und Gruppen angezeigt.

### <a name="development-libraries"></a>Entwicklungsbibliotheken
Um Ihren eigenen Webdienst zu entwickeln, der mit der SCIM-Spezifikation übereinstimmt, sollten Sie sich zuerst mit den folgenden Bibliotheken von Microsoft vertraut machen, mit denen der Entwicklungsprozess beschleunigt wird: 

1. Common Language Infrastructure-Bibliotheken (CLI) werden zur Verwendung mit Sprachen angeboten, die auf der betreffenden Infrastruktur basieren, z.B. C#. Eine dieser Bibliotheken ([Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/)) deklariert eine Schnittstelle (Microsoft.SystemForCrossDomainIdentityManagement.IProvider), die in der Abbildung unten dargestellt ist. Ein Entwickler, der die Bibliotheken verwendet, würde diese Schnittstelle mit einer Klasse implementieren, die generisch als „Anbieter“ bezeichnet werden kann. Die Bibliotheken ermöglichen dem Entwickler die Bereitstellung eines Webdiensts, der die Vorgaben der SCIM-Spezifikation erfüllt. Der Webdienst kann entweder in Internetinformationsdienste oder einer beliebigen ausführbaren Common Language Infrastructure-Assembly gehostet werden. Anforderungen werden in Aufrufe der Anbietermethoden übersetzt, die vom Entwickler für den Betrieb über einen Identitätsspeicher programmiert werden.
  
  ![][3]
  
2. [ExpressRouten-Handler](http://expressjs.com/guide/routing.html) sind zum Analysieren von Node.js-Anforderungsobjekten verfügbar, die für Aufrufe an einen Node.js-Webdienst stehen (gemäß SCIM-Spezifikation).   

### <a name="building-a-custom-scim-endpoint"></a>Erstellen eines benutzerdefinierten SCIM-Endpunkts
Mit den CLI-Bibliotheken können Entwickler ihre Dienste in einer ausführbaren Common Language Infrastructure-Assembly oder in Internetinformationsdienste hosten. Dies ist ein Beispielcode zum Hosten eines Diensts in einer ausführbaren Assembly unter der Adresse http://localhost:9000: 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

Dieser Dienst muss über eine HTTP-Adresse und über ein Serverauthentifizierungszertifikat mit einer der folgenden Stammzertifizierungsstellen verfügen: 

* CNNIC
* Comodo
* CyberTrust
* DigiCert
* GeoTrust
* GlobalSign
* Go Daddy
* VeriSign
* WoSign

Ein Serverauthentifizierungszertifikat kann an einen Port auf einem Windows-Host gebunden werden, indem das Netzwerk-Shell-Hilfsprogramm verwendet wird: 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  

Hier ist der für das certhash-Argument angegebene Wert der Fingerabdruck des Zertifikats, und der für das appid-Argument bereitgestellte Wert ist ein beliebiger eindeutiger Bezeichner (Globally Unique Identifier, GUID).  

Um den Dienst in Internetinformationsdienste zu hosten, würde ein Entwickler eine CLA-Codebibliothekassembly mit einer Klasse mit dem Namen „Startup“ im Standardnamespace der Assembly erstellen.  Dies ist ein Beispiel für eine Klasse dieser Art: 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

### <a name="handling-endpoint-authentication"></a>Behandeln der Endpunktauthentifizierung
Anforderungen aus Azure Active Directory enthalten ein OAuth 2.0-Bearertoken.   Alle Dienste, die die Anforderung empfangen, sollten den Aussteller als Azure Active Directory im Namen des erwarteten Azure Active Directory-Mandanten authentifizieren, was den Zugriff auf den Graph-Webdienst von Azure Active Directory betrifft.  Im Token wird der Aussteller mit einem iss-Anspruch angegeben, z. B. „iss":"https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/“.  In diesem Beispiel wird die Basisadresse des Anspruchswerts (https://sts.windows.net) zum Identifizieren von Azure Active Directory als Aussteller verwendet, und das Segment mit der relativen Adresse (cbb1a5ac-f33b-45fa-9bf5-f37db0fed422) ist ein eindeutiger Bezeichner des Azure Active Directory-Mandanten, in dessen Namen das Token ausgestellt wurde.  Wenn das Token zum Zugreifen auf den Graph-Webdienst von Azure Active Directory ausgestellt wurde, sollte sich der Bezeichner dieses Diensts (00000002-0000-0000-c000-000000000000) im Wert des aud-Anspruchs für das Token befinden.  

Entwickler, die die von Microsoft bereitgestellten CLA-Bibliotheken zum Erstellen eines SCIM-Diensts verwenden, können Anforderungen von Azure Active Directory authentifizieren, indem sie das Microsoft.Owin.Security.ActiveDirectory-Paket verwenden. Hierzu sind folgende Schritte erforderlich: 

1. Implementieren Sie in einem Anbieter die Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior-Eigenschaft so, dass eine Methode zurückgegeben wird, die bei jedem Start des Diensts aufgerufen werden soll: 

  ````
    public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
    {
      get
      {
        return this.OnServiceStartup;
      }
    }

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
      System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
    {
    }
  ````

2. Fügen Sie dieser Methode den folgenden Code hinzu, damit alle Anforderungen an die Endpunkte des Diensts so authentifiziert werden, dass sie über ein Token mit folgenden Eigenschaften verfügen: Es wird von Azure Active Directory im Namen eines angegebenen Mandanten ausgestellt und dient dem Zugriff auf den Graph-Webdienst von Azure AD: 

  ````
    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
      System.Web.Http.HttpConfiguration HttpConfiguration configuration)
    {
      // IFilter is defined in System.Web.Http.dll.  
      System.Web.Http.Filters.IFilter authorizationFilter = 
        new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

      // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
      // System.IdentityModel.Token.Jwt.dll.
      SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
        new TokenValidationParameters()
        {
          ValidAudience = "00000002-0000-0000-c000-000000000000"
        };

      // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
      // Microsoft.Owin.Security.ActiveDirectory.dll
      Microsoft.Owin.Security.ActiveDirectory.
      WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
        TokenValidationParameters = tokenValidationParameters,
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute the appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }
  ````


## <a name="user-and-group-schema"></a>Benutzer- und Gruppenschema
Azure Active Directory kann für SCIM-Webdienste zwei Arten von Ressourcen bereitstellen.  Diese Arten von Ressourcen sind Benutzer und Gruppen.  

Benutzerressourcen werden über den Schemabezeichner „urn:ietf:params:scim:schemas:extension:enterprise:2.0:User“ identifiziert, der in dieser Protokollspezifikation enthalten ist: http://tools.ietf.org/html/draft-ietf-scim-core-schema.  Die Standardzuordnung der Attribute von Benutzern in Azure Active Directory zu den Attributen von urn:ietf:params:scim:schemas:extension:enterprise:2.0:User-Ressourcen ist unten in Tabelle 1 angegeben.  

Gruppenressourcen werden durch die Schema-ID http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group identifiziert.  Unten in Tabelle 2 ist die Standardzuordnung der Attribute von Gruppen in Azure Active Directory zu den Attributen von http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group-Ressourcen angegeben.  

### <a name="table-1-default-user-attribute-mapping"></a>Tabelle 1: Standardzuordnung von Benutzerattributen
| Azure Active Directory-Benutzer | urn:ietf:params:scim:schemas:extension:enterprise:2.0:User |
| --- | --- |
| IsSoftDeleted |aktiv |
| displayName |displayName |
| Facsimile-TelephoneNumber |phoneNumbers[type eq "fax"].value |
| givenName |name.givenName |
| jobTitle |title |
| mail |emails[type eq "work"].value |
| mailNickname |externalId |
| manager |manager |
| mobile |phoneNumbers[type eq "mobile"].value |
| objectId |id |
| postalCode |addresses[type eq "work"].postalCode |
| proxy-Addresses |emails[type eq "other"].Value |
| physical-Delivery-OfficeName |addresses[type eq "other"].Formatted |
| streetAddress |addresses[type eq "work"].streetAddress |
| surname |name.familyName |
| telephone-Number |phoneNumbers[type eq "work"].value |
| user-PrincipalName |userName |

### <a name="table-2-default-group-attribute-mapping"></a>Tabelle 2: Standardzuordnung von Gruppenattributen
| Azure Active Directory-Gruppe | http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group |
| --- | --- |
| displayName |externalId |
| mail |emails[type eq "work"].value |
| mailNickname |displayName |
| members |members |
| objectId |id |
| proxyAddresses |emails[type eq "other"].Value |

## <a name="user-provisioning-and-de-provisioning"></a>Durchführen und Aufheben der Benutzerbereitstellung
In der folgenden Abbildung sind die Nachrichten dargestellt, die von Azure Active Directory an einen SCIM-Dienst gesendet werden, um den Lebenszyklus eines Benutzers in einem anderen Identitätsspeicher zu verwalten. In der Abbildung ist auch zu sehen, wie ein SCIM-Dienst, der mit den CLI-Bibliotheken von Microsoft zum Erstellen dieser Dienste implementiert wird, diese Anforderungen in Aufrufe der Methoden eines Anbieters übersetzt.  

![][4]
*Abbildung 5: Sequenz der Durchführung und Aufhebung einer Benutzerbereitstellung*

1. Azure Active Directory fragt den Dienst nach einem Benutzer mit einem externalId-Attributwert ab, der mit dem mailNickname-Attributwert eines Benutzers in Azure AD übereinstimmt. Die Abfrage wird als Hypertext Transfer-Protokoll-Anforderung (HTTP) wie in diesem Beispiel ausgedrückt, wobei „jyoung“ ein Beispiel für ein mailNickname-Element eines Benutzers in Azure Active Directory ist: 
  ````
    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...
  ````
  Wenn der Dienst mit den Common Language Infrastructure-Bibliotheken von Microsoft zum Implementieren von SCIM-Diensten erstellt wurde, wird die Anforderung in einen Aufruf der Query-Methode des Dienstanbieters übersetzt.  Dies ist die Signatur dieser Methode: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);
  ````
  Dies ist die Definition der Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters-Schnittstelle: 
  ````
    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
        { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
      system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
      { get; }
      System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
      { get; }
      string SchemaIdentifier 
      { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
          { get; set; }
        string AttributePath 
          { get; } 
        Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
          { get; }
        string ComparisonValue 
          { get; }
    }

    public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
    {
        Equals
    }
  ````
  Im folgenden Beispiel für eine Abfrage für einen Benutzer mit einem bestimmten Wert für das externalId-Attribut lauten die Werte der Argumente, die an die Query-Methode übergeben werden, wie folgt: 
  * parameters.AlternateFilters.Count: 1
  * parameters.AlternateFilters.ElementAt(0).AttributePath: "externalId"
  * parameters.AlternateFilters.ElementAt(0).ComparisonOperator: ComparisonOperator.Equals
  * parameters.AlternateFilter.ElementAt(0).ComparisonValue: "jyoung"
  * correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin.RequestId"] 

2. Wenn in der Antwort auf eine Abfrage an den Webdienst für einen Benutzer mit einem externalId-Attributwert, der mit dem mailNickname-Attributwert eines Benutzers übereinstimmt, keine Benutzer zurückgegeben werden, stellt Azure Active Directory die folgende Anforderung: Der Dienst muss einen Benutzer bereitstellen, der dem Benutzer in Azure Active Directory entspricht.  Dies ist ein Beispiel für eine Anforderung dieser Art: 
  ````
    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas":
      [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
      "externalId":"jyoung",
      "userName":"jyoung",
      "active":true,
      "addresses":null,
      "displayName":"Joy Young",
      "emails": [
        {
          "type":"work",
          "value":"jyoung@Contoso.com",
          "primary":true}],
      "meta": {
        "resourceType":"User"},
       "name":{
        "familyName":"Young",
        "givenName":"Joy"},
      "phoneNumbers":null,
      "preferredLanguage":null,
      "title":null,
      "department":null,
      "manager":null}
  ````
  Mit den Common Language Infrastructure-Bibliotheken von Microsoft zum Implementieren von SCIM-Diensten wird diese Anforderung in einen Aufruf der Create-Methode des Dienstanbieters übersetzt.  Die Create-Methode hat die folgende Signatur: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);
  ````
  In einer Anforderung zum Bereitstellen eines Benutzers ist der Wert des Ressourcenarguments eine Instanz von „Microsoft.SystemForCrossDomainIdentityManagement“. Die Core2EnterpriseUser-Klasse, die in der Microsoft.SystemForCrossDomainIdentityManagement.Schemas-Bibliothek definiert ist.  Wenn die Anforderung zum Bereitstellen des Benutzers erfolgreich ist, wird von der Implementierung der Methode erwartet, dass eine Instanz der Microsoft.SystemForCrossDomainIdentityManagement. Core2EnterpriseUser-Klasse zurückgegeben wird, für die der Wert der Identifier-Eigenschaft auf den eindeutigen Bezeichner des neu bereitgestellten Benutzers festgelegt ist.  

3. Zum Aktualisieren eines Benutzers, der in einem Identitätsspeicher mit vorgelagertem SCIM vorhanden ist, geht Azure Active Directory so vor, dass der aktuelle Status dieses Benutzers vom Dienst per Anforderung abgefragt wird. Die Anforderung hierzu sieht wie folgt aus: 
  ````
    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  Für einen Dienst, der mit den Common Language Infrastructure-Bibliotheken von Microsoft zum Implementieren von SCIM-Diensten erstellt wurde, wird die Anforderung in einen Aufruf der Retrieve-Methode des Dienstanbieters übersetzt.  Dies ist die Signatur der Retrieve-Methode: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
    // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
       Retrieve(
         Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
           parameters, 
           string correlationIdentifier);

    public interface 
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters
        {
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
            ResourceIdentifier 
              { get; }
    }
    public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
    {
        string Identifier 
          { get; set; }
        string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
          { get; set; }
    }
  ````
  Im Beispiels für eine Anforderung zum Abrufen des aktuellen Status eines Benutzers lauten die Werte der Eigenschaften des Objekts, das als Wert des parameters-Arguments angegeben wird, wie folgt: 
  
  * Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"

4. Wenn ein Verweisattribut aktualisiert werden soll, fragt Azure Active Directory den Dienst ab, um zu ermitteln, ob der aktuelle Wert des Verweisattributs im Identitätsspeicher mit vorgelagertem Dienst bereits mit dem Wert dieses Attributs in Azure Active Directory übereinstimmt. Bei Benutzern ist das einzige Attribut, für das der aktuelle Wert auf diese Weise abgefragt wird, das manager-Attribut. Dies ist ein Beispiel für eine Anforderung, mit der ermittelt wird, ob das manager-Attribut eines bestimmten Benutzerobjekts derzeit über einen bestimmten Wert verfügt: 
  ````
    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...
  ````
  Der Wert des Attributabfrageparameters (id) gibt Folgendes an: Wenn ein Benutzerobjekt vorhanden ist, das die Anforderungen des als Wert des Filterabfrageparameters angegebenen Ausdrucks erfüllt, wird vom Dienst erwartet, mit einer urn:ietf:params:scim:schemas:core:2.0:User- oder urn:ietf:params:scim:schemas:extension:enterprise:2.0:User-Ressource zu antworten (nur mit dem Wert des id-Attributs dieser Ressource).  Der Wert des **id**-Attributs ist dem Anforderer bekannt. Er ist im Wert des Filterabfrageparameters enthalten. Der Zweck des Nachfragens besteht darin, eine Minimaldarstellung einer Ressource anzufordern, die die Anforderungen des Filterausdrucks erfüllt und angibt, ob solch ein Objekt vorhanden ist.   

  Wenn der Dienst mit den Common Language Infrastructure-Bibliotheken von Microsoft zum Implementieren von SCIM-Diensten erstellt wurde, wird die Anforderung in einen Aufruf der Query-Methode des Dienstanbieters übersetzt. Der Wert der Eigenschaften des Objekts, das als Wert des parameters-Arguments angegeben wird, lautet wie folgt: 
  
  * parameters.AlternateFilters.Count: 2
  * parameters.AlternateFilters.ElementAt(x).AttributePath: "id"
  * parameters.AlternateFilters.ElementAt(x).ComparisonOperator: ComparisonOperator.Equals
  * parameters.AlternateFilter.ElementAt(x).ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * parameters.AlternateFilters.ElementAt(y).AttributePath: "manager"
  * parameters.AlternateFilters.ElementAt(y).ComparisonOperator: ComparisonOperator.Equals
  * parameters.AlternateFilter.ElementAt(y).ComparisonValue: "2819c223-7f76-453a-919d-413861904646"
  * parameters.RequestedAttributePaths.ElementAt(0): "id"
  * parameters.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"

  Hier kann der Wert von Index x „0“ und der Wert von Index y „1“ lauten, oder der Wert von x kann „1“ und der Wert von y „0“ lauten. Dies hängt von der Reihenfolge bei den Ausdrücken des Filterabfrageparameters ab.   

5. Dies ist ein Beispiel für eine Anforderung von Azure Active Directory an einen SCIM-Dienst zum Aktualisieren eines Benutzers: 
  ````
    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas": 
      [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
      "Operations":
      [
        {
          "op":"Add",
          "path":"manager",
          "value":
            [
              {
                "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                "value":"2819c223-7f76-453a-919d-413861904646"}]}]}
  ````
  Mit den Common Language Infrastructure-Bibliotheken von Microsoft zum Implementieren von SCIM-Diensten wird die Anforderung in einen Aufruf der Update-Methode des Dienstanbieters übersetzt. Dies ist die Signatur der Update-Methode: 
  ````
    // System.Threading.Tasks.Tasks and 
    // System.Collections.Generic.IReadOnlyCollection<T>
    // are defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
    // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

    System.Threading.Tasks.Task Update(
      Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
      string correlationIdentifier);

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
    {
    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
      PatchRequest 
        { get; set; }
    Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
      ResourceIdentifier 
        { get; set; }        
    }

    public class PatchRequest2: 
      Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
    {
    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
        Operations
        { get;}

    public void AddOperation(
      Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
    }

    public class PatchOperation
    {
    public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
      Name
      { get; set; }

    public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
      Path
      { get; set; }

    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
      { get; }

    public void AddValue(
      Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
    }

    public enum OperationName
    {
      Add,
      Remove,
      Replace
    }

    public interface IPath
    {
      string AttributePath { get; }
      System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
      Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
    }

    public class OperationValue
    {
      public string Reference
      { get; set; }

      public string Value
      { get; set; }
    }
  ````
    Im Beispiel für eine Anforderung zum Aktualisieren eines Benutzers verfügt das Objekt, das als Wert des patch-Arguments angegeben wird, über diese Eigenschaftswerte: 
  
  * ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * ResourceIdentifier.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"
  * (PatchRequest as PatchRequest2).Operations.Count: 1
  * (PatchRequest as PatchRequest2).Operations.ElementAt(0).OperationName: OperationName.Add
  * (PatchRequest as PatchRequest2).Operations.ElementAt(0).Path.AttributePath: "manager"
  * (PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.Count: 1
  * (PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.ElementAt(0).Reference: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646
  * (PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.ElementAt(0).Value: 2819c223-7f76-453a-919d-413861904646

6. Um die Bereitstellung für einen Benutzer aus einem Identitätsspeicher mit vorgelagertem SCIM-Dienst aufzuheben, sendet Azure AD eine Anforderung der folgenden Art: 
  ````
    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  Wenn der Dienst mit den Common Language Infrastructure-Bibliotheken von Microsoft zum Implementieren von SCIM-Diensten erstellt wurde, wird die Anforderung in einen Aufruf der Delete-Methode des Dienstanbieters übersetzt.   Diese Methode verfügt über folgende Signatur: 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
  ````
  Im Beispiel für eine Anforderung zum Aufheben der Bereitstellung eines Benutzers verfügt das Objekt, das als Wert des resourceIdentifier-Arguments angegeben wird, über diese Eigenschaftswerte: 
  
  * ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"
  * ResourceIdentifier.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"

## <a name="group-provisioning-and-de-provisioning"></a>Durchführen und Aufheben der Gruppenbereitstellung
In der folgenden Abbildung sind die Nachrichten dargestellt, die von Azure AD an einen SCIM-Dienst gesendet werden, um den Lebenszyklus einer Gruppe in einem anderen Identitätsspeicher zu verwalten.  Diese Nachrichten unterscheiden sich von den Nachrichten für Benutzer auf drei Arten: 

* Das Schema einer Gruppenressource wird als http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group identifiziert.  
* Für Anforderungen zum Abrufen von Gruppen wird vorgegeben, dass das members-Attribut aus allen Ressourcen ausgeschlossen wird, die als Antwort auf die Anforderung bereitgestellt werden.  
* Bei Anforderungen für die Ermittlung, ob ein Referenzattribut einen bestimmten Wert hat, handelt es sich um Anforderungen zum members-Attribut.  

![][5]
*Abbildung 6: Sequenz der Durchführung und Aufhebung einer Gruppenbereitstellung*

## <a name="related-articles"></a>Verwandte Artikel
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)
* [Automatisieren der Bereitstellung/Bereitstellungsaufhebung von Benutzern für SaaS-Apps](active-directory-saas-app-provisioning.md)
* [Anpassen von Attributzuordnungen für die Benutzerbereitstellung](active-directory-saas-customizing-attribute-mappings.md)
* [Schreiben von Ausdrücken für Attributzuordnungen](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [Bereichsfilter für die Benutzerbereitstellung](active-directory-saas-scoping-filters.md)
* [Kontobereitstellungsbenachrichtigungen](active-directory-saas-account-provisioning-notifications.md)
* [Liste der Tutorials zur Integration von SaaS-Apps](active-directory-saas-tutorial-list.md)

<!--Image references-->
[0]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[1]: ./media/active-directory-scim-provisioning/scim-figure-2a.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2b.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG

