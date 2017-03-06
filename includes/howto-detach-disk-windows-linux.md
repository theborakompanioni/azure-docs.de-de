Wenn Sie einen Datenträger, der an einen virtuellen Computer angefügt ist, nicht mehr benötigen, können Sie ihn leicht trennen. Beim Trennen eines Datenträgers wird dieser vom virtuellen Computer entfernt, aber er wird nicht aus dem Azure-Speicherkonto gelöscht.

Wenn Sie die vorhandenen Daten erneut auf dem Datenträger verwenden möchten, können Sie ihn erneut an denselben virtuellen Computer oder an einen anderen anfügen.  

> [!NOTE]
> Ein Betriebssystem-Datenträger kann erst getrennt werden, nachdem der virtuelle Computer gelöscht wurde.
>

## <a name="find-the-disk"></a>Suchen des Datenträgers
Wenn Sie den Namen des Datenträgers nicht kennen oder diesen vor dem Trennen überprüfen möchten, führen Sie die folgenden Schritte aus.

1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.

2. Klicken Sie auf **Virtuelle Computer**, und wählen Sie dann den betreffenden virtuellen Computer aus.

3. Klicken Sie links im VM-Dashboard unter **Einstellungen**auf **Datenträger**.

 Im VM-Dashboard werden der Name und Typ aller angefügten Datenträger aufgeführt. Beispielsweise zeigt dieser Bildschirm einen virtuellen Computer mit einer Betriebssystemfestplatte und einem Datenträger:

    ![Datenträger suchen](./media/howto-detach-disk-windows-linux/vmwithdisklist.png)

## <a name="detach-the-disk"></a>Trennen des Datenträgers
1. Klicken Sie im Azure-Portal auf **Virtuelle Computer**, und klicken Sie dann auf den Namen des virtuellen Computers mit dem Datenträger, den Sie trennen möchten.

2. Klicken Sie links im VM-Dashboard unter **Einstellungen** auf **Datenträger**.

3. Klicken Sie auf den Datenträger, den Sie trennen möchten.

  ![Identifizieren des zu trennenden Datenträgers](./media/howto-detach-disk-windows-linux/disklist.png)

4. Klicken Sie auf der Befehlsleiste auf **Trennen**.

  ![Befehl „Trennen“](./media/howto-detach-disk-windows-linux/diskdetachcommand.png)

5. Klicken Sie im Bestätigungsfenster auf **Ja**, um den Datenträger zu trennen.

  ![Bestätigen der Trennung des Datenträgers](./media/howto-detach-disk-windows-linux/confirmdetach.png)

Der Datenträger verbleibt im Speicher, ist jedoch nicht mehr an einen virtuellen Computer angefügt.
