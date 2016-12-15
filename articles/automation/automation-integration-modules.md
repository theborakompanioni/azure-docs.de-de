---
title: Erstellen eines Azure Automation-Integrationsmoduls | Microsoft Docs
description: "Dieses Tutorial führt Sie durch die Erstellung, das Testen und eine Beispielverwendung der Integrationsmodule in Azure Automation."
services: automation
documentationcenter: 
author: mgoedtel
manager: jwhit
editor: 
ms.assetid: 27798efb-08b9-45d9-9b41-5ad91a3df41e
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/12/2016
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: fb8af84ffcddcfd30f7140bde5ea08a31347d273


---
# <a name="azure-automation-integration-modules"></a>Azure Automation-Integrationsmodule
PowerShell ist die grundlegende Technologie hinter Azure Automation. Da Azure Automation auf PowerShell basiert, sind PowerShell-Module für die Erweiterbarkeit von Azure Automation von zentraler Bedeutung. In diesem Artikel beschreiben wir die Verwendung der PowerShell-Module („Integrationsmodule“) durch Azure Automation und erläutern, welche Methoden sich bewährt haben, um eigene PowerShell-Module zu erstellen und sicherzustellen, dass diese als Integrationsmodule in Azure Automation verwendet werden können. 

## <a name="what-is-a-powershell-module"></a>Was ist ein PowerShell-Modul?
Ein PowerShell-Modul ist eine Gruppe von PowerShell-Cmdlets wie **Get-Date** oder **Copy-Item**, die in der PowerShell-Konsole verwendet werden können, sowie Skripts, Workflows, Runbooks und PowerShell DSC-Ressourcen, wie „WindowsFeature“ oder „File“, die in PowerShell DSC-Konfigurationen verwendet werden können. Die gesamten Funktionen von PowerShell werden durch Cmdlets und DSC-Ressourcen bereitgestellt, und alle Cmdlets/DSC-Ressourcen werden von einem PowerShell-Modul unterstützt, von denen viele in PowerShell bereits integriert sind. Das Cmdlet **Get-Date** beispielsweise ist Teil des Microsoft.PowerShell.Utility-PowerShell-Moduls, das Cmdlet **Copy-Item** ist Teil des Microsoft.PowerShell.Management-PowerShell-Moduls, und die Package DSC-Ressource ist Teil des PSDesiredStateConfiguration-PowerShell-Moduls. Beide Module sind in PowerShell integriert. Viele PowerShell-Module sind jedoch nicht in PowerShell enthalten und werden stattdessen in Erst- oder Drittanbieterprodukten wie System Center 2012 Configuration Manager oder durch die große PowerShell-Community z.B. im PowerShell-Katalog angeboten.  Die Module sind praktisch, da sie komplexe Aufgaben über gekapselte Funktionen vereinfachen.  Erfahren Sie mehr über [PowerShell-Module auf MSDN](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx). 

## <a name="what-is-an-azure-automation-integration-module"></a>Was ist ein Azure Automation-Integrationsmodul?
Ein Integrationsmodul unterscheidet sich nicht grundlegend von einem PowerShell-Modul. Es ist lediglich ein PowerShell-Modul, das optional eine zusätzliche Datei enthält – eine Metadatendatei, die einen Azure Automation-Verbindungstyp festlegt, der mit den Cmdlets des Moduls in Runbooks verwendet werden soll. Unabhängig davon, ob diese optionale Datei enthalten ist, können diese PowerShell-Module in Azure Automation importiert werden, damit die Cmdlets in Runbooks und deren DSC-Ressourcen in DSC-Konfigurationen verwendet werden können. Azure Automation speichert diese Module im Hintergrund und lädt sie während der Ausführung der Runbook- und DSC-Kompilierungsaufträge in die Azure Automation-Sandboxes, wo Runbooks ausgeführt und DSC-Konfigurationen kompiliert werden.  DSC-Ressourcen in Modulen werden ebenfalls automatisch auf dem Automation DSC-Pullserver gespeichert, damit sie von Computern beim Versuch, DSC-Konfigurationen anzuwenden, abgerufen werden können.  In Azure Automation sind einige Azure PowerShell-Module im Lieferumfang enthalten, sodass Sie direkt mit der Automatisierung von Azure-Verwaltungsaufgaben beginnen können. Sie können aber auch ganz einfach PowerShell-Module zur Integration in beliebige Systeme, Dienste oder Tools importieren. 

