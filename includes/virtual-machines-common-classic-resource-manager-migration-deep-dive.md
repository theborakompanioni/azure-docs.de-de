## <a name="meaning-of-migration-of-iaas-resources-from-classic-to-resource-manager"></a>Die Bedeutung der Migration von IaaS-Ressourcen vom klassischen Bereitstellungsmodell zu Resource Manager
Bevor wir näher auf die Details eingehen, betrachten wir den Unterschied zwischen Datenebenen- und Verwaltungsebenenvorgängen bei IaaS-Ressourcen.

* *Verwaltungs-/Steuerungsebene:* Beschreibt die Aufrufe, die auf der Verwaltungs-/Steuerungsebene oder bei der API zum Ändern von Ressourcen eingehen. Beispielsweise werden ausgeführte Ressourcen durch Vorgänge verwaltet wie das Erstellen eines virtuellen Computers, das Neustarten eines virtuellen Computers und das Aktualisieren eines virtuellen Netzwerks mit einem neuen Subnetz. Die Verbindung zu den Instanzen ist davon nicht direkt betroffen.
* *Datenebene* (Anwendung): Beschreibt die Laufzeit der Anwendung selbst und umfasst die Interaktion mit Instanzen, die nicht über die Azure-API verlaufen. Das Zugreifen auf Ihre Website oder das Abrufen von Daten von einer ausgeführten SQL Server-Instanz oder einem MongoDB-Server wird als Interaktion mit der Datenebene bzw. der Anwendung angesehen. Das Kopieren eines Blobs aus einem Speicherkonto und das Zugreifen auf eine öffentliche IP-Adresse, um per RDP oder SSH eine Verbindung mit dem virtuellen Computer herzustellen, sind ebenfalls Vorgänge auf der Datenebene. Mit diesen Vorgängen wird die Ausführung der Anwendung für die Bereiche Compute, Netzwerk und Speicher sichergestellt.

![Screenshot zur Veranschaulichung des Unterschieds zwischen Verwaltungs-/Steuerungsebene und Datenebene](../articles/virtual-machines/media/virtual-machines-windows-migration-classic-resource-manager/data-control-plane.png)

> [!NOTE]
> In einigen Migrationsszenarien hält die Azure-Plattform die virtuellen Computer an, hebt ihre Zuordnung auf und startet sie neu. Dies führt zu einer kurzen Downtime auf der Datenebene.
>
>

## <a name="the-migration-experience"></a>Migrationsvorgang
Wir empfehlen Ihnen Folgendes, bevor Sie mit dem Migrationsvorgang beginnen:

* Stellen Sie sicher, dass die Ressourcen, die Sie migrieren möchten, keine nicht unterstützten Features oder Konfigurationen verwenden. Gewöhnlich erkennt die Plattform diese Probleme und generiert einen Fehler.
* Wenn Sie über virtuelle Computer verfügen, die nicht in einem virtuellen Netzwerk enthalten sind, werden diese während der Vorbereitung beendet, und die Zuordnung wird aufgehoben. Wenn Sie die öffentliche IP-Adresse nicht verlieren möchten, sollten Sie sich die Informationen zum Reservieren der IP-Adresse ansehen, bevor Sie den Vorbereitungsvorgang beginnen. Wenn sich die virtuellen Computer aber in einem virtuellen Netzwerk befinden, werden sie nicht beendet, und die Zuordnung wird nicht aufgehoben.
* Planen Sie die Migration für einen Zeitraum außerhalb der Geschäftszeiten, um Raum für unerwartete Fehler zu lassen, die während der Migration unter Umständen auftreten können.
* Laden Sie die aktuelle Konfiguration Ihrer virtuellen Computer mit PowerShell, CLI-Befehlen (Befehlszeilenschnittstelle) oder REST-APIs herunter, um die Überprüfung zu vereinfachen, nachdem der Vorbereitungsschritt abgeschlossen ist.
* Aktualisieren Sie Ihre Skripts für die Automatisierung und Operationalisierung, um sie an das Resource Manager-Bereitstellungsmodell anzupassen, bevor Sie die Migration starten. Sie können optional GET-Vorgänge durchführen, wenn sich die Ressourcen im Zustand „Vorbereitet“ befinden.
* Werten Sie die RBAC-Richtlinien aus, die für die klassischen IaaS-Ressourcen konfiguriert sind, und stellen Sie einen Plan für die Vorgehensweise nach dem Abschluss der Migration auf.

Der Migrationsworkflow sieht wie folgt aus:

