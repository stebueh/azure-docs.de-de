## <a name="using-azure-portal"></a>Verwenden des Azure-Portals
1. Wählen Sie den virtuellen Computer aus, der erneut bereitgestellt werden soll, und klicken Sie auf dem Blatt „Einstellungen“ auf die Schaltfläche „Erneut bereitstellen“. Scrollen Sie nach unten, um den Abschnitt **Support und Problembehandlung** anzuzeigen, in dem die Schaltfläche „Erneut bereitstellen“ enthalten ist, wie im folgenden Beispiel gezeigt:
   
    ![Blatt „Azure-VM“](./media/virtual-machines-common-redeploy-to-new-node/vmoverview.png)
2. Klicken Sie auf die Schaltfläche „Erneut bereitstellen“, um den Vorgang zu bestätigen:
   
    ![Blatt „Erneutes Bereitstellen eines virtuellen Computers“](./media/virtual-machines-common-redeploy-to-new-node/redeployvm.png)
3. Der **Status** des virtuellen Computers ändert sich zu *Wird aktualisiert* , wenn der virtuelle Computer zur erneuten Bereitstellung vorbereitet wird, wie im folgenden Beispiel gezeigt:
   
    ![Virtueller Computer wird aktualisiert](./media/virtual-machines-common-redeploy-to-new-node/vmupdating.png)
4. Der **Status** ändert sich dann zu *Wird gestartet* , wenn der virtuelle Computer auf einem neuen Azure-Host gestartet wird, wie im folgenden Beispiel gezeigt:
   
    ![Virtueller Computer wird gestartet](./media/virtual-machines-common-redeploy-to-new-node/vmstarting.png)
5. Nachdem der virtuelle Computer den Startvorgang abgeschlossen hat, wechselt der **Status** wieder zu *Wird ausgeführt*und gibt damit an, dass der virtuelle Computer erfolgreich erneut bereitgestellt wurde:
   
    ![Virtueller Computer wird ausgeführt](./media/virtual-machines-common-redeploy-to-new-node/vmrunning.png)



<!--HONumber=Nov16_HO3-->


