<properties
   pageTitle="Planen der Service Fabric-Clusterkapazität | Microsoft Azure"
   description="Überlegungen zur Kapazitätsplanung für Service Fabric-Cluster. Knotentypen, Beständigkeit und Zuverlässigkeitsstufen"
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
   ms.date="09/09/2016"
   ms.author="chackdan"/>


# Überlegungen zur Kapazitätsplanung für Service Fabric-Cluster

Die Kapazitätsplanung ist ein wichtiger Schritt bei jeder Produktionsbereitstellung. Nachfolgend sind einige Aspekte aufgeführt, die Sie dabei berücksichtigen müssen.

- Die Anzahl von Knotentypen, über die Ihr Cluster anfänglich verfügen muss
- Die Eigenschaften der einzelnen Knotentypen (Größe, primärer Knotentyp, Internetzugriff, Anzahl von VMs usw.)
- Die Zuverlässigkeits- und Dauerhaftigkeitsmerkmale des Clusters

Im Folgenden gehen wir kurz auf diese Aspekte ein.

## Die Anzahl von Knotentypen, über die Ihr Cluster anfänglich verfügen muss

Zunächst müssen Sie herausfinden, zu welchem Zweck der Cluster verwendet werden soll und welche Arten von Anwendungen in diesem Cluster bereitgestellt werden sollen. Wenn Sie bezüglich des Verwendungszwecks des Clusters unsicher sind, ist es vermutlich noch zu früh für die Kapazitätsplanung.

Legen Sie die Anzahl von Knotentypen fest, über die Ihr Cluster anfänglich verfügen muss. Jeder Knotentyp wird einer VM-Skalierungsgruppe zugeordnet. Jeden Knotentyp kann dann unabhängig zentral hoch- oder herunterskaliert werden, bei jedem Typ können unterschiedliche Portgruppen geöffnet sein, und die Typen können verschiedene Kapazitätsmetriken aufweisen. Wenn Sie die Anzahl von Knotentypen festlegen, sind also die folgenden Aspekte entscheidend:

- Weist Ihre Anwendung mehrere Dienste auf, und müssen einige dieser Dienste öffentlich sein oder über Internetzugriff verfügen? Typische Anwendungen umfassen einen Front-End-Gatewaydienst, der Eingaben von einem Client empfängt, und einen oder mehrere Back-End-Dienste, die mit den Front-End-Diensten kommunizieren. In diesem Fall verfügen Sie also über mindestens zwei Knotentypen.

- Haben Ihre Dienste (aus denen sich Ihre Anwendung zusammensetzt) unterschiedliche Infrastrukturanforderungen, z. B. höhere RAM-Anforderungen oder längere CPU-Zyklen? Nehmen wir z. B. an, dass die Anwendung, die bereitgestellt werden soll, einen Front-End-Dienst und einen Back-End-Dienst umfasst. Der Front-End-Dienst kann auf kleineren virtuellen Computern (VM-Größen wie D2) platziert werden, die über geöffnete Ports für das Internet verfügen. Der rechenintensive Back-End-Dienst hingegen muss auf größeren virtuellen Computern (mit VM-Größen wie D4, D6, D15) platziert werden, die nicht vom Internet aus zugänglich sind.

 Auch wenn beide Dienste in diesem Beispiel auf einem Knotentyp verwendet werden können, wird empfohlen, sie in einem Cluster mit zwei Knotentypen zu platzieren. Dadurch können für die Knotentypen unterschiedliche Eigenschaften (z. B. Internetkonnektivität oder VM-Größe) festgelegt werden. Außerdem kann die Anzahl von VMs individuell skaliert werden.

- Da Sie nicht in die Zukunft blicken können, sollten Sie sich auf die Ihnen bekannten Fakten verlassen und die Anzahl von Knotentypen entsprechend festlegen, über die Ihre Anwendungen anfänglich verfügen müssen. Später können Knotentypen hinzugefügt oder entfernt werden. Ein Service Fabric-Cluster muss über mindestens einen Knotentyp verfügen.

