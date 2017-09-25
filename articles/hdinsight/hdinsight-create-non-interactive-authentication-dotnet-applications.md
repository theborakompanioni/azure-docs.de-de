---
title: "Erstellen von .NET-Anwendungen für die nicht interaktive Authentifizierung in Azure HDInsight | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Microsoft .NET-Anwendungen für die nicht interaktive Authentifizierung in Azure HDInsight erstellen."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 8e32430f-6404-498a-9fcd-f20338d964af
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2017
ms.author: jgao
ms.translationtype: HT
ms.sourcegitcommit: 190ca4b228434a7d1b30348011c39a979c22edbd
ms.openlocfilehash: 9ad482a932c56aa2585560eb74cf4cef06a6fa52
ms.contentlocale: de-de
ms.lasthandoff: 09/09/2017

---
# <a name="create-a-non-interactive-authentication-net-hdinsight-application"></a>Erstellen einer .NET HDInsight-Anwendung für die nicht interaktive Authentifizierung
Sie können Ihre Microsoft .NET Azure HDInsight-Anwendung entweder unter der eigenen Identität der Anwendung (nicht interaktiv) oder unter der Identität des angemeldeten Benutzers der Anwendung (interaktiv) ausführen. In diesem Artikel erfahren Sie, wie Sie eine .NET-Anwendung für die nicht interaktive Authentifizierung erstellen, um eine Verbindung mit Azure herzustellen und HDInsight zu verwalten. Ein Beispiel für die interaktive Anwendung finden Sie unter [Verbinden mit Azure HDInsight](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight). 

Für die nicht interaktive .NET-Anwendung benötigen Sie Folgendes:

* Mandanten-ID Ihres Azure-Abonnements auch *Verzeichnis-ID*). Siehe [Abrufen der Mandanten-ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).
* Client-ID der Azure Active Directory-Anwendung (Azure AD). Weitere Informationen finden Sie unter [Erstellen einer Azure Active Directory-Anwendung](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application) und [Abrufen der Anwendungs-ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key).
* Geheimer Schlüssel der Azure AD-Anwendung. Siehe [Abrufen des Anwendungsauthentifizierungsschlüssels](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key).

## <a name="prerequisites"></a>Voraussetzungen
* Einen HDInsight-Cluster Siehe [Erste-Schritte-Tutorial](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).

## <a name="assign-a-role-to-the-azure-ad-application"></a>Zuweisen einer Rolle zur Azure AD-Anwendung
Weisen Sie Ihrer Azure AD-Anwendung eine [Rolle](../active-directory/role-based-access-built-in-roles.md) zu, um ihr Berechtigungen zum Ausführen von Aktionen zu gewähren. Sie können den Umfang auf Abonnement-, Ressourcengruppen- oder Ressourcenebene festlegen. Die Berechtigungen werden von niedrigeren Ebenen mit geringerem Umfang geerbt. (Wenn z.B. der Leserolle für eine Ressourcengruppe eine Anwendung hinzugefügt wird, kann diese Anwendung die Ressourcengruppe und alle darin enthaltenen Ressourcen lesen.) In diesem Tutorial legen Sie den Bereich auf Ressourcengruppenebene fest. Weitere Informationen finden Sie unter [Verwenden von Rollenzuweisungen zum Verwalten Ihrer Azure-Abonnementressourcen](../active-directory/role-based-access-control-configure.md).

**So fügen Sie der Azure AD-Anwendung die Rolle „Besitzer“ hinzu**

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie im Menü auf der linken Seite die Option **Ressourcengruppe** aus.
3. Wählen Sie die Ressourcengruppe mit dem HDInsight-Cluster aus, in dem Sie später in diesem Tutorial die Hive-Abfrage ausführen möchten. Wenn Sie über eine große Anzahl von Ressourcengruppen verfügen, können Sie mit dem Filter die gewünschte suchen.
4. Wählen Sie im Ressourcengruppenmenü **Zugriffssteuerung (IAM)**.
5. Wählen Sie unter **Benutzer** die Option **Hinzufügen**.
6. Führen Sie die Anweisungen zum Hinzufügen der Rolle „Besitzer“ zu Ihrer Azure AD-Anwendung aus. Nachdem Sie die Rolle erfolgreich hinzugefügt haben, wird die Anwendung unter **Benutzer** mit der Rolle „Besitzer“ aufgelistet. 

## <a name="develop-an-hdinsight-client-application"></a>Entwickeln einer HDInsight-Clientanwendung

1. Erstellen Sie eine C#-Konsolenanwendung.
2. Fügen Sie die folgenden NuGet-Pakete hinzu:

        Install-Package Microsoft.Azure.Common.Authentication -Pre
        Install-Package Microsoft.Azure.Management.HDInsight -Pre
        Install-Package Microsoft.Azure.Management.Resources -Pre

3. Führen Sie den folgenden Code aus:

    ```csharp
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
    
            private static Guid SubscriptionId = new Guid("<Enter your Azure subscription ID>");
            private static string tenantID = "<Enter your tenant ID (also called directory ID)>";
            private static string applicationID = "<Enter your application ID>";
            private static string secretKey = "<Enter the application secret key>";
    
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
    
            /// Get the access token for a service principal and provided key.          
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
    ```


## <a name="next-steps"></a>Nächste Schritte
* [Erstellen einer Azure Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals](../azure-resource-manager/resource-group-create-service-principal-portal.md).
* Weitere Informationen finden Sie unter [Authentifizieren eines Dienstprinzipals mit Azure Resource Manager](../azure-resource-manager/resource-group-authenticate-service-principal.md).
* Weitere Informationen finden Sie unter [Verwenden der rollenbasierten Zugriffssteuerung zum Verwalten des Zugriffs auf Ihre Azure-Abonnementressourcen](../active-directory/role-based-access-control-configure.md).

