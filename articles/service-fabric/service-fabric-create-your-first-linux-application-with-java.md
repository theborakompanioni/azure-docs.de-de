<properties
   pageTitle="Erstellen Ihrer ersten Service Fabric-Anwendung unter Linux mithilfe von Java | Microsoft Azure"
   description="Erstellen und Bereitstellen einer Service Fabric-Anwendung mithilfe von Java"
   services="service-fabric"
   documentationCenter="java"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="java"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="seanmck"/>


# Erstellen Ihrer ersten Azure Service Fabric-Anwendung

> [AZURE.SELECTOR]
- [C#](service-fabric-create-your-first-application-in-visual-studio.md)
- [Java](service-fabric-create-your-first-linux-application-with-java.md)

Service Fabric bietet SDKs, mit denen sich Dienste unter Linux sowohl in .NET Core als auch in Java erstellen lassen. In diesem Tutorial erfahren Sie, wie Sie eine Anwendung für Linux und einen Dienst unter Verwendung von Java erstellen.

## Voraussetzungen

Vor Beginn des Tutorials müssen Sie zunächst [Ihre Linux-Entwicklungsumgebung einrichten](service-fabric-get-started-linux.md). Bei Verwendung von Mac OS X können Sie [mithilfe von Vagrant eine Linux-One-Box-Umgebung auf einem virtuellen Computer einrichten](service-fabric-get-started-mac.md).

## Erstellen der Anwendung

Eine Service Fabric-Anwendung kann einen oder mehrere Dienste enthalten, die jeweils eine bestimmte Rolle bei der Bereitstellung von Funktionen der Anwendung haben. Das Service Fabric SDK für Linux enthält ein [Yeoman](http://yeoman.io/)-Generator, mit dem Sie problemlos Ihren ersten Dienst erstellen und später weitere Dienste hinzufügen können. Im nächsten Schritt erstellen wir mithilfe von Yeoman eine neue Anwendung mit einem einzelnen Dienst.

1. Geben Sie in einem Terminal die Zeichenfolge **yo azuresfjava** ein.

2. Benennen Sie Ihre Anwendung.

3. Wählen Sie die Art Ihres ersten Diensts aus, und benennen Sie ihn. Im Rahmen dieses Tutorials wählen wir einen Reliable Actor-Dienst aus.

  ![Service Fabric-Yeoman-Generator für Java][sf-yeoman]

>[AZURE.NOTE] Weitere Informationen zu den Optionen finden Sie unter [Übersicht über die Service Fabric-Programmiermodelle](service-fabric-choose-framework.md).

## Erstellen der Anwendung

Die Yeoman-Vorlagen von Service Fabric enthalten ein Buildskript für [Gradle](https://gradle.org/). Damit können Sie die App über das Terminal erstellen.

  ```bash
  gradle
  ```

## Bereitstellen der Anwendung

Die erstellte Anwendung kann mithilfe der Azure-Befehlszeilenschnittstelle im lokalen Cluster bereitgestellt werden.

1. Stellen Sie eine Verbindung mit dem lokalen Service Fabric-Cluster her.

    ```bash
    azuresfcli servicefabric cluster connect
    ```

2. Verwenden Sie das in der Vorlage bereitgestellte Installationsskript, um das Anwendungspaket in den Imagespeicher des Clusters zu kopieren, den Anwendungstyp zu registrieren und eine Instanz der Anwendung zu erstellen.

    ```bash
    cd myapp
    ./install.sh
    ```

3. Navigieren Sie in einem Browser zu Service Fabric Explorer (http://localhost:19080/Explorer). Falls Sie Vagrant unter Mac OS X verwenden, ersetzen Sie „localhost“ durch die private IP-Adresse des virtuellen Computers.

4. Erweitern Sie den Knoten „Anwendungen“. Hier finden Sie nun einen Eintrag für Ihren Anwendungstyp und einen weiteren für die erste Instanz dieses Typs.

## Starten des Testclients und Ausführen eines Failovers

Actor-Projekte führen keine eigenständigen Aktionen durch. Sie benötigen einen anderen Dienst oder Client, der ihnen Nachrichten sendet. Die Actor-Vorlage enthält ein einfaches Testskript, das Sie für die Interaktion mit dem Actor-Dienst verwenden können.

1. Führen Sie das Skript mithilfe des watch-Hilfsprogramms aus, um die Ausgabe des Actor-Diensts zu erhalten.

    ```bash
    cd myactorsvcTestClient
    watch -n 1 ./testclient.sh
    ```

2. Suchen Sie in Service Fabric Explorer den Knoten, der das primäre Replikat für den Actor-Dienst hostet. Im folgenden Screenshot ist das „Node\_3“:

    ![Suchen des primären Replikats in Service Fabric Explorer][sfx-primary]

3. Klicken Sie auf den Knoten, den Sie im vorherigen Schritt ermittelt haben, und wählen Sie im Aktionsmenü die Option **Deaktivieren (neu starten)** aus. Dadurch wird einer der fünf Knoten in Ihrem lokalen Cluster neu gestartet und ein Failover auf eines der sekundären Replikate erzwungen, das auf einem anderen Knoten ausgeführt wird. Behalten Sie hierbei die Ausgabe des Testclients im Auge, und beachten Sie, dass sich der Zähler trotz des Failovers weiter erhöht.

## Erstellen und Bereitstellen einer Anwendung mit dem Eclipse Neon-Plug-In

Wenn Sie das Dienst-Plug-In für Eclipse Neon installiert haben, können Sie damit Java-basierte Service Fabric-Anwendungen erstellen und bereitstellen.

### Erstellen der Anwendung

Das Service Fabric-Plug-In ist im Rahmen der Erweiterbarkeit von Eclipse verfügbar.

1. Wählen Sie in Eclipse **Datei > Sonstiges > Service Fabric** aus. Daraufhin wird eine Reihe von Optionen angezeigt. Hierzu zählen auch Actors und Container.

    ![Service Fabric-Vorlagen in Eclipse][sf-eclipse-templates]

2. Wählen Sie in diesem Szenario die Option für den zustandslosen Dienst aus.

3. Sie werden aufgefordert, die Verwendung der Service Fabric-Perspektive zu bestätigen. Dadurch wird Eclipse für die Verwendung mit Service Fabric-Projekten optimiert. Wählen Sie die Option „Ja“ aus.

### Bereitstellen der Anwendung

Die Service Fabric-Vorlagen enthalten eine Reihe von Gradle-Aufgaben zum Erstellen und Bereitstellen von Anwendungen, die Sie über Eclipse auslösen können.

1. Wählen Sie **Ausführen > Run Configurations** (Ausführungskonfigurationen) aus.

2. Erweitern Sie **Gradle Project** (Gradle-Projekt), und wählen Sie **ServiceFabricDeployer** aus.

3. Klicken Sie auf **Run** (Ausführen).

Ihre App wird innerhalb weniger Augenblicke erstellt und bereitgestellt. Der Status kann über Service Fabric Explorer verfolgt werden.

## Nächste Schritte

- [Erfahren Sie mehr über Reliable Actors.](service-fabric-reliable-actors-introduction.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-yeoman.png
[sfx-primary]: ./media/service-fabric-create-your-first-linux-application-with-java/sfx-primary.png
[sf-eclipse-templates]: ./media/service-fabric-create-your-first-linux-application-with-java/sf-eclipse-templates.png

<!---HONumber=AcomDC_0928_2016-->