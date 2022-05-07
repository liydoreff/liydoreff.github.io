hello *World* ...
````diff
+ and hello Zhao Nan ...
+ and hello Chris ..
+ and hello Y .
````
>now, this is just a temporary page with only a few plain words and simply made up of *html* and *markdown* syntax.

I shall update and maintain this tiny site with RUBY 😁 later, as sooooon as possible... or never... who knows ..

> shall definitely stuff this site with some of my expierences and opinions and so on.<br>
or may make this site my blog, ... sounds interenting, at least not boring ..

let me kickoff with a small tech discussion (I'm a nerd... 😂)


> there are total seven USB connectors on MicroServer Gen8 Server; including an ***internal** USB 2.0 connector* that is embedded on the system board, four ***external*** USB 2.0 connectors on the chassis which are two each on the front and rear panels, and two external USB 3.0 connectors are on the rear panel.
>  + although these five are all USB 2.0 guys, but in ESXi's hardware description inventory, they are *not* sharing an exectly same device controller. one of the differences is the *numeric code* assigned to the USB 2.0 controllers such as *Intel Corporation 6 Series/C200 Series Chipset Family USB Enhanced Host Controller **#2***, or ***#1***. that's because HPE ProLiant MicroServer Gen8 server assigned a *dedicated* controller to the *internal* USB  2.0 connector and MicroSD card slot, and the other controller is for the four external USB connectors on chassis.
>  + if we already have an plugged storage device on the internal USB connector or MicroSD card slot<sup>1</sup> before, then when we plug an external USB storage device in to an external USB connector, on ESXi web console's **Storage** entry > **Adapters** tag, two USB Storage Controllers show up, such as *vmhba32*, *vmhba33* or *34*. and on **Devices** tag, there are two USB devices listed, such as ***xxx USB xxx***, ***Type:Disk***, ***Capacity:xxGB***, and so on.<br>
>  
> I have to differentiate the external connector form the internal connector for *passing* directly *through* the external connector to a VM.<br>
> 
> a convenient method is **SSH** connecting to ESXi CLI, like so (on MacOS Terminal):
````diff
~$ ssh username@domain name/IP address
````
> enter password, then,
````diff
~$ lspci
````
> PCIe devices inventory should be listed, now I can observe adapters' code number of *Controller **#1*** and ***#2***.
>  + unplug external USB device(s), refresh ESXi web console, and now the only remained adapter code number is the internal USB controller code number.

based on the prior steps, I am able to decide which controller should be dedicated to a VM. (of course the hidden one.)


````diff
+ 1: in fact, the internal USB connector and MicroSD card slot share the same USB controllor
````


May 8, 2022
