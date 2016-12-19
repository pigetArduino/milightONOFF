[Version française](https://github.com/pigetArduino/milightONOFF/blob/master/readme.fr.md)

MILIGHT switch
------------------------------------
How to control MILIGHT bulbs with Arduino.           
Based on Henryk Plötz works : https://hackaday.io/project/5888-reverse-engineering-the-milight-on-air-protocol     

![milight_photo](https://github.com/pigetArduino/milightONOFF/raw/master/doc/milightONOFF.jpg)

![milight](https://github.com/pigetArduino/milightONOFF/raw/master/doc/milight.png)

Milight bulbs are not controlled using WIFI, it uses its own protocol on **2.4Ghz**.

![milightRouting](https://github.com/pigetArduino/milightONOFF/raw/master/doc/milightRouting.png)
Our arduino will directly control the milight bulbs using **nrf24l01+** radio modules without using the WIFI gateway.

# Components
* 1 Arduino nano : 2.50€	   
* 1 nrf24l01+ (radio module) : 1€	   
* 2 Tactiles switchs : 2€   
Total : 5.50€    

# Wiring
**nrf modules use 3.3V not 5V**

![nrf_pinout](https://github.com/pigetArduino/milightONOFF/raw/master/doc/nrf_pinout.png)

![touchsensor](https://github.com/pigetArduino/milightONOFF/raw/master/doc/touchsensor.png)

```
NRF
D9	: orange
D10	: yellow
D11	: blue
D12	: purple
D13	: green
3.3V: red

SWITCHS
D4	OFF SIG
D5	ON SIG
5V: VCC
```

# Check (milightONOFF_test.ino)
First we need to check if everything is correctly wired using a test program.
* Download : http://milightONOFF.madnerd.org
* Open sketch **milightONOFF/milightONOFF_test** and upload it to your **Arduino Nano**.     
* Open the **serial monitor**    

## Check radio module
You should see your radio module parameters, if all parameters are set to **0x00**, the module is not correctly wired       
**Example** 
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

## Switchs
Press the **tactile switchs**, you should see this message :    
````
Button ON Pressed 
Button OFF Pressed
````     

# Get the radio codes (milight_receive.ino)
* Download http://milightreceive.madnerd.org
* Open **milight_receive/milight_receive.ino** sketches and upload it.
* Open the **serial monitor**
* **Turn on** your lamp
* **Turn off** your lamp 
* Copy the received code.
```
Press a button on your smartphone to receive radiocodes

0xD8 0x34 0x4A 0x00 0x01 0x04 0x2C .......
0xD8 0x34 0x4A 0x00 0x01 0x03 0x2D .......
```

# Send radio codes (milightONOFF.ino)

* Open **milightONOFF/milightONOFF.ino**
* Change the code in line **23/24**
* Upload the code
```
int on [7] = { 0x00, 0x00, 0x00 ,0x00, 0x00, 0x00, 0x01};
int off [7] = { 0x00, 0x00, 0x00 ,0x00, 0x00, 0x00, 0x01};
```

