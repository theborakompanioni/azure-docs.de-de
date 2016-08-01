<properties
   pageTitle="Erstellen eines Dienstprinzipals im Portal | Microsoft Azure"
   description="Beschreibt das Erstellen einer neuen Active Directory-Anwendung und eines Dienstprinzipals, der mit der rollenbasierten Zugriffskontrolle in Azure-Resource Manager zum Verwalten des Zugriffs auf Ressourcen verwendet werden kann."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/19/2016"
   ms.author="tomfitz"/>

# Erstellen einer Active Directory-Anwendung und eines Dienstprinzipals mit Ressourcenzugriff mithilfe des Portals

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure-Befehlszeilenschnittstelle](resource-group-authenticate-service-principal-cli.md)
- [Portal](resource-group-create-service-principal-portal.md)


Bei Verwendung einer Anwendung, die auf Ressourcen zugreifen oder diese ändern muss, müssen Sie eine Active Directory-Anwendung (AD) einrichten und ihr die erforderlichen Berechtigungen zuweisen. In diesem Thema erfahren Sie, wie diese Schritte über das Portal ausgeführt werden. Derzeit müssen Sie das klassische Portal verwenden, um eine neue Active Directory-Anwendung zu erstellen, und dann zum Azure-Portal wechseln, um der Anwendung eine Rolle zuzuweisen.

> [AZURE.NOTE] Unter Umständen ist es für Sie einfacher, die AD-Anwendung und den Dienstprinzipal über die [PowerShell](resource-group-authenticate-service-principal.md) oder die [Azure-Befehlszeilenschnittstelle](resource-group-authenticate-service-principal-cli.md) einzurichten. Dies gilt besonders, wenn Sie für die Authentifizierung ein Zertifikat verwenden möchten. In diesem Thema wird die Verwendung eines Zertifikats nicht beschrieben.

Eine Erläuterung der Active Directory-Konzepte finden Sie unter [Anwendungsobjekte und Dienstprinzipalobjekte](./active-directory/active-directory-application-objects.md). Weitere Informationen zur Active Directory-Authentifizierung finden Sie unter [Authentifizierungsszenarien für Azure AD](./active-directory/active-directory-authentication-scenarios.md).

Ausführliche Schritte zum Integrieren einer Anwendung in Azure zur Verwaltung von Ressourcen finden Sie im [Entwicklerhandbuch für die Autorisierung mit der Azure Resource Manager-API](resource-manager-api-authentication.md).

## Erstellen einer Active Directory-Anwendung

