# PINOUT_ATLAS_CYC1000
Subida del fichero TCL asociado al pineado de la placa CYC1000, Con sus múltiples variantes recogidas en un fichero de texto.

# https://t.me/INICIATIVAATLAS -> El grupo ATLAS esta en Telegram pueden encontrarnos allí, Group ATLAS in Telegram

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

### Acelerómetro  integrado en la CYC1000 LIS3DH de ST.

https://www.st.com/resource/en/datasheet/lis3dh.pdf

```
## ACELERÓMETRO_CYC1000
set_location_assignment PIN_G2 -to SDI ##
set_location_assignment PIN_G1 -to SDO ##
set_location_assignment PIN_F3 -to SPC ##
set_location_assignment PIN_D1 -to CS ##
set_location_assignment PIN_B1 -to INT1 ##
set_location_assignment PIN_C2 -to INT2 ##
```

Trabajos asociados al manejo del acelerómetro: https://github.com/jakubcabal/spi-fpga



### Memoria W9864G6JT-6 en configuración de 8Mbytes con un bus de 16 bits, el régimen de esta memoria de celular son los 166Mhz de funcionamiento, y alcanza los 233Mhz sin inmutarse.

```
##SDRAM (W9864G6JT-6): 64 Mbits
##sdram ATLAS_V002_CYC1000
set_location_assignment PIN_B14 -to SDRAM_CLK
set_location_assignment PIN_B13 -to SDRAM_DQML
set_location_assignment PIN_D12 -to SDRAM_DQMH
set_location_assignment PIN_A7 -to SDRAM_NWE
set_location_assignment PIN_C8 -to SDRAM_NCAS
set_location_assignment PIN_B7 -to SDRAM_NRAS
set_location_assignment PIN_A4 -to SDRAM_BA[0]
set_location_assignment PIN_B6 -to SDRAM_BA[1]
set_location_assignment PIN_A3 -to SDRAM_A[0]
set_location_assignment PIN_B5 -to SDRAM_A[1]
set_location_assignment PIN_B4 -to SDRAM_A[2]
set_location_assignment PIN_B3 -to SDRAM_A[3]
set_location_assignment PIN_C3 -to SDRAM_A[4]
set_location_assignment PIN_D3 -to SDRAM_A[5]
set_location_assignment PIN_E6 -to SDRAM_A[6]
set_location_assignment PIN_E7 -to SDRAM_A[7]
set_location_assignment PIN_D6 -to SDRAM_A[8]
set_location_assignment PIN_D8 -to SDRAM_A[9]
set_location_assignment PIN_A5 -to SDRAM_A[10]
set_location_assignment PIN_E8 -to SDRAM_A[11]
set_location_assignment PIN_A2 -to SDRAM_A[12]
set_location_assignment PIN_C6 -to SDRAM_A[13]
set_location_assignment PIN_B10 -to SDRAM_DQ[0]
set_location_assignment PIN_A10 -to SDRAM_DQ[1]
set_location_assignment PIN_B11 -to SDRAM_DQ[2]
set_location_assignment PIN_A11 -to SDRAM_DQ[3]
set_location_assignment PIN_A12 -to SDRAM_DQ[4]
set_location_assignment PIN_D9 -to SDRAM_DQ[5]
set_location_assignment PIN_B12 -to SDRAM_DQ[6]
set_location_assignment PIN_C9 -to SDRAM_DQ[7]
set_location_assignment PIN_D11 -to SDRAM_DQ[8]
set_location_assignment PIN_E11 -to SDRAM_DQ[9]
set_location_assignment PIN_A15 -to SDRAM_DQ[10]
set_location_assignment PIN_E9 -to SDRAM_DQ[11]
set_location_assignment PIN_D14 -to SDRAM_DQ[12]
set_location_assignment PIN_F9 -to SDRAM_DQ[13]
set_location_assignment PIN_C14 -to SDRAM_DQ[14]
set_location_assignment PIN_A14 -to SDRAM_DQ[15]
set_location_assignment PIN_A6 -to SDRAM_CS
set_location_assignment PIN_F8 -to SDRAM_CKE
```

### Cuestión PS2 Teclado, la mejor opción para los poseedores de la placa prototipo morada invertir las señales con un cable ps2 que invierta las señales de forma física.

Este prototipo morado se ha desestimado por diversos fallos. 

El PS2 y la anulación del ultimo prototipo, en la que se invirtió el par de datos del teclado por el de reloj.

### Todas las ALTAS así como la Morada con recolocador teclado.

```
##PS2 Keyboard ATLAS_V002_CYC1000               
set_location_assignment PIN_K2	-to PS2_DATA
set_location_assignment PIN_L2	-to PS2_CLK
```

