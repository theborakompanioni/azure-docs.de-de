---
title: "Herunterladen des Azure SDK für Java | Microsoft Docs"
description: "Erfahren Sie, wie Sie das Azure SDK für Java mit Beispielcode für Maven-Projekte herunterladen."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 4b8f8fe6-1b26-4bb4-9be9-6ae757a59e66
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm;asirveda
translationtype: Human Translation
ms.sourcegitcommit: 0933eabf99ef94f2c4b425f1435530edfde65e3f
ms.openlocfilehash: 9e55eb52797f1363cbb65050574e549c575ca5d8
ms.lasthandoff: 02/17/2017


---
# <a name="download-the-azure-sdk-for-java"></a>Herunterladen des Azure SDK für Java
Dieser Artikel enthält eine Anleitung zum Herunterladen und Installieren der Azure-Verwaltungsbibliotheken für Java.

> [!NOTE]
> Die Azure-Verwaltungsbibliotheken für Java werden unter der [Apache-Lizenz, Version 2.0][license] zur Verfügung gestellt.
>

## <a name="azure-libraries-for-java---manual-download"></a>Azure-Bibliotheken für Java – manueller Download
Klicken Sie zum manuellen Installieren der Azure-Verwaltungsbibliotheken für Java auf <http://go.microsoft.com/fwlink/?LinkId=690320>. Es wird eine ZIP-Datei heruntergeladen, die alle Bibliotheken und alle Abhängigkeiten enthält.

Nachdem Sie die ZIP-Datei auf Ihren Computer heruntergeladen haben, extrahieren Sie den Inhalt, und fügen Sie die JAR-Dateien über eine der folgenden Optionen Ihrem Projekt hinzu:

* Importieren Sie die JAR-Dateien in Ihr Java-Projekt in Eclipse oder IntelliJ.
* Konfigurieren Sie die Buildpfade für die Java-Projekte in Eclipse oder IntelliJ, damit sie den Pfad der JAR-Dateien enthalten.

> [!NOTE]
> Informationen zur Lizenz und weitere Informationen finden Sie in den Dateien „license.txt“ und „ThirdPartyNotices.txt“ in der ZIP-Datei.
>

## <a name="azure-management-libraries-for-java---building-with-maven"></a>Azure-Verwaltungsbibliotheken für Java – Erstellung mit Maven
### <a name="step-1---set-up-your-project-to-use-maven-for-build"></a>Schritt 1: Einrichten des Projekts für die Verwendung von Maven zum Erstellen
Informationen zum Erstellen von Maven-Projekten in Eclipse, die die Azure-Verwaltungsbibliotheken für Java verwenden, finden Sie im Artikel [Erste Schritte mit Azure-Verwaltungsbibliotheken für Java][maven-getting-started].

### <a name="step-2---configure-your-maven-settings-with-the-requisite-dependencies"></a>Schritt 2: Konfigurieren Ihrer Maven-Einstellungen mit den erforderlichen Abhängigkeiten
Nachdem das Projekt für die Verwendung von Maven für die Erstellung konfiguriert wurde, können Sie Ihrer Datei „pom.xml“ die erforderlichen Abhängigkeiten mit einer Syntax wie im folgenden Beispiel hinzufügen. Beachten Sie, dass Sie nicht jede im folgenden Beispiel aufgeführte Abhängigkeit hinzufügen müssen. Sie müssen nur die spezifischen Abhängigkeiten hinzufügen, die für Ihr Projekt erforderlich sind.

> [!NOTE]
> Ersetzen Sie in jedem `<version>`-Element im folgenden Beispiel den Platzhalter „n.n.n“ durch gültige Versionsnummern, die Sie im [Azure-Bibliothekenrepository auf Maven] erhalten.
>
>

    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-compute</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-network</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-sql</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-storage</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-websites</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-svc-mgmt-media</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-servicebus</artifactId>
        <version>n.n.n</version>
    </dependency>
    <dependency>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-serviceruntime</artifactId>
        <version>n.n.n</version>
    </dependency>

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zu den Azure-Toolkits für Java-IDEs finden Sie unter den folgenden Links:

* [Azure-Toolkit für Eclipse]
  * [Installation des Azure Toolkit für Eclipse]
  * [Erstellen einer „Hello World“-Web-App für Azure in Eclipse]
  * [Neuerungen im Azure-Toolkit für Eclipse]
* [Azure Toolkit für IntelliJ]
  * [Installieren des Azure Toolkit für IntelliJ]
  * [Erstellen einer „Hello World“-Web-App für Azure in IntelliJ]
  * [Neuerungen im Azure-Toolkit für IntelliJ]

Weitere Informationen zum Verwenden von Azure mit Java finden Sie im [Azure Java Developer Center].

> [!NOTE]
> Ausführliche Informationen zum Einrichten des Buildpfads in Eclipse finden Sie im Artikel [Java Build Path] (in englischer Sprache) auf der Eclipse-Website.
>

<!-- URL List -->

[Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit für IntelliJ]: ./azure-toolkit-for-intellij.md
[Erstellen einer „Hello World“-Web-App für Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Erstellen einer „Hello World“-Web-App für Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installation des Azure Toolkit für Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installieren des Azure Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Neuerungen im Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Neuerungen im Azure-Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure-Bibliothekenrepository auf Maven]: http://go.microsoft.com/fwlink/?LinkID=286274
[Java Build Path]: http://help.eclipse.org/luna/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Freference%2Fref-properties-build-path.htm
[license]: http://www.apache.org/licenses/LICENSE-2.0.html
[maven-getting-started]: http://go.microsoft.com/fwlink/?LinkID=622998

