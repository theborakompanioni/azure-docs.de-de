Melden Sie sich mit dem Befehl [az login](/cli/azure/#login) bei Ihrem Azure-Abonnement an, und befolgen Sie die Anweisungen auf dem Bildschirm. Weitere Informationen zur Protokollierung finden Sie unter [Get Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli) (Erste Schritte mit Azure CLI 2.0).

```azurecli
az login
```

Listen Sie die Abonnements für das Konto auf, wenn Sie über mehr als ein Azure-Abonnement verfügen.

```azurecli
az account list --all
```

Geben Sie das Abonnement an, das Sie verwenden möchten.

```azurecli
az account set --subscription <replace_with_your_subscription_id>
```