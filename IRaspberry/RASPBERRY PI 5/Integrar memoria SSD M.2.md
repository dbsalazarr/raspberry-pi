Para integrar este tipo de memoria es necesario usar un adaptador M.2 a la ranura PCIe de la Pi.
> Se pueden comprar opciones en internet (aliexpress) y la compra de una SSD compatible.

Una vez integrada la parte física, es necesaria la **configuración del software para habilitar el puerto PCIe de la Pi**

# Configuración
## Actualizar el sistema e archivo de arranque

Para integrar la memoria es necesario ejecutar los siguientes comandos para actualizar las librerías actuales, el archivo de configuración para el arranque y reiniciar.

> sudo apt update && sudo apt full-upgrade -y
> sudo rpi-eeprom-update -a
> sudo reboot

## Habilitar PCIe

Para esto es necesario ejecutar los siguientes comandos para modificar los archivos de configuración 

> sudo nano /boot/firmware/config.txt

Ya en el archivo se deben agregar las siguientes líneas para habilitar el puerto PCIe:
 
 > # Habilita el conector PCIe externo (necesario para adaptadores que no son HAT+)
 > dtparam=pciex1
 > # Fuerza velocidad Gen 3 (8 GT/s) en lugar de Gen 2 (por defecto)  
 > dtparam=pciex1_gen=3
 > sudo reboot
 
 dtparam=nvme es el equivalente a colocar pciex1
## Modificar el orden de arranque de la Pi

Para esto es necesario modificar la memoria eeprom mediante los siguientes comandos:

> asdf

Y en el archivo de configuración agregar y modificar:

> PCIE_PROBE=1
> BOOT_ORDER=0xf416

Donde:
4: Puerto USB
1: SD Card
6: Memoria PCIe

*Se debe considerar que la Pi lee el orden de ejecución de derecha a izquierda*  y por otro lado
PCIE_PROBE=1 es un comando que indica a la raspberry que revise el puerto PCIe aunque no se detecte un adaptador oficial
