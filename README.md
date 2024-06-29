# Multi-Speeder für das Commodore 1541 Diskettenlaufwerk I und II (V1.7)
 
![Multispeeder 1541 I und II](https://raw.githubusercontent.com/FraEgg/commodore-1541-floppydrive-8x-multi-floppy-speeder/master/images/PCB_V1.5.jpg)

Mein Multi-Speeder-Projekt für das Commodore 1541 Diskettenlaufwerk besteht aus einer Erweiterungsplatine, die zwischen der 6502 CPU gesetzt wird. Die Platine erweitert den Arbeitsspeicher der 1541 um 32 KB und stellt 8x 64 KB Bänke für DOS-Betriebssysteme auf der 1541 zur Verfügung. Die Erweiterungsplatine lässt sich in den beiden Versionen 1541-I und 1541-II betreiben. Ich habe ebenfalls eine Version für den Klone Oceanic OC-168 gestaltet (auf Anfrage).
Mit der Multi-Speeder-Erweiterungsplatine können u.a. das Original CBM-DOS 2.6, DolphinDos 2.0, SpeedDos 2.7 +, SpeedDos Expert, JiffyDOS und S-JiffyDOS umschaltbar betrieben werden.

Die Adressierung der ROM- und RAM-Bereiche variiert, deshalb kann der Multi-Speeder für die unterschiedlichen Floppyspeeder verwendet werden.

Nicht Bestandteil dieses Projekt ist ein SpeedDos kompatibles Parallelkabel zum Userport des C64 und ein ROM-Adapter-Sockel für die Kernals im C64. 
Dieses können bei bekannten Retro-Shops bezogen werden z.B. Fazination64 (siehe Links unten).   

Ich habe den Schaltplan sowie die entsprechende Eprommer-Datei für den ATF16V8 (GAL16V8 kompatibel) Adressdecoder und die Gerber-Dateien z.B. für PCBWay hier zur Verfügung gestellt. 
Dies ermöglicht die eigene Herstellung der Multi-Speeder-Platine.

# Bauteile / BOM
Neben der Platine werden folgende Bauteile benötigt:

 - 2x Präzisions-Stiftleiste 20 polig, 2,54mm (Platine zum Mainboard
 - 1x IC-Sockel, 40-polig (für U1) (6502 CPU)
 - 1x IC-Sockel, 20-polig (U4b1) (Atmel F16V8 BQL-15PV) (GAL16V8 komp.)
 - 1x IC-Sockel PLCC 32, 32-polig (EEPROM/EPROM)
 - 1x IC Atmel F16V8 BQL-15PV (U4b1) (Adressdecoder)
 - 1x IC SRAM HM62256ALF-70G 28-PIN SOP oder ISSI 62C256AL-45ULI 28-PIN SOP
 - 1x EPROM-OTP AT27C040-70JU oder AM27C040-120LC (PLCC32) oder EEPROM 29LV040 (PLCC32) (ab Version 1.6 die ensprechende Lötbrücke setzen!).
 - 2x Tantalkondensator, 100 nF radial (C1 und C2)
 - 3x Widerstände 4,7 kOhm / 1/4 Watt (R1, R2, R3) 
 - optional 3,3 kOhm (R4 ist nur optional für eine WDC65C02 CPU und wird bei einer originalen MOS 6502 nicht benötigt!)
 - 3x Jumper (JP1,JP2,JP3)
 - 2x 3x1 Header PIN-PIN

Auf der Platine ab Version v1.5 braucht der Wiederstand R4 nicht bestückt werden und der JP1 bleibt geschlossen. Diese sind nur für den Testbetrieb einer neuren WDC W65C02 CPU gedacht.  

![Multispeeder PCB Foto](https://raw.githubusercontent.com/FraEgg/commodore-1541-floppydrive-8x-multi-floppy-speeder/master/images/PCB_V1.5_1541_back.jpg)

# EPROMS
Du kannst ein EPROM-OTP 27c040 verwenden oder ein Flash-EPROM 29LV040. 
Bitte achte darauf, dass der jeweilige Jumper JP5 oder JP6 gesetzt wird! 
Ansonsten funktioniert das Umschalten der Banks nicht korrekt, da sich das PIN-Layout dieser EPROM-Typen leicht unterscheiden (PIN 1/PIN 32)

# ROMs
Die DOS-KERNALs werden in einem EPROM abgelegt. 
Das EPROM z. B. 27C040/29VL040 ist ein 512 KB EPROM. Es ist in 8x 64KB Bänke (Bank 0-7) aufgeteilt. 
Jede Bank $x0000 - $xFFFF spiegelt den 64 KB Speicherbereich der Floppy 1:1 wieder. 
Wobei nur ein bestimmter Teil der Bereiche in den Speicherbereich der 1541 eingeblendet werden (siehe Tabelle unten). 
Das nutzt natürlich die Kapazität des EPROM nicht besonders aus, macht aber das Adressdecoding und die Konfiguration sehr einfach und flexibel. 
Zudem kostet Speicher nicht mehr die Welt, ganz im Gegensatz zu den 80er-Jahren :-). 
Beim Betrieb der Multi-Speeder-Platine müssen alle Original ROM-Bausteine ICs der 1541 entfernt bzw. deaktiviert werden (CS/CE/OE auf dauer High), da diese sich sonst mit dem ROM des Multi-Speeder im Adressenkonflikt befinden. Die Folge wäre ein Absturz der Floppy direkt beim einschalten.  

## Welche ROM-Bereiche werden wann eingeblendet?
> BANK 0-3 $A000 - $FFFF<br> 
> BANK 4-5 $8000 - $FFFF <br>
> BANK 6-7 $8000 - $9FFF & $C000 - $FFFF (bis V1.6 wird $8000-8fff - $C000 - $FFFF)<br>

# RAM
Ebenso wie das ROM wird das 32K RAM auch in verschiedenen Bereichen der 1541 eingeblendet. 
Wo das RAM eingeblendet wird, ist von der BANK abhängig, die gerade aktiv ist.

## Welche RAM-Bereche werden wann eingeblendet?
> BANK 0-3 $6000 - $9FFF (32KB)<br>
> BANK 4-5 $6000 - $7FFF (16KB)<br>
> BANK 6-7 $A000 - $BFFF (16KB)<br>

## 1541 Adressdecoder-Spiegel-Problematik
Bei der 1541 hat Commodore am Adressdecoder gespart. 
Die VIAs IC bei $1800 und $1C00 werden in den Adressbereichen bis $8000 immer wieder gespiegel. 
Das kollidiert mit der RAM Erweiterung. Das Problem habe ich behoben, in dem ich die Adressleitung A15 über die Platine entsprechend auf dem Motherboard anpasse. 
Das stoppt das Spiegel-Problem für das RAM. Somit kollidiert ab der Adresse $2000 in der 1541 nichts mehr. 

Mit diesem System können dann verschiedene Floppyspeeder ohne weitere Anpassungen betrieben werden.

![Tabelle mit den Bank 0-7 und welche Speeder dort funktionieren.](https://github.com/FraEgg/commodore-1541-floppydrive-8x-multi-floppy-speeder/blob/master/images/BankTableSpeeder.png?raw=true) 
Das ist ein Beispiel, wie z. B. die verschiedenen Speeder in welcher Bank funktionieren.

# Commodore 1541-I
![Betrieb in der älteren 1541 I](https://github.com/FraEgg/commodore-1541-floppydrive-8x-multi-floppy-speeder/blob/master/images/PCB_V1.5_1541_front.jpg?raw=true)
Hier ist ein Beispiel für den Betrieb in einer älteren 1541-I. Die 6502 CPU und der Adressdecoder und EPROM sind gesockelt. Hier ist genug Platz. Dieses ist eine ältere Version 1.1 und das EPROM ist oben auf der Platine angebracht. Ab der Version 1.4 befindet sich das EPROM auf der Unterseite der Platine.

# Commodore 1541-II
![Multi-Speeder in der 1541 II](https://github.com/FraEgg/commodore-1541-floppydrive-8x-multi-floppy-speeder/blob/master/images/1541_II_PCB_V1.4-0.jpg?raw=true)
Im Gegensatz zur älteren 1541-I ist die 1541-II viel kompakter gebaut. Die Platine findet sich unter der Laufwerksmechanik. Somit ist für Erweiterungen nur wenig Platz. Deshalb löte ich dann die CPU (U1) und den Adressdecoder (U4b1) ohne IC-Sockel direkt auf die Platine. Somit kann die Platine in einem IC-Sockel auf das Mainboard der 1541-II gesteckt werden und stört die Laufwerksmechanik nicht. 

# Bank Switching (JP1-JP3 / DIP-Switch)
Die Kabel links (Gelb, Grün, Blau) (DIP1) legen die aktive Bank fest. Hier kann auch ein DIP-Switch oder Binär-Schalter eingesetzt werden. Macht aber bei der 1541-II wenig Sinn, da man im Betrieb keinen Zugang hat. In desem Beispiel habe ich hier einen Binärschalter über ein Kabel nach außen gelegt. Dieser legt diese Kontakte auf Masse GND (0). Ohne Schalter sind die Adressleitungen A16/A17/18 auf +5V (1) und die Bank 7 ist aktiv. Liegen die drei Kontakte am DIP1 auf Masse GND (0), dann ist die Bank 0 aktiv. 
Ab der Version v1.6 verwende ich jetzt keinen DIP-Schalter mehr, sondern drei Jumper.
 
# Downloads
 - [Meine PCBWay.com Shared Project Page](https://www.pcbway.com/project/shareproject/Multi_Speeder_for_the_Commodore_1541_floppy_disk_drive_I_and_II_d4b53270.html)
 - [Platinen Gerber Datei](https://github.com/FraEgg/commodore-1541-floppydrive-8x-multi-floppy-speeder/tree/master/gerber)
 - [JED-Datei für ATF16V8 (GAL16V8)](https://github.com/FraEgg/commodore-1541-floppydrive-8x-multi-floppy-speeder/tree/master/gal16v8_(pld))
 - [Bilder](https://github.com/FraEgg/commodore-1541-floppydrive-8x-multi-floppy-speeder/tree/master/images)
 - [Schaltplan](https://github.com/FraEgg/commodore-1541-floppydrive-8x-multi-floppy-speeder/tree/master/schematic)
 - [Platine](https://github.com/FraEgg/commodore-1541-floppydrive-8x-multi-floppy-speeder/tree/master/pcb)
 - [Faszination64: Retro Teile wie 1541 Userport-Parallelkabel und C64 ROM-Kernal-Adapter](https://www.faszinationc64.de/)

# WDC 65C02 CPU und andere CPUs
Ab der Platinenversion v1.5 kann auch eine neue WDC W65C02 CPU verwendet werden. Hierzu muss jedoch der optionale Widerstand R1 mit 3,3K und der JP4 geöffnet werden!
Jedoch wird die 1541 dadurch nicht sehr kompatibel. Das original CBM-DOS läuft, die meisten Floppyspeeder nutzen jedoch auch illegale OP-Codes der originalen alten MOS 6502A CPU. Diese kennt die neure WDC W65C02 CPU nicht. Die Folge ist, dass vieles an Software die Schnelllader mit illegalen OP-Codes verwenden nicht funktionieren. Deshalb suche ich noch eine FPGA Emulation die man für meinen Multi-Speeder anstelle einer originalen der MOS 6502A CPU. Rockwell 6502 CPUs und die von UMC sowie SY6502A funktionieren auch problemlos. 

# Spenden
Wer meine Arbeit unterstützen möchte, der kann mir gerne eine Spende über Paypal senden.
[> Spenden <](https://www.paypal.com/donate/?cmd=_s-xclick&hosted_button_id=Q8HXKYARXKT4L&ssrt=1714757590172)

# Haftungsausschluss
Ich gebe mir sehr viel Mühe und lege Wert auf Sorgfalt. Aber ich kann natürlich keine Fehler ausschließen. Das ist ein Hobbyprojekt und ich übernehme deshalb keine Garantie für die Funktion oder Folgen, die ein Umbau/Einbau in deine Geräte mit meinen Projekten mit sich bringen kann. Auch für mögliche Folgeschäden bei beschriebener korrekter Nutzung meiner Platine übernehme ich keinerlei Garantie oder Haftung.

Ich wünsche euch dennoch viel Spaß und bin für Anregungen oder Verbesserungen immer offen. Wie schon beschrieben, wenn einer eine kompatible FPGA MOS 6502 CPU gebaut hat, dann bitte melden :-))) 

# Changes
V1.7 vom 20.06.2024
- BOM aktualisiert.
- Adressdecoder V1.7 optimiert. Bei BANK 6-7 wurde die Einblendung des ROM erweitert. Von $8000 - $8FFF auf $8000 - $9FFF.
- RAM Footprint optimiert.
- Beschriftung JP1-JP3 berichtigt (invertiert)
- PCB Beschriftung optimiert. (TOP/BACK) PIN-Header auf der Rückseite eingerahmt.


Viele Grüße
Frank
