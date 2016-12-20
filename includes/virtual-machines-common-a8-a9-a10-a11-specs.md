
## <a name="key-features"></a>Wichtige Features
* **Leistungsfähige Hardware:** Diese Instanzen sind für rechenintensive Anwendungen mit hoher Netzwerkauslastung konzipiert und optimiert. Hierzu zählen beispielsweise HPC-Anwendungen (High Performance Computing), Batchcomputing-Anwendungen, Modellanwendungen und umfangreiche Simulationen. 
  
    Details zum (in der H-Serie verwendeten) Intel Xeon E5-2667 v3-Prozessor sowie zum (in A8 bis A11 verwendeten) Intel Xeon E5-2670-Prozessor finden Sie auf der Website „Intel.com“. Dort können Sie sich unter anderem über die unterstützten Befehlssatzerweiterungen informieren. 
* **Für HPC-Cluster konzipiert:** Stellen Sie mehrere rechenintensive Instanzen in Azure bereit, um einen eigenständigen HPC-Cluster zu erstellen oder die Kapazität eines lokalen Clusters zu erhöhen. Bei Bedarf können Sie auch Tools zur Clusterverwaltung und Auftragsplanung bereitstellen. Alternativ können Sie die Instanzen für rechenintensive Aufgaben in einem anderen Azure-Dienst (etwa Azure Batch) verwenden.
* **RDMA-Netzwerkverbindung für MPI-Anwendungen:** Ein Teil der rechenintensiven Instanzen (H16r, H16mr, A8 und A9) verfügt über eine zweite Netzwerkschnittstelle für RDMA-Verbindungen (Remote Direct Memory Access). Diese Schnittstelle steht zusätzlich zur standardmäßigen Azure-Netzwerkschnittstelle anderer VM-Größen zur Verfügung. 
  
    Mit dieser Schnittstelle können RDMA-fähige Instanzen über ein InfiniBand-Netzwerk miteinander kommunizieren. Dabei können FDR-Raten (virtuelle Computer der Größe H16r oder H16mr) bzw. QDR-Raten (virtuelle Computer der Größe A8 oder A9) verwendet werden. Die auf diesen virtuellen Computern verfügbar gemachten RDMA-Funktionen können zur Verbesserung der Skalierbarkeit und Leistung bestimmter Linux- und Windows-basierter MPI-Anwendungen (Message Passing Interface) beitragen. Informationen zu den Voraussetzungen finden Sie in diesem Artikel unter [Zugreifen auf das RDMA-Netzwerk](#access-to-the-rdma-network).

## <a name="deployment-considerations"></a>Überlegungen zur Bereitstellung
* **Azure-Abonnement:** Wenn Sie eine größere Anzahl von rechenintensiven Instanzen bereitstellen möchten, sollten Sie ein nutzungsbasiertes Abonnement oder andere Kaufoptionen in Erwägung ziehen. Bei Verwendung eines [kostenlosen Azure-Kontos](https://azure.microsoft.com/free/)können Sie nur eine begrenzte Anzahl von Azure-Compute-Kernen nutzen.
* **Preise und Verfügbarkeit:** Die rechenintensiven VM-Größen werden Tarif nur im Standard-Tarif angeboten. Informationen zur Verfügbarkeit in den Azure-Regionen finden Sie unter [Verfügbare Produkte nach Region](https://azure.microsoft.com/regions/services/) . 
* **Kernkontingent:** Unter Umständen müssen Sie das Kontingent an CPU-Kernen in Ihrem Azure-Abonnement erhöhen. Standardmäßig stehen 20 Kerne pro Abonnement (im klassischen Bereitstellungsmodell) oder 20 Kerne pro Region (im Resource Manager-Bereitstellungsmodell) zur Verfügung. Möglicherweise ist bei Ihrem Abonnement auch die Anzahl von Kernen beschränkt, die in bestimmten VM-Größenkategorien bereitgestellt werden können. (Dies gilt auch für die H-Serie.) In diesem Fall können Sie kostenlos [eine Anfrage an den Onlinekundensupport richten](../articles/azure-supportability/how-to-create-azure-support-request.md) und eine Erhöhung des Kontingents anfordern. (Standardgrenzwerte variieren unter Umständen je nach Abonnementkategorie.)
  
  > [!NOTE]
  > Wenn Sie einen umfangreichen Kapazitätsbedarf haben, wenden Sie sich an den Azure-Support. Azure-Kontingente sind Angebotsbeschränkungen, keine Kapazitätsgarantien. Unabhängig von Ihrem Kontingent werden nur die tatsächlich verwendeten Kerne in Rechnung gestellt.
  > 
  > 
* **Virtuelles Netzwerk:** Ein [virtuelles Azure-Netzwerk](https://azure.microsoft.com/documentation/services/virtual-network/) ist nicht erforderlich, um die rechenintensiven Instanzen zu verwenden. In vielen Bereitstellungsszenarien wird jedoch mindestens ein cloudbasiertes virtuelles Azure-Netzwerk oder auch eine Site-to-Site-Verbindung für den Zugriff auf lokale Ressourcen (etwa ein Anwendungslizenzserver) benötigt. Ist eines erforderlich, erstellen Sie ein neues virtuelles Netzwerk zum Bereitstellen der Instanzen. Das Hinzufügen rechenintensiver virtueller Computer zu einem virtuellen Netzwerk in einer Affinitätsgruppe wird nicht unterstützt.
* **Clouddienst oder Verfügbarkeitsgruppe:** Stellen Sie die RDMA-fähigen virtuellen Computer zur Verwendung des Azure RDMA-Netzwerks im gleichen Clouddienst (klassisches Bereitstellungsmodell) oder in der gleichen Verfügbarkeitsgruppe (Azure Resource Manager-Bereitstellungsmodell) bereit. Bei Verwendung von Azure Batch müssen sich die RDMA-fähigen virtuellen Computer im gleichen Pool befinden.
* **Größenanpassung:** Aufgrund der speziellen Hardware für rechenintensive Instanzen können Sie die Größe rechenintensiver Instanzen nur innerhalb der gleichen Größenfamilie (H-Serie oder rechenintensive A-Serie) anpassen. So können Sie beispielsweise die Größe eines virtuellen Computers der H-Serie nur auf eine andere Größe der H-Serie festlegen. Wechsel von einer nicht rechenintensiven Größe zu einer rechenintensiven Größe werden nicht unterstützt.  
* **RDMA-Netzwerkadressbereich:** Für das RDMA-Netzwerk in Azure wird der Adressbereich 172.16.0.0/16 reserviert. Wenn Sie MPI-Anwendungen auf Instanzen ausführen möchten, die in einem virtuellen Azure-Netzwerk bereitgestellt wurden, vergewissern Sie sich, dass der Adressraum des virtuellen Netzwerks sich nicht mit dem RDMA-Netzwerk überschneidet.



<!--HONumber=Nov16_HO4-->


