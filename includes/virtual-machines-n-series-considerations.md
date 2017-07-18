## <a name="deployment-considerations"></a>Überlegungen zur Bereitstellung

* Informationen zur Verfügbarkeit von virtuellen Computern der N-Serie finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/en-us/regions/services/).

* Virtuelle Computer der N-Serie können nur im Resourcen Manager-Bereitstellungsmodell bereitgestellt werden.

* Wählen Sie beim Erstellen eines virtuellen Computers der N-Serie mit dem Azure-Portal auf dem Blatt **Grundlagen** den **VM-Datenträgertyp** **HDD** aus. Zum Auswählen einer verfügbaren Größe der N-Serie klicken Sie auf dem Blatt **Größe** auf **Alle anzeigen**.

* Virtuelle Computer der N-Serie unterstützen keine VM-Datenträger, die durch Azure Storage Premium gesichert sind.

* Wenn Sie eine größere Anzahl von virtuellen Computern der N-Serie bereitstellen möchten, sollten Sie ein nutzungsbasiertes Abonnement oder andere Kaufoptionen in Erwägung ziehen. Bei Verwendung eines [kostenlosen Azure-Kontos](https://azure.microsoft.com/free/)können Sie nur eine begrenzte Anzahl von Azure-Compute-Kernen nutzen.

* Möglicherweise müssen Sie das Kernnutzungskontingent (pro Region) in Ihrem Azure-Abonnement sowie das separate Kontingent für NC- oder NV-Kerne erhöhen. In diesem Fall können Sie kostenlos [eine Anfrage an den Onlinekundensupport richten](../articles/azure-supportability/how-to-create-azure-support-request.md) und eine Erhöhung des Kontingents anfordern. Standardgrenzwerte variieren unter Umständen je nach Abonnementkategorie.

* Ein VM-Image, das Sie für virtuelle Computer der N-Serie bereitstellen können, ist der [virtuelle Azure-Computer für Data Science](../articles/machine-learning/machine-learning-data-science-virtual-machine-overview.md). Beim virtuellen Azure-Computer für Data Science sind viele beliebte Data Science- und Deep Learning-Tools bereits installiert und konfiguriert. NVIDIA Tesla GPU-Treiber für NC-Instanzen sind ebenfalls bereits vorinstalliert.





