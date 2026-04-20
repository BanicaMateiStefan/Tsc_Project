# Tsc_Project

## Descriere scurta
    Proiectul are ca obiectiv realizarea designului hardware complet pentru un smartwatch open-source numit InkTime, incluzand schema electrica, PCB-ul, integrarea in carcasa, fisierele de productie si documentatia necesara.

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
