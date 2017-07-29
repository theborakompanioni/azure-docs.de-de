---
title: Azure Service Fabric-Anwendungsmodell | Microsoft-Dokumentation
description: Informationen zum Modellieren und Beschreiben von Anwendungen und Diensten in Service Fabric.
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: mani-ramaswamy
ms.assetid: 17a99380-5ed8-4ed9-b884-e9b827431b02
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 7/27/2017
ms.author: ryanwi
ms.translationtype: Human Translation
ms.sourcegitcommit: 3716c7699732ad31970778fdfa116f8aee3da70b
ms.openlocfilehash: fa7b48225698a2e210d61656ffff0225227e2ab5
ms.contentlocale: de-de
ms.lasthandoff: 06/30/2017

---
# <a name="model-an-application-in-service-fabric"></a>Modellieren von Anwendungen in Service Fabric
Dieser Artikel bietet eine Übersicht über das Azure Service Fabric-Anwendungsmodell und erläutert, wie eine Anwendung und ein Dienst über Manifestdateien definiert werden.

## <a name="understand-the-application-model"></a>Informationen zum Anwendungsmodell
Eine Anwendung ist eine Sammlung von einzelnen Diensten, die eine bzw. mehrere bestimmte Funktionen ausführen. Ein Dienst führt eine vollständige und eigenständige Funktion aus und kann unabhängig von anderen Diensten gestartet und ausgeführt werden.  Ein Dienst besteht aus Code, Konfiguration und Daten. Für jeden Dienst besteht der Code aus den ausführbaren Binärdateien, die Konfiguration umfasst Diensteinstellungen, die zur Laufzeit geladen werden können, und die Daten bestehen aus beliebigen statischen Daten, die vom Dienst verarbeitet werden. Jede Komponente in diesem hierarchischen Anwendungsmodell kann unabhängig mit einer Versionsangabe versehen und aktualisiert werden.

![Das Service Fabric-Anwendungsmodell][appmodel-diagram]

Ein Anwendungstyp ist eine Kategorisierung einer Anwendung und besteht aus einem Bündel von Diensttypen. Ein Diensttyp ist eine Kategorisierung eines Diensts. Bei der Kategorisierung können die Einstellungen und Konfigurationen variieren, die Kernfunktionen bleiben jedoch gleich. Die Instanzen eines Diensts sind die verschiedenen Dienstkonfigurationsvarianten desselben Diensttyps.  

Klassen (oder „Typen“) von Anwendungen und Diensten werden über XML-Dateien (Anwendungsmanifeste und Dienstmanifeste) beschrieben.  Die Manifeste sind die Vorlagen, anhand derer Anwendungen aus dem Imagespeicher des Clusters instanziiert werden können. Die Schemadefinition für die Dateien „ServiceManifest.xml“ und „ApplicationManifest.xml“ wird mit dem Service Fabric SDK und den Service Fabric-Tools unter *C:\Programme\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd* installiert.

Die Codes für verschiedene Anwendungsinstanzen werden als separate Prozesse ausgeführt, selbst wenn sie im gleichen Service Fabric-Knoten gehostet werden. Darüber hinaus kann der Lebenszyklus jeder Anwendungsinstanz unabhängig verwaltet (z.B. Durchführen eines Upgrades). Die folgende schematische Darstellung zeigt die Gliederung von Anwendungstypen in Diensttypen, die wiederum aus Code, Konfiguration und Datenpaketen bestehen. Obwohl jeder Diensttyp normalerweise einige oder alle dieser Pakettypen enthält, werden nur die Code-/Konfigurations-/Datenpakete für `ServiceType4` angezeigt, um das Diagramm zu vereinfachen.

![Service Fabric-Anwendungstypen und Diensttypen][cluster-imagestore-apptypes]

Anwendungen und Dienste werden mit zwei verschiedenen Manifestdateien beschrieben: dem Dienstmanifest und dem Anwendungsmanifest. Manifeste werden in den folgenden Abschnitten ausführlich behandelt.

Im Cluster können eine oder mehrere Instanzen eines Diensttyps aktiv sein. Zustandsbehaftete Dienstinstanzen oder Replikate erzielen beispielsweise eine hohe Zuverlässigkeit durch die Replikation des Zustands zwischen Replikaten, die sich im Cluster in verschiedenen Knoten befinden. Die Replikation gewährleistet im Wesentlichen die Redundanz für den Dienst, sodass dieser auch bei Ausfall eines Knotens in einem Cluster verfügbar ist. Bei einem [partitionierten Dienst](service-fabric-concepts-partitioning.md) unterteilt sich der Zustand (und die Zugriffsmuster auf diesen Zustand) noch weiter auf verschiedene Knoten im Cluster.

Die folgende schematische Darstellung zeigt die Beziehung zwischen Anwendungen und Dienstinstanzen, Partitionen und Replikaten.

