<properties
   pageTitle="Upgrade eines Service Fabric-Clusters | Microsoft Azure"
   description="Upgraden Sie den Service Fabric-Code und/oder die Konfiguration für die Ausführung eines Service Fabric-Clusters, und machen Sie sich unter anderem mit dem Festlegen des Clusteraktualisierungsmodus, dem Upgraden von Zertifikaten, dem Hinzufügen von Anwendungsports und dem Anwenden von Betriebssystempatches vertraut. Was können Sie erwarten, wenn die Upgrades durchgeführt werden?"
   services="service-fabric"
   documentationCenter=".net"
   authors="ChackDan"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="chackdan"/>


# Upgrade von Service Fabric-Clustern

Ein Azure Service Fabric-Cluster ist eine Ressource, die Sie besitzen, die jedoch teilweise von Microsoft verwaltet wird. In diesem Artikel wird beschrieben, was automatisch verwaltet wird und was Sie selbst konfigurieren können.

## Steuern der in Ihrem Cluster ausgeführten Fabric-Version

Sie können Ihren Cluster so konfigurieren, dass er Fabric-Upgrades entweder automatisch erhält oder wenn Microsoft eine neue Version veröffentlicht. Alternativ können Sie eine unterstützte Fabric-Version auswählen, die in Ihrem Cluster verwendet werden soll.

Hierzu legen Sie die Clusterkonfiguration „upgradeMode“ über das Portal oder mithilfe von Resource Manager fest – entweder zum Zeitpunkt der Clustererstellung oder später für einen aktiven Cluster.

>[AZURE.NOTE] Achten Sie immer darauf, dass in Ihrem Cluster eine unterstützte Fabric-Version ausgeführt wird. Nach der Ankündigung einer neuen Service Fabric-Version beträgt die verbleibende Lebensdauer der vorherigen Version noch mindestens 60 Tage. Neue Versionen werden im [Blog des Service Fabric-Teams](https://blogs.msdn.microsoft.com/azureservicefabric/) angekündigt. Ab dann kann die neue Version ausgewählt werden.

14 Tage vor Ablauf der in Ihrem Cluster verwendeten Version wird ein Integritätsereignis generiert, das den Cluster in einen Warnzustand versetzt. Der Cluster bleibt so lange in dem Warnzustand, bis Sie ein Upgrade auf eine unterstützte Fabric-Version durchführen.


### Festlegen des Upgrademodus über das Portal 

Bei der Clustererstellung haben Sie die Wahl zwischen einem automatischen und einem manuellen Modus:

![Create\_Manualmode][Create_Manualmode]

Einen aktiven Cluster können Sie über die Verwaltungsoberfläche in den automatischen oder manuellen Modus versetzen.

#### Upgraden auf eine neue Version in einem Cluster im manuellen Modus über das Portal
 
Für ein Upgrade auf eine neue Version müssen Sie lediglich die verfügbare Version im Dropdownmenü auswählen und auf „Speichern“ klicken. Das Fabric-Upgrade wird automatisch initiiert. Bei dem Upgrade werden die Clusterintegritätsrichtlinien (eine Kombination aus Knotenintegrität und Integrität aller im Cluster ausgeführten Anwendungen) berücksichtigt.

Wenn die Integritätsrichtlinien des Clusters nicht erfüllt sind, wird das Upgrade zurückgesetzt. Weitere Informationen zum Festlegen dieser benutzerdefinierten Integritätsrichtlinien finden Sie weiter unten in diesem Dokument.

Beheben Sie die Probleme, die zu dem Rollback geführt haben, und initiieren Sie das Upgrade erneut, indem Sie die gleichen Schritte ausführen wie zuvor.

![Manage\_Automaticmode][Manage_Automaticmode]

### Festlegen des Upgrademodus mithilfe einer Resource Manager-Vorlage 

Fügen Sie der Microsoft.ServiceFabric/Clusterressourcendefinition die Konfiguration „upgradeMode“ hinzu, legen Sie „clusterCodeVersion“ wie unten gezeigt auf eine der unterstützten Fabric-Versionen fest, und stellen Sie dann die Vorlage bereit. Gültige Werte für „upgradeMode“ sind „Manual“ und „Automatic“.
 
![ARMUpgradeMode][ARMUpgradeMode]

#### Upgraden auf eine neue Version in einem Cluster im manuellen Modus mithilfe einer Resource Manager-Vorlage
 
Wenn sich der Cluster im manuellen Modus befindet und Sie ein Upgrade auf eine neue Version durchführen möchten, legen Sie „clusterCodeVersion“ auf eine unterstützte Version fest, und stellen Sie sie bereit. Das Fabric-Upgrade wird durch die Bereitstellung der Vorlage automatisch initiiert. Bei dem Upgrade werden die Clusterintegritätsrichtlinien (eine Kombination aus Knotenintegrität und Integrität aller im Cluster ausgeführten Anwendungen) berücksichtigt.

Wenn die Integritätsrichtlinien des Clusters nicht erfüllt sind, wird das Upgrade zurückgesetzt. Weitere Informationen zum Festlegen dieser benutzerdefinierten Integritätsrichtlinien finden Sie weiter unten in diesem Dokument.

Beheben Sie die Probleme, die zu dem Rollback geführt haben, und initiieren Sie das Upgrade erneut, indem Sie die gleichen Schritte ausführen wie zuvor.

### Abrufen der Liste mit allen verfügbaren Versionen für alle Umgebungen eines bestimmten Abonnements

Führen Sie den folgenden Befehl aus, um eine Ausgabe wie die folgende zu erhalten.

„supportExpiryUtc“ gibt an, wann eine bestimmte Version abläuft oder abgelaufen ist. Die neueste Version besitzt anstelle eines gültigen Datums den Wert „9999-12-31T23:59:59.9999999“, was bedeutet, dass noch kein Ablaufdatum feststeht.

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/clusterVersions?api-version= 2016-09-01

Output:
{
                  "value": [
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
                      "name": "5.0.1427.9490",
                      "type": "Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.0.1427.9490",
                        "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
                      "name": "5.1.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.1.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
                      "name": "4.4.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "4.4.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Linux"
                      }
                    }
                  ]
                }


