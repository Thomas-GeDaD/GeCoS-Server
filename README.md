# GeCoS-Server
Python GeCoS Server Script. Ansprechbar per WebSocket<br>


### Befehlsaufbau:<br>
Befehl in geschweiften klammern verpackt: ```{}``` <br>
Trennzeichen: ```;``` <br>
Erste Info: Funktion (3Buchstaben) <br>
2. Info: BUS 0-2 <br>
3. Info: Modul Adresse <br> 
n. Sachinformation (z.B.: Port Status) <br> 
Antwort mit Befehl + Status <br>

Bsp.: Output setzen, alle Ausgänge einschalten:  <br> 
Befehl:     ```{SOM;1;0x24;65535}``` <br>
Antwort:    ```{SOM;1;0x24;65535;OK}``` <br>

Es werden nur Module ausgelesen die bei der Modulsuche(MOD) gefunden wurde. <br>

### Funktionen:<br>
"SAI" = Status All IN -> Liest alle Eingangsmodule und sendet aktuellen Status<br>
"SAO" = Status All Out -> Liest alle Ausgangsmodule und sendet aktuellen Status<br>
"MOD" = Modulsuche -> Sucht nach Modulen und Antwortet mit Moduladressen<br>
"SPWM" = Status All PWM -> Liest alle PWM Module aus und sendet aktuellen Status<br>
"SRGBW" = Status All RGBW -> Liest alle RGBW Module aus und sendet aktuellen Status<br>
"SOM" = "Set Output Module" -> INT Big = port A; Litte = Port B<br>
"PWM" = "Set PWM Module" -> {PWM;I2C-Kanal;Adresse;PWMKanal;Status;Wert} Status=0/1 (0=Aus,1=Ein), Wert=0-4095<br>
"RGBW" = "Set RGBW Module" -> {RGBW;I2C-Kanal;Adresse;RGBWKanal;StatusRGB;StatusW;R;G;B;W} Status=0/1 (0=Aus,1=Ein), R/G/B/W=0-4095 <br>
"SAM" = "Status Analog Module" -> {SAM;0;0x69;AnalogChannel;Resolution;Amplifier}  <br>
"RRTC" = Read RTC  -> {RRTC} -> {RRTC;TT;MM;JJJJ;HH;MM;SS;OK}  <br>
"SRTC" = Set RTC    ->  {SRTC;TT;MM;JJJJ;HH;MM;SS;TEMP} <br>
Kanal 0-2<br>

### MOD - Antworten<br>
{MOD;0;0x24;OUT}    -> 16Out erkannt<br>
{MOD;0;0x20;IN}     -> 16In erkannt<br>
{MOD;0;0x50;PWM}    -> PWM erkannt<br>
{MOD;0;0x58;RGBW}   -> RGBW erkannt<br>
{MOD;0;0x68;ANA}    -> Analog erkannt<br>
{MOD;0;0x05;UNB}    -> Unbekanntes i2c device<br>

### Einrichten als Service:<br>

sudo nano gecos.service<br>
```
[Unit]
Description=GeCoS WebService
After=multi-user.target

[Service]
Type=simple
ExecStart=/usr/bin/python3 /usr/local/bin/Symcon.py
Restart=on-abort

[Install]
WantedBy=multi-user.target
```
sudo chmod 644 /lib/systemd/system/gecos.service<br>
chmod +x /usr/local/bin/Symcon.py<br>
sudo systemctl daemon-reload<br>
sudo systemctl enable gecos.service<br>
sudo systemctl start gecos.service<br>
sudo systemctl status gecos.service<br>
sudo systemctl restart gecos.service<br>
sudo systemctl stop gecos.service<br>



