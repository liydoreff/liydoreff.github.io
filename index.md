hello *World* ...
````diff
+ and hello Z ...
+ and hello C ..
+ and hello Y .
````
>now, this is just a temporary page with only a few plain words and simply made up of *html* and *markdown* syntax.

I shall update and maintain this tiny site with RUBY 😁 later, as sooooon as possible... or never... who knows ..

> shall definitely stuff this site with some of my expierences and opinions and so on.<br>
or may make this site my blog, ... sounds interenting, at least not boring ..

let me kickoff with a small tech discussion (I'm a nerd... 😂)

### MicroServer Gen8 USB devices passthrough ESXi VMs
> there are total seven USB connectors on MicroServer Gen8 Server; including an ***internal** USB 2.0 connector* that is embedded on the system board, four ***external*** USB 2.0 connectors on the chassis which are two each on the front and rear panels, and two external USB 3.0 connectors are on the rear panel.
>  + although these five are all USB 2.0 guys, but in ESXi's hardware description inventory, they are *not* sharing an exectly same device controller. one of the differences is the *numeric code* assigned to the USB 2.0 controllers, such as *Intel Corporation 6 Series/C200 Series Chipset Family USB Enhanced Host Controller **#2*** and ***#1***. that may be because that iLO or ESXi has assigned a *dedicated* controller to the *internal* USB 2.0 connector and MicroSD card slot, and the other controller is for the four external USB connectors on chassis.
>  + if we already have an plugged storage device on the internal USB connector or MicroSD card slot<sup>1</sup> before, then when we plug an external USB storage device in to an external USB connector, on ESXi web console's **Storage** entry > **Adapters** tag, two USB Storage Controllers show up, such as *vmhba32*, *vmhba33* or *34*. and on **Devices** tag, there are two USB devices listed, such as ***xxx USB xxx***, ***Type:Disk***, ***Capacity:xxGB***, and so on.<br>
>  
> I have to differentiate the external connector from the internal connector for *passing* directly *through* the external connector to a VM.<br>
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

### RHEL 7 Server, locale error  
> 状况：
> + 登录账号后，系统提示警告⚠️：”-bash: warning: setlocale: LC_CTYPE: cannot change locale (UTF-8): No such file or directory“
> + 根用户若下载或升级packages，会提示警告⚠️："Failed to set locale, defaulting to C"
>  
> RedHat官方给出的解决方案是：
> + [RHEL 6](https://access.redhat.com/solutions/1267213 "RHEL 6环境")和[RHEL 8](https://access.redhat.com/solutions/4735471 "RHEL 8环境")  
>
>  而我在RHEL 7.9系统下按照RedHat提出的RHEL 6的解决方案进行测试，并没有解决实际问题；另外，因为我的系统是RHEL 7.9，所以无法验证RedHat提出的RHEL 8的解决方案是否有效，因为pool不同，我的系统在试图列出"glibc-langpack-en"包时，提示搜索没有结果，可能的原因是在RHEL 7的池子里并没有这个包，而在8的池子里或许有，我不确定。   
> 总之，这两种解决方案对我来说都没有实际意义。 


***在RHEL 7系统下的有效解决方案其实很简单，既然这是因为locale引起的问题，那就加上环境变量就可以了。***
````diff   
vi /etc/environment #系统缺省在/etc下没有environment
````
***在vi中输入:***
````diff
LANG=en_US.utf-8
LC_ALL=en_US.utf-8
````
> 处女座强迫症从此轻松许多 .. 其实这个问题不是很严重，在7上并不影响升级和安装各种包，只是有提示而已 ..  

good night guys<br>
May 9, 2022 21:57 UTC+8
