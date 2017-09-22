---
title: "Übersicht über Jenkins und Azure | Microsoft-Dokumentation"
description: "Verwenden Sie Azure für das Hosten des Jenkins-Builds und das Bereitstellen von Automatisierungsservern. Nutzen Sie darüber hinaus Azure Compute- und Azure Storage-Ressourcen, um die Pipelines für Continuous Integration und Continuous Deployment (CI/CD) zu erweitern."
services: jenkins
author: rloutlaw
manager: justhe
ms.service: jenkins
ms.devlang: NA
ms.topic: article
ms.workload: na
ms.date: 08/22/2017
ms.author: routlaw
ms.custom: mvc
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: 720d4c592256389f9494cb1b2be56393f4e9f03f
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---

# <a name="azure-and-jenkins"></a>Azure und Jenkins

[Jenkins](https://jenkins.io/) ist ein beliebter Open-Source-Automatisierungsserver zum Einrichten von Continuous Integration und Continuous Deployment (CI/CD) für Ihre Softwareprojekte. Sie können Ihrer Jenkins-Bereitstellung in Azure hosten oder Ihre bestehende Jenkins-Konfiguration mithilfe von Azure-Ressourcen erweitern. Außerdem sind Jenkins-Plug-Ins verfügbar, die die CI/CD Ihrer Anwendungen in Azure vereinfachen.

Dieser Artikel bietet eine Einführung in die Verwendung von Azure mit Jenkins. Sie erhalten darin Informationen zu den wichtigsten Features von Azure für Jenkins-Benutzer. Erste Schritte mit Ihrem eigenen Jenkins-Server in Azure finden Sie in unserem [Schnellstart](install-jenkins-solution-template.md).

## <a name="host-your-jenkins-servers-in-azure"></a>Hosten von Jenkins-Servern in Azure

Hosten Sie Jenkins in Azure, um Ihre Buildautomatisierung zu zentralisieren und Ihre Bereitstellung zu skalieren, wenn die Anforderungen Ihrer Softwareprojekte steigen. Sie können Jenkins in Azure wie folgt bereitstellen:
 
- Mit der [Jenkins-Projektmappenvorlage](install-jenkins-solution-template.md) in Azure Marketplace
- Mit [Azure Virtual Machines](/azure/virtual-machines/linux/overview). In unserem [Tutorial](/azure/virtual-machines/linux/tutorial-jenkins-github-docker-cicd) finden Sie Informationen zum Erstellen einer Jenkins-Instanz auf einem virtuellen Computer.
- Informationen zu einem Kubernetes-Cluster im [Azure Container Service](/azure/container-service/kubernetes/container-service-kubernetes-walkthrough) finden Sie in unserer [Anleitung](/azure/container-service/kubernetes/container-service-kubernetes-jenkin).

Überwachen und verwalten Sie Ihre Azure-Jenkins-Bereitstellung mit [Log Analytics](/azure/log-analytics/log-analytics-overview), [Operations Management Suite](/azure/operations-management-suite/operations-management-suite-overview) und der [Azure-Befehlszeilenschnittstelle](/cli/azure/overview).

## <a name="scale-your-build-automation-on-demand"></a>Skalieren der Buildautomatisierung nach Bedarf

Fügen Sie Ihrer vorhandenen Jenkins-Bereitstellung Build-Agents hinzu, um Ihre Jenkins-Buildkapazität zu skalieren, wenn sich die Anzahl von Builds und die Komplexität Ihrer Aufträge und Pipelines erhöhen. Sie können diese Build-Agents mithilfe des [Azure VM-Agents-Plug-Ins](jenkins-azure-vm-agents.md) auf virtuellen Azure-Computern ausführen. Weitere Einzelheiten finden Sie in unserem [Tutorial](/azure/jenkins/jenkins-azure-vm-agents).

Nach der Konfiguration mit einem [Azure-Dienstprinzipal](/azure/azure-resource-manager/resource-group-overview) können Jenkins-Aufträge und -Pipelines diese Anmeldeinformationen für Folgendes verwenden:

- Sicheres Speichern und Archivieren von Buildartefakten in [Azure Storage](/azure/storage/common/storage-introduction) mithilfe des [Azure Storage-Plug-Ins](https://plugins.jenkins.io/windows-azure-storage). Weitere Informationen finden Sie in der [Jenkins-Speicheranleitung](/azure/storage/common/storage-java-jenkins-continuous-integration-solution).
- Sie verwalten und konfigurieren Azure-Ressourcen mit der [Azure-Befehlszeilenschnittstelle](/azure/jenkins/execute-cli-jenkins-pipeline).

## <a name="deploy-your-code-into-azure-services"></a>Bereitstellen Ihres Codes in Azure-Diensten

Verwenden Sie Jenkins-Plug-Ins zum Bereitstellen von Anwendungen in Azure als Teil Ihrer Jenkins-CI/CD-Pipelines. Durch das Bereitstellen in [Azure App Service](/azure/app-service-web/) und [Azure Container Service](/azure/container-service/kubernetes/) können Sie Updates Ihrer Anwendungen ohne Verwalten der zugrunde liegenden Infrastruktur bereitstellen, testen und veröffentlichen.

 Plug-Ins stehen für die Bereitstellung der folgenden Dienste und Umgebungen zur Verfügung:

- [Azure-Web-App unter Linux](/azure/app-service-web/app-service-linux-intro). Informationen zum Einstieg finden Sie im [Tutorial](java-deploy-webapp-tutorial.md).
- [Azure-Web-App](/azure/app-service/app-service-web-overview). Informationen zum Einstieg finden Sie in der [Anleitung](deploy-Jenkins-app-service-plugin.md).


