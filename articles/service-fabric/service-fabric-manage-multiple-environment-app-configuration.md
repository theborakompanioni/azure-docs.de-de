<properties
   pageTitle="Verwalten mehrerer Umgebungen in Service Fabric | Microsoft Azure"
   description="Service Fabric-Anwendungen können auf Clustern ausgeführt werden, die eine Größe von einem Computer bis zu Tausenden von Computern aufweisen. In einigen Fällen möchten Sie die Anwendung für diese verschiedenen Umgebungen unterschiedlich konfigurieren. In diesem Artikel wird beschrieben, wie unterschiedliche Anwendungsparameter pro Umgebung definiert werden."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="04/26/2016"
   ms.author="seanmck"/>

# Verwalten von Anwendungsparametern für mehrere Umgebungen

Sie können Azure Service Fabric-Cluster erstellen, indem Sie zwischen einem und mehreren Tausend Computern verwenden. Während Anwendungsbinärdateien ohne Änderungen in diesem breiten Spektrum von Umgebungen ausgeführt werden können, kann es häufig vorkommen, dass Sie die Anwendung abhängig von der Anzahl an Computern, auf denen Sie sie bereitstellen, anders konfigurieren möchten.

Sehen Sie sich `InstanceCount` als Beispiel für einen zustandslosen Dienst an. Wenn Sie Anwendungen in Azure ausführen, sollten Sie diese Parameter im Allgemeinen auf den Sonderwert „-1“ festlegen. Hierdurch wird sichergestellt, dass der Dienst auf jedem Knoten im Cluster ausgeführt wird. Diese Konfiguration eignet sich aber nicht für einen Cluster mit nur einem Computer, da nicht mehrere Prozesse auf einem einzigen Computer denselben Endpunkt überwachen können. Legen Sie `InstanceCount` in diesem Fall auf „1“ fest.

## Angeben von umgebungsspezifischen Parametern

Die Lösung dieses Konfigurationsproblems ist ein Satz von parametrisierten Standarddiensten und Anwendungsparameterdateien, mit denen die Parameterwerte für eine bestimmte Umgebung angegeben werden. Standarddienste und Anwendungsparameter werden in den Anwendungs- und Dienstmanifesten konfiguriert. Die Schemadefinition für die Dateien „ServiceManifest.xml“ und „ApplicationManifest.xml“ wird über das Service Fabric SDK und die Service Fabric-Tools unter *C:\\Programm\\Microsoft SDKs\\Service Fabric\\schemas\\ServiceFabricServiceModel.xsd* installiert.

### Standarddienste

Service Fabric-Programme bestehen aus einer Sammlung von Dienstinstanzen. Sie können zwar eine leere Anwendung und dann alle Dienstinstanzen dynamisch erstellen, aber die meisten Anwendungen verfügen über eine Reihe von Basisdiensten, die immer erstellt werden sollten, wenn die Anwendung instanziiert wird. Diese werden als Standarddienste (Default Services) bezeichnet. Sie werden im Anwendungsmanifest angegeben und enthalten Platzhalter für die Konfiguration pro Umgebung in eckigen Klammern:

    <DefaultServices>
        <Service Name="Stateful1">
            <StatefulService
                ServiceTypeName="Stateful1Type"
                TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]"
                MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">

                <UniformInt64Partition
                    PartitionCount="[Stateful1_PartitionCount]"
                    LowKey="-9223372036854775808"
                    HighKey="9223372036854775807"
                />
        </StatefulService>
    </Service>
  </DefaultServices>

Jeder benannte Parameter muss im Parameters-Element des Anwendungsmanifests definiert werden:

    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>

Die DefaultValue-Attribute geben den zu verwendenden Wert an, wenn ein spezifischerer Parameter für eine bestimmte Umgebung fehlt.

>[AZURE.NOTE] Nicht alle Dienstinstanzparameter eignen sich für die Konfiguration pro Umgebung. Im Beispiel oben werden die Werte „LowKey“ und „HighKey“ für das Partitionierungsschema des Diensts explizit für alle Instanzen des Diensts definiert, da der Partitionsbereich eine Funktion der Datendomäne und nicht der Umgebung ist.


### Dienstkonfigurationseinstellungen pro Umgebung