![Partitionen und Replikate in einem Dienst][cluster-application-instances]

> [!TIP]
> Sie können das Layout von Anwendungen in einem Cluster mit dem Service Fabric Explorer-Tool anzeigen, das hier verfügbar ist: http://&lt;IhreClusterAdresse&gt;:19080/Explorer. Weitere Informationen finden Sie unter [Visualisieren Ihres Clusters mit Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
> 
> 

## <a name="describe-a-service"></a>Beschreiben eines Diensts
Das Dienstmanifest definiert deklarativ der Diensttyp und die Version. Es legt Dienstmetadaten wie Diensttyp, Integritätseigenschaften, Lastenausgleichsmetriken, Dienstbinärdateien und Konfigurationsdateien fest.  Anders ausgedrückt: Es beschreibt den Code, die Konfiguration und die Datenpakete, die ein Dienstpaket bilden, mit dem ein oder mehrere Diensttypen unterstützt werden. Hier ein einfaches Beispiel eines Dienstmanifests:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="MyCode" Version="CodeVersion1">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
    <EnvironmentVariables>
      <EnvironmentVariable Name="MyEnvVariable" Value=""/>
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
    </EnvironmentVariables>
  </CodePackage>
  <ConfigPackage Name="MyConfig" Version="ConfigVersion1" />
  <DataPackage Name="MyData" Version="DataVersion1" />
</ServiceManifest>
```

**Version** -Attribute sind unstrukturierte Zeichenfolgen und werden im System nicht analysiert. Version-Attribute werden dazu verwendet, jede Komponente für Upgrades mit Versionsangaben zu versehen.

**ServiceTypes** deklariert, welche Diensttypen von **CodePackages** in diesem Manifest unterstützt werden. Wenn ein Dienst mit einem dieser Diensttypen instanziiert wird, werden alle Codepakete, die in diesem Manifest deklariert sind, durch Ausführen ihrer Einstiegspunkte aktiviert. Die resultierenden Prozesse registrieren die unterstützten Diensttypen zur Laufzeit. Diensttypen werden auf Ebene der Manifeste und nicht auf Ebene der Codepakete deklariert. Wenn mehrere Codepakete vorhanden sind, werden sie daher alle aktiviert, wenn im System nach einem der deklarierten Diensttypen gesucht wird.

**SetupEntryPoint** ist ein privilegierter Einstiegspunkt, der mit den gleichen Anmeldeinformationen wie Service Fabric (i.d.R. dem *LocalSystem*-Konto) vor jedem anderen Einstiegspunkt ausgeführt wird. Die durch **EntryPoint** angegebene ausführbare Datei ist üblicherweise der Diensthost mit langer Ausführungsdauer. Das Vorhandensein eines separaten Setupeinstiegspunkts vermeidet, dass der Diensthost über längere Zeiträume mit hohen Berechtigungen ausgeführt werden muss. Die von **EntryPoint** angegebene ausführbare Datei wird ausgeführt, nachdem **SetupEntryPoint** erfolgreich beendet wurde. Wenn der Prozess beendet wird oder abstürzt, wird der resultierende Prozess überwacht und neu gestartet (er beginnt wieder mit **SetupEntryPoint**).  

Gängige Szenarios für die Verwendung von **SetupEntryPoint** sind die Ausführung einer ausführbaren Datei vor dem Starten des Diensts sowie die Ausführung eines Vorgangs mit erhöhten Rechten. Beispiel:

* Einrichten und Initialisieren von Umgebungsvariablen, die die ausführbare Datei des Diensts benötigt. Dies ist nicht auf Dateien beschränkt, die mit den Service Fabric-Programmiermodellen geschrieben wurden. „npm.exe“ benötigt beispielsweise einige Umgebungsvariablen, die zum Bereitstellen einer node.js-Anwendung konfiguriert wurden.
* Einrichten einer Zugriffssteuerung durch Installieren von Sicherheitszertifikaten.

Weitere Informationen zum Konfigurieren von **SetupEntryPoint** finden Sie unter [Konfigurieren der Richtlinie für einen Setupeinstiegspunkt für Dienste](service-fabric-application-runas-security.md).

**EnvironmentVariables** enthält eine Liste von Umgebungsvariablen, die für dieses Codepaket festgelegt wurden. Umgebungsvariablen können in `ApplicationManifest.xml` außer Kraft gesetzt werden, um unterschiedliche Werte für verschiedene Dienstinstanzen bereitzustellen. 

**DataPackage** deklariert einen Ordner, der durch das **Name**-Attribut benannt wird und beliebige statische Daten enthält, die zur Laufzeit vom Prozess verarbeitet werden sollen.

**ConfigPackage** deklariert einen Ordner, der durch das **Name**-Attribut benannt wird und die Datei *Settings.xml* enthält. Diese Einstellungsdatei enthält Abschnitte mit benutzerdefinierten Einstellungen für Schlüssel-Wert-Paare, die der Prozess zur Laufzeit einliest. Wenn sich während eines Upgrades nur die **Version** von **ConfigPackage** geändert hat, wird der ausgeführte Prozess nicht neu gestartet. Stattdessen benachrichtigt ein Rückruf den Prozess, dass sich Konfigurationseinstellungen geändert haben, sodass sie dynamisch neu geladen werden können. Hier ein Beispiel der Datei *Settings.xml* :

```xml
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

