---
redirect_url: https://docs.microsoft.com/azure/documentdb/documentdb-performance-levels
ROBOTS: NOINDEX, NOFOLLOW
translationtype: Human Translation
ms.sourcegitcommit: 94ae24e605f99e23766702a3954db567bb64bb51
ms.openlocfilehash: 1c055187405b9c858676b2b80e4bc5c4157ef580


---
# <a name="migrate-your-documentdb-s1-s2-or-s3-account"></a>Migrieren Ihres DocumentDB-S1, -S2- oder -S3-Kontos
Führen Sie die hier beschriebenen Schritte durch, um den Durchsatz für Ihr Azure DocumentDB-Konto zu steigern, indem Sie zum Standard-Tarif wechseln. Mit geringen bis gar keinen Zusatzkosten können Sie den Durchsatz Ihres vorhandenen S1-Kontos von 250 [RU/s](documentdb-request-units.md) auf 400 RU/s oder noch mehr erhöhen! Alle Standard-Konten profitieren von der Möglichkeit, den Durchsatz zu skalieren, um die Anforderungen Ihrer Anwendungen zu erfüllen. Sie müssen nicht mehr aus vordefinierten Durchsatzoptionen auswählen, sondern können jederzeit eine Skalierung durchführen, wenn Sie den Durchsatz für Ihre Anwendungen erhöhen oder verringern müssen. 

## <a name="change-to-user-defined-performance-in-the-azure-portal"></a>Umstellen auf benutzerdefinierte Leistung im Azure-Portal
1. Navigieren Sie in Ihrem Browser zum [**Azure-Portal**](https://portal.azure.com). 
2. Klicken Sie im linken Menü auf **NoSQL (DocumentDB)**, oder klicken Sie auf **Weitere Dienste**, scrollen Sie zu **Datenbanken**, und wählen Sie dann **NoSQL (DocumentDB)** aus.   
3. Wählen Sie auf dem Blatt **NoSQL (DocumentDB)** das Konto aus, das Sie ändern möchten.
4. Klicken Sie auf dem neuen Blatt im Menü unter **Sammlungen** auf **Skalieren**. 

      ![Screenshot der DocumentDB-Blätter „Einstellungen“ und „Preisstufe auswählen“](./media/documentdb-supercharge-your-account/documentdb-change-performance.png)
5. Gehen Sie folgendermaßen vor, wie im Screenshot oben dargestellt: 

 - Verwenden Sie auf dem neuen Blatt das Dropdownmenü, um die Sammlung mit dem Tarif S1, S2 oder S3 auszuwählen. 
 - Klicken Sie auf **Tarif S1**, **S2** oder **S3**.
 - Klicken Sie auf dem Blatt **Preisstufe auswählen** auf **Standard** und anschließend auf **Auswählen**, um die Änderung zu speichern.
   
6. Auf dem Blatt **Skalieren** wird im Feld **Durchsatz (RU/s)** der Standardwert „400“ angezeigt, und der **Tarif** wurde in **Standard** geändert.  Sie können den Durchsatz auf jeden Wert ändern, den Ihre Anwendung erfordert. Es empfiehlt sich, den [DocumentDB-Kapazitätsplaner](https://www.documentdb.com/capacityplanner) zu verwenden, um zu ermitteln, wie viele [Anforderungseinheiten](documentdb-request-units.md)/Sekunde (RU/s) Ihre Anwendung benötigt. Der Bereich **Geschätzter Betrag der Monatsrechnung** am unteren Rand der Seite wird automatisch aktualisiert, um eine Schätzung der monatlichen Kosten anzugeben. Klicken Sie zum Speichern der Änderungen auf **Speichern**. 
      
    ![Screenshot des Blatts „Einstellungen“, der veranschaulicht, wo Sie den Durchsatzwert ändern können](./media/documentdb-supercharge-your-account/documentdb-change-performance-set-thoughput.png)
7. Die Sammlung wird skaliert, um die neuen Anforderungen zu erfüllen, und es wird eine Erfolgsmeldung angezeigt.  
   
    ![Screenshot des Blatts „Datenbank“ mit geänderter Sammlung](./media/documentdb-supercharge-your-account/documentdb-change-performance-confirmation.png)

Weitere Informationen zu Änderungen in Verbindung mit benutzerdefiniertem und vordefiniertem Durchsatz finden Sie im Blogbeitrag [DocumentDB: Everything you need to know about using the new pricing options](https://azure.microsoft.com/blog/documentdb-use-the-new-pricing-options-on-your-existing-collections/)(DocumentDB: Alles Wissenswerte zur Verwendung der neuen Preisoptionen).

## <a name="next-steps"></a>Nächste Schritte

Im Artikel [Partitionieren und Skalieren von Daten in DocumentDB](documentdb-partition-data.md) erfahren Sie mehr darüber, wie Sie in DocumentDB Daten partitionieren und eine globale Skalierung erzielen.



<!--HONumber=Feb17_HO2-->


