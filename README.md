# PINOUT_ATLAS_CYC1000
Subida del fichero TCL asociado al pineado de la placa CYC1000, Con sus múltiples variantes recogidas en un fichero de texto.

### Configuración de variables de entorno global para QUARTUS II

```
set_global_assignment -name FAMILY "Cyclone 10 LP"
set_global_assignment -name DEVICE 10CL025YU256C8G
set_global_assignment -name TOP_LEVEL_ENTITY placa_ATLAS_V002
set_global_assignment -name ORIGINAL_QUARTUS_VERSION 15.0.0
set_global_assignment -name PROJECT_CREATION_TIME_DATE "16:02:09  JULY 15, 2015"
set_global_assignment -name LAST_QUARTUS_VERSION "17.1.0 Lite Edition"
set_global_assignment -name PROJECT_OUTPUT_DIRECTORY .
set_global_assignment -name MIN_CORE_JUNCTION_TEMP 0
set_global_assignment -name MAX_CORE_JUNCTION_TEMP 85
set_global_assignment -name ERROR_CHECK_FREQUENCY_DIVISOR 1
```

Vemos que enumera el chip usado en la CYC1000 el 10CL025YU256C8G, la entidad de la que depende todo el proyecto, la versión de quartus la fecha, el directorio donde se vuelcan los resultados, las temperaturas de funcionamieto, y gestion de relojes.

### Entradas de relojes de cuarzo, la placa CYC1000 posee un reloj de 12Mhz pero tiene zócalo para un reloj de cuarzo, y es posible inyectar en un futuro relojes programados por un SI5351 en el futuro.

```
set_location_assignment PIN_M2 -to CLK_12MHZ
set_location_assignment PIN_E15 -to CLK_50MHZ #Futuro reloj "CLOCK" de métrica 2520 PAL 28,37516Mhz
```

### Botón de usuario se activa a nivel bajo 0V

```
set_location_assignment PIN_N6 -to userbutton

```

###  LA PLACA ATLAS ES POR AHORA LA PRIMERA PLACA EN USAR LAS PULL CONFIGURABLES DE LA CYC1000 K1 & L1 PARA OBTENER SOPORTE USB

```
PULLUP/PULLDOWN_CYC1000 para selección de modo keyboard
Valor de la resistencia de L1 y K1 4K7 Ohm
si L1 Y K1 Estan en triestado el comportamiento es PS2
si L1 Y K1 Estan en logica alta un pull up estan en modo PS2
si L1 Y K1 Estan en logica baja es decir un pull down en L1 y K1 afectando a k2 y l2 el modelo pasa a ser usb alta velocidad soportando conectores unifiying.
set_location_assignment  PIN_K1 -to USBSELECTA ## solo afecta al puerto de teclado
set_location_assignment  PIN_L1 -to USBSELECTB ## solo afecta al puerto de teclado
```

### Diodos leds en la CYC1000. Nota: por defecto trae la cyc1000 un modo de varios ejercicios que se parecen a las luces de KIT el coche fantástico. 

```
set_location_assignment  PIN_M6 -to LED1 ##
set_location_assignment  PIN_T4 -to LED2 ##
set_location_assignment  PIN_T3 -to LED3 ##
set_location_assignment  PIN_R3 -to LED4 ##
set_location_assignment  PIN_T2 -to LED5 ##
set_location_assignment  PIN_R4 -to LED6 ##
set_location_assignment  PIN_N5 -to LED7 ##
set_location_assignment  PIN_N3 -to LED8 ##
```

### Una de las partes mejores que tiene trabajar con CYCLONE 10 LP, es que la memoria SPI usada es fácil de acceder, pudiendo alojar de forma o absoluta o relativa al alojamiento del flujo de programación, todo tipo de datos y programas.

la nomenclatura usada en diferentes tipos de códigos y su referencia asociada al pin de la fpga 10CL025YU256C8G


| Flash (EPCQ16A): 16 Mbits| Nomenclatura A |Nomenclatura B | Explicación VARIABLE |
| ----- | ---- | ---- | ---- |
| set_location_assignment  PIN_H2 -to | AS_DATA  | I_MISO | Data In |
| set_location_assignment  PIN_H1 -to | AS_DCLK  | O_SCLK | Clock |
| Cset_location_assignment  PIN_D2 -to | AS_NCS  | O_CS_n | Chip Select |
| set_location_assignment  PIN_C1 -to | AS_ASDO  | O_MOSI | Data Out |

### El lujo de esta placa es disponer de todo un FTDI FT2232H, y tener asociados bastantes pines de la fpga.

bus DBBUS y ADBUS directamente conectados a pines de la FT2232H.

| FTDI FT2232H | Nomenclatura FTDI | Nomenclatura B |  Explicación VARIABLE|
| ----- | ---- | ---- | ---- |
| set_location_assignment PIN_R7 -to | BDBUS0 |FSDI| TX ##Transmitter output of FT2232H (Tx) 3.3 V - ESTA CAMBIADO ES RX |
| set_location_assignment PIN_T7 -to | BDBUS1 |FSCLK| RX ##Receiver input of FT2232H (Rx) 3.3 V - ESTA CAMBIADO ES TX    | 
| set_location_assignment PIN_R6 -to | BDBUS2 |FSDO| RTS ##Ready To Send handshake output (RTS) 3.3 V                    | 
| set_location_assignment PIN_T6 -to | BDBUS3 |FSCTS| CTS ##Clear To Send handshake input (CTS) 3.3 V                    | 
| set_location_assignment PIN_R5 -to | BDBUS4 | | DTR ##Data Transmit Ready (DTR) 3.3 V                                  |
| set_location_assignment PIN_T5 -to | BDBUS5 | | DSR ##Data Set Ready (DSR) 3.3 V                                       |
| set_location_assignment PIN_M8 -to | ADBUS4 | |                                                                        |
| set_location_assignment PIN_N8 -to | ADBUS7 | |                                                                        |

Con la ultima revisión del 29 de septiembre 2022 conocemos la conexión de los pines FPGA M8 y N8.


Estudio de un posible modo de alta velocidad FTDI para la CYC1000: https://www.hackster.io/MichalsTC/how-to-use-the-fast-serial-mode-on-a-ftdi-ft2232h-7f0682
