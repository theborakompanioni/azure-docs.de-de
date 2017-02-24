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
ms.date: 02/22/2017
ms.author: jgao
translationtype: Human Translation
ms.sourcegitcommit: a423975e8a091183154217678706817694f3e346
ms.openlocfilehash: d5256250d6d3a6d7df3a90ae4a0801af131b830e


---
# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a>Erstellen von .NET HDInsight-Anwendungen für die nicht interaktive Authentifizierung 
Sie können Ihre .NET Azure HDInsight-Anwendung entweder unter der eigenen Identität der Anwendung (nicht interaktiv) oder unter der Identität des angemeldeten Benutzers der Anwendung (interaktiv) ausführen. Ein Beispiel für die interaktive Anwendung, finden Sie unter [Verbinden mit Azure HDInsight](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight). In diesem Artikel erfahren Sie, wie Sie eine .NET-Anwendung für die nicht interaktive Authentifizierung erstellen, um eine Verbindung mit Azure herzustellen und HDInsight zu verwalten.

Für die nicht interaktive .NET-Anwendung benötigen Sie Folgendes:

* Mandanten-ID des Azure-Abonnements (bzw. Verzeichnis-ID). Siehe [Abrufen der Mandanten-ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).
* Client-ID der Azure Directory-Anwendung. Siehe [Erstellen einer Active Directory-Anwendung](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-active-directory-application) und [Abrufen der Anwendungs-ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key).
* Geheimen Schlüssel der Azure Directory-Anwendung. Siehe [Abrufen des Anwendungsauthentifizierungsschlüssels](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key).

## <a name="prerequisites"></a>Voraussetzungen
* HDInsight-Cluster. Siehe [Erste-Schritte-Tutorial](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).



## <a name="assign-ad-application-to-role"></a>Zuweisen der AD-Anwendung zur Rolle
Sie müssen die Anwendung einer [Rolle](../active-directory/role-based-access-built-in-roles.md) zuweisen, um ihr Berechtigungen zum Ausführen von Aktionen zu gewähren. Sie können den Umfang auf Abonnement-, Ressourcengruppen- oder Ressourcenebene festlegen. Die Berechtigungen werden in niedrigere Umfangsebenen übernommen (wird der Leserrolle für eine Ressourcengruppe beispielsweise eine Anwendung hinzugefügt, kann sie die Ressourcengruppe und alle darin enthaltenen Ressourcen lesen). In diesem Tutorial legen Sie den Bereich auf Ressourcengruppenebene fest. Weitere Informationen finden Sie unter [Verwenden von Rollenzuweisungen zum Verwalten Ihrer Azure-Abonnementressourcen](../active-directory/role-based-access-control-configure.md).

**So fügen Sie der AD-Anwendung die Rolle „Besitzer“ hinzu**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie im linken Bereich auf **Ressourcengruppe**.
3. Klicken Sie auf die Ressourcengruppe mit dem HDInsight-Cluster, in dem Sie später in diesem Tutorial die Hive-Abfrage ausführen möchten. Wenn zu viele Ressourcengruppen vorhanden sind, können Sie den Filter verwenden.
4. Klicken Sie im Ressourcengruppenmenü auf **Zugriffssteuerung (IAM)**.
5. Klicken Sie auf dem Blatt **Benutzer** auf **Hinzufügen**.
6. Befolgen Sie die Anleitung zum Hinzufügen der Rolle **Besitzer** zu der AD-Anwendung, die Sie oben im letzten Verfahren erstellt haben. Wenn Sie den Vorgang erfolgreich abgeschlossen haben, wird die Anwendung auf dem Blatt „Benutzer“ mit der Rolle „Besitzer“ aufgeführt.

## <a name="develop-hdinsight-client-application"></a>Entwickeln der HDInsight-Clientanwendung

1. Erstellen Sie eine C#-Konsolenanwendung.
2. Fügen Sie die folgenden NuGet-Pakete hinzu:

        Install-Package Microsoft.Azure.Common.Authentication -Pre
        Install-Package Microsoft.Azure.Management.HDInsight -Pre
        Install-Package Microsoft.Azure.Management.Resources -Pre

3. Verwenden Sie das folgende Codebeispiel:

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Common.Authentication;
        using Microsoft.Azure.Common.Authentication.Factories;
        using Microsoft.Azure.Common.Authentication.Models;
        using Microsoft.Azure.Management.Resources;
        using Microsoft.Azure.Management.HDInsight;
        
        namespace CreateHDICluster
        {
            internal class Program
            {
                private static HDInsightManagementClient _hdiManagementClient;
        
                private static Guid SubscriptionId = new Guid("<Enter Your Azure Subscription ID>");
                private static string tenantID = "<Enter Your Tenant ID (A.K.A. Directory ID)>";
                private static string applicationID = "<Enter Your Application ID>";
                private static string secretKey = "<Enter the Application Secret Key>";
        
                private static void Main(string[] args)
                {
                    var key = new SecureString();
                    foreach (char c in secretKey) { key.AppendChar(c); }

                    var tokenCreds = GetTokenCloudCredentials(tenantID, applicationID, key);
                    var subCloudCredentials = GetSubscriptionCloudCredentials(tokenCreds, SubscriptionId);
        
                    var resourceManagementClient = new ResourceManagementClient(subCloudCredentials);
                    resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        
                    _hdiManagementClient = new HDInsightManagementClient(subCloudCredentials);
        
                    var results = _hdiManagementClient.Clusters.List();
                    foreach (var name in results.Clusters)
                    {
                        Console.WriteLine("Cluster Name: " + name.Name);
                        Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
                        Console.WriteLine("\t Cluster location: " + name.Location);
                        Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
                    }
                    Console.WriteLine("Press Enter to continue");
                    Console.ReadLine();
                }

                /// Get the access token for a service principal and provided key                
                public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
                {
                    var authFactory = new AuthenticationFactory();
                    var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
                    var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
                    var accessToken =
                        authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
        
                    return new TokenCloudCredentials(accessToken);
                }
        
                public static SubscriptionCloudCredentials GetSubscriptionCloudCredentials(SubscriptionCloudCredentials creds, Guid subId)
                {
                    return new TokenCloudCredentials(subId.ToString(), ((TokenCloudCredentials)creds).Token);
                }
            }
        }

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen einer Active Directory-Anwendung und eines Dienstprinzipals mithilfe des Portals](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [Authentifizieren eines Dienstprinzipals mit Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [Rollenbasierte Access Control in Azure](../active-directory/role-based-access-control-configure.md)



<!--HONumber=Feb17_HO3-->


