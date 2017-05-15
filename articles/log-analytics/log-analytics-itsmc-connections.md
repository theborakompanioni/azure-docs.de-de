---
title: ITSM-Verbindungen im OMS IT Service Management-Connector | Microsoft-Dokumentation
description: "Verbinden Sie Ihre ITSM-Produkte/-Dienste mit dem IT Service Management-Connector (ITSMC) in OMS, um die ITSM-Arbeitselemente zentral zu überwachen und zu verwalten."
documentationcenter: 
author: JYOTHIRMAISURI
manager: riyazp
editor: 
ms.assetid: 8231b7ce-d67f-4237-afbf-465e2e397105
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: v-jysur
ms.translationtype: Human Translation
ms.sourcegitcommit: 54b5b8d0040dc30651a98b3f0d02f5374bf2f873
ms.openlocfilehash: 283ae010aafe309bb09f7c49248b61717cb56dd0
ms.contentlocale: de-de
ms.lasthandoff: 04/28/2017

---
# <a name="connect-itsm-productsservices-with-it-service-management-connector-preview"></a>Verbinden von ITSM-Produkten/-Diensten mit dem IT Service Management-Connector (Vorschau)
Dieser Artikel enthält Informationen zur Verbindung Ihres ITSM-Produkts/-Dienstes mit IT Service Management Connector in OMS sowie zur zentralen Verwaltung Ihrer Arbeitselemente. Weitere Informationen zum IT Service Management-Connector finden Sie unter [Übersicht](log-analytics-itsmc-overview.md).

Die folgenden Produkte/Dienste werden unterstützt:

