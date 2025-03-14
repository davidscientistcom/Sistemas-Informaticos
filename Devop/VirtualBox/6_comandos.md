 VBoxManage list vms

 VBoxManage createhd `
  --filename "/ruta/disk_50GB.vdi" `
  --size 51200

- Esto hay que hacerlo con la maquina parada, vagrant halt.
VBoxManage storagectl "varios_discos_default_1741629754579_59633" `
 --name "SATA Controller" `
 --add sata `
 --controller IntelAHCI


  VBoxManage showvminfo "ubuntu_xenial64_vagrant"

  VBoxManage storageattach "varios_discos_default_1741629754579_59633" `
  --storagectl "SATA Controller" `
  --port 2 `
  --device 0 `
  --type hdd `
  --medium "/ruta/disk_50GB.vdi"


Nos muestra la información de la máquina.
vboxmanage showvminfo varios_discos_default_1741629754579_59633
  