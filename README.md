# Multi-Speeder für das Commodore 1541 Diskettenlaufwerk I/II V1.5
 
![Multispeeder 1541 I und II](https://raw.githubusercontent.com/FraEgg/commodore-1541-floppydrive-8x-multi-floppy-speeder/master/images/PCB_V1.1-3.jpg) 

Mein Multi-Speeder-Projekt für das Commodore 1541 Diskettenlaufwerk besteht aus einer Erweiterungsplatine, die zwischen 6502 CPU gesetzt wird. Die Platine erweitert den Arbeitsspeicher der 1541 um 32 KB und stellt 8x 64 KB Bänke für DOS-Betriebssysteme auf der 1541 zur Verfügung. Die Erweiterungsplatine lässt sich in den beiden Versionen 1541 I und 1541 II betreiben. Ich habe ebenfalls eine Version für den Klone Oceanic OC-168 gestaltet (auf Anfrage).
Mit der Multi-Speeder-Erweiterungsplatine können u.a. das Original CBM-DOS 2.6, DolphinDos 2.0, SpeedDos 2.7 +, SpeedDos Expert, JiffyDOS und S-JiffyDOS umschaltbar betrieben werden.

Die Adressierung der ROM- und RAM-Bereiche variiert, sodass der Multi-Speeder für die unterschiedlichen Floppyspeeder verwendet werden kann.

Nicht Bestandteil dieses Projekt ist ein SpeedDos kompatibles Parallelkabel zum Userport des C64 und ein ROM-Adapter-Sockel für die Kernals im C64. Dieses können bei bekannten Retro-Shops bezogen werden z.B. Fazination64 (siehe Links unten).   

Ich habe den Schaltplan sowie die entsprechende Eprommer-Datei für den ATF16V8 (GAL16V8 kompatibel) Adressdecoder und die Gerber-Dateien z.B. für PCBWay hier zur Verfügung gestellt. Dies ermöglicht die eigene Herstellung der Milti-Speeder-Platine.

# Bauteile / BOM
Neben der Platine werden folgende Bauteile benötigt:

 - 2x Präzisions-Stiftleiste 20 polig, 2,54mm (Platine zum Mainboard
 - 1x IC-Sockel, 40-polig (U1) (6502 CPU)
 - 1x IC-Sockel, 20-polig (U4b1) (Atmel F16V8 BQL-15PV) (GAL16V8 komp.)
 - 1x IC-Sockel PLCC 32, 32-polig (EEPROM/EPROM)
 - 1x IC Atmel F16V8 BQL-15PV (U4b1) (Adressdecoder)
 - 1x IC SRAM HM62256ALF-70G 28-PIN SOP oder ISSI 62C256AL-45ULI 28-PIN SOP
 - 1x EEPROM AM29LV040B (PLCC32) oder EPROM-OTP AT27C040 (PLCC32)
 - 2x Tantalkondensator, 100 nF bzw. 0,1µF, 35 V radial (C2 und C3) (C1 gibt es nicht mehr :-)
 - 3x Widerstände 4,7 kOhm / 1/4 Watt
 - 1x Dip-Schalter, liegend, 3-polig

Auf der Platine ab Version v1.5 braucht der Wiederstand R4 nicht bestückt werden und der JP1 bleibt offen. Diese sind nur für den Testbetrieb einer WDC W65C02 CPU gedacht.  

# ROMs
Die DOS-KERNALs werden in einem EPROM/EEPROM abgelegt. Das EPROM z. B. 27C040 ist ein 512 KB ROM. Es ist in 8x 64KB Bänke (Bank 0-7) aufgeteilt. Jede Bank $x0000 - $xFFFF spiegelt den 64 KB Speicherbereich der Floppy 1:1 wieder. Wobei nur ein bestimmter Teil der Bereiche in den Speicherbereich der 1541 eingeblendet werden. Das nutzt natürlich den Speicher des EPROM nicht besonders aus, macht aber das Adressdecoding sehr einfach und flexibel. Zudem kostet Speicher nicht mehr die Welt, ganz im Gegensatz zu den 80er-Jahren :-). Beim Betrieb der Multi-Speeder-Platine müssen alle Original ROMs der 1541 entfernt werden, da diese sich sonst mit dem ROM des Multi-Speeder im Adressenkonflikt befinden. Die Folge wäre ein Absturz der Floppy direkt beim einschalten.  

## Welche ROM-Bereiche werden wan eingeblendet?
> BANK 0-3 $A000 - $FFFF<br> 
> BANK 4-5 $8000 - $FFFF <br>
> BANK 6-7 $8000 - $8FFF & $C000 - $FFFF<br>

# RAM
Ebenso wie das ROM wird das 32K RAM auch in verschiedenen Bereichen der 1541 eingeblendet. Wo das RAM eingeblendet wird, ist von der BANK abhängig, die gerade aktiv ist.

## Welche RAM-Bereche werden wann eingeblendet?
> BANK 0-3 $6000 - $9FFF (32KB)<br>
> BANK 4-5 $6000 - $7FFF (16KB)<br>
> BANK 6-7 $A000 - $BFFF (16KB)<br>

## 1541 Adressdecoder-Spiegel-Problematik
Bei der 1541 hat Commodore am Adressdecoder gespart. Die VIAs IC bei $1800 und $1C00 werden in den Adressbereichen bis $8000 immer wieder gespiegel. Das kollediert mit der RAM Erweiterung. Das Problem habe ich behoben, in dem ich die Adressleitung A15 entsprechend auf dem Motherboard verändere. Das stoppt das Spiegel-Problem für das RAM. Somit kollediert ab Adresse $2000 in der 1541 nichts mehr. 

Mit diesem System können dann verschiedene Floppyspeeder ohne weitere Anpassungen betrieben werden.

![Tabelle mit den Bank 0-7 und welche Speeder dort funktionieren.](https://github.com/FraEgg/commodore-1541-floppydrive-8x-multi-floppy-speeder/blob/master/images/BankTableSpeeder.png?raw=true) 
Das ist ein Beispiel, wie z. B. die verschiedenen Speeder in welcher Bank funktionieren.

# Commodore 1541-I
![Betrieb in der älteren 1541 I](https://github.com/FraEgg/commodore-1541-floppydrive-8x-multi-floppy-speeder/blob/master/images/1541_I_PCB_V1.1-0.jpg?raw=true)
Hier ein Beispiel für den Betrieb in einer älteren 1541-I. Die 6502 CPU und der Adressdecoder sind gesockelt. Hier ist genug Platz. Dieses ist eine ältere Version 1.1 und das EPROM ist oben auf der Platine angebracht.

# Commodore 1541-II
![Multi-Speeder in der 1541 II](https://github.com/FraEgg/commodore-1541-floppydrive-8x-multi-floppy-speeder/blob/master/images/1541_II_PCB_V1.4-0.jpg?raw=true)
Im Gegensatz zur älteren 1541-I ist die 1541-II viel kompakter gebaut. Die Platine findet sich unter der Laufwerksmechanik. Somit ist für Erweiterungen nur wenig Platz. 
Deshalb löte ich dann die U1 CPU und den Adressdecoder U4b1 ohne IC-Sockel auf die Platine. 
Somit kann die Platine in einem IC-Sockel auf das Mainboard gesteckt werden und stört die Laufwerksmechanik nicht. 

# Bank Switching (DIP1)
Die Kabel links (Gelb, Grün, Blau) legen die aktive Bank fest. Hier kann auch ein DIP-Switch eingesetzt werden. Macht aber bei der 1541 II wenig Sinn. Ich habe hier einen Binärschalter nach außen gelegt. Dieser legt diese Kontakte auf Masse (0). Ohne Schalter sind die Adressleitungen A16/A17/18 auf 1 und die Bank 7 ist aktiv. Liegen die drei Kontakte auf Masse, dann ist die Bank 0 aktiv. 

# Downloads
 - [Platinen Gerber Datei für PCWay.com](https://github.com/FraEgg/commodore-1541-floppydrive-8x-multi-floppy-speeder/tree/master/gerber)
 - [JED-Datei für ATF16V8 (GAL16V8)](https://github.com/FraEgg/commodore-1541-floppydrive-8x-multi-floppy-speeder/tree/master/gal16v8_pld)
 - [Bilder](https://github.com/FraEgg/commodore-1541-floppydrive-8x-multi-floppy-speeder/tree/master/images)
 - [Schaltplan](https://github.com/FraEgg/commodore-1541-floppydrive-8x-multi-floppy-speeder/tree/master/schematic)
 - [Platine](https://github.com/FraEgg/commodore-1541-floppydrive-8x-multi-floppy-speeder/tree/master/pcb)
 - [Weitere Dokumentation](https://github.com/FraEgg/commodore-1541-floppydrive-8x-multi-floppy-speeder/tree/master/docs)
 - [Faszination64: Retro Teile wie 1541 Userport-Parallelkabel und C64 ROM-Kernal-Adapter](https://www.faszinationc64.de/)

# WDC 65C02 CPU und andere CPUs
Ab der Platinenversion v1.5 kann auch eine neue WDC W65C02 CPU verwendet werden. Jedoch wird die 1541 dadurch nicht sehr kompatibel. Das original CBM-DOS läuft, die meisten Floppyspeeder nutzen jedoch auch illegale OP-Codes der originalen MOS 6502A CPU. Diese kann die WDC W65C02 CPU nicht. Die Folge ist, dass vieles an Software die Schnelllader verwenden nicht funktionieren. Deshalb suche ich noch eine FPGA Emulation die man für meinen Multi-Speeder anstelle einer originalen MOS 6502A CPU verwenden kann. Rockwell 6502 CPUs und die von UMC und SY6502A funktionieren auch problemlos.

# Spenden
Wer meine Arbeit unterstützen möchte, der kann mir gerne eine Spende über Paypal senden.
[> Spenden <](https://www.paypal.com/donate/?cmd=_s-xclick&hosted_button_id=Q8HXKYARXKT4L&ssrt=1714757590172)

# Haftungsausschluss
Ich gebe mir sehr viel Mühe. Aber ich kann natürlich keine Fehler ausschließen. Ich übernehme deshalb keine Garantie für die Funktion oder Folgen, die ein Umbau deiner Geräte mit meinem Projekt mit sich bringen kann. Auch für Folgeschäden bei beschriebener Nutzung meiner Platine übernehme ich keinerlei Garantie oder Haftung.

Ich wünsche euch dennoch viel Spaß und bin für Anregungen oder Verbesserungen immer offen.

Viele Grüße
Frank