- [System Center Service Manager](#connect-system-center-service-manager-to-it-service-management-connector-in-oms)
- [ServiceNow](#connect-servicenow-to-it-service-management-connector-in-oms)
- [Provance](#connect-provance-to-it-service-management-connector-in-oms)
- [Cherwell](#connect-cherwell-to-it-service-management-connector-in-oms)

## <a name="connect-system-center-service-manager-to-it-service-management-connector-in-oms"></a>Verbinden von System Center Service Manager mit dem IT Service Management Connector in OMS

Die folgenden Abschnitte enthalten ausführliche Informationen zum Verbinden Ihres System Center Service Manager-Produkts mit dem IT Service Manager-Connector in OMS.

### <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

- IT Service Management-Connector ist installiert.
Weitere Informationen: [Konfiguration](log-analytics-itsmc-overview.md#configuration).
- Die Service Manager-Webanwendung (Web-App) ist bereitgestellt und konfiguriert. Informationen zur Web-App finden Sie [hier](#create-and-deploy-service-manager-web-app-service).
- Hybridverbindung wurde erstellt und konfiguriert. Weitere Informationen: [Konfigurieren der Hybridverbindung](#configure-the-hybrid-connector-role).
- Unterstützte Versionen von Service Manager: 2012 R2 oder 2016.
- Benutzerrolle: [Erweiterter Operator](https://technet.microsoft.com/library/ff461054.aspx).

### <a name="connection-procedure"></a>Verbindungsverfahren

Verwenden Sie das folgende Verfahren zum Verbinden Ihrer System Center Service Manager-Instanz mit dem IT Service Management-Connector:

1. Rufen Sie **OMS** >**Einstellungen** > **Verbundene Quellen** auf.
2. Wählen Sie **ITSM-Connector**, klicken Sie auf **Neue Verbindung hinzufügen**.

    ![Service Manager ](./media/log-analytics-itsmc/itsmc-service-manager-connection.png)
3. Geben Sie die Informationen gemäß der Beschreibung in der folgenden Tabelle an, und klicken Sie auf **Speichern**, um die Verbindung zu erstellen:

> [!NOTE]
> Alle diese Parameter sind erforderlich.

| **Field** | **Beschreibung** |
| --- | --- |
| **Name**   | Geben Sie einen Namen für die System Center Service Manager-Instanz ein, die Sie mit dem IT Service Management-Connector verbinden möchten.  Diesen Namen verwenden Sie später beim Konfigurieren von Arbeitselementen in dieser Instanz/Anzeigen ausführlicher Protokollanalysen. |
| **Auswählen des Verbindungstyps**   | Wählen Sie **System Center Service Manager** aus. |
| **Server-URL**   | Geben Sie die URL der Service Manager-Web-App ein. Weitere Informationen zur Service Manager-Web-App finden Sie [hier](#create-and-deploy-service-manager-web-app-service).
| **Client-ID**   | Geben Sie die Client-ID ein, die Sie (mithilfe des automatischen Skripts) zum Authentifizieren der Web-App generiert haben. Weitere Informationen zum automatisierten Skript finden Sie [hier](log-analytics-itsmc-service-manager-script.md).|
| **Geheimer Clientschlüssel**   | Geben Sie den für diese ID generierten geheimen Clientschlüssel ein.   |
| **Datensynchronisierungsbereich**   | Wählen Sie die Service Manager-Arbeitselemente aus, die Sie über den IT Service Management-Connector synchronisieren möchten.  Diese Arbeitselemente werden in Log Analytics importiert. **Optionen:** Incidents, Änderungsanforderungen.|
| **Daten synchronisieren** | Geben Sie die Anzahl der vergangenen Tage ein, aus denen die Daten abgerufen werden sollen. **Maximaler Grenzwert**: 120 Tage. |
| **Erstellen des neuen Konfigurationselements in der ITSM-Lösung** | Wählen Sie diese Option, wenn Sie die Konfigurationselemente im ITSM-Produkt erstellen möchten. Bei Auswahl dieser Option erstellt OMS die entsprechenden CIs als Konfigurationselemente (falls keine CIs vorhanden sind) im unterstützten ITSM-System. **Standard**: deaktiviert. |

Bei erfolgreicher Verbindung und Synchronisierung:

- Ausgewählte Arbeitselemente aus Service Manager werden in OMS **Log Analytics** importiert. Die Zusammenfassung dieser Arbeitselemente können Sie auf der Kachel des IT Service Management-Connectors anzeigen.

- In der OMS können Sie Incidents über OMS-Warnungen oder über die Protokollsuche in dieser Service Manager-Instanz erstellen.

Weitere Informationen: [Erstellen von ITSM-Arbeitselementen für OMS-Warnungen](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) und [Erstellen von ITSM-Arbeitselementen aus OMS-Protokollen](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

### <a name="create-and-deploy-service-manager-web-app-service"></a>Erstellen und Bereitstellen des Service Manager-Web-App-Dienstes

Zur Verbindung der lokalen Service Manager-Instanz mit dem IT Service Management-Connector in OMS hat Microsoft in GitHub eine Service Manager-Web-App erstellt.

Führen Sie die folgenden Schritte aus, um die ITSM-Web-App für Ihre Service Manager-Instanz einzurichten:

- **Bereitstellen der Web-App**: Stellen Sie die Web-App bereit, legen Sie die Eigenschaften fest, und führen Sie die Authentifizierung über Azure AD durch. Sie können die Web-App mithilfe des [automatisierten Skripts](log-analytics-itsmc-service-manager-script.md) von Microsoft bereitstellen.
- **Konfigurieren der Hybridverbindung** - [Konfigurieren Sie diese Verbindung](#configure-the-hybrid-connection)manuell.

#### <a name="deploy-the-web-app"></a>Bereitstellen der Web-App
Verwenden Sie das automatisierte [Skript](log-analytics-itsmc-service-manager-script.md) zum Bereitstellen der Web-App, Festlegen der Eigenschaften und Authentifizierung über Azure AD.

Führen Sie das Skript nach Bereitstellung der folgenden erforderlichen Details aus:

- Details zum Azure-Abonnement
- Ressourcengruppenname
- Ort
- Details zum Service Manager-Server (Servername, Domäne, Benutzername und Kennwort)
- URL für Bereitstellung
- Websitename für Ihre Web-App
- Name des BizTalk-Diensts.

Das Skript erstellt die Web-App mit dem Namen, den Sie (zusammen mit einigen zusätzlichen Zeichenfolgen zum Sicherstellen der Eindeutigkeit) angegeben haben. Es generiert die **Web-App-URL**, **Client-ID** und das **Clientgeheimnis**.

Speichern Sie diese Werte, da Sie sie beim Erstellen einer Verbindung mit dem IT Service Management-Connector verwenden.

**Überprüfen der Web-App-Installation**

1. Navigieren Sie zum **Azure-Portal** > **Ressourcen**.
2. Wählen Sie die Web-App aus, und klicken Sie auf **Einstellungen** > **Anwendungseinstellungen**.
3. Bestätigen Sie die Informationen zur Service Manager-Instanz, die Sie zum Zeitpunkt der App-Bereitstellung über das Skript angegeben haben.

### <a name="configure-the-hybrid-connection"></a>Konfigurieren der Hybridverbindung

Verwenden Sie das folgende Verfahren, um die Hybrid-Connector-Rolle zu verbinden, die die Service Manager-Instanz mit dem IT Service Management-Connector in OMS verbindet.

1. Suchen Sie die Service Manager-Web-App unter **Azure-Ressourcen**.
2. Klicken Sie auf **Einstellungen** > **Netzwerk**.
3. Klicken Sie unter **Hybrid Connections** auf **Endpunkte der Hybridverbindung konfigurieren**.

    ![Hybridverbindung – Netzwerk](./media/log-analytics-itsmc/itsmc-hybrid-connection-networking-and-end-points.png)
4. Klicken Sie auf dem Blatt **Hybrid Connections** auf **Hybridverbindung hinzufügen**.

    ![Hybridverbindung hinzufügen](./media/log-analytics-itsmc/itsmc-new-hybrid-connection-add.png)

5. Klicken Sie auf dem Blatt **Hybridverbindungen hinzufügen** auf **Neue Hybridverbindung erstellen**.

    ![Neue Hybridverbindung](./media/log-analytics-itsmc/itsmc-create-new-hybrid-connection.png)

6. Geben Sie die folgenden Werte ein:

    - **Endpunktname**: Legen Sie einen Namen für die neue Hybridverbindung fest.
    -  **Endpunkt-Host**: FQDN des Service Manager-Verwaltungsservers.
    - **Endpunktport**: Geben Sie „5724“ ein.
    - **Servicebus-Namespace**: Verwenden Sie einen vorhandenen Service Bus-Namespace, oder erstellen Sie einen neuen.
    - **Standort:** Wählen Sie den Standort aus.
    -  **Namen**: Legen Sie einen Namen für den Service Bus fest, falls Sie ihn erstellen.

    ![Hybridverbindungswerte](./media/log-analytics-itsmc/itsmc-new-hybrid-connection-values.png)
6. Klicken Sie auf **OK**, um das Blatt **Hybridverbindung erstellen** zu schließen und mit der Erstellung der Hybridverbindung zu beginnen.

    Sobald die Hybridverbindung erstellt wurde, wird sie unter dem Blatt angezeigt.

7. Nachdem die Hybridverbindung erstellt wurde, wählen Sie die Verbindung aus, und klicken Sie auf **Ausgewählte Hybridverbindung hinzufügen**.

    ![Neue Hybridverbindung](./media/log-analytics-itsmc/itsmc-new-hybrid-connection-added.png)

#### <a name="configure-the-listener-setup"></a>Konfigurieren der Listener-Einrichtung

Verwenden Sie das folgende Verfahren, um die Listener-Einrichtung für die Hybridverbindung zu konfigurieren.

1. Klicken Sie auf dem Blatt **Hybrid Connections** auf **Download the Connection Manager** (Verbindungs-Manager herunterladen), und installieren Sie ihn auf dem Computer, auf dem die System Center Service Manager-Instanz ausgeführt wird.

    Nach Abschluss der Installation ist die Option **Hybrid Connection Manager UI** (Benutzeroberfläche des Managers für Hybridverbindungen) im **Startmenü** verfügbar.

2. Wenn Sie auf **Hybrid Connection Manager UI** (Benutzeroberfläche des Managers für Hybridverbindungen) klicken, werden Sie zur Eingabe Ihrer Azure-Anmeldeinformationen aufgefordert.

3. Melden Sie sich mit Ihren Azure-Anmeldeinformationen an, und wählen Sie Ihr Abonnement aus, in dem die Hybridverbindung erstellt wurde.

4. Klicken Sie auf **Speichern**.

Ihre Hybridverbindung wurde hergestellt.

![erfolgreiche Hybridverbindung](./media/log-analytics-itsmc/itsmc-hybrid-connection-listener-set-up-successful.png)
> [!NOTE]

> Nach dem Herstellen der Hybridverbindung überprüfen testen Sie die Verbindung, indem Sie die bereitgestellte Service Manager-Web-App aufrufen. Stellen Sie sicher, dass die Verbindung erfolgreich ist, bevor Sie versuchen, in der OMS eine Verbindung mit dem IT Service Management-Connector herzustellen.

Die folgende Abbildung zeigt die Details einer erfolgreiche Verbindung:

![Hybridverbindungstest](./media/log-analytics-itsmc/itsmc-hybrid-connection-test.png)

## <a name="connect-servicenow-to-it-service-management-connector-in-oms"></a>Verbinden von ServiceNow mit dem IT Service Management-Connector in OMS

Die folgenden Abschnitte enthalten ausführliche Informationen zum Verbinden Ihres ServiceNow-Produkts mit dem IT Service Manager-Connector in OMS.

### <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

- IT Service Management-Connector ist installiert. Weitere Informationen: [Konfiguration](log-analytics-itsmc-overview.md#configuration).
- Client-ID und geheimer Clientschlüssel für das ServiceNow-Produkt wurden generiert und sind verfügbar.  Informationen zum Generieren der Client-ID und des geheimen Clientschlüssels finden Sie unter [OAuth-Setup](http://wiki.servicenow.com/index.php?title=OAuth_Setup).
- Unterstützte ServiceNow-Versionen: Fuji, Geneva, Helsinki.
- Benutzer-App für Microsoft OMS-Integration (ServiceNow-App) ist installiert und die Benutzerrolle „Integration“ ist konfiguriert. [detaillierte Kapazitätsplanung](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.0 )
- Benutzerrolle: Benutzerrolle „Integration“. Informationen zum Erstellen der Benutzerrolle „Integration“ finden Sie [hier](#create-integration-user-role-in-servicenow-app).


### <a name="connection-procedure"></a>**Verbindungsverfahren**

Verwenden Sie das folgende Verfahren, um eine ServiceNow-Verbindung zu erstellen:

1. Rufen Sie **OMS** > **Einstellungen** > **Verbundene Quellen** auf.
2. Wählen Sie **ITSM-Connector**, klicken Sie auf **Neue Verbindung hinzufügen**.

    ![ServiceNow-Verbindung](./media/log-analytics-itsmc/itsmc-servicenow-connection.png)

3. Geben Sie die Informationen gemäß der Beschreibung in der folgenden Tabelle an, und klicken Sie auf **Speichern**, um die Verbindung zu erstellen:

> [!NOTE]
> Alle diese Parameter sind erforderlich.

| **Field** | **Beschreibung** |
| --- | --- |
| **Name**   | Geben Sie einen Namen für die ServiceNow-Instanz ein, die Sie mit dem IT Service Management-Connector verbinden möchten.  Diesen Namen verwenden Sie später in OMS beim Konfigurieren von Arbeitselementen in dieser ITSM-Instanz/Anzeigen ausführlicher Protokollanalysen. |
| **Auswählen des Verbindungstyps**   | Wählen Sie **ServiceNow** aus. |
| **Benutzername**   | Geben Sie den Integrationsbenutzernamen ein, den Sie in der ServiceNow-App zur Unterstützung der Verbindung mit dem IT Service Management-Connector erstellt haben. Weitere Informationen: [Erstellen der ServiceNow-App-Benutzerrolle](#create-integration-user-role-in-servicenow-app).|
| **Kennwort**   | Geben Sie das diesem Benutzernamen zugeordnete Kennwort ein. **Hinweis**: Benutzername und Kennwort werden nur zum Generieren von Authentifizierungstoken verwendet und werden nicht im OMS-Dienst gespeichert.  |
| **Server-URL**   | Geben Sie die URL der ServiceNow-Instanz ein, die Sie mit dem IT Service Management-Connector verbinden möchten. |
| **Client-ID**   | Geben Sie die Client-ID ein, die Sie für die OAuth2-Authentifizierung verwenden möchten, die Sie zuvor generiert haben.  Weitere Informationen zum Generieren der Client-ID und des geheimen Clientschlüssels: [OAuth-Setup](http://wiki.servicenow.com/index.php?title=OAuth_Setup). |
| **Geheimer Clientschlüssel**   | Geben Sie den für diese ID generierten geheimen Clientschlüssel ein.   |
| **Datensynchronisierungsbereich**   | Wählen Sie die ServiceNow-Arbeitselemente aus, die Sie über den IT Service Management-Connector mit OMS synchronisieren möchten.  Die ausgewählten Werte werden in Log Analytics importiert.   **Optionen:** Incidents und Änderungsanforderungen.|
| **Daten synchronisieren** | Geben Sie die Anzahl der vergangenen Tage ein, aus denen die Daten abgerufen werden sollen. **Maximaler Grenzwert**: 120 Tage. |
| **Erstellen des neuen Konfigurationselements in der ITSM-Lösung** | Wählen Sie diese Option, wenn Sie die Konfigurationselemente im ITSM-Produkt erstellen möchten. Bei Auswahl dieser Option erstellt OMS die entsprechenden CIs als Konfigurationselemente (falls keine CIs vorhanden sind) im unterstützten ITSM-System. **Standard**: deaktiviert. |


Bei erfolgreicher Verbindung und Synchronisierung:

- Ausgewählte Arbeitselemente aus der ServiceNow-Verbindung werden in OMS Log Analytics importiert.  Die Zusammenfassung dieser Arbeitselemente können Sie auf der Kachel des IT Service Management-Connectors anzeigen.
- Sie können Incidents, Warnungen und Ereignisse über OMS-Warnungen oder die Protokollsuche in dieser ServiceNow-Instanz erstellen.  


Weitere Informationen: [Erstellen von ITSM-Arbeitselementen für OMS-Warnungen](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) und [Erstellen von ITSM-Arbeitselementen aus OMS-Protokollen](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

### <a name="create-integration-user-role-in-servicenow-app"></a>Erstellen der Benutzerrolle „Integration“ in der ServiceNow-App

Gehen Sie dazu wie folgt vor:

1.    Rufen Sie den [ServiceNow Store](https://store.servicenow.com/sn_appstore_store.do#!/store/application/ab0265b2dbd53200d36cdc50cf961980/1.0.0) auf, und installieren Sie die **Benutzer-App für die ServiceNow- und Microsoft OMS-Integration** in Ihrer ServiceNow-Instanz.
2.    Nach der Installation suchen Sie in der linken Navigationsleiste der ServiceNow-Instanz nach dem Microsoft OMS-Integrator und wählen ihn aus.  
3.    Klicken Sie auf **Installation Checklist** (Installationsprüfliste).

    Der Status **Nicht abgeschlossen** wird angezeigt, wenn die Benutzerrolle noch nicht erstellt wurde.

4.    Geben Sie in den Textfeldern neben **Create integration user** (Integrationsbenutzer erstellen) den Benutzernamen für den Benutzer ein, der in OMS eine Verbindung mit dem IT Service Management-Connector herstellen kann.
5.    Geben Sie das Kennwort für diesen Benutzer ein, und klicken Sie auf **OK**.  

>[!NOTE]

> Diese Anmeldeinformationen verwenden Sie zum Herstellen der ServiceNow-Verbindung in OMS.

Der neu erstellte Benutzer wird mit den zugewiesenen Standardrollen angezeigt.

Standardrollen:
- personalize_choices
- import_transformer
-     x_mioms_microsoft.user
-     itil
-     template_editor
-     view_changer

Sobald der Benutzer erfolgreich erstellt wurde, wechselt der Status von **Check Installation Checklist** (Installationsprüfliste überprüfen) zu „Abgeschlossen“, und die Details der für die App erstellten Benutzerrolle werden aufgeführt.

> [!NOTE]

> So ermöglichen Sie einem Benutzer das Erstellen von **Warnungen** und **Ereignissen** in ServiceNow über OMS:

> - Stellen Sie sicher, dass in Ihrer ServiceNow-Instanz das Ereignisverwaltungsmodul installiert ist.

> - Fügen Sie dem Integrationsbenutzer die folgenden Rollen hinzu:
>      - evt_mgmt_integration
>      - evt_mgmt_operator  


## <a name="connect-provance-to-it-service-management-connector-in-oms"></a>Verbinden von Provance mit dem IT Service Management-Connector in OMS

Die folgenden Abschnitte enthalten ausführliche Informationen zum Verbinden Ihres Provance-Produkts mit dem IT Service Manager-Connector in OMS.

### <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

- IT Service Management-Connector ist installiert. Weitere Informationen: [Konfiguration](log-analytics-itsmc-overview.md#configuration).
- Die Provance-App muss bei Azure AD registriert sein, und die Client-ID muss verfügbar gemacht werden. Ausführliche Informationen finden Sie unter [Konfigurieren der Active Directory-Authentifizierung](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md).
- Benutzerrolle : Administrator.

### <a name="connection-procedure"></a>Verbindungsverfahren

Verwenden Sie das folgende Verfahren, um eine Provance-Verbindung zu erstellen:

1. Rufen Sie **OMS** > **Einstellungen** > **Verbundene Quellen** auf.
2. Wählen Sie **ITSM-Connector**, klicken Sie auf **Neue Verbindung hinzufügen**.  

    ![Provance-Verbindung](./media/log-analytics-itsmc/itsmc-provance-connection.png)
3. Geben Sie die Informationen gemäß der Beschreibung in der folgenden Tabelle an, und klicken Sie auf **Speichern**, um die Verbindung zu erstellen.

> [!NOTE]
> Alle diese Parameter sind erforderlich.

| **Field** | **Beschreibung** |
| --- | --- |
| **Name**   | Geben Sie einen Namen für die Provance-Instanz ein, die Sie mit dem IT Service Management-Connector verbinden möchten.  Diesen Namen verwenden Sie später in OMS beim Konfigurieren von Arbeitselementen in dieser ITSM-Instanz/Anzeigen ausführlicher Protokollanalysen. |
| **Auswählen des Verbindungstyps**   | Wählen Sie **Provance** aus. |
| **Benutzername**   | Geben Sie den Benutzernamen ein, der eine Verbindung mit dem IT Service Management-Connector herstellen kann.    |
| **Kennwort**   | Geben Sie das diesem Benutzernamen zugeordnete Kennwort ein. **Hinweis**: Benutzername und Kennwort werden nur zum Generieren von Authentifizierungstoken verwendet und werden nicht im OMS-Dienst gespeichert.|
| **Server-URL**   | Geben Sie die URL Ihrer Provance-Instanz ein, die Sie mit dem IT Service Management-Connector verbinden möchten. |
| **Client-ID**   | Geben Sie die Client-ID für die Authentifizierung dieser Verbindung ein, die Sie in Ihrer Provance-Instanz generiert haben.  Weitere Informationen zur Client-ID finden Sie unter [Konfigurieren der Active Directory-Authentifizierung](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md). |
| **Datensynchronisierungsbereich**   | Wählen Sie die Provance-Arbeitselemente aus, die Sie über den IT Service Management-Connector mit OMS synchronisieren möchten.  Diese Arbeitselemente werden in Log Analytics importiert.   **Optionen:** Incidents, Änderungsanforderungen.|
| **Daten synchronisieren** | Geben Sie die Anzahl der vergangenen Tage ein, aus denen die Daten abgerufen werden sollen. **Maximaler Grenzwert**: 120 Tage. |
| **Erstellen des neuen Konfigurationselements in der ITSM-Lösung** | Wählen Sie diese Option, wenn Sie die Konfigurationselemente im ITSM-Produkt erstellen möchten. Bei Auswahl dieser Option erstellt OMS die entsprechenden CIs als Konfigurationselemente (falls keine CIs vorhanden sind) im unterstützten ITSM-System. **Standard**: deaktiviert.|

Bei erfolgreicher Verbindung und Synchronisierung:

- Ausgewählte Arbeitselemente aus der Provance-Verbindung werden in OMS **Log Analytics** importiert.  Die Zusammenfassung dieser Arbeitselemente können Sie auf der Kachel des IT Service Management-Connectors anzeigen.
- Sie können Incidents und Ereignisse über OMS-Warnungen oder die Protokollsuche in dieser Provance-Instanz erstellen.

Weitere Informationen: [Erstellen von ITSM-Arbeitselementen für OMS-Warnungen](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) und [Erstellen von ITSM-Arbeitselementen aus OMS-Protokollen](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

## <a name="connect-cherwell-to-it-service-management-connector-in-oms"></a>Verbinden von Cherwell mit dem IT Service Management-Connector in OMS

Die folgenden Abschnitte enthalten ausführliche Informationen zum Verbinden Ihres Cherwell-Produkts mit dem IT Service Manager-Connector in OMS.

### <a name="prerequisites"></a>Voraussetzungen

Stellen Sie sicher, dass die folgenden Voraussetzungen erfüllt sind:

- IT Service Management-Connector ist installiert. Weitere Informationen: [Konfiguration](log-analytics-itsmc-overview.md#configuration).
- Client-ID wurde generiert. Weitere Informationen: [Generieren der Client-ID für Cherwell](#generate-client-id-for-cherwell).
- Benutzerrolle : Administrator.

### <a name="connection-procedure"></a>Verbindungsverfahren

Verwenden Sie das folgende Verfahren, um eine Cherwell-Verbindung zu erstellen:

1. Rufen Sie **OMS** >  **Einstellungen** > **Verbundene Quellen** auf.
2. Wählen Sie **ITSM-Connector**, klicken Sie auf **Neue Verbindung hinzufügen**.  

    ![Cherwell-Benutzer-ID](./media/log-analytics-itsmc/itsmc-cherwell-connection.png)

3. Geben Sie die Informationen gemäß der Beschreibung in der folgenden Tabelle an, und klicken Sie auf **Speichern**, um die Verbindung zu erstellen.

> [!NOTE]
> Alle diese Parameter sind erforderlich.

| **Field** | **Beschreibung** |
| --- | --- |
| **Name**   | Geben Sie einen Namen für die Cherwell-Instanz ein, die Sie mit dem IT Service Management-Connector verbinden möchten.  Diesen Namen verwenden Sie später in OMS beim Konfigurieren von Arbeitselementen in dieser ITSM-Instanz/Anzeigen ausführlicher Protokollanalysen. |
| **Auswählen des Verbindungstyps**   | Wählen Sie **Cherwell** aus. |
| **Benutzername**   | Geben Sie den Cherwell-Benutzernamen ein, der eine Verbindung mit dem IT Service Management-Connector herstellen kann. |
| **Kennwort**   | Geben Sie das diesem Benutzernamen zugeordnete Kennwort ein. **Hinweis**: Benutzername und Kennwort werden nur zum Generieren von Authentifizierungstoken verwendet und werden nicht im OMS-Dienst gespeichert.|
| **Server-URL**   | Geben Sie die URL Ihrer Cherwell-Instanz ein, die Sie mit dem IT Service Management-Connector verbinden möchten. |
| **Client-ID**   | Geben Sie die Client-ID für die Authentifizierung dieser Verbindung ein, die Sie in Ihrer Cherwell-Instanz generiert haben.   |
| **Datensynchronisierungsbereich**   | Wählen Sie die Cherwell-Arbeitselemente aus, die Sie über den IT Service Management-Connector synchronisieren möchten.  Diese Arbeitselemente werden in Log Analytics importiert.   **Optionen:** Incidents, Änderungsanforderungen. |
| **Daten synchronisieren** | Geben Sie die Anzahl der vergangenen Tage ein, aus denen die Daten abgerufen werden sollen. **Maximaler Grenzwert**: 120 Tage. |
| **Erstellen des neuen Konfigurationselements in der ITSM-Lösung** | Wählen Sie diese Option, wenn Sie die Konfigurationselemente im ITSM-Produkt erstellen möchten. Bei Auswahl dieser Option erstellt OMS die entsprechenden CIs als Konfigurationselemente (falls keine CIs vorhanden sind) im unterstützten ITSM-System. **Standard**: deaktiviert. |

Bei erfolgreicher Verbindung und Synchronisierung:

- Ausgewählte Arbeitselemente aus dieser Cherwell-Verbindung werden in OMS Log Analytics importiert. Die Zusammenfassung dieser Arbeitselemente können Sie auf der Kachel des IT Service Management-Connectors anzeigen.
- In dieser Cherwell-Instanz können Sie über OMS Incidents und Ereignisse erstellen. Weitere Informationen: „Erstellen von ITSM-Arbeitselementen für OMS-Warnungen“ und „Erstellen von ITSM-Arbeitselementen aus OMS-Protokollen“.

Weitere Informationen: [Erstellen von ITSM-Arbeitselementen für OMS-Warnungen](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts) und [Erstellen von ITSM-Arbeitselementen aus OMS-Protokollen](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs).

### <a name="generate-client-id-for-cherwell"></a>Generieren der Client-ID für Cherwell

Führen Sie folgende Schritte aus, um die Client-ID/den Schlüssel für Cherwell zu generieren:

1. Melden Sie sich als Administrator bei Ihrer Cherwell-Instanz an.
2. Klicken Sie auf **Sicherheit** > **Edit REST API client settings (REST-API-Clienteinstellungen bearbeiten)**.
3. Wählen Sie **Create new client (Neuen Client erstellen)** > **Geheimer Clientschlüssel**.

    ![Cherwell-Benutzer-ID](./media/log-analytics-itsmc/itsmc-cherwell-client-id.png)


## <a name="next-steps"></a>Nächste Schritte
 - [Erstellen von ITSM-Arbeitselementen für OMS-Warnungen](log-analytics-itsmc-overview.md#create-itsm-work-items-for-oms-alerts)

 - [Erstellen von ITSM-Arbeitselementen aus OMS-Protokollen](log-analytics-itsmc-overview.md#create-itsm-work-items-from-oms-logs)

- [Anzeigen von Log Analytics für Ihre Verbindung](log-analytics-itsmc-overview.md#using-the-solution)

