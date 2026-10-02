---
title: "configuring Wildcard TDM400P"
date: 2007-11-19
categories: 
  - "asterisk"
  - "asterisk"
  - "freepbx"
  - "technical"
  - "voip"
tags: 
  - "asterisk-dubai-uae"
  - "freepbx-voip-uae"
  - "ip-phones-dubai"
  - "voip-dubai"
---

In this tutorial we will give you an example which includes one FXO and one FXS station interfaces. TDM400P card with such modules is called TDM11B. Below you will find description of the name model used by Digium.  
  
The Wildcard TDM11B is a half-length PCI card that supports FXO and FXS interfaces for connecting analogue or ADSI telephones and regular POTS through a computer. TDM11B card is from the TDM400p family. This card can be extended with additional FXO or FXS modules (maximum 4 modules per card).  
Can we find what modules are plugged in the card just from the name? Yes, the name is divided in several parts: **TDM X Y B**. **X** – shows how many FXS modules are in the card, **Y** – is the number FXO modules. So TDM11B – has one FXS module, and one FXO module  
  
**1.PREREQUISITES**

  
To install such Wildcard we assume that you already have installed Asterisk PBX on the PC you would like to install the TDM card.  
For the hardware requirements you need minimum 500Mhz Pentium III or better with 64 MB.  

  
  
