---
title: "Erstellen von .NET HDInsight-Anwendungen für die nicht interaktive Authentifizierung | Microsoft Docs"
description: "Erfahren Sie, wie Sie .NET HDInsight-Anwendungen für die nicht interaktive Authentifizierung erstellen."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 8e32430f-6404-498a-9fcd-f20338d964af
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/02/2016
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: cc59d7785975e3f9acd574b516d20cd782c22dac
ms.openlocfilehash: d4324ac080bc68af467652a9942fd3e97eb2d517


---
# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a>Erstellen von .NET HDInsight-Anwendungen für die nicht interaktive Authentifizierung 
Sie können Ihre .NET Azure HDInsight-Anwendung entweder unter der eigenen Identität der Anwendung (nicht interaktiv) oder unter der Identität des angemeldeten Benutzers der Anwendung (interaktiv) ausführen. Ein Beispiel für die interaktive Anwendung finden Sie unter [Übermitteln von Hive-/Pig-/Sqoop-Aufträgen mit dem HDInsight .NET SDK](hdinsight-submit-hadoop-jobs-programmatically.md). In diesem Artikel erfahren Sie, wie Sie eine .NET-Anwendung für die nicht interaktive Authentifizierung erstellen, um eine Verbindung mit Azure HDInsight herzustellen und einen Hive-Auftrag zu übermitteln.

Für die .NET-Anwendung benötigen Sie Folgendes:

* Mandanten-ID des Azure-Abonnements
* Client-ID der Azure Directory-Anwendung
* Geheimen Schlüssel der Azure Directory-Anwendung  

Der Hauptprozess umfasst die folgenden Schritte:

1. Erstellen Sie eine Azure Directory-Anwendung.
2. Weisen Sie der AD-Anwendung Rollen zu.
3. Entwickeln Sie Ihre Clientanwendung.

## <a name="prerequisites"></a>Voraussetzungen
* HDInsight-Cluster. Sie können einen Cluster gemäß der Anleitung im [Tutorial zu den ersten Schritten](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster)erstellen.

## <a name="create-azure-directory-application"></a>Erstellen der Azure Active Directory-Anwendung
Wenn Sie eine Active Directory-Anwendung erstellen, wird sowohl die Anwendung als auch ein Dienstprinzipal erstellt. Sie können die Anwendung unter der Identität der Anwendung ausführen.

Derzeit müssen Sie das klassische Azure-Portal zum Erstellen einer neuen Active Directory-Anwendung verwenden. Das Azure-Portal wird in einer späteren Version um diese Möglichkeit erweitert. Sie können diese Schritte auch über Azure PowerShell oder Azure CLI ausführen. Informationen zur Verwendung von PowerShell oder der CLI mit dem Dienstprinzipal finden Sie unter [Authentifizieren eines Dienstprinzipals mit Azure Resource Manager](../resource-group-authenticate-service-principal.md).

**So erstellen Sie eine Azure Directory-Anwendung**

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com/)an.
2. Wählen Sie im linken Bereich **Active Directory** aus.

   ![Klassisches Azure-Portal – Active Directory](.\\media\\hdinsight-create-non-interactive-authentication-dotnet-application\\active-directory.png)
3. Wählen Sie das Verzeichnis aus, das Sie zum Erstellen der neuen Anwendung nutzen möchten. Wir verwenden das vorhandene Verzeichnis.
4. Klicken Sie oben auf **Anwendungen**, um die Listen mit den vorhandenen Anwendungen anzuzeigen.
5. Klicken Sie unten auf **Hinzufügen**, um eine neue Anwendung hinzuzufügen.
6. Geben Sie den **Namen** ein, wählen Sie die Option **Webanwendung und/oder Web-API**, und klicken Sie dann auf **Weiter**.

   ![Neue Azure Active Directory-Anwendung](.\\media\\hdinsight-create-non-interactive-authentication-dotnet-application\\hdinsight-add-ad-application.png)
7. Geben Sie die **Anmelde-URL** und den **App-ID-URI** ein. Geben Sie für die **ANMELDE-URL**die URI einer Website an, die Ihre Anwendung beschreibt. Das Vorhandensein der Website wird nicht überprüft. Geben Sie als App-ID-URI den URI an, mit dem Ihre Anwendung identifiziert wird. Klicken Sie danach auf **Abschließen**.
   Es dauert einen Moment, bis die Anwendung erstellt wird.  Nachdem die Anwendung erstellt wurde, wird im Portal die Seite „Auf einen Blick“ für die neue Anwendung angezeigt. Schließen Sie das Portal nicht.

   ![Neue Azure Active Directory-Anwendung – Eigenschaften](.\\media\\hdinsight-create-non-interactive-authentication-dotnet-application\\hdinsight-add-ad-application-properties.png)

