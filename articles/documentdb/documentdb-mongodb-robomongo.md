---
title: "Verwenden von Robomongo für MongoDB mit einem Azure DocumentDB-Konto | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie Robomongo mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB verwenden (jetzt als Vorschau verfügbar)."
keywords: Robomongo
services: documentdb
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 352c5fb9-8772-4c5f-87ac-74885e63ecac
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2016
ms.author: anhoh
translationtype: Human Translation
ms.sourcegitcommit: a6aadaae2a9400dc62ab277d89d9a9657833b1b7
ms.openlocfilehash: 141154f9e8236e595f77bd4880c4f63d480da445


---
# <a name="use-robomongo-with-a-documentdb-account-with-protocol-support-for-mongodb"></a>Verwenden von Robomongo mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB
Voraussetzungen für das Herstellen einer Verbindung mit einem Azure DocumentDB-Konto mit Protokollunterstützung für MongoDB mithilfe von Robomongo:

* Herunterladen und Installieren von [Robomongo](https://robomongo.org/)
* Informationen zur [Verbindungszeichenfolge](documentdb-connect-mongodb-account.md) für das DocumentDB-Konto mit Protokollunterstützung für MongoDB

## <a name="connect-using-robomongo"></a>Herstellen einer Verbindung mithilfe von Robomongo
Führen Sie die folgenden Schritte aus, um den MongoDB-Verbindungen von Robomongo Ihr DocumentDB-Konto mit Protokollunterstützung für MongoDB hinzuzufügen.

1. Rufen Sie anhand der [hier](documentdb-connect-mongodb-account.md)bereitgestellten Anweisungen die Verbindungsinformationen für Ihr DocumentDB-Konto mit Protokollunterstützung für MongoDB ab.

    ![Screenshot des Blatts „Verbindungszeichenfolge“](./media/documentdb-mongodb-robomongo/connectionstringblade.png)
2. Führen Sie *Robomongo.exe* aus.

3. Klicken Sie unter **Datei** auf die Verbindungsschaltfläche, um Ihre Verbindungen zu verwalten. Klicken Sie anschließend im Fenster mit den **MongoDB-Verbindungen** auf **Erstellen**. Daraufhin öffnet sich das Fenster mit den **Verbindungseinstellungen**.

4. Wählen Sie im Fenster mit den **Verbindungseinstellungen** einen Namen aus. Ermitteln Sie anschließend den **Host** und den **Port** in Ihren Verbindungsinformationen aus Schritt 1, und geben Sie sie unter **Adresse** bzw. **Port** ein.

    ![Screenshot der Verbindungsverwaltung von Robomongo](./media/documentdb-mongodb-robomongo/manageconnections.png)
5. Klicken Sie auf der Registerkarte **Authentifizierung** auf **Perform authentication** (Authentifizierung durchführen). Geben Sie anschließend die Datenbank (Standardeinstellung: *Admin*) sowie **Benutzername** und **Kennwort** ein.
Sowohl **Benutzername** als auch **Kennwort** finden Sie in den Verbindungsinformationen aus Schritt 1.

    ![Screenshot der Authentifizierungsregisterkarte von Robomongo](./media/documentdb-mongodb-robomongo/authentication.png)
6. Aktivieren Sie auf der Registerkarte **SSL** das Kontrollkästchen **Use SSL protocol** (SSL-Protokoll verwenden), und ändern Sie anschließend die **Authentifizierungsmethode** in **Selbstsigniertes Zertifikat**.

    ![Screenshot der SSL-Registerkarte von Robomongo](./media/documentdb-mongodb-robomongo/SSL.png)
7. Klicken Sie abschließend auf **Test**, um sich zu vergewissern, dass die Verbindung hergestellt werden kann, und klicken Sie schließlich auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte
* Untersuchen Sie DocumentDB mit Protokollunterstützung für MongoDB anhand von [Beispielen](documentdb-mongodb-samples.md).



<!--HONumber=Jan17_HO4-->