> [!NOTE]
> Bestimmte Module werden im Automation-Dienst als „globale Module“ ausgeliefert. Diese globalen Module stehen Ihnen standardmäßig zur Verfügung, wenn Sie ein Automation-Konto erstellen. Sie werden von Zeit zu Zeit aktualisiert, und die Pushübertragung in Ihr Automation-Konto erfolgt automatisch. Falls Sie die automatische Aktualisierung nicht wünschen, können Sie dasselbe Modul auch selbst importieren. Dieser Vorgang hat dann Vorrang vor der globalen Version des Moduls, das wir über den Dienst bereitstellen. 
> 
> 

Das Format, in dem Sie ein Integrationsmodulpaket importieren, ist eine komprimierte Datei mit demselben Namen wie das Modul und der Erweiterung „.zip“. Sie enthält das Windows PowerShell-Modul sowie unterstützende Dateien, einschließlich einer Manifestdatei (.psd1), sofern diese im Modul enthalten ist.

Wenn das Modul einen Azure Automation-Verbindungstyp enthalten soll, muss auch eine Datei mit dem Namen „ *<ModuleName>*-Automation.json“ enthalten sein, mit der die Eigenschaften des Verbindungstyps angegeben werden. Diese Datei ist eine JSON-Datei innerhalb des Modulordners Ihrer komprimierten ZIP-Datei und enthält die Felder einer Verbindung, die zum Verbinden mit dem System oder Dienst, dem das Modul entspricht, erforderlich ist. Dadurch wird ein Verbindungstyp in Azure Automation erstellt. Mithilfe dieser Datei können Sie für den Verbindungstyp des Moduls die Feldnamen und -typen festlegen und angeben, ob die Felder verschlüsselt und/oder optional sein sollen. Im Folgenden finden eine Vorlage im JSON-Dateiformat:

```
{ 
   "ConnectionFields": [
   {
      "IsEncrypted":  false,
      "IsOptional":  false,
      "Name":  "ComputerName",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  false,
      "IsOptional":  true,
      "Name":  "Username",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  true,
      "IsOptional":  false,
      "Name":  "Password",
   "TypeName":  "System.String"
   }],
   "ConnectionTypeName":  "DataProtectionManager",
   "IntegrationModuleName":  "DataProtectionManager"
}
```

Wenn Sie Service Management Automation bereitgestellt und Integrationsmodulpakete für Ihre Automation-Runbooks erstellt haben, sollte Ihnen dies bekannt sein. 

## <a name="authoring-best-practices"></a>Bewährte Methoden für die Erstellung
Auch wenn Integrationsmodule im Wesentlichen PowerShell-Module sind, gibt es dennoch verschiedene Methoden für deren Erstellung. Beim Erstellen eines PowerShell-Moduls sind verschiedene Punkte zu beachten, damit es bestmöglich in Azure Automation verwendet werden kann. Einige dieser Punkte sind spezifisch für Azure Automation, während andere nur dazu dienen, dass Ihre Module im PowerShell-Workflow gut funktionieren, unabhängig davon, ob Sie Automation verwenden. 