1. Melden Sie sich über das [klassische Portal](https://manage.windowsazure.com/) bei Ihrem Azure-Konto an.

2. Wählen Sie im linken Bereich **Active Directory** aus.

     ![Active Directory auswählen](./media/resource-group-create-service-principal-portal/active-directory.png)
     
3. Wählen Sie das Active Directory aus, das Sie zum Erstellen der neuen Anwendung nutzen möchten. Wenn Sie über mehr als eine Active Directory-Instanz verfügen, ist es in der Regel empfehlenswert, die Anwendung in dem Verzeichnis zu erstellen, in dem sich Ihr Abonnement befindet. Sie können nur Anwendungen im selben Verzeichnis wie Ihr Abonnement den Zugriff auf Ressourcen in Ihrem Abonnement gewähren.

     ![Verzeichnis wählen](./media/resource-group-create-service-principal-portal/active-directory-details.png)
     
    Wenn Sie das Verzeichnis für Ihr Abonnement ermitteln müssen, wählen Sie **Einstellungen** aus, und suchen Sie nach dem Namen des Verzeichnisses.
   
     ![Standardverzeichnis suchen](./media/resource-group-create-service-principal-portal/show-default-directory.png)

3. Klicken Sie auf **Anwendungen**, um die Anwendungen in Ihrem Verzeichnis anzuzeigen.

     ![Anwendungen anzeigen](./media/resource-group-create-service-principal-portal/view-applications.png)

4. Wenn Sie in diesem Verzeichnis zuvor noch keine Anwendung erstellt haben, sollten Sie eine Abbildung ähnlich dieser hier sehen. Klicken Sie auf **EINE ANWENDUNG HINZUFÜGEN**

     ![Anwendung hinzufügen](./media/resource-group-create-service-principal-portal/create-application.png)

     Oder klicken Sie im unteren Bereich auf **Hinzufügen**.

     ![Hinzufügen](./media/resource-group-create-service-principal-portal/add-icon.png)

5. Wählen Sie den Anwendungstypen aus, den Sie erstellen möchten. Wählen Sie im Rahmen dieses Tutorials **Eine von meinem Unternehmen entwickelte Anwendung hinzufügen** aus.

     ![neue Anwendung](./media/resource-group-create-service-principal-portal/what-do-you-want-to-do.png)

6. Geben Sie einen Namen für die Anwendung ein, und wählen Sie den Typ der Anwendung aus, die Sie erstellen möchten. Erstellen Sie im Rahmen dieses Tutorials eine **WEBANWENDUNG UND/ODER WEB-API**, und klicken Sie auf die Schaltfläche „Weiter“. Bei Verwendung von **SYSTEMEIGENE CLIENTANWENDUNG** entsprechen die restlichen Schritte in diesem Artikel nicht der tatsächlichen Vorgehensweise.

     ![Anwendung benennen](./media/resource-group-create-service-principal-portal/tell-us-about-your-application.png)

7. Tragen Sie die Eigenschaften Ihrer Anwendung ein. Geben Sie für die **ANMELDE-URL** die URL einer Website an, die Ihre Anwendung beschreibt. Das Vorhandensein der Website wird nicht überprüft. Geben Sie für die **APP-ID URI ** die URI an, die Ihre Anwendung identifiziert.

     ![Anwendungseigenschaften](./media/resource-group-create-service-principal-portal/app-properties.png)

Sie haben Ihre Anwendung erstellt.

## Abrufen der Client-ID und des Authentifizierungsschlüssels

Beim programmgesteuerten Anmelden benötigen Sie die ID für Ihre Anwendung. Wenn die Anwendung unter ihren eigenen Anmeldeinformationen ausgeführt wird, benötigen Sie außerdem einen Authentifizierungsschlüssel.

1. Klicken Sie auf die Registrierkarte **Konfigurieren**, um das Kennwort für Ihre Anwendung zu konfigurieren.

     ![Anwendung konfigurieren](./media/resource-group-create-service-principal-portal/application-configure.png)

2. Kopieren Sie die **CLIENT-ID**.
  
     ![Client-ID](./media/resource-group-create-service-principal-portal/client-id.png)

3. Wenn die Anwendung unter eigenen Anmeldeinformationen ausgeführt wird, navigieren Sie nach unten zum Abschnitt **Schlüssel**, und wählen Sie den gewünschten Gültigkeitszeitraum für Ihr Kennwort aus.

     ![Schlüssel](./media/resource-group-create-service-principal-portal/create-key.png)

4. Wählen Sie **Speichern** aus, um Ihren Schlüssel zu erstellen.

     ![speichern](./media/resource-group-create-service-principal-portal/save-icon.png)

     Der gespeicherte Schlüssel wird angezeigt, sodass Sie ihn kopieren können. Sie können den Schlüssel später nicht mehr abrufen. Sie sollten ihn also jetzt kopieren.

     ![gespeicherter Schlüssel](./media/resource-group-create-service-principal-portal/save-key.png)

## Abrufen der Mandanten-ID

Beim programmgesteuerten Anmelden müssen Sie mit Ihrer Authentifizierungsanforderung die Mandanten-ID übergeben. Bei Web-Apps und Web-API-Apps können Sie die Mandanten-ID abrufen, indem Sie unten auf der Seite **Endpunkte anzeigen** auswählen und die ID wie weiter unten gezeigt abrufen.

   ![Mandanten-ID](./media/resource-group-create-service-principal-portal/save-tenant.png)

Sie können die Mandanten-ID auch über PowerShell abrufen:

    Get-AzureRmSubscription

Oder mit der Azure-Befehlszeilenschnittstelle:

    azure account show --json

## Festlegen der delegierten Berechtigungen

Wenn Ihre Anwendung im Auftrag eines angemeldeten Benutzers auf Ressourcen zugreift, müssen Sie der Anwendung die delegierten Berechtigungen für den Zugriff auf andere Anwendungen gewähren. Dieser Schritt wird auf der Registerkarte **Konfigurieren** im Abschnitt **Berechtigungen für andere Anwendungen** ausgeführt. Eine delegierte Berechtigung ist standardmäßig bereits für die Azure Active Directory-Instanz aktiviert. Lassen Sie diese delegierte Berechtigung unverändert.

1. Wählen Sie **Anwendung hinzufügen** aus.

2. Wählen Sie in der Liste die **Azure-Dienstverwaltungs-API** aus. Klicken Sie dann auf das Symbol zum Fertigstellen.

      ![App auswählen](./media/resource-group-create-service-principal-portal/select-app.png)

3. Aktivieren Sie in der Dropdownliste für delegierte Berechtigungen das Kontrollkästchen **Access Azure Service Management as organization** (Auf Azure-Dienstverwaltung als Organisation zugreifen).

      ![Berechtigung auswählen](./media/resource-group-create-service-principal-portal/select-permissions.png)

4. Speichern Sie die Änderungen.

## Konfigurieren einer mehrinstanzenfähigen Anwendung

Wenn Benutzer aus anderen Azure Active Directory-Instanzen ihre Zustimmung für die Anwendung geben und sich bei ihr anmelden können, müssen Sie die Mehrinstanzenfähigkeit aktivieren. Legen Sie auf der Registerkarte **Konfigurieren** für **Anwendung ist mehrinstanzenfähig** die Option **Ja** fest.

![mehrinstanzenfähig](./media/resource-group-create-service-principal-portal/multi-tenant.png)

## Zuweisen einer Anwendung zur Rolle

Wenn Ihre Anwendung mit ihren eigenen Anmeldeinformationen ausgeführt wird, müssen Sie die Anwendung einer Rolle zuweisen. Sie müssen entscheiden, welche Rolle die geeigneten Berechtigungen für die Anwendung darstellt. Informationen zu verfügbaren Rollen finden Sie unter [RBAC: Integrierte Rollen](./active-directory/role-based-access-built-in-roles.md).

Für die Rollenzuweisung benötigen Sie `Microsoft.Authorization/*/Write`-Zugriff. Dieser wird über die Rolle [Besitzer](./active-directory/role-based-access-built-in-roles.md#owner) oder [Benutzerzugriffsadministrator](./active-directory/role-based-access-built-in-roles.md#user-access-administrator) gewährt.

Sie können den Umfang auf Abonnement-, Ressourcengruppen- oder Ressourcenebene festlegen. Die Berechtigungen werden in niedrigere Umfangsebenen übernommen (wird der Leserrolle für eine Ressourcengruppe beispielsweise eine Anwendung hinzugefügt, kann sie die Ressourcengruppe und alle darin enthaltenen Ressourcen lesen).

1. Zum Zuweisen der Anwendung an eine Rolle wechseln Sie vom klassischen Portal zum [Azure-Portal](https://portal.azure.com).

1. Navigieren Sie im Portal zur Umfangsebenene, der Sie die Anwendung zuweisen möchten. Für dieses Thema können Sie zu einer Ressourcengruppe navigieren und auf dem Blatt der Ressourcengruppe das Symbol **Zugriff** wählen.

     ![Benutzer auswählen](./media/resource-group-create-service-principal-portal/select-users.png)

2. Wählen Sie **Hinzufügen**.

     ![„Hinzufügen“ wählen](./media/resource-group-create-service-principal-portal/select-add.png)

3. Wählen Sie die **Leser**-Rolle (bzw. die Rolle, der die Anwendung zugewiesen werden soll).

     ![Rolle wählen](./media/resource-group-create-service-principal-portal/select-role.png)

4. Beim ersten Anzeigen der Liste mit Benutzern, die Sie der Rolle hinzufügen können, werden keine Anwendungen angezeigt. Nur Gruppen und Benutzer werden angezeigt.

     ![Benutzer anzeigen](./media/resource-group-create-service-principal-portal/show-users.png)

5. Sie müssen nach Ihrer Anwendung suchen. Geben Sie den Anfang des Namens Ihrer Anwendung ein, und die Liste der verfügbaren Optionen ändert sich. Wählen Sie Ihre Anwendung aus, wenn Sie in der Liste angezeigt wird.

     ![An Rolle zuweisen](./media/resource-group-create-service-principal-portal/assign-to-role.png)

6. Wählen Sie **OK**, um das Zuweisen der Rolle abzuschließen. Jetzt sollten Sie Ihre Anwendung in der Liste der Benutzer sehen, die einer Rolle für die Ressourcengruppe zugewiesen sind.

     ![einblenden](./media/resource-group-create-service-principal-portal/show-app.png)

Weitere Informationen zum Zuweisen von Benutzern und Anwendungen zu Rollen über das Portal finden Sie unter [Verwalten des Zugriffs über das Azure-Verwaltungsportal](role-based-access-control-configure.md#manage-access-using-the-azure-management-portal).

## Beispielanwendungen

Die folgenden Beispielanwendungen veranschaulichen die Anmeldung als Dienstprinzipal:

**.NET**

- [Deploy an SSH Enabled VM with a Template with .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/) (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage mit .NET)
- [Manage Azure resources and resource groups with .NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/) (Verwalten von Azure-Ressourcen und -Ressourcengruppen mit .NET)

**Java**

- [Getting Started with Resources - Deploy Using ARM Template - in Java](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/) (Erste Schritte mit Ressourcen – Bereitstellen mithilfe einer Azure Resource Manager-Vorlage – in Java)
- [Getting Started with Resources - Manage Resource Group - in Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//) (Erste Schritte mit Ressourcen – Verwalten von Ressourcengruppen – in Java)

**Python**

- [Deploy an SSH Enabled VM with a Template in Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/) (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage in Python)
- [Manage Azure resources and resource groups with Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/) (Verwalten von Azure-Ressourcen und -Ressourcengruppen mit Python)

**Node.js**

- [Deploy an SSH Enabled VM with a Template in Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/) (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage in Node.js)
- [Manage Azure resources and resource groups with Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/) (Verwalten von Azure-Ressourcen und -Ressourcengruppen mit Node.js)

**Ruby**

- [Deploy an SSH Enabled VM with a Template in Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/) (Bereitstellen eines SSH-fähigen virtuellen Computers mit einer Vorlage in Ruby)
- [Manage Azure resources and resource groups with Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/) (Verwalten von Azure-Ressourcen und -Ressourcengruppen mit Ruby)


## Nächste Schritte

- Informationen zum Festlegen von Sicherheitsrichtlinien finden Sie unter [Rollenbasierte Zugriffssteuerung in Azure](./active-directory/role-based-access-control-configure.md).
- Eine Videodemo dieser Schritte finden Sie unter [Aktivieren der programmgesteuerten Verwaltung einer Azure-Ressource mit Azure Active Directory](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enabling-Programmatic-Management-of-an-Azure-Resource-with-Azure-Active-Directory).

<!---HONumber=AcomDC_0720_2016-->