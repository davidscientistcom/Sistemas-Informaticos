### Error cuando hemos eliminado un adaptador de red
Hay veces que aparecen errores. Por ejemplo, por lo que sea, eliminamos los adaptadores de red, creados en virtualbox, y luego creamos otro de cero. Hay veces que virtualbox, no refresca esta información y tenemos un problema porque intenta acceder a un adaptador que ha sido borrado, a esto se le llama **configuracion residual** de virtualbox y tenemos que eliminarla.

De hecho, intentando levantar la máquina con Vagrant, nos sale:
```bash
 vagrant up
Bringing machine 'default' up with 'virtualbox' provider...
==> default: Checking if box 'ubuntu/bionic64' version '20230607.0.3' is up to date...
==> default: Clearing any previously set forwarded ports...
==> default: Clearing any previously set network interfaces...
==> default: Preparing network interfaces based on configuration...
    default: Adapter 1: nat
    default: Adapter 2: hostonly
==> default: Forwarding ports...
    default: 22 (guest) => 2222 (host) (adapter 1)
==> default: Running 'pre-boot' VM customizations...
==> default: Booting VM...
There was an error while executing `VBoxManage`, a CLI used by Vagrant
for controlling VirtualBox. The command and stderr is shown below.

Command: ["startvm", "167d48c1-101c-46f0-b927-93036bbf60c3", "--type", "headless"]

Stderr: VBoxManage.exe: error: Failed to open/create the internal network 'HostInterfaceNetworking-VirtualBox Host-Only Ethernet Adapter #2' (VERR_INTNET_FLT_IF_NOT_FOUND).
VBoxManage.exe: error: Failed to attach the network LUN (VERR_INTNET_FLT_IF_NOT_FOUND)
VBoxManage.exe: error: Details: code E_FAIL (0x80004005), component ConsoleWrap, interface IConsole
```


En mi caso, he eliminado los adaptadores, pero si hago 


```bash
VBoxManage list hostonlyifs
```

Obtenemos:

```bash
PS C:\maquinas\practica_ansible> VBoxManage list hostonlyifs
Name:            VirtualBox Host-Only Ethernet Adapter #2
GUID:            7563ac7e-2b29-4ecf-bb86-b6845d84428e
DHCP:            Disabled
IPAddress:       192.168.33.1
NetworkMask:     255.255.255.0
IPV6Address:     fe80::c3f1:6ba2:436d:a791
IPV6NetworkMaskPrefixLength: 64
HardwareAddress: 0a:00:27:00:00:17
MediumType:      Ethernet
Wireless:        No
Status:          Up
VBoxNetworkName: HostInterfaceNetworking-VirtualBox Host-Only Ethernet Adapter #2

Name:            VirtualBox Host-Only Ethernet Adapter
GUID:            e2b934aa-27ab-4802-964d-fdc8bae29543
DHCP:            Disabled
IPAddress:       169.254.246.173
NetworkMask:     255.255.0.0
IPV6Address:     fe80::5425:1ed9:e94c:55b8
IPV6NetworkMaskPrefixLength: 64
HardwareAddress: 0a:00:27:00:00:05
MediumType:      Ethernet
Wireless:        No
Status:          Up
VBoxNetworkName: HostInterfaceNetworking-VirtualBox Host-Only Ethernet Adapter
```

Y si nos fijamos, aparece el adptador "VirtualBox Host-Only Ethernet Adapter #2", que hemos eliminado manualmente.

Para repararlo tenemos que eliminarlo con: 

```bash
VBoxManage hostonlyif remove "VirtualBox Host-Only Ethernet Adapter #X"


```

Y en nuestro caso:

```bash
VBoxManage hostonlyif remove HostInterfaceNetworking-VirtualBox Host-Only Ethernet Adapter #2
```

Hay veces en el que esto falla:

```bash
VBoxManage hostonlyif remove "HostInterfaceNetworking-VirtualBox Host-Only Ethernet Adapter #2"
VBoxManage.exe: error: The host network interface named 'HostInterfaceNetworking-VirtualBox Host-Only Ethernet Adapter #2' could not be found
VBoxManage.exe: error: Details: code E_INVALIDARG (0x80070057), component HostWrap, interface IHost, callee IUnknown
VBoxManage.exe: error: Context: "FindHostNetworkInterfaceByName(Bstr(pszName).raw(), hif.asOutParam())" at line 150 of file VBoxManageHostonly.cpp
```

En ese punto, lo que queda es eliminar manualmente la configuracion de nuestro usuario de virtualbox.

- C:\Users\TU_USUARIO\.VirtualBox