**So rufen Sie die Anwendungsclient-ID und den geheimen Schlüssel ab**

1. Klicken Sie auf der neu erstellten AD-Anwendungsseite oben im Menü auf **Konfigurieren**.
2. Erstellen Sie eine Kopie der **Client-ID**. Sie benötigen diese Angabe in Ihrer .NET-Anwendung.
3. Klicken Sie unter **Schlüssel** auf die Dropdownliste **Dauer auswählen**, und wählen Sie entweder **1 Jahr** oder **2 Jahre**. Der Schlüsselwert wird erst angezeigt, wenn Sie die Konfiguration speichern.
4. Klicken Sie unten in der Seite auf **Speichern**. Erstellen Sie eine Kopie des Schlüssels, wenn der geheime Schlüssel angezeigt wird. Sie benötigen diese Angabe in Ihrer .NET-Anwendung.

## <a name="assign-ad-application-to-role"></a>Zuweisen der AD-Anwendung zur Rolle
Sie müssen die Anwendung einer [Rolle](../active-directory/role-based-access-built-in-roles.md) zuweisen, um ihr Berechtigungen zum Ausführen von Aktionen zu gewähren. Sie können den Umfang auf Abonnement-, Ressourcengruppen- oder Ressourcenebene festlegen. Die Berechtigungen werden in niedrigere Umfangsebenen übernommen (wird der Leserrolle für eine Ressourcengruppe beispielsweise eine Anwendung hinzugefügt, kann sie die Ressourcengruppe und alle darin enthaltenen Ressourcen lesen). In diesem Tutorial legen Sie den Bereich auf Ressourcengruppenebene fest.  Da das klassische Azure-Portal keine Ressourcengruppen unterstützt, muss dieser Teil im Azure-Portal durchgeführt werden.

**So fügen Sie der AD-Anwendung die Rolle „Besitzer“ hinzu**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie im linken Bereich auf **Ressourcengruppe**.
3. Klicken Sie auf die Ressourcengruppe mit dem HDInsight-Cluster, in dem Sie später in diesem Tutorial die Hive-Abfrage ausführen möchten. Wenn zu viele Ressourcengruppen vorhanden sind, können Sie den Filter verwenden.
4. Klicken Sie im Blatt „Cluster“ auf **Zugriff**.

   ![Cloud- und Blitzsymbol = Schnellstart](./media/hdinsight-hadoop-create-linux-cluster-portal/quickstart.png)
5. Klicken Sie auf dem Blatt **Benutzer** auf **Hinzufügen**.
6. Befolgen Sie die Anleitung zum Hinzufügen der Rolle **Besitzer** zu der AD-Anwendung, die Sie oben im letzten Verfahren erstellt haben. Wenn Sie den Vorgang erfolgreich abgeschlossen haben, wird die Anwendung auf dem Blatt „Benutzer“ mit der Rolle „Besitzer“ aufgeführt.

## <a name="develop-hdinsight-client-application"></a>Entwickeln der HDInsight-Clientanwendung
Erstellen Sie eine C# .NET-Konsolenanwendung, indem Sie die Schritte unter [Übermitteln von Hadoop-Aufträgen in HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md) ausführen. Ersetzen Sie dann die GetTokenCloudCredentials-Methode durch Folgendes:

    public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
    {
        var authFactory = new AuthenticationFactory();

        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };

        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];

        var accessToken =
            authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never)
                .AccessToken;

        return new TokenCloudCredentials(accessToken);
    }

So rufen Sie die Mandanten-ID über PowerShell ab

    Get-AzureRmSubscription

Oder mit der Azure-Befehlszeilenschnittstelle:

    azure account show --json


## <a name="see-also"></a>Siehe auch
* [Übermitteln von Hadoop-Aufträgen in HDInsight](hdinsight-submit-hadoop-jobs-programmatically.md)
* [Erstellen einer Active Directory-Anwendung und eines Dienstprinzipals mithilfe des Portals](../resource-group-create-service-principal-portal.md)
* [Authentifizieren eines Dienstprinzipals mit Azure Resource Manager](../resource-group-authenticate-service-principal.md)
* [Rollenbasierte Access Control in Azure](../active-directory/role-based-access-control-configure.md)



<!--HONumber=Nov16_HO3-->


