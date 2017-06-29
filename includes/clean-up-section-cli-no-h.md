In den vorherigen Schritten haben Sie Azure-Ressourcen in einer Ressourcengruppe erstellt. Wenn Sie diese Ressourcen in Zukunft nicht mehr benötigen, können Sie sie löschen, indem Sie die Ressourcengruppe löschen.
 
1. Führen Sie den folgenden Befehl aus, um sicherzustellen, dass die Ressourcengruppe keine Ressourcen enthält, die Sie speichern möchten:

  ```azurecli
  az group show --name myResourceGroup
  ```

2. Wenn Sie alle angezeigten Ressourcen löschen möchten, führen Sie den folgenden Befehl aus:
 
  ```azurecli
  az group delete --name myResourceGroup
  ```
