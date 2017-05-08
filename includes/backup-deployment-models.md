Der Azure Backup-Dienst umfasst zwei Arten von Tresoren: den Backup-Tresor und den Recovery Services-Tresor. Der Backup-Tresor wurde zuerst eingeführt. Anschließend folgte der Recovery Services-Tresor als Unterstützung für die erweiterten Resource Manager-Bereitstellungen. Microsoft empfiehlt die Verwendung von Resource Manager-Bereitstellungen, sofern nicht eine besondere Anforderung für die Nutzung der klassischen Bereitstellung besteht.

| **Bereitstellung** | **Portal** | **Tresor** |
| --- | --- | --- |
| Klassisch |[Klassisch](https://manage.windowsazure.com) |Sicherung |
| Ressourcen-Manager |[Azure](https://portal.azure.com) |Recovery Services |

> [!NOTE]
> Mit Resource Manager bereitgestellte Lösungen können nicht mit einem Sicherungstresor geschützt werden. Sie können einen Recovery Services-Tresor jedoch zum Schützen von Servern und virtuellen Computern verwenden, die mit dem klassischen Modell bereitgestellt wurden.  
> 
> 

