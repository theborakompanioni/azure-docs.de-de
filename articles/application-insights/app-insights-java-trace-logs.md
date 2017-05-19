---
title: Untersuchen von Java-Ablaufverfolgungsprotokollen in Azure Application Insights | Microsoft Docs
description: Durchsuchen von Log4J- oder Logback-Ablaufverfolgungen in Application Insights
services: application-insights
documentationcenter: java
author: CFreemanwa
manager: carmonm
ms.assetid: fc0a9e2f-3beb-4f47-a9fe-3f86cd29d97a
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/12/2016
ms.author: cfreeman
ms.translationtype: Human Translation
ms.sourcegitcommit: 086091bb3be6659436ec8e371acbe4b8e86bfacb
ms.openlocfilehash: 3ef5f6240d7e4704118b86fb0cfee8cf502859ff
ms.contentlocale: de-de
ms.lasthandoff: 12/13/2016


---
# <a name="explore-java-trace-logs-in-application-insights"></a>Untersuchen von Java-Ablaufverfolgungsprotokollen in Application Insights
Wenn Sie für die Ablaufverfolgung Logback oder Log4J (Version&1;.2 bzw.&2;.0) verwenden, werden Ihre Ablaufverfolgungsprotokolle automatisch an Application Insights gesendet. Hier können Sie sie durchsuchen und untersuchen.

## <a name="install-the-java-sdk"></a>Installieren des Java SDK

Installieren Sie das [Application Insights SDK für Java][java], falls noch nicht geschehen.

(Wenn Sie keine HTTP-Anforderungen verfolgen möchten, können Sie den größten Teil der XML-Konfigurationsdatei weglassen, das `InstrumentationKey`-Element muss jedoch mindestens enthalten sein. Sie sollten auch `new TelemetryClient()` aufrufen, um das SDK zu initialisieren.)


## <a name="add-logging-libraries-to-your-project"></a>Hinzufügen von Protokollierungsbibliotheken zu Ihrem Projekt
*Wählen Sie die geeignete Methode für Ihr Projekt.*

#### <a name="if-youre-using-maven"></a>Wenn Sie Maven verwenden...
Wenn Ihr Projekt bereits für die Verwendung von Maven für den Buildprozess eingerichtet ist, fügen Sie einen der folgenden Codeabschnitte Ihrer Datei "pom.xml" hinzu:

Aktualisieren Sie dann die Projektabhängigkeiten, damit die Binärdateien heruntergeladen werden.

*Logback*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-logback</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J v2.&0;*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

*Log4J v1.&2;*

```XML

    <dependencies>
       <dependency>
          <groupId>com.microsoft.azure</groupId>
          <artifactId>applicationinsights-logging-log4j1_2</artifactId>
          <version>[1.0,)</version>
       </dependency>
    </dependencies>
```

#### <a name="if-youre-using-gradle"></a>Wenn Sie Gradle verwenden...
Wenn Ihr Projekt bereits für die Verwendung von Gradle für Buildprozesse eingerichtet ist, fügen Sie eine der folgenden Zeilen der Gruppe `dependencies` in der Datei "build.gradle" hinzu:

Aktualisieren Sie dann die Projektabhängigkeiten, damit die Binärdateien heruntergeladen werden.

**Logback**

```

    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-logback', version: '1.0.+'
```

**Log4J v2.&0;**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j2', version: '1.0.+'
```

**Log4J v1.&2;**

```
    compile group: 'com.microsoft.azure', name: 'applicationinsights-logging-log4j1_2', version: '1.0.+'
```

#### <a name="otherwise-"></a>Andernfalls...
Laden Sie den entsprechenden Appender herunter, extrahieren Sie ihn, und fügen Sie Ihrem Projekt die entsprechende Bibliothek hinzu:

| Protokollierungstool | Download | Bibliothek |
| --- | --- | --- |
| Logback |[SDK mit Logback-Appender](https://aka.ms/xt62a4) |applicationinsights-logging-logback |
| Log4J v2.&0; |[SDK mit Log4J-v2-Appender](https://aka.ms/qypznq) |applicationinsights-logging-log4j2 |
| Log4J v1.&2; |[SDK mit Log4J-v1.&2;-Appender](https://aka.ms/ky9cbo) |applicationinsights-logging-log4j1_2 |

## <a name="add-the-appender-to-your-logging-framework"></a>Hinzufügen des Appenders zu Ihrem Protokollierungsframework
Zum Starten von Ablaufverfolgungen führen Sie den relevanten Codeausschnitt mit der Konfigurationsdatei für Log4J oder Logback zusammen: 

*Logback*

```XML

    <appender name="aiAppender" 
      class="com.microsoft.applicationinsights.logback.ApplicationInsightsAppender">
    </appender>
    <root level="trace">
      <appender-ref ref="aiAppender" />
    </root>
```

*Log4J v2.&0;*

```XML

    <Configuration packages="com.microsoft.applicationinsights.Log4j">
      <Appenders>
        <ApplicationInsightsAppender name="aiAppender" />
      </Appenders>
      <Loggers>
        <Root level="trace">
          <AppenderRef ref="aiAppender"/>
        </Root>
      </Loggers>
    </Configuration>
```

*Log4J v1.&2;*

```XML

    <appender name="aiAppender" 
         class="com.microsoft.applicationinsights.log4j.v1_2.ApplicationInsightsAppender">
    </appender>
    <root>
      <priority value ="trace" />
      <appender-ref ref="aiAppender" />
    </root>
```

Die Application Insights-Appender können von jeder konfigurierten Protokollierung und müssen nicht unbedingt von der Stammprotokollierung referenziert werden (siehe die obigen Codebeispiele).

## <a name="explore-your-traces-in-the-application-insights-portal"></a>Untersuchen Ihrer Ablaufverfolgungen im Application Insights-Portal
Nachdem Sie das Projekt so konfiguriert haben, dass Ablaufverfolgungen an Application Insights gesendet werden, können Sie diese Ablaufverfolgungen im Application Insights-Portal auf dem Blatt [Suche][diagnostic] anzeigen und durchsuchen.

![Öffnen Sie im Application Insights-Portal das Blatt „Suche“.](./media/app-insights-java-trace-logs/10-diagnostics.png)

## <a name="next-steps"></a>Nächste Schritte
[Diagnosesuche][diagnostic]

<!--Link references-->

[diagnostic]: app-insights-diagnostic-search.md
[java]: app-insights-java-get-started.md



