## <a name="set-up-azure-cli-for-azure-dns"></a>Einrichten der Azure-CLI für Azure DNS

### <a name="before-you-begin"></a>Voraussetzungen

Vergewissern Sie sich vor Beginn der Konfiguration, dass Sie über Folgendes verfügen:

* Ein Azure-Abonnement. Wenn Sie noch kein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Abonnentenvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) aktivieren oder sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) registrieren.
* Installieren Sie die neueste Version der für Windows, Mac und Linux verfügbaren Azure-Befehlszeilenschnittstelle. Weitere Informationen finden Sie unter [Installieren der Azure-Befehlszeilenschnittstelle](../articles/cli-install-nodejs.md).

### <a name="sign-in-to-your-azure-account"></a>Anmelden bei Ihrem Azure-Konto

Öffnen Sie ein Konsolenfenster, und authentifizieren Sie sich mit Ihren Anmeldeinformationen. Weitere Informationen finden Sie unter [Log in to Azure from the Azure CLI](../articles/xplat-cli-connect.md) (Anmelden bei Azure über die Azure-CLI).

```azurecli
azure login
```

### <a name="switch-cli-mode"></a>Wechseln des CLI-Modus

Azure DNS verwendet den Azure-Ressourcen-Manager. Achten Sie darauf, zur Verwendung von Azure Resource Manager-Befehlen zum CLI-Modus zu wechseln.

```azurecli
azure config mode arm
```

### <a name="select-the-subscription"></a>Auswählen des Abonnements

Überprüfen Sie die Abonnements für das Konto.

```azurecli
azure account list
```

Wählen Sie aus, welches Azure-Abonnement Sie verwenden möchten.

```azurecli
azure account set "subscription name"
```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Der Azure-Ressourcen-Manager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dieser wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Da alle DNS-Ressourcen global und nicht regional sind, hat die Auswahl des Speicherorts für die Ressourcengruppe jedoch keine Auswirkungen auf Azure DNS.

Dieser Schritt kann übersprungen werden, wenn Sie eine vorhandene Ressourcengruppe verwenden.

```azurecli
azure group create -n myresourcegroup --location "West US"
```

### <a name="register-resource-provider"></a>Registrieren des Ressourcenanbieters

Der Azure DNS-Dienst wird vom Ressourcenanbieter "Microsoft.Network" verwaltet. Ihr Azure-Abonnement muss für die Verwendung dieses Ressourcenanbieters registriert werden, bevor Sie Azure DNS verwenden können. Dieser Schritt muss einmal für jedes Abonnement ausgeführt werden.

```azurecli
azure provider register --namespace Microsoft.Network
```