![Screenshot zum Migrationsworkflow](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-workflow.png)

> [!NOTE]
> Alle in den folgenden Abschnitten beschriebenen Vorgänge sind idempotent. Sollte ein Problem auftreten, das nicht auf ein nicht unterstütztes Feature oder auf einen Konfigurationsfehler zurückzuführen ist, wiederholen Sie den Vorbereitungs-, Abbruch- oder Commitvorgang. Die Azure-Plattform versucht erneut, die Aktion auszuführen.
>
>

### <a name="validate"></a>Überprüfen
Der Überprüfungsvorgang ist der erste Schritt im Migrationsprozess. Das Ziel dieses Schritts ist, im Hintergrund die Daten für die zu migrierenden Ressourcen zu analysieren und durch „Erfolgreich“/„Fehler“ anzugeben, ob die Ressourcen für die Migration bereit sind.

Sie wählen das virtuelle Netzwerk oder den gehosteten Dienst aus (sofern es kein virtuelles Netzwerk ist), das bzw. den Sie für die Migration überprüfen möchten.

* Wenn die Migration für die Ressource nicht möglich ist, werden auf der Azure-Plattform alle Gründe angegeben, aus denen die Migration nicht unterstützt wird.

Beim Überprüfen von Speicherdiensten befindet sich das migrierte Konto in einer Ressourcengruppe mit dem gleichen Namen wie Ihr Speicherkonto (mit dem Zusatz „-Migrated“).  Beispiel: Wenn Ihr Speicherkonto „mystorage“ heißt, befindet sich die Azure Resource Manager-fähige Ressource in einer Ressourcengruppe namens „mystorage-Migrated“ und enthält ein Speicherkonto namens „mystorage“.

### <a name="prepare"></a>Vorbereiten
Der Vorbereitungsvorgang ist der zweite Schritt im Migrationsprozess. Das Ziel dieses Schritts besteht darin, die Transformation der IaaS-Ressourcen von Ressourcen des klassischen Bereitstellungsmodells zu Resource Manager-Ressourcen zu simulieren und diese zu Visualisierungszwecken nebeneinander darzustellen.

Sie wählen das virtuelle Netzwerk oder den gehosteten Dienst aus (sofern es kein virtuelles Netzwerk ist), das bzw. den Sie für die Migration vorbereiten möchten.

* Wenn die Migration für die Ressource nicht möglich ist, beendet die Azure-Plattform den Migrationsprozesses und gibt den Grund an, aus dem der Vorbereitungsvorgang nicht erfolgreich war.
* Wenn die Migration für die Ressource durchgeführt werden kann, sperrt die Azure-Plattform zuerst die Vorgänge der Verwaltungsebene für die zu migrierenden Ressourcen. Beispielsweise können Sie einem in der Migration befindlichen virtuellen Computer keinen Datenträger hinzuzufügen.

Die Azure-Plattform startet dann für die zu migrierenden Ressourcen die Migration der Metadaten vom klassischen Bereitstellungsmodell zu Resource Manager.

Nach Abschluss der Vorbereitung können Sie die Ressourcen sowohl im klassischen Bereitstellungsmodell als auch im Resource Manager-Bereitstellungsmodell visualisieren. Für jeden Clouddienst im klassischen Bereitstellungsmodell erstellt die Azure-Plattform einen Ressourcengruppennamen nach dem Muster `cloud-service-name>-Migrated`.

> [!NOTE]
> Sie können nicht den Namen der Ressourcengruppe auswählen, die für migrierte Ressourcen erstellt wurde (d.h. „-Migrated“), Sie können aber nach Abschluss der Migration das Feature zum Verschieben von Azure Resource Manager verwenden, um Ressourcen in beliebige Ressourcengruppe zu verschieben. Weitere Informationen hierzu finden Sie unter [Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement](../articles/resource-group-move-resources.md).

Die folgenden beiden Screenshots zeigen das Ergebnis nach einem erfolgreichen Vorbereitungsvorgang. Der ersten Screenshot zeigt eine Ressourcengruppe, die den ursprünglichen Clouddienst enthält. Der zweite Screenshot zeigt die neue „-Migrated“-Ressourcengruppe mit den entsprechenden Azure Resource Manager-Ressourcen.

![Screenshot mit klassischem Portal-Clouddienst](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-classic.png)

![Screenshot mit Azure Resource Manager-Portalressourcen in Vorbereitung](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-arm.png)