#### La morada hay 10 unidades en la comunidad, si se usa sin recolocador PS2.

```
##PS2 Keyboard ATLAS_MORADA 
set_location_assignment PIN_K2	-to  PS2_CLK_MSD
set_location_assignment PIN_L2	-to  PS2_DATA_MSD
```

las pistas asociadas al ratón no se vieron afectadas son comunes a la totalidad de las ATLAS.

```
##PS2 Mouse ATLAS_V002
set_location_assignment PIN_B16	-to MOUSE_DATA
set_location_assignment PIN_C16	-to MOUSE_CLK
```


### Audio analógico con una resistencia y un condensador para uso tanto de deltasigma (Máximo teórico 12Bits de resolución) como de sonidos generados por pwm (Muy inferior en resolución de sonido al deltasigma).

```
##Audio PWM_CYC1000
set_location_assignment PIN_T12	-to delta_L
set_location_assignment PIN_R11	-to delta_R
```

### Circuito de escucha de cintas de casette, se pidió a Antonio Villena si se podía incorporar el mismo circuito que usa ZXUNO.

```
##EAR_CYC1000
set_location_assignment PIN_P11	-to EAR
```

### Bus DB9, con pull ups lo que permite el uso del mismo como expansor GPIOS por I2C en total 3 buses I2C, función retro un JOYSTICK directo Norma ATARI a la que se añade un segundo disparo, 4 posiciones y 2 disparos.

```
##BUS MULTIPROPÓSITO PROTEGIDO ATLAS_V002_CYC1000
#el pin select tiene doble funcion es también ear
##set_location_assignment PIN_P11	-to JOY_SELECT ### placa mandos de super Nintendo
set_location_assignment PIN_T15	-to JOY_LEFT   # to clock
set_location_assignment PIN_N16	-to JOY_RIGHT  # to lacht
set_location_assignment PIN_J14	-to JOY_UP     # to data1
set_location_assignment PIN_R1	-to JOY_DOWN   # to data2
set_location_assignment PIN_K15	-to JOY_FIRE1  # to data3
set_location_assignment PIN_K16	-to JOY_FIRE2  # to data4 
```
### 


### Bus SD, y sus direntes nomenclaturas.


| TCL y colocación pines 10CL025YU256C8G | Asignación U16 | Explicación modo SPI | Modo MMC/SD Física | nomenclatura modo SPI mayoritario |
| ----- | ---- | ---- | ---- | ---- |
| set_location_assignment PIN_R12	-to SD_CS |           #SD_NCS |         #Señal de selección                     |   #pin2_mmc/sd CD/DAT3 entrada/salida       |  # CS -> Selección de chip estado normalmente negado |
| set_location_assignment PIN_T13	-to SD_CLK |         #SD_CLK   |       #Señal de reloj                          |  #pin5_mmc/sd CLK     reloj                 | # SCLK-> Reloj |
| set_location_assignment PIN_R13	-to SD_MISO |        #SD_SO     |      #Señal de entrada al ser maestro el spi  |  #pin3_mmc/sd CMD     respuesta de comandos | # DI -> Entrada de datos |
| set_location_assignment PIN_T14	-to SD_MOSI  |       #SD_SI      |     #Señal de salida al ser maestro el spi   |  #pin7_mmc/sd DAT0    entrada/salida        | # DO -> Salida de datos |
| set_location_assignment PIN_P14	-to SD_DATA1  |                  |                                             | #pin8_mmc/sd DAT1    entrada/salida        | # ---------------------- |
| set_location_assignment PIN_R14	-to SD_DATA2   |                  |                                            | #pin1_mmc/sd DAT2    entrada/salida        | # ---------------------- |

### Modo DVI/HDMI y sucesivos dacs analógicos en proyecto, Así como buscar las señales diferenciales necesarias para poder usar una pantalla portatil MIPI-8.

Uno de los objetivos irrenunciables de ATLAS es tener una consola de mano, por eso se contempla el protocolo mipi.

