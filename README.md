# Tsc_Project

## Descriere scurta
    Proiectul are ca obiectiv realizarea designului hardware complet pentru un smartwatch open-source numit InkTime, incluzand schema electrica,PCB-ul, integrarea in carcasa, fisierele de productie si documentatia necesara.

## Diagrama Bloc

Arhitectura hardware este centrata pe microcontrolerul nRF52840, bazat pe ARM Cortex-M4F, care coordoneaza toate perifericele sistemului. Acesta comunica cu:

E-Paper Display-ul, pentru afisarea informatiilor;
accelerometrul BMA423, pentru functii de detectie miscare;
driverul haptic DRV2605, pentru feedback tactil;
cele trei butoane de control: Up, Enter si Down.

Subsistemul de alimentare include:

BQ25180 pentru incarcare LiPo;
RT610A pentru conversie DC/DC;
MAX17048 pentru monitorizarea starii bateriei.

Intrarea de alimentare este realizata prin USB-C, protejata ESD, iar energia este distribuita catre nucleul de procesare si perifericele asociate.
## Diagrama Bloc

```text
+----------------------+
| USB-C Input          |
| + ESD Protection     |
+----------------------+
          |
          v
+----------------------+
| Power Management     |
| BQ25180 + RT610A     |
+----------------------+
          |
          v
+----------------------+
| MAX17048             |
| Fuel Gauge           |
+----------------------+
          |
          v
+----------------------+
| nRF52840 MCU         |
| ARM Cortex-M4F       |
| BLE / GPIO / SPI     |
+----------------------+
      |      |      |
      v      v      v
+---------+ +------+ +------------+
| E-Paper | |BMA423| | DRV2605    |
| Display | |Accel.| | Haptic     |
+---------+ +------+ +------------+
      ^
      |
+----------------------+
| Buttons              |
| Up / Enter / Down    |
+----------------------+
<<<<<<< HEAD
=======
```
## Bill of Materials (BOM)

Lista de materiale se gaseste in fiserul BOM.

Lista scurta cu materialele mai esentiale: 
- U1 - nRF52840 - microcontroller principal cu BLE 5.0 - AQFN
- IC1 - BQ25180 - incarcator Li-Ion / LiPo - DSBGA
- U3 - MAX17048G+T10 - fuel gauge pentru baterie - TDFN
- IC9 - RT6160 - convertor buck-boost - WL-CSP
- IC2 - DRV2605YZFR - driver haptic - BGA
- IC3 - BMA423 - accelerometru - LGA
- ANT1 - 2450AT18B100E - antena 2.45 GHz - chip antenna
- J4 - KH-TYPE-C-16P - conector USB-C
- J1 - 503480-2400 - conector FPC pentru display
- J2 - TC2030 - conector debug/programare
- D3 - USBLC6-2SC6Y - protectie ESD pentru USB
- X1 - 32 MHz crystal - oscilator principal
- X2 - 32.768 kHz crystal - oscilator low power
- SW_UP / SW_ENT / SW_DN - butoane pentru interfata utilizator

## Descriere Hardware

Sistemul hardware este construit in jurul microcontrolerului nRF52840, care reprezinta unitatea principala de procesare si coordoneaza toate modulele periferice. Acesta comunica cu afisajul E-Paper, cu accelerometrul BMA423 pentru detectia miscarii si cu driverul haptic DRV2605 pentru feedback tactil. Alimentarea este gestionata printr-un subsistem dedicat format din incarcatorul BQ25180, circuitul de monitorizare a bateriei MAX17048 si convertorul RT6160, care asigura tensiunile necesare functionarii stabile. Interactiunea cu utilizatorul se realizeaza prin cele trei butoane fizice Up, Enter si Down, iar conectivitatea si depanarea sunt sustinute prin conectorii si test pad-urile integrate pe placa.

## Estimare Consumului de Putere

Consumul de putere este dominat de microcontrolerul nRF52840 in modurile active, in special in timpul comunicatiei BLE, unde valorile tipice sunt de ordinul catorva miliamperi. In modurile low-power, consumul poate cobori la nivel de microamperi, in functie de configurarea sistemului. Circuitele de management energetic, precum MAX17048, BQ25180 si RT6160A, sunt alese pentru aplicatii portabile cu eficienta ridicata, iar perifericele precum DRV2605 si afisajul E-Paper introduc varfuri temporare de consum in timpul vibratiei sau al refresh-ului de ecran. Per ansamblu, sistemul este optimizat pentru autonomie buna, cu consum foarte mic in standby si consum moderat in utilizare activa

| State | Components active | Estimated current |
|---|---|---:|
| Deep sleep | nRF52840 RTC only | ~3 uA |
| BLE advertising | MCU + BLE radio | ~3 mA avg |
| E-paper refresh | MCU + EPD + power stage | ~30 mA peak (1-2 s) |
| IMU active | MCU + BMA423 | ~1.5 mA |
| Haptic feedback | MCU + DRV2605 + actuator | ~10-20 mA peak |
| Full operation (BLE + EPD refresh + IMU) | All | ~35 mA peak |