**2.INSTALLATION**

  
Find empty PCI slot  
Have a look at the card before placing it in the PCI slot. Find empty PCI slot and plug the card there, next you have to plug power cable in the card. There is a red and a green module on it:  

  
Green Module is FXS – Foreign Exchange Station  
Red Module is FXO – Foreign Exchange Office  

  
[![](/images/wildcard_tdm400p_image273605.png)](https://www.asteriskguru.com/tutorials/wildcard_tdm400p_image339.png)  
An FXS device initiates and sends signals to an FXO device. The telephone that receives the calls is the last FXO device (if you have several FXO devices) and when the signal is received from the FXS device the telephone has to ring.  
  
Plug the cables  
To connect the device to your network – connect the FXO port to the outside lines and local PSTN phone to the FXS port. The FXO uses FXS signalling and the FXS device uses FXO signalling.  
  
When you have everything connected, turn on the Asterisk PBX PC and configure the device there.  
  
Did your OS recognize the card  
By typing 'lspci' you will receive a list of all the PCI devices you have. Note that Digium cards are recognized under different name:  

  
\- **Communication controller: Xilinx Corporation: Unknown device 0314 (rev 01)** for the TE410p / TE405p  
\- **Communication controller: Tiger Jet Network Inc. Tiger3XX Modem/ISDN interface / Network controller: Tiger Jet Network Inc. Tiger3XX Modem/ISDN interface** for the TE110p or the TDM400p  
\- **Network controller: Jens Scoenfeld** for the TE110p or the TDM400p  
[![](/images/wildcard_tdm400p_image273606.png)](https://www.asteriskguru.com/tutorials/wildcard_tdm400p_image340.png)  

  
Zaptel installation  
If you have not install the zaptel on your asterisk you have to do it now. The PSTN calls on asterisk are passing through Zap channel. For zaptel you need to install the zlib1g-dev module.  

```bash
apt-get install zlib1g-dev
```

  
Now in order to compile it  

```bash
cd /usr/src/asterisk/zaptel
make linux26
make install
```

  
And then recompile the Asterisk  

```bash
cd /usr/src/asterisk/asterisk
make
make install
```

  

  
  
**3.CONFIGURATION**

  
In order to make calls you have to register users and extensions (respectively in **iax.conf** and **extensions.conf**) and to adjust the zaptel protocol settings in **/etc/zaptel.conf** and **/etc/asterisk/zapata.conf** .  
  
configuring zaptel.conf  
In **zaptel.conf** it is enough to set the type of signalling for FXO and FXS. As we mentioned above FXO uses fxs signalling and FXS uses fxo signalling. For FXO devices you can use one of the following signalling methods.  

  
fxsls : Channel(s) are signalled using FXS Loopstart protocol  
fxsgs : Channel(s) are signalled using FXS Groundstart protocol  
fxsks : Channel(s) are signalled using FXS Koolstart protocol  

  
And for FXS one of the types standing below  

  
fxols : Channel(s) are signalled using FXO Loopstart protocol  
fxogs : Channel(s) are signalled using FXO Groundstart protocol  
fxoks : Channel(s) are signalled using FXO Koolstart protocol  

  
  
Here is my simple configuration  
![](/images/wildcard_tdm400p_image341.png)  
Note that **fxoks=1** means that the FXO Koolstart protocol signalling used for the first device which has to be FXS. So have in mind the order of the devices on the TDM card when you register the signalling for each one.  
  
With **loadzone** and **defaultzone** you can define the tone zone which to be loaded where the zone is a two-letter country code.  
  
configuring zapata.conf  
In **zapata.conf** some channel configurations are to be made. As I have just two devices, an FXS and an FXO, so I will have just two channels. Here is how I configured my zapata.conf.  
![](/images/wildcard_tdm400p_image342.png)  
  
As you see there some 'global' settings above – they take effect for each channel defined.  

  
**Context** is the extensions context where the channels will be used (if omitted the default context will be used)  
**Usecalledid** is for using or not called id (may be omitted)  
**Hidecalledid** is whether or not to hide the caller id (may be omitted)  
**Immediate** specifies whether a channel should be answered immediately or not  
  
**Signaling** is the signalling defined in **/etc/zaptel.conf**. Just add an underscore before the protocol type (signaling = fxo\_ks)  
**Echocancelation** is used for enabling echo cancellation. The valid values are: 'yes', 'no' or a power of two from 32 to 256. 
**Group** is used for device logically the outgoing calls. Groups can be defined from 0 to 63 as well as multiple groups can be specified.  
**Channel** is channel number for each channel.  

  
  
User registration  
I also have a user registered in **iax.conf**, but you can also place calls without any user registered from CLI (see the dial command).  
![](/images/wildcard_tdm400p_image343.png)  
  
Register extensions  
And the last step is to register valid numbers in **extensions.conf**.  
[![](/images/wildcard_tdm400p_image273607.png)](https://www.asteriskguru.com/tutorials/wildcard_tdm400p_image344.png)  
  
Here when you enter the standard extension an operator picks up, the variables concerning the caller is displayed in the CLI – the name of the caller, the number of the caller and the id of the caller, then some welcome message is played. After this you can leave a mail on the voicemail and then the operator hangs you up. You can register a voicemail in **/etc/asterisk/voicemail.conf** by writing the example below. Here you can learn more about [voicemail](https://www.asteriskguru.com/tutorials/asterisk_voicemail.html).  

  
**\[vm-test\]**  

_voicemail context is created_

**1111 => 123,gogh,gogh@some\_domain.com**

_**1111** mailbox is created with password **123** and assigned to user named **gogh** with email **gogh@some\_domain.com** to notify the user when new message is received._

  

  
When you dial some extension like 0ZXXXXX (where 'Z' is number from 1-9 and 'X' is number from 0-9) you are send to a Macro which places a zero in front of the number and dials the new number. The reason to have additional zero in the beginning of the number is that all outgoing calls for my server must have a zero in front.  

  
**exten => s,2,Dial(Zap/2/0{$ARG})**  

_this dials the number you want through Zap on channel 2. As you saw above in my case channel 2 is FXO channel with fxsks signalling and this channel is for outgoing calls._

  

  
I have also created a conference room. By dialling **9090** you can enter in it. Conference room can be defined in **/etc/asterisk/meetme.conf**. Here is a simple example:  

```ini
[rooms]
conf => 9090[,pin][,adminpin]
```

_This creates conference room with number **9090**, without authorization when entering._

  

  
  
By dialling **1001** you call to the user we registered on asterisk PC.  
  
Extensions **1111** and **2222** dial some mobile phones – in the first case through group 2 **Dial(Zap/g2/…)** and in the second through channel 2 **Dial(Zap/2)**. In the example here channel 2 and group 2 are the same and they use the FXO channel, i.e. outgoing calls.  
  
Extension **3333** dials channel one on Zap.  

**exten => 3333,1,Dial(Zap/1)**  

_This dials channel 1 which is controlled by FXS. As we said in the beginning the local analogue line is connected to this place, so your local PSTN phone will be dialled._

  

  

  
**4.TECHNICAL DATA**\*

  
**Target Application:**  

\- Small Office Home Office (SOHO) applications  
\- Gateway termination to analogue telephones  
\- Add inexpensive analogue phones to existing PBX  
\- Wireless point-to-point applications between Asterisk servers  

  
**Services and features:**  

\- CallerID and Call Waiting CallerID  
\- ADSI phones  
\- PCI half-length slot  
\- RJ-11C connector  

  
**Environment conditions:**  

\- Operation range: 0º to 50º C, 32º to 122º F  
\- Storage range: -20º to 65º C, 4º to 149º F  
\- Huminidity: 10-90% non-condensing  

  
**Standard Configuration**  

  
[![](/images/wildcard_tdm400p_image273664.jpg)](https://www.asteriskguru.com/tutorials/wildcard_tdm400p_image426.jpg)  

  

  

  

\* -_Official Information from Digium for [Wildcard TDM400P Family](https://www.digium.com/index.php?menu=product_detail&category=hardware&product=TDM400P)_
