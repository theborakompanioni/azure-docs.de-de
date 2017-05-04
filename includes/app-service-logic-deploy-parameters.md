Mit dem Azure-Ressourcen-Manager definieren Sie die Parameter für Werte, die Sie bei der Bereitstellung der Vorlage angeben möchten. Die Vorlage enthält einen Abschnitt namens "Parameters", der alle Parameterwerte enthält.
Definieren Sie einen Parameter für die Werte, die basierend auf dem bereitgestellten Projekt oder der bereitgestellten Umgebung variieren. Definieren Sie keine Parameter für Werte, die sich nicht ändern. Jeder Parameterwert wird in der Vorlage verwendet, um die bereitgestellten Ressourcen zu definieren. 

Verwenden Sie beim Definieren von Parametern das Feld **allowedValues** , um anzugeben, welche Werte eine Benutzer während der Bereitstellung angeben kann. Verwenden Sie das Feld **defaultValue** , um dem Parameter einen Wert zuweisen, wenn kein Wert während der Bereitstellung angegeben wird.

Nachfolgend wird jeder der in der Vorlage verwendeten Parameter beschrieben.

### <a name="logicappname"></a>logicAppName
Der Name der zu erstellenden Logik-App.

    "logicAppName": {
        "type": "string"
    }