## Die Eigenschaften der einzelnen Knotentypen

Der **Knotentyp** kann als Äquivalent zu Rollen in Cloud Services betrachtet werden. Knotentypen definieren die Größe, die Anzahl und die Eigenschaften der virtuellen Computer. Jeder Knotentyp, der in einem Service Fabric-Cluster definiert ist, wird als separate VM-Skalierungsgruppe eingerichtet. VM-Skalierungsgruppen sind eine Azure-Computeressource, mit der Sie eine Sammlung von virtuellen Computern bereitstellen und verwalten können. Da die einzelnen Knotentypen als separate VM-Skalierungsgruppen definiert werden, können sie unabhängig voneinander zentral hoch- oder herunterskaliert werden. Außerdem können bei den verschiedenen Typen unterschiedliche Portgruppen geöffnet sein, und die Typen können unterschiedliche Kapazitätsmetriken aufweisen.

Der Cluster kann über mehrere Knotentypen verfügen. Der primäre Knotentyp (der erste, den Sie im Portal definieren) muss bei Clustern, die für Produktionsworkloads eingesetzt werden, jedoch mindestens fünf VMs aufweisen (für Testcluster sind mindestens drei VMs erforderlich). Wenn Sie den Cluster anhand einer Resource Manager-Vorlage erstellen, enthält die Knotentypdefinition ein Attribut **Ist Primary**. Der primäre Knotentyp ist der Knotentyp mit den Service Fabric-Systemdiensten.

### Primärer Knotentyp
Bei Clustern mit mehreren Knotentypen muss ein Knotentyp als primärer Knotentyp festgelegt werden. Nachfolgend sind die Merkmale eines primären Knotentyps aufgeführt:

- Die Mindestgröße von VMs für den primären Knotentyp hängt von der gewählten Dauerhaftigkeitsstufe ab. Der Standardwert für die Dauerhaftigkeitsstufe ist „Bronze“. Scrollen Sie nach unten, um Einzelheiten zur Dauerhaftigkeitsstufe und den möglichen Werten anzuzeigen.

- Die Mindestanzahl von VMs für den primären Knotentyp hängt von der gewählten Zuverlässigkeitsstufe ab. Der Standardwert für die Zuverlässigkeitsstufe ist „Silber“. Scrollen Sie nach unten, um Einzelheiten zur Zuverlässigkeitsstufe und den möglichen Werten anzuzeigen.

- Die Service Fabric-Systemdienste (z. B. der Cluster-Manager-Dienst oder der Imagespeicherdienst) werden auf dem primären Knotentyp platziert. Die Zuverlässigkeit und die Dauerhaftigkeit des Clusters hängt also von der Zuverlässigkeitsstufe und der Dauerhaftigkeitsstufe ab, die Sie für den primären Knotentyp auswählen.

![Screenshot eines Clusters mit zwei Knotentypen][SystemServices]


### Nicht primärer Knotentyp
Cluster mit mehreren Knotentypen verfügen über einen primären Knotentyp. Die übrigen Knotentypen sind keine primären Knotentypen. Nachfolgend sind die Merkmale eines nicht primären Knotentyps aufgeführt:

- Die Mindestgröße von VMs für diesen Knotentyp hängt von der gewählten Dauerhaftigkeitsstufe ab. Der Standardwert für die Dauerhaftigkeitsstufe ist „Bronze“. Scrollen Sie nach unten, um Einzelheiten zur Dauerhaftigkeitsstufe und den möglichen Werten anzuzeigen.

- Die Mindestanzahl von VMs für diesen Knotentyp beträgt 1. Diese Anzahl sollte jedoch basierend auf der Anzahl von Replikaten der Anwendung/Dienste gewählt werden, die auf diesem Knotentyp ausgeführt werden soll bzw. sollen. Die Anzahl von VMs auf einem Knotentyp kann nach der Bereitstellung des Clusters erhöht werden.


