---
title: "Bereitstellung einer Azure Service Fabric-Anwendung für einen Partycluster | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie eine Anwendung für einen Partycluster bereitstellen können."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: mikhegn
ms.translationtype: HT
ms.sourcegitcommit: 14915593f7bfce70d7bf692a15d11f02d107706b
ms.openlocfilehash: c0546fd5b1398759ef98afa267146ced8a4084da
ms.contentlocale: de-de
ms.lasthandoff: 08/10/2017

---

# <a name="deploy-an-application-to-a-party-cluster-in-azure"></a>Bereitstellen einer Anwendung für einen Partycluster in Azure
Dieses Tutorial ist der zweite Teil einer Serie und zeigt, wie Sie eine Azure Service Fabric-Anwendung für einen Partycluster in Azure bereitstellen.

Im zweiten Teil der Tutorialserie lernen Sie Folgendes:
> [!div class="checklist"]
> * Bereitstellung einer Anwendung in einem Remotecluster mithilfe von Visual Studio
> * Entfernen einer Anwendung aus einem Cluster mithilfe von Service Fabric Explorer

In dieser Tutorialserie lernen Sie Folgendes:
> [!div class="checklist"]
> * [Erstellen einer .NET Service Fabric-Anwendung](service-fabric-tutorial-create-dotnet-app.md)
> * Bereitstellen der Anwendung in einem Remotecluster
> * [Konfigurieren von CI/CD mit Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

## <a name="prerequisites"></a>Voraussetzungen
Bevor Sie mit diesem Tutorial beginnen können, müssen Sie Folgendes tun:
- Wenn Sie kein Azure-Abonnement besitzen, erstellen Sie ein [kostenloses Konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- [Installieren Sie Visual Studio 2017](https://www.visualstudio.com/) und die Workloads **Azure-Entwicklung** und **ASP.NET und Webentwicklung**.
- [Installieren Sie das Service Fabric SDK](service-fabric-get-started.md).

## <a name="set-up-a-party-cluster"></a>Einrichten eines Partyclusters
Partycluster sind kostenlose, zeitlich begrenzte Service Fabric-Cluster, die in Azure gehostet und vom Service Fabric-Team ausgeführt werden, in denen jeder Benutzer Anwendungen bereitstellen und mehr über die Plattform erfahren kann. Kostenlos!

Um Zugriff auf einen Partycluster zu erhalten, navigieren Sie zu dieser Website: http://aka.ms/tryservicefabric und befolgen Sie die Anweisungen. Sie benötigen ein Facebook- oder GitHub-Konto für den Zugriff auf einen Partycluster.

> [!NOTE]
> Partycluster werden nicht gesichert, sodass Ihre Anwendungen und Daten, die Sie in diesem abgelegt haben, möglicherweise für andere Personen sichtbar sind. Stellen Sie nichts bereit, das nicht von anderen gesehen werden soll. Lesen Sie die Nutzungsbedingungen für alle weiteren Details.

## <a name="make-your-application-ready-for-deployment"></a>Bereiten Sie Ihre Anwendung für die Bereitstellung vor
Da unser ASP.NET Core Web-API-Dienst als Front-End für diese Anwendung fungiert und externen Datenverkehr akzeptiert, möchten wir diesen Dienst an einen festen und bekannten Port binden. Angabe des Ports in den Diensten in der **ServiceManifest.xml**-Datei.

1. Öffnen Sie **WebAPIFrontEnd->PackageRoot->ServiceManifest.xml** im Projektmappen-Explorer.
2. Ändern Sie das Attribut **Port** des vorhandenen Elements **Endpoint** in **80**, und speichern Sie die Änderungen.

## <a name="deploy-the-app-to-the-azure"></a>Bereitstellen der Anwendung in Azure
Nachdem die Anwendung nun bereit ist, können Sie sie direkt aus Visual Studio für den Partycluster bereitstellen.

1. Rechtsklicken Sie auf **MyApplication** im Projektmappen-Explorer, und klicken Sie auf **Veröffentlichen**.

    ![Dialogfeld „Veröffentlichen“](./media/service-fabric-tutorial-deploy-app-to-party-cluster/publish-app.png)

2. Tragen Sie den Verbindungsendpunkt des Partyclusters in das Feld **Verbindungendspunkt** ein, und klicken Sie auf **Veröffentlichen**.

    Sobald die Veröffentlichung abgeschlossen ist, sollten Sie über den Browser eine Anforderung an die Anwendung senden können.

3. Öffnen Sie Ihren bevorzugten Browser, geben Sie die Clusteradresse ein (den Verbindungsendpunkt ohne die Portinformationen – z.B. win1kw5649s.westus.cloudapp.azure.com), und fügen Sie der URL `/api/values` hinzu.

    Ihnen sollte nun das gleiche Ergebnis angezeigt werden wie bei der lokalen Ausführung der Anwendung.

    ![API-Antwort aus dem Cluster](./media/service-fabric-tutorial-deploy-app-to-party-cluster/response-from-cluster.png)

## <a name="remove-the-application-from-a-cluster-using-service-fabric-explorer"></a>Entfernen einer Anwendung aus einem Cluster mithilfe von Service Fabric Explorer
Service Fabric Explorer ist eine grafische Benutzeroberfläche, um Ihre Anwendungen in einem Service Fabric Cluster zu durchsuchen und zu verwalten.

Eine bereitgestellte Anwendung aus dem Partycluster entfernen:

1. Navigieren Sie über den von der Party Cluster-Anmeldeseite bereitgestellten Link zu Service Fabric Explorer. Beispielsweise: http://win1kw5649s.westus.cloudapp.azure.com:19080/Explorer/index.html.

2. Navigieren Sie in Service Fabric Explorer zum Knoten **Fabric://MyApplication** in der Strukturansicht auf der linken Seite.

3. Klicken Sie auf der rechten Seite des Bereichs **Zusammenfassung** auf die Schaltfläche **Aktion**, und klicken Sie auf **Anwendung löschen**. Bestätigen Sie die Löschung der Anwendungsinstanz, um die Instanz Ihrer Anwendung zu entfernen, die im Cluster ausgeführt wird.

![Anwendung in Service Fabric Explorer löschen](./media/service-fabric-tutorial-deploy-app-to-party-cluster/delete-application.png)

Anwendungen werden als Anwendungstypen in einem Service Fabric-Cluster bereitgestellt. Dadurch können Sie mehrere Instanzen und Versionen der Anwendung innerhalb des Clusters ausführen. Nachdem Sie die ausgeführte Instanz der Anwendung entfernt haben, kann auch der Typ entfernt werden, um die Bereinigung der Bereitstellung abzuschließen.

Weitere Informationen zum Anwendungsmodell in Service Fabric finden Sie unter [Modellieren von Anwendungen in Service Fabric](service-fabric-application-model.md).

1. Navigieren Sie zum Knoten **MyApplicationType** in der Strukturansicht.

2. Klicken Sie auf der rechten Seite des Bereichs **Zusammenfassung** auf die Schaltfläche **Aktion**, und klicken Sie auf **Unprovision Type** (Bereitstellung des Typs aufheben). Bestätigen Sie die Aufhebung der Bereitstellung des Anwendungstyps.

![Aufhebung der Bereitstellung des Anwendungstyps in Service Fabric Explorer](./media/service-fabric-tutorial-deploy-app-to-party-cluster/unprovision-type.png)

Damit ist das Lernprogramm beendet.

## <a name="next-steps"></a>Nächste Schritte
In diesem Tutorial haben Sie Folgendes gelernt:

> [!div class="checklist"]
> * Bereitstellung einer Anwendung in einem Remotecluster mithilfe von Visual Studio
> * Entfernen einer Anwendung aus einem Cluster mithilfe von Service Fabric Explorer

Fahren Sie mit dem nächsten Tutorial fort:
> [!div class="nextstepaction"]
> [Einrichten der Continuous Integration in Visual Studio Team Services](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
