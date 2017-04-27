---
title: Exportieren der DocumentDB-Emulatorzertifikate | Microsoft-Dokumentation
description: "Wenn Sie in Sprachen und Laufzeiten entwickeln, die nicht den Windows-Zertifikatspeicher verwenden, müssen Sie die SSL-Zertifikate exportieren und verwalten. Dieser Artikel bietet Schrittanleitungen dafür."
services: documentdb
documentationcenter: 
keywords: DocumentDB-Emulator
author: voellm
manager: jhubbard
editor: 
ms.assetid: ef43deda-c2e9-4193-99e2-7f6a88a0319f
ms.service: documentdb
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/22/2017
ms.author: tvoellm
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 094729399070a64abc1aa05a9f585a0782142cbf
ms.openlocfilehash: 5f2fd8810041d47be2ef0b21e1487426a643d541
ms.lasthandoff: 03/07/2017


---

# <a name="export-the-documentdb-emulator-certificates-for-use-with-java-python-and-nodejs"></a>Exportieren Sie die DocumentDB-Emulatorzertifikate für die Verwendung mit Java, Python und Node.js

[**Emulator herunterladen**](https://aka.ms/documentdb-emulator)

Der Azure DocumentDB-Emulator bietet eine lokale Umgebung, die zu Entwicklungszwecken den Azure DocumentDB-Dienst einschließlich der Verwendung von SSL-Verbindungen emuliert. Dieser Artikel zeigt, wie Sie die SSL-Zertifikate exportieren, um sie in Sprachen und Laufzeiten zu verwenden, die nicht im Windows-Zertifikatspeicher integriert sind. Beispiele sind die Sprachen Java, die einen eigenen [Zertifikatspeicher](https://docs.oracle.com/cd/E19830-01/819-4712/ablqw/index.html) verwendet, Python, die [Wrapper für Socketobjekte](https://docs.python.org/2/library/ssl.html) verwendet, und Node.js, die [tlsSocket](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback) verwendet. Weitere Informationen zum Emulator finden Sie unter [Verwenden des Azure DocumentDB-Emulators für Entwicklungs- und Testzwecke](./documentdb-nosql-local-emulator.md).

## <a name="certification-rotation"></a>Rotierende Zertifizierung

Die Zertifikate im lokalen DocumentDB-Emulator werden bei der ersten Ausführung des Emulators generiert. Es gibt zwei Zertifikate. Das eine Zertifikat dient zum Herstellen der Verbindung mit dem lokalen Emulator, das andere zum Verwalten der Geheimnisse innerhalb des Emulators. Das Zertifikat, das Sie exportieren, ist das Verbindungszertifikat mit dem Anzeigenamen „DocumentDBEmulatorCertificate“.

Sie können beide Zertifikate erneut generieren, indem Sie im DocumentDB-Emulator, der in der Windows-Taskleiste ausgeführt wird, auf **Daten zurücksetzen** klicken (siehe Abbildung unten). Wenn Sie die Zertifikate, die Sie erneut generieren, im Java-Zertifikatspeicher installiert oder an anderer Stelle verwendet haben, müssen Sie diese aktualisieren. Andernfalls kann Ihre Anwendung keine Verbindung mehr mit dem lokalen Emulator herstellen.

![Zurücksetzen von Daten im lokalen DocumentDB-Emulator](./media/documentdb-nosql-local-emulator/azure-documentdb-database-local-emulator-reset-data.png)

## <a name="how-to-export-the-documentdb-ssl-certificate"></a>Exportieren des DocumentDB-SSL-Zertifikats

1. Starten Sie den Windows-Zertifikat-Manager, indem Sie „certlm.msc“ ausführen. Navigieren Sie zum Ordner „Persönlich->Zertifikate“, und öffnen Sie das Zertifikat mit dem Anzeigenamen **DocumentDBEmulatorCertificate**.

    ![Exportieren des lokalen DocumentDB-Emulators, Schritt 1](./media/documentdb-nosql-local-emulator/azure-documentdb-database-local-emulator-export-step-1.png)

2. Klicken Sie nacheinander auf **Details** und **OK**.

    ![Exportieren des lokalen DocumentDB-Emulators, Schritt 2](./media/documentdb-nosql-local-emulator/azure-documentdb-database-local-emulator-export-step-2.png)

3. Klicken Sie auf **In Datei kopieren...**.

    ![Exportieren des lokalen DocumentDB-Emulators, Schritt 3](./media/documentdb-nosql-local-emulator/azure-documentdb-database-local-emulator-export-step-3.png)

4. Klicken Sie auf **Weiter**.

    ![Exportieren des lokalen DocumentDB-Emulators, Schritt 4](./media/documentdb-nosql-local-emulator/azure-documentdb-database-local-emulator-export-step-4.png)

5. Klicken Sie auf **Nein, privaten Schlüssel nicht exportieren** und dann auf **Weiter**.

    ![Exportieren des lokalen DocumentDB-Emulators, Schritt 5](./media/documentdb-nosql-local-emulator/azure-documentdb-database-local-emulator-export-step-5.png)

6. Klicken Sie auf **Base-64-codiert X.509 (.CER)** und dann auf **Weiter**.

    ![Exportieren des lokalen DocumentDB-Emulators, Schritt 6](./media/documentdb-nosql-local-emulator/azure-documentdb-database-local-emulator-export-step-6.png)

7. Benennen Sie das Zertifikat. Wählen Sie in diesem Fall den Namen **documentdbemulatorcert**, und klicken Sie auf **Weiter**.

    ![Exportieren des lokalen DocumentDB-Emulators, Schritt 7](./media/documentdb-nosql-local-emulator/azure-documentdb-database-local-emulator-export-step-7.png)

8. Klicken Sie auf **Fertig stellen**.

    ![Exportieren des lokalen DocumentDB-Emulators, Schritt 8](./media/documentdb-nosql-local-emulator/azure-documentdb-database-local-emulator-export-step-8.png)

## <a name="how-to-use-the-certificate-in-java"></a>Verwenden des Zertifikats in Java

Wenn Sie Java-Anwendungen oder MongoDB-Anwendungen ausführen, die den Java-Client verwenden, ist es einfacher, das Zertifikat im standardmäßigen Java-Zertifikatspeicher zu installieren als die Flags "-Djavax.net.ssl.trustStore=<keystore> -Djavax.net.ssl.trustStorePassword="<password>" zu übergeben. Die enthaltene [Java-Demoanwendung](https://localhost:8081/_explorer/index.html) beispielsweise ist vom Standardzertifikatspeicher abhängig.

Befolgen Sie die Anweisungen unter [Hinzufügen eines Zertifikats zum Java CA-Zertifikatspeicher](https://docs.microsoft.com/en-us/azure/java-add-certificate-ca-store), um das X.509-Zertifikat in den standardmäßigen Java-Zertifikatspeicher zu importieren. Denken Sie daran, dass Sie im Verzeichnis „%JAVA_HOME%“ arbeiten, wenn Sie „keytool“ ausführen.

Sobald das SSL-Zertifikat „DocumentDBEmulatorCertificate“ installiert ist, sollte Ihre Anwendung eine Verbindung mit dem lokalen DocumentDB-Emulator herstellen und diesen verwenden können. Wenn Probleme auftreten, finden Sie möglicherweise eine Lösung im Dokument [Debugging SSL/TLS Connections](http://docs.oracle.com/javase/7/docs/technotes/guides/security/jsse/ReadDebug.html) (Debuggen von SSL-/TLS-Verbindungen). Es ist sehr wahrscheinlich, dass das Zertifikat nicht im Speicher „%JAVA_HOME%/jre/lib/security/cacerts“ installiert ist. Wenn Sie beispielsweise mehrere Java-Versionen installiert haben, verwendet Ihre Anwendung möglicherweise einen anderen cacerts-Speicher als den, den Sie aktualisiert haben.

## <a name="how-to-use-the-certificate-in-python"></a>Verwenden des Zertifikats in Python

Das [Python SDK (ab Version 2.0.0)](https://docs.microsoft.com/en-us/azure/documentdb/documentdb-sdk-python) für DocumentDB verwendet beim Herstellen einer Verbindung mit dem lokalen Emulator standardmäßig nicht das SSL-Zertifikat. Wenn Sie die SSL-Validierung verwenden möchten, befolgen Sie die Beispiele in der Dokumentation zu [Python-Wrappern für Socketobjekte](https://docs.python.org/2/library/ssl.html).

## <a name="how-to-use-the-certificate-in-nodejs"></a>Verwenden des Zertifikats in Node.js

Das [Node.js SDK (ab Version 1.10.1)](https://docs.microsoft.com/en-us/azure/documentdb/documentdb-sdk-node) für DocumentDB verwendet beim Herstellen einer Verbindung mit dem lokalen Emulator standardmäßig nicht das SSL-Zertifikat. Wenn Sie die SSL-Validierung verwenden möchten, befolgen Sie die Beispiele in der [Node.js](https://nodejs.org/api/tls.html#tls_tls_connect_options_callback)-Dokumentation.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zu DocumentDB finden Sie unter [Einführung in Azure DocumentDB](documentdb-introduction.md).
* Um mit dem Entwickeln mithilfe des DocumentDB-Emulators zu beginnen, laden Sie eins der [unterstützten DocumentDB SDKs](documentdb-sdk-dotnet.md) herunter.

