<properties
   pageTitle="Azure Service Fabric unter Linux | Microsoft Azure"
   description="Service Fabric-Cluster unterstützen Linux und Java, d.h. Sie können in Java bzw. unter Linux geschriebene Anwendungen bereitstellen und hosten."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="Java"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="05/13/2016"
   ms.author="SubramaR"/>

# Service Fabric unter Linux

Service Fabric ist derzeit als eingeschränkte Vorschauversion unter Linux verfügbar. Dies ermöglicht es Ihnen, hoch verfügbare und skalierbare Anwendungen in dieser Umgebung (genauso wie unter Windows) zu erstellen, bereitzustellen und zu verwalten. Darüber hinaus können nun die Service Fabric-Frameworks für anspruchsvolle Aufgaben (Reliable Services und Reliable Actors) in Java erstellt werden.

## Unterstützte Betriebssysteme und Programmiersprachen

Die eingeschränkte Vorschauversion unterstützt das Erstellen von One-Box-Entwicklungsclustern sowie Clustern mit mehreren Computern in Azure, auf denen Ubuntu Server 15.10 ausgeführt wird.

Sie können [ausführbare Gastdienste](service-fabric-deploy-existing-app.md) mit jeder beliebigen Sprache und jedem beliebigen Framework erstellen. Sie können Java auch zum Erstellen von Diensten basierend auf den Reliable Services- und Reliable Actor-Frameworks verwenden.

>[AZURE.NOTE] Reliable Collections werden in Java noch nicht unterstützt.

## Am Vorschauprogramm teilnehmen

Wenn Sie Interesse daran haben, am Vorschauprogramm teilzunehmen, [füllen Sie bitte die Umfrage aus](http://aka.ms/sflinuxsurvey), damit wir Ihr Szenario und Ihre Voraussetzungen besser verstehen können. Die Vorschauversion wird zu Beginn sehr klein sein und mit der Zeit erweitert werden.

Bitte beachten Sie, dass Service Fabric unter Linux im Prinzip der verfügbaren Version unter Windows entspricht (außer bezüglich Betriebssystemeigenschaften und des Supports von Programmiersprachen). Der Großteil unserer [vorhandenen Dokumentation](http://aka.ms/servicefabricdocs) gilt also auch unter Linux und hilft Ihnen dabei, sich mit der Technologie vertraut zu machen.

## Nächste Schritte

Einführung in die Programmierumgebung von [Reliable Actors](service-fabric-reliable-actors-introduction.md) und [Reliable Services](service-fabric-reliable-services-introduction.md).

<!---HONumber=AcomDC_0518_2016-->