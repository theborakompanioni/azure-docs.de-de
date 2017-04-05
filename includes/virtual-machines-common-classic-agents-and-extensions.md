

VM-Erweiterungen helfen Ihnen bei folgenden Aufgaben:

* Ändern von Sicherheits- und Identitätsfeatures, z. B. Zurücksetzen von Kontowerten und Verwenden von Antischadsoftware
* Starten, Beenden oder Konfigurieren der Überwachung und Diagnose
* Zurücksetzen oder Installieren von Konnektivitätsfeatures, z. B. RDP und SSH
* Diagnostizieren, Überwachen und Verwalten Ihrer virtuellen Computer

Darüber hinaus stehen noch zahlreiche weitere Features zur Verfügung. Es werden regelmäßig neue VM-Erweiterungsfeatures veröffentlicht. Dieser Artikel beschreibt die Azure-VM-Agents für Windows und Linux und deren Unterstützung der Funktionalität von VM-Erweiterungen. Eine Liste mit VM-Erweiterungen nach Featurekategorie finden Sie unter [Azure-VM-Erweiterungen und Features](../articles/virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="azure-vm-agents-for-windows-and-linux"></a>Azure-VM-Agents für Windows und Linux
Der Azure-Agent für virtuelle Computer (VM-Agent) ist ein sicherer, einfacher Prozess zum Installieren, Konfigurieren und Entfernen von VM-Erweiterungen auf Instanzen von virtuellen Azure-Computern. Der VM-Agent dient als sicherer lokaler Steuerungsdienst für Ihre Azure-VM. Die Erweiterungen, die der Agent lädt, bieten bestimmte Features, um Ihre Produktivität bei der Nutzung der Instanz zu erhöhen.

Es gibt zwei Azure-VM-Agents: einen für virtuelle Windows-Computer und einen für virtuelle Linux-Computer.

Wenn eine Instanz eines virtuellen Computers VM-Erweiterungen verwenden soll, muss die Instanz über einen installierten VM-Agent verfügen. Bei einem Image eines virtuellen Computers, das über das Azure-Portal erstellt wird, sowie bei einem Image aus dem **Marketplace** wird im Rahmen der Erstellung automatisch ein VM-Agent installiert. Falls eine Instanz eines virtuellen Computers nicht über einen VM-Agent verfügt, können Sie den VM-Agent nachträglich installieren. Außerdem haben Sie die Möglichkeit, den Agent in einem benutzerdefinierten Image eines virtuellen Computers zu installieren und dieses Image anschließend hochzuladen.

> [!IMPORTANT]
> Diese VM-Agents sind sehr einfache Dienste, die eine sichere Verwaltung von Instanzen virtueller Computer ermöglichen. Es gibt möglicherweise Fälle, in denen Sie den VM-Agent nicht benötigen. Erstellen Sie in diesem Fall virtuelle Computer, auf denen der VM-Agent nicht installiert ist (entweder mithilfe der Azure-Befehlszeilenschnittstelle oder mithilfe von PowerShell). Der VM-Agent kann zwar physisch entfernt werden, das Verhalten von VM-Erweiterungen in der Instanz ist jedoch nicht definiert. Das Entfernen eines installierten VM-Agents wird daher nicht unterstützt.
>

Der VM-Agent wird in den folgenden Situationen aktiviert:

* Wenn Sie eine Instanz eines virtuellen Computers über das Azure-Portal erstellen und ein Image aus dem **Marketplace** auswählen.
* Wenn Sie eine Instanz eines virtuellen Computers mit dem Cmdlet [New-AzureVM](https://msdn.microsoft.com/library/azure/dn495254.aspx) oder [New-AzureQuickVM](https://msdn.microsoft.com/library/azure/dn495183.aspx) erstellen. Sie können einen virtuellen Computer ohne VM-Agent erstellen, indem Sie dem Cmdlet [Add-AzureProvisioningConfig](https://msdn.microsoft.com/library/azure/dn495299.aspx) den Parameter **–DisableGuestAgent** hinzufügen.

* Wenn Sie den VM-Agent manuell herunterladen und in einer vorhandenen Instanz eines virtuellen Computers installieren und dabei den Wert **ProvisionGuestAgent** auf **true** festlegen. Dieses Verfahren ist für Windows- und Linux-Agents geeignet und kann mithilfe eines PowerShell-Befehls oder eines REST-Aufrufs ausgeführt werden. (Wenn Sie den Wert **ProvisionGuestAgent** nach der manuellen Installation des VM-Agents nicht festlegen, wird das Hinzufügen des VM-Agents nicht richtig erkannt.) Das folgende Codebeispiel veranschaulicht die Vorgehensweise unter Verwendung von PowerShell, wobei die Argumente `$svc` und `$name` bereits bestimmt wurden:

      $vm = Get-AzureVM –ServiceName $svc –Name $name
      $vm.VM.ProvisionGuestAgent = $TRUE
      Update-AzureVM –Name $name –VM $vm.VM –ServiceName $svc

* Wenn Sie ein Image eines virtuellen Computers mit einem installierten VM-Agent erstellen. Sobald das Image mit dem VM-Agent vorhanden ist, können Sie es in Azure hochladen. Laden Sie für einen virtuellen Windows-Computer die [MSI-Datei des Windows-VM-Agents](http://go.microsoft.com/fwlink/?LinkID=394789) herunter, und installieren Sie den VM-Agent. Installieren Sie den VM-Agent bei einem virtuellen Linux-Computer über das GitHub-Repository unter <https://github.com/Azure/WALinuxAgent>. Weitere Informationen zum Installieren des VM-Agents unter Linux finden Sie im [Benutzerhandbuch für den Azure Linux-VM-Agent](../articles/virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

> [!NOTE]
> In PaaS heißt der VM-Agent **WindowsAzureGuestAgent** und ist auf virtuellen Computern mit der Web- und Workerrolle immer verfügbar. (Weitere Informationen finden Sie unter [Azure Role Architecture](http://blogs.msdn.com/b/kwill/archive/2011/05/05/windows-azure-role-architecture.aspx) (Azure-Rollenarchitektur).) Der VM-Agent für Rollen-VMs kann jetzt den Clouddienst-VMs auf die gleiche Weise Erweiterungen hinzufügen wie den dauerhaften virtuellen Computern. Der größte Unterschied zwischen VM-Erweiterungen für Rollen-VMs und dauerhafte virtuelle Computer besteht darin, wann die VM-Erweiterungen hinzugefügt werden. Bei Rollen-VMs werden Erweiterungen zuerst dem Clouddienst und anschließend den Bereitstellungen mit diesem Clouddienst hinzugefügt.
>
> Verwenden Sie das Cmdlet [Get-AzureServiceAvailableExtension](https://msdn.microsoft.com/library/azure/dn722498.aspx) zum Auflisten aller verfügbaren Rollen-VM-Erweiterungen.
>
>

## <a name="find-add-update-and-remove-vm-extensions"></a>Suchen, Hinzufügen, Aktualisieren und Entfernen von VM-Erweiterungen
Weitere Informationen zu diesen Aufgaben finden Sie unter [Hinzufügen, Suchen, Aktualisieren und Entfernen von Azure-VM-Erweiterungen](../articles/virtual-machines/windows/classic/manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