| HDMI Direct ATLAS_V002_CYC1000 | Pares positivos y negativos con numeración | ### VGA222 adaptador de 64Colores | #SCART232-128C | #MIPI-8 pantalla tipo ILI9486L|
| ----- | ---- | ---- | ---- | ---- |
| set_location_assignment PIN_L16 -to TMDS[0] | # CLK-                        | ### HS                            | # CSYNC        | #MIPI_TX/RX0_D3N |
| set_location_assignment PIN_L15 -to TMDS[1] | # CLK+ # clock channel        | ### VS                            | # GREEN[0]     | #MIPI_TX/RX0_D3P |
| set_location_assignment PIN_P1 -to TMDS[2]  | # 0-                          | ### BLUE[0]                       | # BLUE[0]      | #MIPI_TX/RX0_D2N |
| set_location_assignment PIN_P2 -to TMDS[3]  | # 0+   # blue channel         | ### BLUE[1]                       | # BLUE[1]      | #MIPI_TX/RX0_D2P |
| set_location_assignment PIN_J1 -to TMDS[4]  | # 1-                          | ### GREEN[0]                      | # GREEN[1]     | #MIPI_TX/RX0_D1N |
| set_location_assignment PIN_J2 -to TMDS[5]  | # 1+   # green channel        | ### GREEN[1]                      | # GREEN[2]     | #MIPI_TX/RX0_D1P | 
| set_location_assignment PIN_N1 -to TMDS[6]  | # 2-                          | ### RED[0]                        | # RED[0]       | #MIPI_TX/RX0_D0N |
| set_location_assignment PIN_N2 -to TMDS[7]  | # 2+   # red channel          | ### RED[1]                        | # RED[1]       | #MIPI_TX/RX0_D0P |
| | | | |#MIPI_TX/RX0_CLKN (Esta señal hay que buscarla,DIFFIO_R4N -> PIN_F16 |
| | | | |#MIPI_TX/RX0_CLKP (Esta señal hay que buscarla,DIFFIO_R4P -> PIN_F15 |


## INTERCONEXIÓN FPGA <-> SBC O Microcontrolador.

Se han destinado 6 señales de la fpga directas sin pull alguna, accesibles desde el bus de 40 pines normalizado 20x2 a una distancia entre taladros de 2,54mm.
Se contemplan los usos has la fecha de diferentes desarrolladores.

#Serie o I2C si se colocan pull ups.


| SERIAL_SBC/Micro_CYC1000 | Serie | Propuesto I2C |
| ----- | ---- | ---- |
| set_location_assignment PIN_D16	-to | RX2 | SCL -> Reloj |
| set_location_assignment PIN_D15	-to | TX2 | SDA -> Datos |


| Uso inicial SPI SBC 40 pines_CYC1000  | Conexión con microcontrolador STM32F103F  | uso  UDA1334 minijack shaeon/Kyp | Búsqueda de 2 nuevos pares para el MIPI 8BITS |
| ----- | ---- | ---- | ---- |
| set_location_assignment PIN_F13	-to PI_CLK  | ## CLK ->SPI_SCK  | ## DATA                  | #                 |
| set_location_assignment PIN_F15	-to PI_MISO | ## MISO->SPI_DI   | ## BCLK                  | # MIPI_TX0_CLKP   |
| set_location_assignment PIN_F16	-to PI_MOSI | ## MOSI->SPI_DO   | ## LRCLK                 | # MIPI_TX0_CLKN   |
| set_location_assignment PI1N_C15 -to PI_CS  | ## CS  ->SPI_SS2  | ##                       | #                 |


# INCISOS ACLARATORIOS


### COLOCACIÓN DE LA PROGRAMACIÓN JTAG CON TERMINOLOGÍA SBC RASPBERRY PI USANDO YA UN BUS DE 40 PINES.

```
######################################################
# Inciso sobre la colocación JTAG en la pi por gpio  #
######################################################
# TCK -> GPIO17                                      #
# TDI -> GPIO22                                      #
# TDO -> GPIO27                                      #
# TMS -> GPIO4                                       #
######################################################

######################################################
#Señales del puerto JTAG en Cyclone  10CL025YU256C8G #
######################################################
# PIN_H3 JTAG TCK                                    #
# PIN_H4 JTAG TDI                                    #
# PIN_J4 JTAG TDO                                    #
# PIN_J5 JTAG TMS                                    #
# PIN_J3      nCE                                    #
# PIN_H5      nCONFIG                                #
######################################################
```

### Desarrollo de los modos de alta velocidad del FT2232H y futuros análisis.

```
######################################################################################################################
#Señales del FT2232H de alta velocidad                                                                               #
######################################################################################################################
#FSDI, input, data, receive serial data                                                                              #
#FSCLK, input, clock in serial data from FSDI and FSDO                                                               #
#FSDO, output, serial data out from FT2232H                                                                          #
#FSCTS, output, driven low to indicate the FT2232H is ready to send data                                             #
######################################################################################################################
#CYC1000 FPGA pin out to the FT2232H                                                                                 #
#The cyclone 10LP fpga 10CL025YU256C8G on the CYC1000 is connected to the FT2232H chip using the following pines:    #
#FSDI -> PIN_R7                                                                                                      #
#FSCLK -> PIN_T7                                                                                                     #
#FSDO -> PIN_M0 Revisar puede estar intercambiado con PIN_N8                                                         #
#FSCTS -> PIN_N8 Revisar                                                                                             #
######################################################################################################################``

```
