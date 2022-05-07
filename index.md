Hello World ...

and Hello Zhao Nan ...

now this is a temporary page with only a few plain words.

and I will update and maintain this site with RUBY.

definately will stuff this site with some of my expierences and opinions and so on.

or may make this site as my blog, sounds interenting.


kickoff with a small tech discussion (I'm a nerd... :))


* There are total seven USB connectors on MicroServer Gen8 Server; including an internal USB 2.0 connector that is embedded on the system board, four external USB 2.0 connectors on the chassis which two each on the front and rear panels, and two external USB 3.0 connectors on the rear panel.
  + Although these five are all USB 2.0 device connectors but in ESXi's hardware description inventory they are not sharing the same device controller. One of the differences is the numeric code assigned to the USB 2.0 controllers such as Intel Corporation 6 Series/C200 Series Chipset Family USB Enhanced Host Controller #2, or #1. That's because HPE ProLiant MicroServer Gen8 server assigned a dedicated controller to the internal USB connector and MicroSD card slot, and the other controller for the four external USB connectors on chassis.
  + If we already have plugged an internal USB or a MicroSD storage device in to the internal USB connector or MicroSD card slot before, then when we plug an external USB hard disk in to an external USB connector, on ESXi web console's Storage entry > Adapters tag, two USB Storage Controllers show up, such as vmhba32, vmhba33 and so on. And on Devices tag, there are two devices listed such as xxx USB xxx, Type:Disk, Capacity:xxGB, and so on.
* I have to differentiate the external connector form the internal connector for directly passing through the external connector to a VM. A convenient method is SSH connecting to ESXi's terminal, like so:
