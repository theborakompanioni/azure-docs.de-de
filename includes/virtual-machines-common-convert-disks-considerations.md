
* Da für die Konvertierung ein Neustart des virtuellen Computers erforderlich ist, sollten Sie die Migration Ihrer virtuellen Computer während eines bereits vorhandenen Wartungsfensters durchführen. 

* Die Konvertierung kann nicht rückgängig gemacht werden. 

* Testen Sie die Konvertierung unbedingt. Migrieren Sie einen virtuellen Testcomputer, bevor Sie die Migration in der Produktion durchführen.

* Bei der Konvertierung heben Sie die Zuordnung der VM auf. Die VM erhält eine neue IP-Adresse, wenn sie nach der Konvertierung gestartet wird. Bei Bedarf können Sie dem virtuellen Computer [eine statische IP-Adresse zuweisen](../articles/virtual-network/virtual-network-ip-addresses-overview-arm.md).

* Die ursprünglichen VHDs und das Speicherkonto, die vor der Konvertierung vom virtuellen Computer verwendet wurden, werden nicht gelöscht. Sie verursachen weiterhin Kosten. Um zu vermeiden, dass diese Artefakte in Rechnung gestellt werden, löschen Sie die ursprünglichen VHD-Blobs, nachdem Sie sichergestellt haben, dass die Konvertierung abgeschlossen ist.
