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
<details>
  <pre><code>
  there are total seven USB connectors on MicroServer Gen8 Server;<br>
  including an internal USB 2.0 connector that is embedded on<br>
  the system board, and four external USB 2.0 connectors on the<br>
  chassis which are two each on the front and rear panels, and two<br>
  external USB 3.0 connectors are on the rear panel.<br>
  
>  + although these five are all USB 2.0 guys, but in ESXi's<br>
  hardware description inventory, they are not sharing an<br>
  exectly same device controller. one of the differences is<br>
  the numeric code assigned to the USB 2.0 controllers, such<br>
  as Intel Corporation 6 Series/C200 Series Chipset Family USB<br>
  Enhanced Host Controller #2 and #1. that may be because that<br>
  iLO or ESXi has assigned a *dedicated* controller to the<br>
  internal USB 2.0 connector and MicroSD card slot <sup>1</sup>, and the<br>
  other controller is for the four external USB connectors<br>
  on chassis.<br>
  
>  + if we already have an plugged storage device on the internal<br>
  USB connector or MicroSD card slot before, then when<br>
  we plug an external USB storage device in to an external USB<br>
  connector, on ESXi web console's Storage entry > Adapters<br>
  tag, two USB Storage Controllers show up, such as vmhba32, vmhba33<br>
  or 34. and on Devices tag, there are two USB devices listed, such<br>
  as xxx USB xxx, Type:Disk, Capacity:xxGB, and so on.<br>
  
  I have to differentiate the external connector from the internal<br>
  connector for passing directly through the external connector<br>
  to a VM.<br>
  a convenient method is SSH connecting to ESXi CLI, like so (on<br>
  MacOS Terminal):<br>
> + ~$ ssh username@domain name/IP address<br>
  
  enter password, then,<br>
> + ~$ lspci<br>
  
  PCIe devices inventory should be listed, now I can observe<br>
  adapters' code number of Controller #1 and #2.<br>
  
>  + unplug external USB device(s), refresh ESXi web console, and now<br>
  the only remained adapter code number is the internal USB controller<br>
  code number.<br>
  
  based on the prior steps, I am able to decide which controller should<br>
  be dedicated to a VM. (of course the hidden one.)<br>
  <sup>1</sup> in fact, the internal USB connector and MicroSD card slot<br>
  share the same USB controllor<br>

</code></pre>
</details>
May 8, 2022

### RHEL 7 Server, locale error
<details>
  <pre><code>
  状况：<br>

> + 登录账号后，系统提示警告⚠️：”-bash: warning: setlocale: LC_CTYPE: cannot<br>
    change locale (UTF-8): No such file or directory“<br>
    根用户若下载或升级packages，会提示警告⚠️："Failed to set locale, defaulting<br>
    to C"<br>

  RedHat官方给出的解决方案：<br>
> + [RHEL 6](https://access.redhat.com/solutions/1267213 "RHEL 6环境")和[RHEL 8](https://access.redhat.com/solutions/4735471 "RHEL 8环境")  

   而我在RHEL 7.9系统下按照RedHat提出的RHEL 6的解决方案进行测试，并没有解决实际问题；<br>
   另外，因为我的系统是RHEL 7.9，所以无法验证RedHat提出的RHEL 8的解决方案是否有效，因<br>
   为pool不同，我的系统在试图列出"glibc-langpack-en"包时，提示搜索没有结果，可能的原<br>
   因是在RHEL 7的池子里并没有这个包，而在8的池子里或许有，我不确定。<br>
   总之，这两种解决方案对我来说都没有实际意义。<br>
  在RHEL 7系统下的有效解决方案其实很简单，既然这是因为locale引起的问题，那就加上环境变量<br>
  就可以了。<br>
> + vi /etc/environment #系统缺省在/etc下没有environment<br>

  vi中输入:<br>
> + LANG=en_US.utf-8<br>
  LC_ALL=en_US.utf-8<br>

  处女座强迫症从此轻松许多 .. 其实这个问题不是很严重，在7上并不影响升级和安装各种包，只是<br>
  有提示而已 ..
  
</code></pre>
</details>
good night guys<br>
May 9, 2022 21:57 UTC+8

### install RVM and set up Passenger for nginx
<details>
  <pre><code>
  issue:
  
> + error occured with a hint: failed connect<br>
   raw_dot_githubxxx_dot_com 443 connection refused.<br>
   LOL. I am not judging this but what misconduct of<br>
   behaviours github is to that our authority has to<br>
   ban this good tech site?

  solution:
  
> + anyway, assign an ip address such as 185 199 110 133<br>
  to raw_dot_githubxxx_dot_com instead of directly using<br>
  the url so I am able to cross the damn barrier and<br>
  fetch the rvm package.<br>

  configure passenger.conf for nginx<br>
  
> + vi /etc/nginx/conf.d/passenger.conf # edit or create with:<br>
    passenger_root /usr/share/ruby/vendor_ruby/phusion_passenger/locations.ini;<br>
    passenger_ruby /home/hli/.rvm/rubies/ruby-2.7.2/bin/ruby;<br>
    passenger_instance_registry_dir /var/run/passenger-instreg;<br>
    
</code></pre>
  </details>
good morning guys<br>
May 15, 2022

### error occured when start nginx
<details>
  <pre><code>
issue:<br>
  
  nginx: [emerg] bind() to 0.0.0.0:80 faild (98: Address already in use).<br>
  obviously that is because that some app occupied 0.0.0.0:80. I should<br>
  find it and kill it.<br>
> + sudo netstat -ntlp # this command lists all active programs with<br>
    their pid, protocol, ip address and port<br>
    sudo kill xxxx # kill the one occupied 0.0.0.0:80

  restart nginx service<br>
> + sudo service nginx restart

  check nginx's status<br>
> + sudo systemctl status nginx.service

  check passenger configuration status<br>
> + sudo passenger-config validate-install

  check passenger memory usage status<br>
> + sudo passenger-memory-stats

</code></pre>
  </details>
good noon<br>
May 15, 2022
