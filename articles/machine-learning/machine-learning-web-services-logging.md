---
title: Protokollierung für Machine Learning-Webdienste | Microsoft Docs
description: Erfahren Sie, wie Sie die Protokollierung für Machine Learning-Webdienste aktivieren können. Die Protokollierung stellt zusätzliche Informationen zur Problembehandlung von APIs bereit.
services: machine-learning
documentationcenter: ''
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun

ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/05/2016
ms.author: raymondl;garye

---
# <a name="enable-logging-for-machine-learning-web-services"></a>Aktivieren der Protokollierung für Machine Learning-Webdienste
Dieses Dokument enthält Informationen zur Protokollierungsfunktion klassischer Webdienste. Durch Aktivieren der Protokollierung in Webdiensten erhalten Sie zusätzliche Informationen, die über die bloße Nummer und Meldung hinausgehen und Ihnen helfen können, Probleme beim Aufrufen der Machine Learning-APIs zu beheben.  

So aktivieren Sie die Protokollierung in Webdiensten im klassischen Azure-Portal   

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com/) an.
2. Klicken Sie links in der Spalte mit den Features auf **MACHINE LEARNING**.
3. Klicken Sie auf den Namen Ihres Arbeitsbereichs und dann auf **WEBDIENSTE**.
4. Klicken Sie in der Liste der Webdienste auf den Namen des Webdiensts.
5. Klicken Sie in der Liste der Endpunkte auf den Namen des Endpunkts.
6. Klicken Sie auf **KONFIGURIEREN**.
7. Legen Sie die **ABLAUFVERFOLGUNGSEBENE DER DIAGNOSE** auf *Fehler* oder *Alle* fest, und klicken Sie dann auf **SPEICHERN**.

So aktivieren Sie die Protokollierung im Azure Machine Learning Web Services-Portal

1. Melden Sie sich beim [Azure Machine Learning Web Services-Portal](https://services.azureml.net) an.
2. Klicken Sie auf „Classic Web Services“.
3. Klicken Sie in der Liste der Webdienste auf den Namen des Webdiensts.
4. Klicken Sie in der Liste der Endpunkte auf den Namen des Endpunkts.
5. Klicken Sie auf **Konfigurieren**.
6. Legen Sie **Logging** auf *Error* oder *All* fest, und klicken Sie dann auf **SAVE**.

## <a name="the-effects-of-enabling-logging"></a>Die Auswirkungen der Aktivierung der Protokollierung
Wenn die Protokollierung aktiviert ist, werden alle Diagnoseergebnisse/Fehler vom ausgewählten Endpunkt in dem Azure Storage-Konto protokolliert, das mit dem Arbeitsbereich des Benutzers verknüpft ist. Dieses Speicherkonto wird in der Dashboardansicht des klassischen Azure-Portals (am unteren Rand des Abschnitts „Auf einen Blick“) im Arbeitsbereich angezeigt.  

Die Protokolle können mit jedem der zum Durchsuchen von Azure Storage-Konten verfügbaren Tools angezeigt werden. Am einfachsten ist es, im klassischen Azure-Portal zum Speicherkonto zu navigieren und dann auf **CONTAINER** zu klicken. Sie sollten dann einen Container mit dem Namen **ml-diagnostics**sehen. Dieser Container enthält alle Diagnoseinformationen für alle Webdienst-Endpunkte für alle Arbeitsbereiche, die diesem Speicherkonto zugeordnet sind. 

## <a name="log-blob-detail-information"></a>Protokollieren von Detailinformationen zu Blobs
Jedes Blob im Container enthält die Diagnoseinformationen für genau eines der folgenden Ereignisse:

* Eine Ausführung der Batch-Execution-Methode  
* Eine Ausführung der Request-Response-Methode  
* Initialisierung des Request-Response-Containers

Der Name der einzelnen Blobs weist ein Präfix in der folgenden Form auf: 

{Arbeitsbereich-ID}-{Webdienst-ID}-{Endpunkt-ID}/{Protokolltyp}  

Dabei hat „Protokolltyp“ einen der folgenden Werte:  

* batch  
* score/requests  
* score/init  

<!--HONumber=Oct16_HO2-->


