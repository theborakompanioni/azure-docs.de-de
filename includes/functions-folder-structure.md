
Der Code für alle Funktionen in einer bestimmten Funktionen-App befindet sich in einem Stammordner, der eine Hostkonfigurationsdatei und mindestens einen Unterordner enthält. Jeder dieser Unterordner enthält den Code für eine separate Funktion, wie im folgenden Beispiel gezeigt:

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

Die Datei *host.json* enthält die laufzeitspezifische Konfiguration und befindet sich im Stammordner der Funktionen-App. Informationen zu den verfügbaren Einstellungen finden Sie unter [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) im Wiki zum WebJobs.Script-Repository.

Jede Funktion verfügt über einen Ordner, der mindestens eine Codedatei, die function.json-Konfiguration sowie weitere Abhängigkeiten enthält.

