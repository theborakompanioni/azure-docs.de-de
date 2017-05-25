

Seit der Einführung der benutzerdefinierten Skripterweiterung wird diese häufig verwendet, um Workloads auf virtuellen Windows und Linux-Computern zu konfigurieren. Seit der Einführung von Azure-Ressourcen-Manager-Vorlagen können Benutzer nun eine einzelne Vorlage erstellen, die nicht nur den virtuellen Computer bereitstellt, sondern auch den Workload auf ihm konfiguriert.

## <a name="about-azure-resource-manager-templates"></a>Informationen zu Azure Resource Manager-Vorlagen
Mithilfe von Azure Resource Manager-Vorlagen können Sie deklarativ die Azure IaaS-Infrastruktur in der Json-Sprache angeben, indem Sie die Abhängigkeiten zwischen Ressourcen definieren. Eine ausführliche Übersicht über Azure-Ressourcen-Manager-Vorlagen finden Sie in den folgenden Artikeln:

* [Übersicht über Ressourcengruppen](../articles/azure-resource-manager/resource-group-overview.md)
* [Bereitstellen von Vorlagen mit Azure PowerShell](../articles/virtual-machines/windows/ps-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

### <a name="prerequisites"></a>Voraussetzungen
1. Laden Sie die Azure-Befehlszeilentools für Ihr Betriebssystem [hier](https://azure.microsoft.com/downloads/) herunter.
2. Wenn die Skripts auf einer vorhandenen VM ausgeführt werden, müssen Sie sicherstellen, dass der VM-Agent auf der VM aktiviert ist. Andernfalls folgen Sie den Anweisungen zu [Linux](../articles/virtual-machines/linux/classic/manage-extensions.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json) oder [Windows](../articles/virtual-machines/windows/classic/manage-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json), um einen Agent zu installieren.
3. Laden Sie die Skripts, die Sie auf der VM ausführen möchten, nach Azure Storage hoch. Die Skripts können aus einem einzelnen oder mehreren Speichercontainern stammen.
4. Alternativ können die Skripts in ein GitHub-Konto hochgeladen werden.
5. Das Skript sollte so geschrieben sein, dass das Eingangsskript, das durch die Erweiterung gestartet wird, weitere Skripts startet.

## <a name="using-the-custom-script-extension"></a>Verwenden der benutzerdefinierten Skripterweiterung
Zum Bereitstellen mit Vorlagen wird die gleiche Version der CustomScript-Erweiterung verwendet, die auch für Azure-Service-Verwaltungs-APIs verfügbar ist. Die Erweiterung unterstützt dieselben Parameter und Szenarios, etwa das Hochladen von Dateien in ein Azure-Speicherkonto oder in einen GitHub-Speicherort. Der wesentliche Unterschied bei einer Verwendung von Vorlagen besteht darin, dass die genaue Version der Erweiterung angegeben werden muss im Gegensatz zur Angabe der Version im Format Hauptversion.*.