> [!NOTE]
> Ein Dienstmanifest kann mehrere Code-, Konfigurations- und Datenpakete enthalten. Diese können unabhängig voneinander mit Versionsangaben versehen werden.
> 
> 

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Resources
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application"></a>Beschreiben einer Anwendung
Das Anwendungsmanifest beschreibt deklarativ den Typ und die Version der Anwendung. Es gibt Dienstzusammensetzungs-Metadaten wie z. B. dauerhafte Namen, Partitionierungsschema, Anzahl der Instanzen/Replikationsfaktor, Richtlinie für Sicherheit/Isolation, Platzierungseinschränkungen, Konfigurationsüberschreibungen und zugehörige Diensttypen an. Zudem werden die Lastenausgleichsdomänen beschrieben, in denen die Anwendung eingefügt wird.

Somit beschreibt ein Anwendungsmanifest Elemente auf Anwendungsebene, verweist auf ein oder mehrere Dienstmanifeste und bildet so einen Anwendungstyp. Hier ein einfaches Beispiel eines Anwendungsmanifests:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ApplicationManifest
      ApplicationTypeName="MyApplicationType"
      ApplicationTypeVersion="AppManifestVersion1"
      xmlns="http://schemas.microsoft.com/2011/01/fabric"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example application manifest</Description>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyServiceManifest" ServiceManifestVersion="SvcManifestVersion1"/>
    <ConfigOverrides/>
    <EnvironmentOverrides CodePackageRef="MyCode"/>
  </ServiceManifestImport>
  <DefaultServices>
     <Service Name="MyService">
         <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
             <SingletonPartition/>
         </StatelessService>
     </Service>
  </DefaultServices>
</ApplicationManifest>
```

Wie bei Dienstmanifesten sind **Version** -Attribute unstrukturierte Zeichenfolgen, die im System nicht analysiert werden. Version-Attribute werden zudem dazu verwendet, jede Komponente für Upgrades mit Versionsangaben zu versehen.

**ServiceManifestImport** enthält Verweise auf Dienstmanifeste zum Erstellen des entsprechenden Anwendungstyps. Importierte Dienstmanifeste legen fest, welche Diensttypen innerhalb des Anwendungstyps gültig sind. In ServiceManifestImport überschreiben Sie Konfigurationswerte in „Settings.xml“ und Umgebungsvariablen in „ServiceManifest.xml“. 


**DefaultServices** deklariert Dienstinstanzen, die automatisch erstellt werden, wenn eine Anwendung mit diesem Anwendungstyp instanziiert wird. Standarddienste dienen lediglich der Benutzerfreundlichkeit und verhalten sich nach der Erstellung in jeder Hinsicht wie normale Dienste. Sie werden zusammen mit allen anderen Diensten in der Anwendungsinstanz aktualisiert und können auch entfernt werden.

> [!NOTE]
> Ein Anwendungsmanifest kann mehrere Dienstmanifestimporte und Standarddienste enthalten. Jedes Dienstmanifest kann unabhängig mit Versionsangaben versehen werden.
> 
> 

Weitere Informationen zum Verwalten verschiedener Anwendungs- und Dienstparameter für einzelne Umgebungen finden Sie unter [Verwalten von Anwendungsparametern für mehrere Umgebungen](service-fabric-manage-multiple-environment-app-configuration.md).

<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Application parameters
*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->



## <a name="next-steps"></a>Nächste Schritte
[Packen einer Anwendung](service-fabric-package-apps.md) und Vorbereiten der Anwendung für die Bereitstellung.

[Bereitstellen und Entfernen von Anwendungen][10] beschreibt, wie Sie mit PowerShell Anwendungsinstanzen verwalten.

[Verwalten von Anwendungsparametern für mehrere Umgebungen][11] beschreibt, wie Parameter und Umgebungsvariablen für verschiedene Anwendungsinstanzen konfiguriert werden.

[Konfigurieren von Sicherheitsrichtlinien für Ihre Anwendung][12] beschreibt, wie Sie Dienste unter Sicherheitsrichtlinien ausführen, um den Zugriff einzuschränken.

[Anwendungshostingmodelle][13] beschreibt die Beziehung zwischen Replikaten (oder Instanzen) eines bereitgestellten Diensts und dem Service-Hostprozess.

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md
[13]: service-fabric-hosting-model.md