> [!NOTE]
> Virtuelle Computer, die sich nicht in einem klassischen virtuellen Netzwerk befinden, erhalten in dieser Phase der Migration den Status „Beendet (Zuordnung aufgehoben)“.
>
>

### <a name="check-manual-or-scripted"></a>Überprüfung (manuell oder per Skript)
Im Überprüfungsschritt können Sie bei Bedarf die Konfiguration verwenden, die Sie zuvor heruntergeladen haben, um zu überprüfen, ob die Migration korrekt aussieht. Alternativ dazu können Sie sich auch beim Portal anmelden und Stichproben der Eigenschaften und Ressourcen durchführen, um sicherzustellen, dass die Migration der Metadaten in Ordnung ist.

Wenn Sie ein virtuelles Netzwerk migrieren, wird der Großteil der Konfiguration für virtuelle Computer nicht neu gestartet. Für die Anwendungen auf diesen VMs können Sie überprüfen, ob die Anwendung noch betriebsbereit ist und ausgeführt wird.

Sie können Ihre Skripts für Überwachung/Automatisierung und den Betrieb testen, um zu ermitteln, ob die VMs wie erwartet ausgeführt werden und ob die aktualisierten Skripts richtig funktionieren. GET-Vorgänge werden nur dann unterstützt, wenn sich die Ressourcen im Status „Vorbereitet“ befinden.

Es gibt kein festes Zeitfenster, vor dem Sie für die Migration ein Commit durchführen müssen. Sie können sich in diesem Zustand so viel Zeit wie nötig lassen. Die Verwaltungsebene ist aber für diese Ressourcen gesperrt, bis Sie entweder einen Abbruch (abort) oder ein Commit (commit) durchführen.

Falls Probleme auftreten, können Sie die Migration immer abbrechen und zurück zum klassischen Bereitstellungsmodell wechseln. Nach dem Wechsel zurück öffnet die Azure-Plattform die Vorgänge der Verwaltungsebene für die Ressourcen , damit Sie den normalen Betrieb für diese virtuellen Computer im klassischen Bereitstellungsmodell wieder aufnehmen können.

### <a name="abort"></a>Abbruch
Der Abbruch ist ein optionaler Schritt, mit dem Sie Ihre Änderungen auf das klassische Bereitstellungsmodell zurücksetzen und die Migration beenden können.

> [!NOTE]
> Dieser Vorgang kann nicht mehr ausgeführt werden, nachdem Sie den Commitvorgang ausgelöst haben.     
>
>

### <a name="commit"></a>Commit
Nach Abschluss der Überprüfung können Sie einen Commit für die Migration durchführen. Die Ressourcen werden nicht mehr im klassischen Bereitstellungsmodell angezeigt und stehen nur noch im Resource Manager-Bereitstellungsmodell zur Verfügung. Die migrierten Ressourcen können nur im neuen Portal verwaltet werden.

> [!NOTE]
> Dies ist ein idempotenter Vorgang. Wenn hierbei ein Fehler auftritt, sollten Sie den Vorgang wiederholen. Sollten weiterhin Fehler auftreten, erstellen Sie ein Supportticket, oder posten Sie unter Verwendung des Tags ClassicIaaSMigration in unserem [VM-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows).
>
>
<br>
Hier sehen Sie ein Flussdiagramm der Schritte während einer Migration.