Das [Service Fabric-Anwendungsmodel](service-fabric-application-model.md) ermöglicht es Diensten, Konfigurationspakete zu einzubinden, die zu Laufzeit lesbare, benutzerdefinierte Schlüssel-Wert-Paare enthalten. Die Werte dieser Einstellungen können auch nach Umgebung unterschieden werden, indem `ConfigOverride` im Anwendungsmanifest angegeben wird.

Angenommen, es gibt die folgende Einstellung in der Datei „Config\\Settings.xml“ für den `Stateful1`-Dienst:


    <Section Name="MyConfigSection">
      <Parameter Name="MaxQueueSize" Value="25" />
    </Section>

Erstellen Sie `ConfigOverride` beim Importieren des Dienstmanifests in das Anwendungsmanifest, um diesen Wert für ein bestimmtes Paar aus Anwendung und Umgebung zu überschreiben.

    <ConfigOverrides>
     <ConfigOverride Name="Config">
        <Settings>
           <Section Name="MyConfigSection">
              <Parameter Name="MaxQueueSize" Value="[Stateful1_MaxQueueSize]" />
           </Section>
        </Settings>
     </ConfigOverride>
  </ConfigOverrides>

Dieser Parameter kann dann wie oben gezeigt nach der jeweiligen Umgebung konfiguriert werden. Hierfür deklarieren Sie ihn im Abschnitt „Parameters“ des Anwendungsmanifests und geben umgebungsspezifische Werte in den Anwendungsparameterdateien an.

>[AZURE.NOTE] Im Fall von Dienstkonfigurationseinstellungen gibt es drei Stellen, um den Wert eines Schlüssels festzulegen: das Dienstkonfigurationspaket, das Anwendungsmanifest und die Anwendungsparameterdatei. Service Fabric wählt immer zuerst die Anwendungsparameterdatei (falls angegeben), dann das Anwendungsmanifest und schließlich das Konfigurationspaket aus.


### Anwendungsparameterdateien

Das Service Fabric-Anwendungsprojekt kann eine oder mehrere Anwendungsparameterdateien enthalten. In jeder Datei sind die spezifischen Werte für die Parameter definiert, die im Anwendungsmanifest definiert sind:

    <!-- ApplicationParameters\Local.xml -->

    <Application Name="fabric:/Application1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <Parameters>
            <Parameter Name ="Stateful1_MinReplicaSetSize" Value="2" />
            <Parameter Name="Stateful1_PartitionCount" Value="1" />
            <Parameter Name="Stateful1_TargetReplicaSetSize" Value="3" />
        </Parameters>
    </Application>

Standardmäßig enthält eine neue Anwendung zwei Anwendungsparameterdateien mit den Namen „Local.xml“ und „Cloud.xml“:

![Anwendungsparameterdateien im Projektmappen-Explorer][app-parameters-solution-explorer]

Um eine neue Parameterdatei zu erstellen, kopieren Sie einfach eine vorhandene Datei und geben ihr einen neuen Namen.

## Identifizieren umgebungsspezifischer Parameter während der Bereitstellung

Während der Bereitstellung müssen Sie die geeignete Anwendungsparameterdatei für Ihre Anwendung auswählen. Sie können hierfür das Dialogfeld „Veröffentlichen“ in Visual Studio oder PowerShell verwenden.

### Bereitstellen von Visual Studio aus

Sie können in der Liste mit den verfügbaren Parameterdateien eine Auswahl treffen, wenn Sie die Anwendung in Visual Studio veröffentlichen.

![Auswählen einer Parameterdatei im Dialogfeld „Veröffentlichen“][publishdialog]

### Bereitstellen über PowerShell

Das PowerShell-Skript `Deploy-FabricApplication.ps1` in der Projektvorlage der Anwendung akzeptiert ein Veröffentlichungsprofil als Parameter, und PublishProfile enthält einen Verweis auf die Anwendungsparameterdatei.

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## Nächste Schritte

Weitere Informationen zu einigen grundlegenden Konzepten, die in diesem Thema behandelt werden, finden Sie unter [Service Fabric – Technische Übersicht](service-fabric-technical-overview.md). Informationen zu anderen App-Verwaltungsfunktionen in Visual Studio finden Sie unter [Verwalten Ihrer Service Fabric-Anwendungen in Visual Studio](service-fabric-manage-application-in-visual-studio.md).

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]: ./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png

<!---HONumber=AcomDC_0518_2016-->