## Die Dauerhaftigkeitsmerkmale des Clusters

Über die Dauerhaftigkeitsstufe wird dem System angezeigt, über welche Berechtigungen Ihre VMs für die zugrunde liegende Azure-Infrastruktur verfügen. Auf dem primären Knotentyp kann Service Fabric mit dieser Berechtigung Infrastrukturanforderungen auf VM-Ebene anhalten (z. B. einen VM-Neustart, ein VM-Reimaging oder eine VM-Migration), die sich auf die Quorumanforderungen für die Systemdienste und Ihre zustandsbehafteten Dienste auswirken. Auf den nicht primären Knotentypen kann Service Fabric mit dieser Berechtigung Infrastrukturanforderungen auf VM-Ebene (z.B. einen VM-Neustart, ein VM-Reimaging oder eine VM-Migration) anhalten, die sich auf die Quorumanforderungen für Ihre zustandsbehafteten Dienste auf diesem Knoten auswirken.

Für diese Berechtigung können die folgenden Werte festgelegt werden:

- Gold: Die Infrastrukturaufträge können für eine Dauer von 2 Stunden pro UD angehalten werden

- Silber: Die Infrastrukturaufträge können für eine Dauer von 30 Minuten pro UD angehalten werden. (Diese Option steht derzeit nicht zur Verfügung. Sobald aktiviert, wird diese Option für alle Standard-VMs ab einem Kern verfügbar sein.)

- Bronze: Keine Berechtigungen Dies ist die Standardoption.

## Die Zuverlässigkeitsmerkmale des Clusters

Über die Zuverlässigkeitsstufe wird die Anzahl von Replikaten der Systemdienste festgelegt, die in diesem Cluster auf dem primären Knotentyp ausgeführt werden sollen. Je mehr Replikate vorhanden sind, desto größer ist die Zuverlässigkeit der Systemdienste in Ihrem Cluster.

Für die Zuverlässigkeitsstufe können folgende Werte festgelegt werden:

- Platin: Systemdienste mit einer Replikatgruppen-Zielanzahl von 9 ausführen

- Gold: Systemdienste mit einer Replikatgruppen-Zielanzahl von 7 ausführen

- Silber: Systemdienste mit einer Replikatgruppen-Zielanzahl von 5 ausführen

- Bronze: Systemdienste mit einer Replikatgruppen-Zielanzahl von 3 ausführen

>[AZURE.NOTE] Die gewählte Zuverlässigkeitsstufe bestimmt die Mindestanzahl von Knoten, über die Ihr primärer Knotentyp verfügen muss. Die Zuverlässigkeitsstufe hat keinen Einfluss auf die maximale Größe des Clusters. Sie können also einen Cluster mit 20 Knoten ausführen, der über die Zuverlässigkeitsstufe „Bronze“ verfügt.

 Sie können die Zuverlässigkeitsstufe Ihres Clusters jederzeit ändern. Durch diesen Vorgang werden die erforderlichen Clusterupgrades ausgelöst, um die Replikatgruppenanzahl der Systemdienste zu ändern. Warten Sie, bis das laufende Upgrade abgeschlossen ist, ehe Sie Änderungen am Cluster vornehmen, beispielsweise Knoten hinzufügen usw. Sie können den Fortschritt des Upgrades im Service Fabric Explorer oder durch Ausführen von [Get-ServiceFabricClusterUpgrade](https://msdn.microsoft.com/library/mt126012.aspx) verfolgen.

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## Nächste Schritte

Wenn Sie die Kapazitätsplanung abgeschlossen haben und einen Cluster einrichten, sollten Sie folgende Artikel lesen:
- [Service Fabric-Clustersicherheit](service-fabric-cluster-security.md)
- [Einführung in das Service Fabric-Integritätsmodell](service-fabric-health-introduction.md)

<!--Image references-->
[SystemServices]: ./media/service-fabric-cluster-capacity/SystemServices.png

<!---HONumber=AcomDC_0921_2016-->