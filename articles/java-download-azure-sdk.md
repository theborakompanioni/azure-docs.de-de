---
title: "Herunterladen des Azure SDK für Java"
description: "Erfahren Sie, wie Sie das Azure SDK für Java herunterladen, mit Beispielcode für Maven-Projekte und grundlegenden Installationsschritten für das Azure-Toolkit für Eclipse."
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
ms.date: 12/22/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: 9b2d456d8dba33af224ea147f5f8ec49ba7397f9
ms.openlocfilehash: 8d4042a270af2ba143b45d23c3f0c01f836c4592


---
# <a name="download-the-azure-sdk-for-java"></a>Herunterladen des Azure SDK für Java
Dieser Artikel enthält eine Anleitung zum Herunterladen und Installieren der Azure-Bibliotheken für Java.

**Hinweis:** Die Azure-Bibliotheken für Java werden unter der [Apache-Lizenz, Version 2.0][license] zur Verfügung gestellt.

## <a name="azure-libraries-for-java---manual-download"></a>Azure-Bibliotheken für Java – manueller Download
Klicken Sie zum manuellen Installieren der Azure-Bibliotheken für Java auf <http://go.microsoft.com/fwlink/?LinkId=690320>. Es wird eine ZIP-Datei heruntergeladen, die alle Bibliotheken und alle Abhängigkeiten enthält.

Nachdem Sie die ZIP-Datei auf Ihren Computer heruntergeladen haben, extrahieren Sie den Inhalt, und fügen Sie die JAR-Dateien über eine der folgenden Optionen Ihrem Projekt hinzu:

* Importieren Sie die JAR-Dateien in Ihr Java-Projekt in Eclipse.
* Konfigurieren Sie den **Build Path** für das Java-Projekt in Eclipse, sodass er den Pfad der JAR-Dateien enthält.

Ausführliche Informationen zum Einrichten des Buildpfads in Eclipse finden Sie im Artikel [Java Build Path] (in englischer Sprache) auf der Eclipse-Website.

**Hinweis:** Informationen zur Lizenz und weitere Informationen finden Sie in den Dateien „license.txt“ und „ThirdPartyNotices.txt“ in der ZIP-Datei.

## <a name="azure-libraries-for-java---building-with-maven"></a>Azure-Bibliotheken für Java – Erstellung mit Maven
### <a name="step-1---set-up-your-project-to-use-maven-for-build"></a>Schritt 1: Einrichten des Projekts für die Verwendung von Maven zum Erstellen
Informationen zum Erstellen von Maven-Projekten in Eclipse, die die Azure-Bibliotheken für Java verwenden, finden Sie im Artikel [Erste Schritte mit Azure-Verwaltungsbibliotheken für Java][maven-getting-started].

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

## <a name="installing-the-azure-toolkit-for-eclipse"></a>Installieren des Azure-Toolkits für Eclipse
Dieser Abschnitt enthält grundlegende Informationen zum Installieren des Azure-Toolkits für Eclipse. Weitere Informationen finden Sie unter[ Installing the Azure Toolkit for Eclipse] (in englischer Sprache).

### <a name="prerequisites"></a>Voraussetzungen
1. Windows-Betriebssystem, wie im Artikel [Neuigkeiten im Azure-Toolkit für Eclipse] aufgeführt.
2. Macintosh- oder Linux-Betriebssystem, wie im Artikel [Neuigkeiten im Azure-Toolkit für Eclipse] aufgeführt.
3. Eclipse IDE für Java EE-Entwickler, Indigo oder höher. Dies kann von <http://www.eclipse.org/downloads/> heruntergeladen werden.

### <a name="basic-installation-steps"></a>Grundlegende Installationsschritte
1. Wählen Sie im Menü von Eclipse **Help** und dann **Install New Software** aus.
2. Geben Sie den Websitespeicherort <http://dl.msopentech.com/eclipse ein>, und drücken Sie die **EINGABETASTE**.
3. Wählen Sie die zu installierenden Elemente aus, und klicken Sie auf **Finish**.

Im Azure-Toolkit für Eclipse wird die aktuelle Version des Azure SDK verwendet. Diese können Sie mithilfe des Webplattform-Installer (WebPI) unter <http://go.microsoft.com/fwlink/?LinkID=252838> herunterladen. Falls Sie diesen jedoch noch nicht installiert haben, wenn Sie Ihr erstes Azure-Bereitstellungsprojekt erstellen, installiert das Azure-Toolkit für Eclipse automatisch die entsprechende Version von Azure SDK.

## <a name="see-also"></a>Weitere Informationen
[Azure-Toolkit für Eclipse]

[ Installing the Azure Toolkit for Eclipse]

[Creating a Hello World Application for Azure in Eclipse (in englischer Sprache)]

Weitere Informationen zum Verwenden von Azure mit Java finden Sie im [Azure Java Developer Center](in englischer Sprache).

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure-Bibliothekenrepository auf Maven]: http://go.microsoft.com/fwlink/?LinkID=286274
[Azure-Toolkit für Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse (in englischer Sprache)]: http://go.microsoft.com/fwlink/?LinkID=699533
[ Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Java Build Path]: http://help.eclipse.org/luna/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2Freference%2Fref-properties-build-path.htm
[license]: http://www.apache.org/licenses/LICENSE-2.0.html
[maven-getting-started]: http://go.microsoft.com/fwlink/?LinkID=622998
[zip-download]: http://go.microsoft.com/fwlink/?LinkId=690320
[Neuigkeiten im Azure-Toolkit für Eclipse]: http://go.microsoft.com/fwlink/?LinkId=690333



<!--HONumber=Dec16_HO2-->