```

## Fabric-Upgradeverhalten, wenn sich der Cluster im automatischen Upgrademodus befindet

Microsoft verwaltet den Fabric-Code und die Konfiguration, die in einem Cluster ausgeführt werden. Wir führen bei Bedarf automatische, überwachte Upgrades für die durch. Diese Upgrades können sich auf den Code, die Konfiguration oder beides beziehen. Um sicherzustellen, dass Ihre Anwendung während dieser Upgrades nicht oder nur minimal beeinträchtigt wird, führen wir die Upgrades in den folgenden Phasen durch:

### Phase 1: Ein Upgrade erfolgt unter Befolgung aller Integritätsrichtlinien für den Cluster

In dieser Phase erfolgen die Upgrades nacheinander in den einzelnen Upgradedomänen. Die Anwendungen, die im Cluster ausgeführt wurden, werden ohne Ausfallzeit fortgesetzt. Bei dem Upgrade werden die Clusterintegritätsrichtlinien (eine Kombination aus Knotenintegrität und Integrität aller im Cluster ausgeführten Anwendungen) berücksichtigt.

Wenn die Integritätsrichtlinien des Clusters nicht erfüllt sind, wird das Upgrade zurückgesetzt. Anschließend wird eine E-Mail an den Besitzer des Abonnements gesendet. Die E-Mail enthält folgende Informationen:

- Benachrichtigung, dass wir ein Clusterupgrade zurücksetzen mussten.
- Vorgeschlagene Abhilfemaßnahmen, falls vorhanden.
- Die Anzahl der Tage (n), bis wir Phase 2 ausführen werden.

Wir versuchen, dasselbe Upgrade ein paar weitere Male auszuführen, falls ein Upgrade aus Gründen der Infrastruktur fehlgeschlagen ist. Nach n Tagen, ab dem Sendedatum der E-Mail, fahren wir mit Phase 2 fort.

Wenn die Clusterintegritätsrichtlinien erfüllt sind, wird das Upgrade als erfolgreich betrachtet und als abgeschlossen markiert. Dies kann in dieser Phase während des anfänglichen Upgrades oder während einer der Wiederholungen des Upgrades erfolgen. Bei einer erfolgreichen Ausführung gibt es keine Bestätigung per E-Mail. Dadurch soll das Senden zu vieler E-Mails verhindert werden. Der Empfang einer E-Mail soll eine Ausnahme vom Normalfall sein. Wir erwarten, dass die meisten Clusterupgrades ohne Beeinträchtigung der Verfügbarkeit der Anwendung funktionieren.

### Phase 2: Ein Upgrade erfolgt unter ausschließlicher Befolgung der Standardintegritätsrichtlinien

Die Integritätsrichtlinien werden in dieser Phase so festgelegt, dass die Anzahl der Anwendungen, die am Anfang des Upgrades fehlerfrei waren, diesen Status für die Dauer des Upgradeprozesses beibehalten. Wie in Phase 1 erfolgen in Phase 2 die Upgrades nacheinander in den einzelnen Upgradedomänen. Die Anwendungen, die im Cluster ausgeführt wurden, werden ohne Ausfallzeit fortgesetzt. Die Clusterintegritätsrichtlinien (eine Kombination aus der Integrität der Knoten und aller im Cluster ausgeführten Anwendungen) werden während des Upgrades befolgt.

Wenn die Integritätsrichtlinien des Clusters tatsächlich nicht erfüllt sind, wird das Upgrade zurückgesetzt. Anschließend wird eine E-Mail an den Besitzer des Abonnements gesendet. Die E-Mail enthält folgende Informationen:

- Benachrichtigung, dass wir ein Clusterupgrade zurücksetzen mussten.
- Vorgeschlagene Abhilfemaßnahmen, falls vorhanden.
- Die Anzahl der Tage (n), bis wir Phase 3 ausführen werden.

Wir versuchen, dasselbe Upgrade ein paar weitere Male auszuführen, falls ein Upgrade aus Gründen der Infrastruktur fehlgeschlagen ist. Mehrere Tage vor Ablauf von „n“ Tagen wird eine Erinnerungs-E-Mail gesendet. Nach „n“ Tagen ab dem Sendedatum der E-Mail fahren wir mit Phase 3 fort. Die E-Mails, die wir Ihnen in Phase 2 senden, müssen ernst genommen werden und Abhilfemaßnahmen müssen erfolgen.

Wenn die Clusterintegritätsrichtlinien erfüllt sind, wird das Upgrade als erfolgreich betrachtet und als abgeschlossen markiert. Dies kann in dieser Phase während des anfänglichen Upgrades oder während einer der Wiederholungen des Upgrades erfolgen. Bei einer erfolgreichen Ausführung gibt es keine Bestätigung per E-Mail.

### Phase 3: Ein Upgrade erfolgt unter Befolgung aggressiver Integritätsrichtlinien

Diese Integritätsrichtlinien in dieser Phase zielen auf die Vervollständigung des Upgrades und nicht auf die Integrität der Anwendungen ab. Nur sehr wenige Clusterupgrades gelangen in diese Phase. Wenn Ihr Cluster in diese Phase gelangt, besteht eine hohe Wahrscheinlichkeit, dass Ihre Anwendung instabil wird und/oder Verfügbarkeit einbüßt.

Ähnlich wie in den beiden anderen Phasen erfolgen Upgrades in Phase 3 nacheinander in den einzelnen Upgradedomänen.

Wenn die Integritätsrichtlinien des Clusters nicht erfüllt sind, wird das Upgrade zurückgesetzt. Wir versuchen, dasselbe Upgrade ein paar weitere Male auszuführen, falls ein Upgrade aus Gründen der Infrastruktur fehlgeschlagen ist. Danach wird der Cluster fixiert, sodass er keine weitere Unterstützung und/oder Upgrades empfängt.

Eine E-Mail mit diesen Informationen und den Abhilfemaßnahmen wird an den Besitzer des Abonnements gesendet. Wir erwarten nicht, dass Cluster in einen Status gelangen, der die Folge des Misslingens von Phase 3 ist.

Wenn die Clusterintegritätsrichtlinien erfüllt sind, wird das Upgrade als erfolgreich betrachtet und als abgeschlossen markiert. Dies kann in dieser Phase während des anfänglichen Upgrades oder während einer der Wiederholungen des Upgrades erfolgen. Bei einer erfolgreichen Ausführung gibt es keine Bestätigung per E-Mail.

## Von Ihnen gesteuerte Clusterkonfigurationen

Sie können nicht nur den Clusterupgrademodus festlegen, sondern in einem aktiven Cluster auch folgende Konfigurationen ändern:

### Zertifikate

Über das Portal können Sie nun Zertifikate für den Cluster und Client mühelos hinzufügen oder löschen. [In diesem Dokument finden Sie ausführliche Anweisungen](service-fabric-cluster-security-update-certs-azure.md).

![Screenshot mit dem Zertifikatfingerabdruck im Azure-Portal.][CertificateUpgrade]


### Anwendungsports

Sie können Anwendungsports ändern, indem Sie die dem Knotentyp zugeordneten Ressourceneigenschaften des Load Balancers ändern. Sie können das Portal oder direkt den PowerShell-Ressourcen-Manager verwenden.

Um einen neuen Port auf allen VMs in einem Knotentyp öffnen zu können, führen Sie die folgenden Schritte aus.

1. Fügen Sie dem entsprechenden Load Balancer einen neuen Test hinzu.

    Wenn Sie den Cluster mithilfe des Portals bereitgestellt haben, heißen die Load Balancer für jeden Knotentyp „LB-Name der Ressourcengruppe-NodeTypename“. Für jeden Knotentyp gibt es einen LB. Da die Load Balancer-Namen nur in einer Ressourcengruppe eindeutig sind, empfiehlt es sich, sie nur in einer bestimmten Ressourcengruppe zu suchen.

    ![Screenshot, der zeigt, wie einem Load Balancer im Portal ein Test hinzugefügt wird.][AddingProbes]

2. Fügen Sie dem Load Balancer eine neue Regel hinzu.

    Fügen Sie demselben Load Balancer eine neue Regel mithilfe des im vorherigen Schritt erstellten Tests hinzu.

    ![Hinzufügen einer neuen Regel für einen Load Balancer über das Portal.][AddingLBRules]


### Placement-Eigenschaften

Für die einzelnen Knotentypen können Sie benutzerdefinierte Placement-Eigenschaften hinzufügen, die Sie in Ihrer Anwendung verwenden möchten. „NodeType“ ist eine Standardeigenschaft, die Sie verwenden können, ohne dass sie explizit hinzugefügt werden muss.

>[AZURE.NOTE] Ausführliche Informationen zur Verwendung von Platzierungseinschränkungen, Knoteneigenschaften und deren Definition finden Sie im Abschnitt „Platzierungseinschränkungen und Knoteneigenschaften“ im Dokument „Clusterressourcen-Manager von Service Fabric“ unter [Beschreiben Ihres Clusters](service-fabric-cluster-resource-manager-cluster-description.md).

### Kapazitätsmetriken

Für die einzelnen Knotentypen können Sie benutzerdefinierte Kapazitätsmetriken hinzufügen, die Sie in Ihrer Anwendung zum Melden der Auslastung verwenden möchten. Ausführliche Informationen zur Verwendung von Kapazitätsmetriken zum Melden der Auslastung finden Sie in den Dokumenten zum Clusterressourcen-Manager von Service Fabric unter [Beschreiben Ihres Clusters](service-fabric-cluster-resource-manager-cluster-description.md) und [Metriken und Auslastung](service-fabric-cluster-resource-manager-metrics.md).

### Fabric-Upgradeeinstellungen – Integritätsrichtlinien

Sie können benutzerdefinierte Integritätsrichtlinien für Fabric-Upgrades angeben. Wenn Sie Ihren Cluster für automatische Fabric-Upgrades konfiguriert haben, werden diese Richtlinien auf die erste Phase der automatischen Fabric-Upgrades angewendet. Wenn sich der Cluster im manuellen Fabric-Upgrademodus befindet, werden diese Richtlinien immer dann angewendet, wenn Sie eine neue Version auswählen und so das Fabric-Upgrade in Ihrem Cluster initiieren. Wenn Sie die Richtlinien nicht überschreiben, werden die Standardwerte verwendet.

Sie können benutzerdefinierte Integritätsrichtlinien angeben oder die aktuellen Einstellungen auf dem Fabric-Upgradeblatt überprüfen, indem Sie die erweiterten Upgradeeinstellungen auswählen. Wie das geht, zeigt die folgende Abbildung:

![Verwalten benutzerdefinierter Integritätsrichtlinien][HealthPolices]

### Anpassen der Fabric-Einstellungen für Ihren Cluster

Informationen zu den anpassbaren Einstellungen und zur jeweiligen Vorgehensweise finden Sie unter [Customize Service Fabric cluster settings and Fabric Upgrade policy](service-fabric-cluster-fabric-settings.md) (Anpassen der Service Fabric-Clustereinstellungen und der Fabric-Upgraderichtlinie).

### Betriebssystem-Patches auf den virtuellen Computern, die den Cluster bilden

Diese Funktion ist als automatisierte Funktion für die Zukunft geplant. Derzeit sind Sie jedoch noch selbst für das Patching Ihrer virtuellen Computer verantwortlich. Dies muss jeweils für eine VM erfolgen, sodass immer nur eine außer Betrieb genommen wird.

### Aktualisierung des Betriebssystems auf den virtuellen Computern, die den Cluster bilden

Wenn Sie das verwendete Betriebssystemimage auf den virtuellen Computern des Clusters aktualisieren müssen, muss das jeweils auf den einzelnen virtuellen Computern nacheinander erfolgen. Dabei sind Sie für dieses Upgrade zuständig, dies kann derzeit noch nicht automatisiert werden.

## Nächste Schritte
- Informieren Sie sich über die Vorgehensweise zum Anpassen einiger [Fabric-Einstellungen des Service Fabric-Clusters](service-fabric-cluster-fabric-settings.md).
- Machen Sie sich mit der Vorgehensweise zum [Skalieren Ihres Clusters](service-fabric-cluster-scale-up-down.md) vertraut.
- Machen Sie sich mit [Anwendungsupgrades](service-fabric-application-upgrade.md) vertraut.

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG

<!---HONumber=AcomDC_0928_2016-->