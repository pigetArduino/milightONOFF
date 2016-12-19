Interrupteur pour ampoules MILIGHT
------------------------------------
Nous allons apprendre à contrôler des ampoules milight depuis un Arduino.

![milight](https://github.com/pigetArduino/milightONOFF/raw/master/doc/milight.png)

Les ampoules milights communiquent en 2.4Ghz avec une passerelle Wifi.

![milightRouting](https://github.com/pigetArduino/milightONOFF/raw/master/doc/milightRouting.png)

Nous pouvons communiquer directement avec les ampoules sans passer par la passerelle wifi, à l'aide de module radio nrf24l01+
Nous allons voir comment fabriquer une télécommande ON/OFF

# Composants nécessaires
* 1 Arduino nano (compatible)	2€	   
* 1 Module radio nrf24l01+	1€	   
* 2 Boutons tactiles	2€   
Total	5€    

# Branchement
**Les modules nrf fonctionnent en 3.3V , ne les relier pas en 5v.**

![nrf_pinout](https://github.com/pigetArduino/milightONOFF/raw/master/doc/nrf_pinout.png)

![touchsensor](https://github.com/pigetArduino/milightONOFF/raw/master/doc/touchsensor.png)

```
D4	Bouton OFF (SIG)
D5	Bouton ON (SIG)
D9	NRF (orange)
D10	NRF (jaune)
D11	NRF (bleu)
D12	NRF (violet)
D13	NRF (vert)
3.3V	NRF (rouge)
5V	Boutons (VCC)
GND	Boutons (GND) / NRF (noir)
```

# Vérification (milightONOFF_test.ino)
Nous allons utiliser un programme de test pour vérifier que tout est correctement branché.
* Télécharger http://milightONOFF.madnerd.org
* Ouvrez le croquis **milightONOFF_test** puis téléverser le sur votre **Arduino Nano**.     
* Ouvrez le **moniteur série** (Outils -> Moniteur série)    
Vous devriez voir les paramètres du module radio, si tout les paramètres sont à **0x00** alors le module n'est pas reconnu    
* Appuyez sur les **boutons tactiles** , vous devriez voir le message :    
````
Button ON Pressed 
Button OFF Pressed
````    

Exemple, paramètres du module radio     
```
STATUS		 = 0x0e RX_DR=0 TX_DS=0 MAX_RT=0 RX_P_NO=7 TX_FULL=0
RX_ADDR_P0-1	 = 0xe7e7e7e7e7 0xc2c2c2c2c2
RX_ADDR_P2-5	 = 0xc3 0xc4 0xc5 0xc6
TX_ADDR		 = 0xe7e7e7e7e7
RX_PW_P0-6	 = 0x00 0x00 0x00 0x00 0x00 0x00
EN_AA		 = 0x3f
EN_RXADDR	 = 0x03
RF_CH		 = 0x4c
RF_SETUP	 = 0x07
NRF_CONFIG		 = 0x0e
DYNPD/FEATURE	 = 0x00 0x00
Data Rate	 = 1MBPS
Model		 = nRF24L01+
CRC Length	 = 16 bits
PA Power	 = PA_MAX
```

# Récupération des codes radio (milight_receive.ino)
* Récupérer http://milightreceive.madnerd.org
* Ouvrez le croquis **milightONOFF_receive** puis téléverser le.
* Ouvrez le moniteur série
**Allumer** votre lampe puis **éteignez là**, à l'aide de votre **smartphone**
Notez les codes reçus

```
Press a button on your smartphone to receive radiocodes

0xD8 0x34 0x4A 0x00 0x01 0x04 0x2C .......
0xD8 0x34 0x4A 0x00 0x01 0x03 0x2D .......
```

# Envoyer des codes radio (milightONOFF.ino)

Il ne nous reste plus qu'à essayer de renvoyer ces codes avec notre arduino.

* Ouvrez le croquis **milightONOFF.ino**
Modifier la ligne 23/24 et mettez le code que vous avez précédemment reçu
```
int on [7] = { 0x00, 0x00, 0x00 ,0x00, 0x00, 0x00, 0x01};
int off [7] = { 0x00, 0x00, 0x00 ,0x00, 0x00, 0x00, 0x01};
```
