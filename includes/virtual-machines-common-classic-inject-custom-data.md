


In diesem Thema wird Folgendes beschrieben:

* Das Einfügen von Daten in einen virtuellen Azure-Computer (VM), während dieser bereitgestellt wird.
* Das Abrufen des virtuellen Computers für Windows und Linux.
* Verwenden von speziellen Tools, die auf einigen Systemen zum automatischen Ermitteln und Verarbeiten benutzerdefinierter Daten verfügbar sind.

> [!NOTE]
> Dieser Artikel beschreibt, wie benutzerdefinierte Daten mithilfe eines virtuellen Computers, der mit der Azure-Dienstverwaltungs-API erstellt wurde, injiziert werden können. Informationen zur Verwendung der Azure-Dienstverwaltungs-API finden Sie in [der Beispielvorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-customdata).
> 
> 

## <a name="injecting-custom-data-into-your-azure-virtual-machine"></a>Einfügen benutzerdefinierter Daten in einem virtuellen Azure-Computer
Dieses Feature wird derzeit nur in der [Azure-Befehlszeilenschnittstelle](https://github.com/Azure/azure-xplat-cli) unterstützt. Hier erstellen wir eine `custom-data.txt`-Datei, die unsere Daten enthält, und fügen sie während der Bereitstellung in den virtuellen Computer ein. Sie können für den Befehl `azure vm create` jede der Optionen verwenden. Der folgende Ansatz ist sehr allgemein gehalten:

```
    azure vm create <vmname> <vmimage> <username> <password> \  
    --location "West US" --ssh 22 \  
    --custom-data ./custom-data.txt  
```


## <a name="using-custom-data-in-the-virtual-machine"></a>Verwenden von benutzerdefinierten Daten im virtuellen Computer
* Wenn es sich bei Ihrer Azure-VM um eine VM auf Windows-Basis handelt, wird die benutzerdefinierte Datendatei unter `%SYSTEMDRIVE%\AzureData\CustomData.bin`gespeichert. Auch wenn sie für die Übertragung vom lokalen Computer zur neuen VM base64-codiert war, wird sie automatisch decodiert und kann sofort geöffnet oder verwendet werden.
  
  > [!NOTE]
  > Wenn die Datei vorhanden ist, wird sie überschrieben. Die Sicherheit im Verzeichnis ist auf **System:Full Control** und **Administrators:Full Control** festgelegt.
  > 
  > 
* Wenn es sich bei Ihrer Azure-VM um eine VM auf Linux-Basis handelt, befindet sich die benutzerdefinierte Datendatei abhängig von Ihrer Distribution an einem der folgenden zwei Orte. Die Daten könnten jedoch base64-codiert sein, sodass Sie sie zunächst decodieren müssen.
  
  * `/var/lib/waagent/ovf-env.xml`
  * `/var/lib/waagent/CustomData`
  * `/var/lib/cloud/instance/user-data.txt` 

## <a name="cloud-init-on-azure"></a>Cloud-Init in Azure
Wenn Ihre Azure-VM von einem Ubuntu- oder CoreOS-Image erstellt wurde, können Sie mit CustomData eine Cloud-Config-Datei an Cloud-Init senden. Wenn Ihre benutzerdefinierte Datendatei ein Skript ist, kann dieses einfach von Cloud-Init ausgeführt werden.

### <a name="ubuntu-cloud-images"></a>Ubuntu-Cloud-Images
In den meisten Azure Linux-Images bearbeiten Sie "/ /etc/waagent.conf", um den temporären Ressourcendatenträger und die Auslagerungsdatei zu konfigurieren. Weitere Informationen erhalten Sie im [Benutzerhandbuch für Azure Linux-Agent](../articles/virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Bei Ubuntu-Cloud-Images muss zum Konfigurieren des Ressourcendatenträgers (auch als "kurzlebiger" Datenträger bezeichnet) und der Swap-Partition jedoch Cloud-Init verwendet werden. Weitere Details finden Sie auf der folgenden Seite im Ubuntu-Wiki: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps-using-cloud-init"></a>Nächste Schritte: Verwenden von Cloud-Init
Weitere Informationen finden Sie unter [cloud-init documentation for Ubuntu](https://help.ubuntu.com/community/CloudInit)(cloud-init-Dokumentation für Ubuntu) (in englischer Sprache).

<!--Link references-->
[REST-API-Referenz zur Dienstverwaltung mit "Add Role"](http://msdn.microsoft.com/library/azure/jj157186.aspx)

[Azure-Befehlszeilenschnittstelle](https://github.com/Azure/azure-xplat-cli)