![Screenshot mit den Migrationsschritten](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="translation-of-classic-to-azure-resource-manager-resources"></a>Umwandlung klassischer Ressourcen in Azure Resource Manager-Ressourcen
Die folgende Tabelle zeigt die Darstellung der Ressourcen im klassischen Bereitstellungsmodell und in Resource Manager. Andere Features und Ressourcen werden derzeit nicht unterstützt.

| Klassische Darstellung | Resource Manager-Darstellung | Ausführliche Hinweise |
| --- | --- | --- |
| Name des Clouddiensts |DNS-Name |Während der Migration wird für jeden Clouddienst eine neue Ressourcengruppe mit dem Benennungsmuster `<cloudservicename>-migrated`erstellt. Diese Ressourcengruppe enthält alle Ihre Ressourcen. Der Name des Clouddiensts wird als DNS-Name verwendet, der der öffentlichen IP-Adresse zugeordnet ist. |
| Virtueller Computer |Virtueller Computer |VM-spezifische Eigenschaften werden unverändert migriert. Bestimmte osProfile-Informationen, wie etwa der Computername, werden im klassischen Bereitstellungsmodell nicht gespeichert und bleiben daher nach der Migration leer. |
| An virtuellen Computer angefügte Datenträgerressourcen |An virtuellen Computer angefügte implizite Datenträger |Datenträger werden im Resource Manager-Bereitstellungsmodell nicht als Ressourcen oberster Ebene modelliert. Sie werden als implizite Datenträger unter dem virtuellen Computer migriert. Derzeit werden nur Datenträger unterstützt, die mit einem virtuellen Computer verbunden sind. Resource Manager-VMs können nun klassische Speicherkonten verwenden, sodass die Datenträger auf einfache Weise ohne jegliche Updates migriert werden können. |
| VM-Erweiterungen |VM-Erweiterungen |Mit Ausnahme von XML-Erweiterungen werden alle Ressourcenerweiterungen aus dem klassischen Bereitstellungsmodell migriert. |
| Zertifikate für virtuelle Computer |Zertifikate in Azure Key Vault |Wenn ein Clouddienst Dienstzertifikate enthält, wird eine neue Azure Key Vault-Instanz pro Clouddienst erstellt, und die Zertifikate werden in den Schlüsseltresor verschoben. Die VMs werden aktualisiert, um auf die Zertifikate aus dem Schlüsseltresor zu verweisen. <br><br> **HINWEIS**: Löschen Sie den Schlüsseltresor nicht, da dies dazu führen kann, dass die VM in einen fehlerhaften Zustand wechselt. Wir arbeiten an Verbesserungen im Back-End, damit Schlüsseltresore sicher gelöscht oder gemeinsam mit der VM in ein neues Abonnement verschoben werden können. |
| WinRM-Konfiguration |WinRM-Konfiguration unter osProfile |Die Konfiguration der Windows-Remoteverwaltung wird im Rahmen der Migration unverändert verschoben. |
| „Availability-set“-Eigenschaft |„Availability-set“-Ressource | Im klassischen Bereitstellungsmodell handelte es sich bei der Angabe der Verfügbarkeitsgruppe um eine Eigenschaft auf dem virtuellen Computer. Bei der Migration werden Verfügbarkeitsgruppen zu einer Ressource der obersten Ebene. Folgende Konfigurationen werden nicht unterstützt: mehrere Verfügbarkeitsgruppen pro Clouddienst sowie eine Kombination aus mindestens einer Verfügbarkeitsgruppe und virtuellen Computern, die keiner Verfügbarkeitsgruppe in einem Clouddienst angehören. |
| Netzwerkkonfiguration auf einem virtuellen Computer |Primäre Netzwerkschnittstelle |Die Netzwerkkonfiguration auf einem virtuellen Computer wird nach der Migration als primäre Netzwerkschnittstellenressource dargestellt. Bei virtuellen Computern, die sich nicht in einem virtuellen Netzwerk befinden, ändert sich im Zuge der Migration die interne IP-Adresse. |
| Mehrere Netzwerkschnittstellen auf einem virtuellen Computer |Netzwerkschnittstellen |Sind einem virtuellen Computer mehrere Netzwerkschnittstellen zugeordnet, wird jede Netzwerkschnittstelle im Rahmen der Migration zu einer Ressource der obersten Ebene im Resource Manager-Bereitstellungsmodell (einschließlich aller Eigenschaften). |
| Endpunktsatz mit Lastenausgleich |Load Balancer |Im klassischen Bereitstellungsmodell wurde von der Plattform für jeden Clouddienst ein impliziter Load Balancer zugewiesen. Im Zuge der Migration wird eine neue Load Balancer-Ressource erstellt, und der Lastenausgleichs-Endpunktsatz wird zu Load Balancer-Regeln. |
| Eingehende NAT-Regeln |Eingehende NAT-Regeln |Auf dem virtuellen Computer definierte Eingabeendpunkte werden bei der Migration in Netzwerkadressenübersetzungs-Eingangsregeln für den Load Balancer umgewandelt. |
| VIP-Adresse |Öffentliche IP-Adresse mit DNS-Name |Die virtuelle IP-Adresse wird zur öffentlichen IP-Adresse, die dem Load Balancer zugeordnet ist. |
| Virtuelles Netzwerk |Virtuelles Netzwerk |Das virtuelle Netzwerk wird mit allen zugehörigen Eigenschaften in das Resource Manager-Bereitstellungsmodell migriert. Eine neue Ressourcengruppe namens `-migrated`wird erstellt. |
| Reservierte IP-Adressen |Öffentliche IP-Adresse mit statischer Zuordnungsmethode |Dem Load Balancer zugeordnete reservierte IP-Adressen werden zusammen mit dem Clouddienst oder virtuellen Computer migriert. Die Migration nicht zugeordneter reservierter IP-Adressen wird derzeit nicht unterstützt. |
| Öffentliche IP-Adresse pro virtuellem Computer |Öffentliche IP-Adresse mit dynamischer Zuordnungsmethode |Die dem virtuellen Computer zugeordnete öffentliche IP-Adresse wird in eine öffentliche IP-Adressressource mit statischer Zuordnungsmethode konvertiert. |
| NSGs |NSGs |Einem Subnetz zugeordnete Netzwerksicherheitsgruppen (Network Security Groups, NSGs) werden im Rahmen der Migration für das Resource Manager-Bereitstellungsmodell geklont. Die NSG aus dem klassischen Bereitstellungsmodell wird bei der Migration nicht entfernt. Während der Migration werden jedoch Vorgänge auf Verwaltungsebene für die NSG blockiert. |
| DNS-Server |DNS-Server |DNS-Server, die einem virtuellen Netzwerk oder dem virtuellen Computer zugeordnet sind, werden im Rahmen der entsprechenden Ressourcenmigration zusammen mit allen Eigenschaften migriert. |
| UDRs |UDRs |Einem Subnetz zugeordnete benutzerdefinierte Routen (User-Defined Routes, UDRs) werden im Rahmen der Migration für das Resource Manager-Bereitstellungsmodell geklont. Die UDR aus dem klassischen Bereitstellungsmodell wird bei der Migration nicht entfernt. Während der Migration werden Vorgänge auf Verwaltungsebene für die UDR blockiert. |
| IP-Weiterleitungseigenschaft für die Netzwerkkonfiguration eines virtuellen Computers |IP-Weiterleitungseigenschaft für die Netzwerkschnittstellenkarte |Die IP-Weiterleitungseigenschaft für einen virtuellen Computer wird im Zuge der Migration in eine Eigenschaft der Netzwerkschnittstelle konvertiert. |
| Load Balancer mit mehreren IP-Adressen |Load Balancer mit mehreren öffentlichen IP-Ressourcen |Sämtliche öffentliche, dem Load Balancer zugeordnete IP-Adressen werden in eine öffentliche IP-Ressource konvertiert und dem Load Balancer nach der Migration zugeordnet. |
| Interne DNS-Namen für den virtuellen Computer |Interne DNS-Namen für die Netzwerkschnittstellenkarte |Während der Migration werden die internen DNS-Suffixe für die virtuellen Computer in eine schreibgeschützte Eigenschaft mit dem Namen InternalDomainNameSuffix auf der NIC migriert. Das Suffix bleibt nach der Migration unverändert, und die virtuelle Computerlösung sollte weiterhin wie zuvor funktionieren. |
| Gateway des virtuellen Netzwerks |Gateway des virtuellen Netzwerks |Eigenschaften des Gateways des virtuellen Netzwerks werden unverändert migriert. Die dem Gateway zugeordnete VIP bleibt ebenfalls unverändert. |
| Standort des lokalen Netzwerks |Gateway des lokalen Netzwerks |Die Eigenschaften des lokalen Netzwerkstandorts werden unverändert in eine neue Ressource namens „Lokales Netzwerkgateway“ migriert. Sie repräsentiert lokale Adresspräfixe und die Remotegateway-IP. |
| Verbindungsverweise |Verbindung |Konnektivitätsverweise zwischen dem Gateway- und dem lokalen Netzwerkstandort in der Netzwerkkonfiguration werden nach der Migration im Resource Manager durch eine neu erstellte Ressource namens „Verbindung“ repräsentiert. Alle Eigenschaften von Konnektivitätsverweisen in Netzwerkkonfigurationsdateien werden unverändert in die neu erstellte Verbindungsressource kopiert. VNET-zu-VNET-Konnektivität im klassischen Modell wird durch das Erstellen von zwei IPSec-Tunneln zu lokalen Netzwerkstandorten erreicht, die VNETs repräsentieren. Im Resource Manager-Modell erfolgt eine Transformation in den Verbindungstyp „Vnet2Vnet“, ohne dass lokale Netzwerkgateways erforderlich sind. |

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Änderungen an der Automatisierung und an den Tools nach der Migration
Im Rahmen der Migration Ihrer Ressourcen vom klassischen zum Resource Manager-Bereitstellungsmodell müssen Sie Ihre bestehende Automatisierung oder die bestehenden Tools aktualisieren, um sicherzustellen, dass sie nach der Migration weiterhin funktionieren.