1. Fügen Sie eine Zusammenfassung, eine Beschreibung und einen Hilfe-URI für jedes Cmdlet im Modul ein. In PowerShell können Sie mit dem Cmdlet **Get-Help** bestimmte Hilfeinformationen für Cmdlets definieren, um den Benutzern bei der Verwendung der Cmdlets zu helfen. So können Sie z.B. eine Zusammenfassung und einen Hilfe-URI für ein in einer PSM1-Datei erstelltes PowerShell-Modul definieren.<br>  
   
    ```
    <#
        .SYNOPSIS
         Gets all outgoing phone numbers for this Twilio account 
    #>
    function Get-TwilioPhoneNumbers {
    [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
    HelpUri='http://www.twilio.com/docs/api/rest/outgoing-caller-ids')]
    param(
       [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [string]
       $AccountSid,
   
       [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [string]
       $AuthToken,
   
       [Parameter(ParameterSetName='UseConnectionObject', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [Hashtable]
       $Connection
    )
   
    $cred = CreateTwilioCredential -Connection $Connection -AccountSid $AccountSid -AuthToken $AuthToken
   
    $uri = "$TWILIO_BASE_URL/Accounts/" + $cred.UserName + "/IncomingPhoneNumbers"
   
    $response = Invoke-RestMethod -Method Get -Uri $uri -Credential $cred
   
    $response.TwilioResponse.IncomingPhoneNumbers.IncomingPhoneNumber
    }
    ```
   <br> Durch Angabe dieser Informationen wird diese Hilfe nicht nur über das Cmdlet **Get-Help** in der PowerShell-Konsole angezeigt, sondern die Hilfefunktion wird auch in Azure Automation bereitgestellt, z.B. wenn beim Erstellen von Runbooks Aktivitäten eingefügt werden. Durch Klicken auf „Detaillierte Hilfe anzeigen“ wird der URI in einer anderen Registerkarte des Webbrowsers geöffnet, den Sie für den Zugriff auf Azure Automation verwenden.<br>![Hilfe zu Integrationsmodulen](media/automation-integration-modules/automation-integration-module-activitydesc.png)
