


## <a name="using-vm-extensions"></a>Verwenden von VM-Erweiterungen
Azure-VM-Erweiterungen implementieren Verhaltensweisen oder Features, mit denen andere Programme auf virtuellen Azure-Computern verwendet werden können. Die Erweiterung **WebDeployForVSDevTest** ermöglicht Visual Studio etwa die Webbereitstellung von Lösungen auf Ihrem virtuellen Azure-Computer. Sie bieten aber auch die Möglichkeit zur Interaktion mit dem virtuellen Computer, um ein anderes Verhalten zu unterstützen. So können Sie beispielsweise die VM-Zugriffserweiterungen von PowerShell, die Azure-Befehlszeilenschnittstelle und REST-Clients verwenden, um die Remotezugriffswerte auf Ihrem virtuellen Azure-Computer zurückzusetzen oder zu ändern.

> [!IMPORTANT]
> Eine vollständige, nach unterstützten Features sortierte Liste mit Erweiterungen finden Sie unter [Azure-VM-Erweiterungen](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Da jede VM-Erweiterung ein bestimmtes Feature unterstützt, hängt es von der Erweiterung ab, was Sie mit der Erweiterung genau machen können. Bevor Sie Ihren virtuellen Computer ändern, sollten Sie daher die Dokumentation für die VM-Erweiterung lesen, die Sie verwenden möchten. Das Entfernen einiger VM-Erweiterungen wird nicht unterstützt. Andere weisen Eigenschaften auf, die festgelegt werden können und das VM-Verhalten grundlegend ändern.
> 
> 

Die häufigsten Aufgaben sind:

1. Suchen nach verfügbaren Erweiterungen
2. Aktualisieren von geladenen Erweiterungen
3. Hinzufügen von Erweiterungen
4. Entfernen von Erweiterungen

## <a name="find-available-extensions"></a>Suchen nach verfügbaren Erweiterungen
Sie können die Erweiterung und die erweiterten Informationen wie folgt finden:

* PowerShell
* Plattformübergreifende Azure-Befehlszeilenschnittstelle (Azure CLI)
* Dienstverwaltungs-REST-API

### <a name="azure-powershell"></a>Azure PowerShell
Einige Erweiterungen verfügen über spezielle PowerShell-Cmdlets, die ihre Konfiguration über PowerShell vereinfachen können. Die folgenden Cmdlets funktionieren jedoch für alle VM-Erweiterungen.

Sie können die folgenden Cmdlets zum Abrufen von Informationen zu den verfügbaren Erweiterungen verwenden:

* Für Instanzen von Webrollen oder Workerrollen können Sie das Cmdlet [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) verwenden.
* Für Instanzen von Virtual Machines können Sie das Cmdlet [Get-AzureVMAvailableExtension](https://msdn.microsoft.com/library/azure/dn722480.aspx) verwenden.
  
   Im folgenden Codebeispiel wird etwa veranschaulicht, wie Informationen für die Erweiterung **IaaSDiagnostics** mithilfe von PowerShell aufgelistet werden.
  
      PS C:\> Get-AzureVMAvailableExtension -ExtensionName IaaSDiagnostics
  
      Publisher                   : Microsoft.Azure.Diagnostics
      ExtensionName               : IaaSDiagnostics
      Version                     : 1.2
      Label                       : Microsoft Monitoring Agent Diagnostics
      Description                 : Microsoft Monitoring Agent Extension
      PublicConfigurationSchema   :
      PrivateConfigurationSchema  :
      IsInternalExtension         : False
      SampleConfig                :
      ReplicationCompleted        : True
      Eula                        :
      PrivacyUri                  :
      HomepageUri                 :
      IsJsonExtension             : True
      DisallowMajorVersionUpgrade : False
      SupportedOS                 :
      PublishedDate               :
      CompanyName                 :

### <a name="azure-command-line-interface-azure-cli"></a>Azure-Befehlszeilenschnittstelle (Command Line Interface, CLI)
Einige Erweiterungen weisen Azure-CLI-Befehle auf, die spezifisch für die jeweilige Erweiterung sind und möglicherweise die Konfiguration vereinfachen. Ein Beispiel für eine solche Erweiterung ist die Docker-VM-Erweiterung. Die folgenden Befehle funktionieren jedoch für alle VM-Erweiterungen.

Mit dem Befehl **azure vm extension list** können Sie Informationen zu verfügbaren Erweiterungen abrufen. Mit der Option **–-json** zeigen Sie alle verfügbaren Informationen zu einzelnen oder mehreren Erweiterungen an. Wenn Sie keinen Erweiterungsnamen verwenden, gibt der Befehl eine JSON-Beschreibung aller verfügbaren Erweiterungen zurück.

Das folgende Codebeispiel veranschaulicht etwa das Auflisten der Informationen für die Erweiterung **IaaSDiagnostics** mit dem Azure-CLI-Befehl **azure vm extension list**. Dabei wird die Option **–-json** verwendet, um vollständige Informationen zurückzugeben.

    $ azure vm extension list -n IaaSDiagnostics --json
    [
      {
        "publisher": "Microsoft.Azure.Diagnostics",
        "name": "IaaSDiagnostics",
        "version": "1.2",
        "label": "Microsoft Monitoring Agent Diagnostics",
        "description": "Microsoft Monitoring Agent Extension",
        "replicationCompleted": true,
        "isJsonExtension": true
      }
    ]



### <a name="service-management-rest-apis"></a>REST-APIs der Dienstverwaltung
Sie können die folgenden REST-APIs zum Abrufen von Informationen zu den verfügbaren Erweiterungen verwenden:

* Für Instanzen von Webrollen oder Workerrollen können Sie den Vorgang [List Available Extensions](https://msdn.microsoft.com/library/dn169559.aspx) verwenden. Zum Auflisten der Versionen der verfügbaren Erweiterungen können Sie den Vorgang [List Extension Versions](https://msdn.microsoft.com/library/dn495437.aspx)verwenden.
* Für Instanzen von Virtual Machines können Sie den Vorgang [List Resource Extensions](https://msdn.microsoft.com/library/dn495441.aspx) verwenden. Zum Auflisten der Versionen der verfügbaren Erweiterungen können Sie den Vorgang [List Resource Extension Versions](https://msdn.microsoft.com/library/dn495440.aspx)verwenden.

## <a name="add-update-or-disable-extensions"></a>Hinzufügen, Aktualisieren oder Deaktivieren von Erweiterungen
Erweiterungen können hinzugefügt werden, wenn eine Instanz erstellt wird. Sie können aber auch einer aktiven Instanz hinzugefügt werden. Erweiterungen können aktualisiert, deaktiviert oder entfernt werden. Sie können diese Aktionen mithilfe von Azure PowerShell-Cmdlets oder mithilfe von REST-API-Vorgängen der Dienstverwaltung ausführen. Parameter sind zum Installieren und Einrichten einiger Erweiterungen erforderlich. Öffentliche und private Parameter werden für Erweiterungen unterstützt.

### <a name="azure-powershell"></a>Azure PowerShell
Die Verwendung von Azure PowerShell-Cmdlets ist die einfachste Möglichkeit zum Hinzufügen und Aktualisieren von Erweiterungen. Wenn Sie die Erweiterungs-Cmdlets verwenden, wird der größte Teil der Konfiguration der Erweiterung für Sie erledigt. Von Zeit zu Zeit müssen Sie möglicherweise eine Erweiterung programmgesteuert hinzufügen. In diesem Fall müssen Sie die Konfiguration der Erweiterung angeben.

Sie können die folgenden Cmdlets verwenden, um herauszufinden, ob für eine Erweiterung die Konfiguration öffentlicher und privater Parameter erforderlich ist:

* Für Instanzen von Webrollen oder Workerrollen können Sie das Cmdlet **Get-AzureServiceAvailableExtension** verwenden.
* Für Instanzen von Virtual Machines können Sie das Cmdlet **Get-AzureVMAvailableExtension** verwenden.

### <a name="service-management-rest-apis"></a>REST-APIs der Dienstverwaltung
Wenn Sie eine Liste der verfügbaren Erweiterungen mithilfe der REST-APIs abrufen, erhalten Sie Informationen zur erforderlichen Konfiguration der Erweiterung. Die zurückgegebenen Informationen enthalten möglicherweise Parameterinformationen, die durch ein öffentliches Schema und ein privates Schema dargestellt werden. Öffentliche Parameterwerte werden in Abfragen zu den Instanzen zurückgegeben. Private Parameterwerte werden nicht zurückgegeben.

Sie können die folgenden REST-APIs verwenden, um herauszufinden, ob für eine Erweiterung die Konfiguration öffentlicher und privater Parameter erforderlich ist:

* Für Instanzen von Web- oder Workerrollen enthalten die Elemente **PublicConfigurationSchema** und **PrivateConfigurationSchema** die Informationen in der Antwort des Vorgangs [List Available Extensions](https://msdn.microsoft.com/library/dn169559.aspx).
* Für Instanzen von Virtual Machines enthalten die Elemente **PublicConfigurationSchema** und **PrivateConfigurationSchema** die Informationen in der Antwort des Vorgangs [List Resource Extensions](https://msdn.microsoft.com/library/dn495441.aspx).

> [!NOTE]
> Erweiterungen können auch Konfigurationen verwenden, die mit JSON definiert sind. Wenn diese Typen von Erweiterungen verwendet werden, wird nur das **SampleConfig** -Element verwendet.
> 
> 

