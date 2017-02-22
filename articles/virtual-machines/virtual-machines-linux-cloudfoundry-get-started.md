---
title: Erste Schritte mit Cloud Foundry in Microsoft Azure | Microsoft-Dokumentation
description: "Ausführen von OSS oder Pivotal Cloud Foundry in Microsoft Azure"
services: virtual-machines-linux
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 2a15ffbf-9f86-41e4-b75b-eb44c1a2a7ab
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 01/19/2017
ms.author: seanmck
translationtype: Human Translation
ms.sourcegitcommit: 89117c59ae8948eeb32b51a0ecd4fdf0a3a2edd7
ms.openlocfilehash: d026d6d901374a91864edf9f6ee38d5450e7f179

---

# <a name="cloud-foundry-on-azure"></a>Cloud Foundry in Azure

Foundry Cloud ist eine Open-Source-PaaS (Platform-as-a-Service) für die Erstellung, Bereitstellung und den Betrieb von 12-Faktor-Anwendungen, die in einer Vielzahl von Sprachen und Frameworks entwickelt werden. In diesem Dokument sind die verfügbaren Optionen zum Ausführen von Cloud Foundry in Azure sowie die ersten Schritte beschrieben.

## <a name="cloud-foundry-offerings"></a>Cloud Foundry-Angebote

Für die Ausführung in Azure sind zwei Arten von Cloud Foundry verfügbar: Open Source-Cloud Foundry (OSS CF) und Pivotal Cloud Foundry (PCF). OSS CF ist eine vollständig auf [Open Source](https://github.com/cloudfoundry) basierende Version von Cloud Foundry, die von der Cloud Foundry Foundation verwaltet wird. Pivotal Cloud Foundry ist eine Unternehmensdistribution von Cloud Foundry der Pivotal Software Inc. Wir sehen uns einige Unterschiede zwischen den beiden Angeboten an.

### <a name="open-source-cloud-foundry"></a>Open Source Cloud Foundry

Sie können OSS Cloud Foundry in Azure bereitstellen, indem Sie zuerst einen BOSH-Director und anschließend Cloud Foundry gemäß den [Anweisungen auf GitHub](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/blob/master/docs/guidance.md) bereitstellen. Weitere Informationen zur Verwendung von OSS CF finden Sie in der [Dokumentation](https://docs.cloudfoundry.org/) der Cloud Foundry Foundation.

Microsoft bietet über die folgenden Community-Kanäle Support für OSS CF nach dem Best-Effort-Prinzip:

- #<a name="bosh-azure-cpi-channel-on-cloud-foundry-slackhttpsslackcloudfoundryorg"></a>Kanal „bosh-azure-cpi“ auf [Cloud Foundry Slack](https://slack.cloudfoundry.org/)
- [Mailingliste „cf-bosh“](https://lists.cloudfoundry.org/pipermail/cf-bosh)
- GitHub-Ausgaben für [CPI](https://github.com/cloudfoundry-incubator/bosh-azure-cpi-release/issues) und [Service Broker](https://github.com/Azure/meta-azure-service-broker/issues)

>[!NOTE]
> Die Supportebene für Ihre Azure-Ressourcen wie die virtuellen Computer, auf denen Sie Cloud Foundry ausführen, basiert auf Ihrer Azure-Supportvereinbarung. Der Community-Support nach dem Best-Effort-Prinzip bezieht sich nur auf die Cloud Foundry-spezifischen Komponenten.

### <a name="pivotal-cloud-foundry"></a>Pivotal Cloud Foundry

Pivotal Cloud Foundry enthält neben der gleichen Kernplattform wie die OSS-Distribution auch einen Satz von proprietären Verwaltungstools und Supportleistungen auf Unternehmensniveau. Für die Ausführung von PCF in Azure müssen Sie eine Lizenz von Pivotal erwerben. Das PCF-Angebot im Azure Marketplace umfasst eine Lizenz für eine 90-Tage-Testversion.

Die Tools umfassen [Pivot Operations Manager](http://docs.pivotal.io/pivotalcf/customizing/), eine Webanwendung, die die Bereitstellung und Verwaltung von Cloud Foundry Foundation, vereinfacht und [Pivotal Apps Manager](https://docs.pivotal.io/pivotalcf/console/), eine Webanwendung für die Verwaltung von Benutzern und Anwendungen.

Zusätzlich zu den aufgeführten Supportkanälen für OSS CF sind Sie mit einer PCF-Lizenz berechtigt, sich an Pivotal zu wenden, um Support zu erhalten. Microsoft und Pivotal haben auch Supportworkflows aktiviert, mit denen Sie beide Parteien kontaktieren können, um Unterstützung zu erhalten, und Ihre Anfrage wird je nachdem, wo das Problem liegt, ordnungsgemäß weitergeleitet.

## <a name="azure-service-broker"></a>Azure Service Broker

Cloud Foundry empfiehlt [„12-Faktor-App“](https://12factor.net/)-Methodik, die eine klare Trennung des zustandslosen Anwendungsprozesses von zustandsbehafteten unterstützenden Diensten fördert. [Service Broker](https://docs.cloudfoundry.org/services/api.html) bieten eine einheitliche Methode zum Bereitstellen und Binden von unterstützenden Diensten an Anwendungen. Der [Azure Service Broker](https://github.com/Azure/meta-azure-service-broker) stellt einige der wichtigen Azure-Dienste über diesen Kanal bereit, darunter Azure-Speicher und Azure SQL.

Bei Verwendung von Pivotal Cloud Foundry ist der Service Broker auch im Pivotal-Netzwerk [als Kachel verfügbar](https://docs.pivotal.io/azure-sb/installing.html).

## <a name="related-resources"></a>Zugehörige Ressourcen

### <a name="visual-studio-team-services-plugin"></a>Visual Studio Team Services-Plug-In

Cloud Foundry eignet sich gut für agile Softwareentwicklung, einschließlich der Verwendung von Continuous Integration (CI) und Continuous Delivery (CD). Wenn Sie Ihre Projekte mithilfe von Visual Studio Team Services (VSTS) verwalten und eine CI/CD-Pipeline für Cloud Foundry einrichten möchten, können Sie die [VSTS Cloud Foundry-Builderweiterung](https://marketplace.visualstudio.com/items?itemName=ms-vsts.cloud-foundry-build-extension) verwenden. Das Plug-In vereinfacht das Konfigurieren und Automatisieren von Bereitstellungen in Cloud Foundry unabhängig davon, ob die Ausführung in Azure oder einer anderen Umgebung erfolgt.

## <a name="next-steps"></a>Nächste Schritte

- [Bereitstellen von Pivotal Cloud Foundry aus dem Azure Marketplace](https://azure.microsoft.com/en-us/marketplace/partners/pivotal/pivotal-cloud-foundryazure-pcf/)



<!--HONumber=Jan17_HO3-->