2. Wenn das Modul auf einem Remotesystem ausgeführt wird: a. Es sollte eine Integrationsmodul-Metadatendatei enthalten, die die für die Verbindung mit dem Remotesystem benötigten Informationen enthält, d.h. den Verbindungstyp. b. Jedes Cmdlet im Modul sollte ein Verbindungsobjekt (eine Instanz dieses Verbindungstyps) als Parameter laden können.  
    Cmdlets im Modul können in Azure Automation einfacher verwendet werden, wenn das Übergeben eines Objekts mit den Feldern des Verbindungstyps als Parameter an das Cmdlet zugelassen ist. Im o.g. Runbook-Beispiel wird ein Twilio-Verbindungsobjekt namens „CorpTwilio“ für den Zugriff auf Twilio verwendet, und alle Telefonnummern im Konto werden zurückgegeben.  Sehen Sie, wie es die Felder der Verbindung den Parametern des Cmdlets zuordnet?<br>
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers 
        -AccountSid $CorpTwilio.AccountSid  
        -AuthToken $CorptTwilio.AuthToken
    }
    ```
   <br>
    Eine einfachere und bessere Möglichkeit hierfür besteht jedoch darin, das Verbindungsobjekt direkt an das Cmdlet zu übergeben:
   
    ```
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'
   
      Get-TwilioPhoneNumbers -Connection $CorpTwilio
    }
    ```
   <br>
    Sie können dieses Verhalten für Ihre Cmdlets aktivieren, indem Sie zulassen, dass ein Verbindungsobjekt direkt als Parameter akzeptiert wird, und nicht nur Verbindungsfelder für Parameter. In der Regel empfiehlt sich jeweils ein Parametersatz, damit ein Benutzer, der Azure Automation nicht benutzt, Ihre Cmdlets aufrufen kann, ohne eine Hashtabelle erstellen zu müssen, die als Verbindungsobjekt dient. Der unten angegebene Parametersatz **SpecifyConnectionFields** wird verwendet, um die Verbindungsfeldeigenschaften einzeln zu übergeben. **UseConnectionObject** können Sie die Verbindung direkt übergeben. Wie Sie sehen, ermöglicht das Cmdlet „Send-TwilioSMS“ im [Twilio-PowerShell-Modul](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) beides: 
   
    ```
    function Send-TwilioSMS {
      [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
      HelpUri='http://www.twilio.com/docs/api/rest/sending-sms')]
      param(
         [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [string]
         $AccountSid,
   
         [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [string]
         $AuthToken,
   
         [Parameter(ParameterSetName='UseConnectionObject', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [Hashtable]
         $Connection
   
       )
    }
    ```
   <br>
3. Definieren Sie einen Ausgabetyp für alle Cmdlets im Modul. Wenn Sie einen Ausgabetyp für ein Cmdlet definieren, können Sie bei der Erstellung mithilfe von IntelliSense die Ausgabeeigenschaften des Cmdlets festlegen, die während der Erstellung verwendet werden sollen. Dies ist bei der grafischen Erstellung von Automation-Runbooks hilfreich, da Kenntnisse zum Zeitpunkt des Entwurfs für die Benutzerfreundlichkeit Ihres Moduls von zentraler Bedeutung sind.<br> ![Ausgabetyp für grafische Runbooks](media/automation-integration-modules/runbook-graphical-module-output-type.png)<br> Dies ähnelt der Textvervollständigung der Ausgabe eines Cmdlets in der PowerShell ISE, allerdings ohne dass diese ausgeführt werden muss.<br> ![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)<br>
4. Cmdlets im Modul sollten keine komplexen Objekttypen für Parameter verwenden. Der PowerShell-Workflow unterscheidet sich von PowerShell darin, dass er komplexe Typen in deserialisierter Form speichert. Primitive Typen bleiben primitiv, komplexe Typen werden jedoch in die deserialisierten Versionen konvertiert, die im Wesentlichen Eigenschaftenbehälter sind. Wenn Sie z.B. das Cmdlet **Get-Process** in einem Runbook (oder auch einem PowerShell-Workflow) verwenden, wird ein Objekt des Typs [Deserialized.System.Diagnostic.Process] zurückgegeben, nicht der erwartete Typ [System.Diagnostic.Process]. Dieser Typ verfügt über die gleichen Eigenschaften wie der nicht deserialisierte Typ, aber keine der Methoden. Wenn Sie versuchen, diesen Wert als Parameter an ein Cmdlet zu übergeben, und das Cmdlet einen [System.Diagnostic.Process]-Wert für diesen Parameter erwartet, erhalten Sie den folgenden Fehler: *Cannot process argument transformation on parameter 'process'. Error: "Cannot convert the "System.Diagnostics.Process (CcmExec)" value of type "Deserialized.System.Diagnostics.Process" to type "System.Diagnostics.Process". (Die Argumenttransformation für den Parameter „Prozess“ kann nicht verarbeitet werden. Fehler: Der Wert "System.Diagnostics.Process (CcmExec)" kann nicht von Typ "Deserialized.System.Diagnostics.Process" in Typ "System.Diagnostics.Process" konvertiert werden).*   Dies liegt daran, dass ein Typenkonflikt zwischen dem erwarteten Typ [System.Diagnostic.Process] und dem angegebenen Typ [Deserialized.System.Diagnostic.Process] vorliegt. Dieses Problem kann vermieden werden, wenn Sie sicherstellen, dass die Cmdlets Ihres Moduls für Parameter keine komplexen Typen akzeptieren. Die im Folgenden dargestellte Methode ist falsch:
   
    ```
    function Get-ProcessDescription {
      param (
            [System.Diagnostic.Process] $process
      )
      $process.Description
    }
    ``` 
   <br>
    Als Nächstes zeigen wir Ihnen die korrekte Methode, bei der ein primitiver Typ geladen wird, der vom Cmdlet intern verwendet werden kann, um das komplexe Objekt abzurufen und zu verwenden. Da Cmdlets im Kontext von PowerShell und nicht des PowerShell-Workflows ausgeführt werden, wird „$process“ im Cmdlet zum korrekten [System.Diagnostic.Process]-Typ.  
   
    ```
    function Get-ProcessDescription {
      param (
            [String] $processName
      )
      $process = Get-Process -Name $processName
   
      $process.Description
    }
    ```
   <br>
   Verbindungsobjekte in Runbooks sind Hashtabellen, die ein komplexer Typ sind. Dennoch können diese Hashtabellen problemlos und ohne Umwandlungsausnahme für den Parameter „–Connection“ übergeben werden. Technisch gesehen können einige PowerShell-Typen von der serialisierten Form in die deserialisierte Form umgewandelt und somit in Cmdlets für Parameter, die den nicht deserialisierten Typ akzeptieren, übergeben werden. Dazu zählen auch Hashtabellen. Es ist möglich, dass die vom Autor des Moduls definierten Typen ebenfalls so implementiert werden, dass eine korrekte Deserialisierung möglich ist, allerdings müssen dabei Abstriche gemacht werden. Der Typ benötigt einen Standardkonstruktor, all seine Eigenschaften müssen öffentlich sein, und er muss über einen PSTypeConverter verfügen. Bei bereits definierten Typen, die nicht im Besitz des Modulautors sind, besteht keine Möglichkeit, diese entsprechend anzupassen. Aus diesem Grund wird empfohlen, komplexe Typen für Parameter komplett zu vermeiden. Tipp für die Runbook-Erstellung: Wenn Ihre Cmdlets aus irgendeinem Grund Parameter des komplexen Typs benötigen oder wenn Sie ein Modul von jemand anderem verwenden, das Parameter des komplexen Typs erfordert, kann dies in PowerShell-Workflow-Runbooks und PowerShell-Workflows in der lokalen PowerShell umgangen werden. Betten Sie hierzu das Cmdlet, das den komplexen Typ erzeugt, und das Cmdlet, das den komplexen Typ nutzt, in der gleichen InlineScript-Aktivität ein. Da der Inhalt von InlineScript als PowerShell und nicht als PowerShell-Workflow ausgeführt wird, erzeugt das Cmdlet, das den komplexen Typ erstellt, den korrekten Typ und nicht den deserialisierten komplexen Typ.
5. Legen Sie alle Cmdlets im Modul als statusfrei fest. Der PowerShell-Workflow führt jedes im Workflow aufgerufene Cmdlet in einer anderen Sitzung auf. Das heißt, dass Cmdlets in PowerShell-Workflow-Runbooks nicht funktionieren, wenn sie von einem Sitzungsstatus abhängen, der von anderen Cmdlets im gleichen Modul erstellt/geändert wurde.  Hier sehen Sie ein Beispiel für eine falsche Vorgehensweise:
   
    ```
    $globalNum = 0
    function Set-GlobalNum {
       param(
           [int] $num
       )
   
       $globalNum = $num
    }
    function Get-GlobalNumTimesTwo {
       $output = $globalNum * 2
   
       $output
    }
    ```
   <br>
6. Das Modul sollte vollständig in einem Xcopy-fähigen Paket enthalten sein. Da Azure Automation-Module auf die Automation-Sandboxes verteilt werden, wenn die Ausführung von Runbooks erforderlich ist, müssen sie unabhängig vom Host funktionieren, auf dem sie ausgeführt werden. Das bedeutet, dass es möglich sein muss, das Modulpaket zu komprimieren, zu einem anderen Host mit der gleichen oder einer neueren PowerShell-Version zu verschieben, und es nach dem Import in die PowerShell-Umgebung des anderen Hosts normal auszuführen. Damit dies möglich ist, sollte das Modul weder von anderen Dateien außerhalb des Modulordners (des Ordners, der beim Import in Azure Automation komprimiert wird) noch von eindeutigen Registrierungseinstellungen auf einem Host (wie etwa den durch die Installation eines Produkts festgelegten Einstellungen) abhängen. Wenn diese bewährte Methode nicht eingehalten wird, kann das Modul in Azure Automation nicht verwendet werden.  

## <a name="next-steps"></a>Nächste Schritte
* Die ersten Schritte mit PowerShell-Workflow-Runbooks sind unter [Mein erstes PowerShell-Workflow-Runbook](automation-first-runbook-textual.md)
* Weitere Informationen zum Erstellen von PowerShell-Modulen finden Sie unter [Writing a Windows PowerShell Module](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)




<!--HONumber=Dec16_HO2-->


