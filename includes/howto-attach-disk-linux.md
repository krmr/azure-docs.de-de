
Weitere Detailinformationen zu Datenträgern finden Sie unter [Informationen zu Datenträgern und VHDs für virtuelle Computer](../articles/virtual-machines/virtual-machines-linux-about-disks-vhds.md).

<a id="attachempty"></a>

## Anfügen eines leeren Datenträgers
1. Öffnen Sie die Azure-Befehlszeilenschnittstelle, und [stellen Sie eine Verbindung mit Ihrem Azure-Abonnement her](../articles/xplat-cli-connect.md). Stellen Sie sicher, dass Sie sich im Azure Service Management-Modus (`azure config mode asm`) befinden.
2. Geben Sie wie nachstehend gezeigt den Befehl `azure vm disk attach-new` ein, um einen neuen Datenträger zu erstellen und anzufügen. Ersetzen Sie dabei *TestVM* durch den Namen Ihres virtuellen Linux-Computers, und geben Sie die Größe des Datenträgers in GB ein, in diesem Fall 100 GB.
   
       azure vm disk attach-new TestVM 100
3. Nachdem der Datenträger erstellt und angefügt wurde, wird er in der Ausgabe von `azure vm disk list <virtual-machine-name>` wie folgt aufgeführt:
   
     $ azure vm disk list TestVM
     info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        TestVM-2645b8030676c8f8.vhd  Linux
     data:    0    100       TestVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK

<a id="attachexisting"></a>

## Anfügen eines vorhandenen Datenträgers
Zum Anfügen eines vorhandenen Datenträgers wird eine VHD-Datei im Speicherkonto benötigt.

1. Öffnen Sie die Azure-Befehlszeilenschnittstelle, und [stellen Sie eine Verbindung mit Ihrem Azure-Abonnement her](../articles/xplat-cli-connect.md). Stellen Sie sicher, dass Sie sich im Azure Service Management-Modus (`azure config mode asm`) befinden.
2. Überprüfen Sie, ob die VHD, die Sie anfügen möchten, bereits in Ihr Azure-Abonnement hochgeladen wurde:
   
     $azure vm disk list
     info:    Executing command vm disk list
   
   * Fetching disk images
     data:    Name                                          OS
     data:    --------------------------------------------  -----
     data:    myTestVhd                                     Linux
     data:    TestVM-ubuntuVMasm-0-201508060029150744  Linux
     data:    TestVM-ubuntuVMasm-0-201508060040530369
     info:    vm disk list command OK
3. Wenn Sie den Datenträger nicht finden, den Sie verwenden möchten, können Sie mit `azure vm disk create` oder `azure vm disk upload` eine lokale virtuelle Festplatte in Ihr Abonnement hochladen. Ein Beispiel für `disk create` sieht wie folgt aus:
   
       $azure vm disk create myTestVhd2 .\TempDisk\test.VHD -l "East US" -o Linux
       info:    Executing command vm disk create
       + Retrieving storage accounts
       info:    VHD size : 10 GB
       info:    Uploading 10485760.5 KB
       Requested:100.0% Completed:100.0% Running:   0 Time:   25s Speed:    82 KB/s
       info:    Finishing computing MD5 hash, 16% is complete.
       info:    https://mystorageaccount.blob.core.windows.net/disks/test.VHD was
       uploaded successfully
       info:    vm disk create command OK
   
   Sie können auch den Befehl `azure vm disk upload` zum Hochladen einer VHD in ein bestimmtes Speicherkonto verwenden. Weitere Informationen zu den Befehlen zum Verwalten der Datenträger Ihrer virtuellen Azure-Computer finden Sie [hier](../articles/virtual-machines-command-line-tools.md#commands-to-manage-your-azure-virtual-machine-data-disks).
4. Nun fügen Sie die gewünschte VHD an Ihren virtuellen Computer an:
   
       $azure vm disk attach TestVM myTestVhd
       info:    Executing command vm disk attach
       + Getting virtual machines
       + Adding Data-Disk
       info:    vm disk attach command OK
   
   Denken Sie daran, *TestVM* durch den Namen Ihres virtuellen Computers und *myTestVhd* durch die gewünschte VHD zu ersetzen.
5. Sie können mit dem Befehl `azure vm disk list <virtual-machine-name>` überprüfen, ob der Datenträger an den virtuellen Computer angefügt ist:
   
     $azure vm disk list TestVM
     info:    Executing command vm disk list
   
   * Fetching disk images
   * Getting virtual machines
   * Getting VM disks
     data:    Lun  Size(GB)  Blob-Name                         OS
     data:    ---  --------  --------------------------------  -----
     data:         30        TestVM-2645b8030676c8f8.vhd  Linux
     data:    1    10        test.VHD
     data:    0    100        TestVM-76f7ee1ef0f6dddc.vhd
     info:    vm disk list command OK

> [!NOTE]
> Nachdem Sie einen Datenträger angefügt haben, müssen Sie sich auf dem virtuellen Computer anmelden und den Datenträger initialisieren, damit der virtuelle Computer ihn zur Speicherung verwenden kann (in den folgenden Schritten wird die entsprechende Vorgehensweise erläutert).
> 
> 

