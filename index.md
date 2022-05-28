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
  there are total seven USB connectors<br>
  on MicroServer Gen8 Server; including<br>
  an internal USB 2.0 connector that is<br>
  embedded on the system board, and<br>
  four external USB 2.0 connectors on the<br>
  chassis which are two each on the front<br>
  and rear panels, and two external USB 3.0<br>
  connectors are on the rear panel.<br>
  although these five are all USB 2.0 guys,<br>
  but in ESXi's hardware description<br>
  inventory, they are not sharing an<br>
  exectly same device controller. one of<br>
  the differences is the numeric code<br>
  assigned to the USB 2.0 controllers, such<br>
  as Intel Corporation 6 Series/C200 Series<br>
  Chipset Family USB Enhanced Host Controller<br>
  #2 and #1. that may be because that<br>
  iLO or ESXi has assigned a *dedicated*<br>
  controller to the internal USB 2.0<br>
  connector and MicroSD card slot <sup>1</sup>,<br>
  and the other controller is for the<br>
  four external USB connectors on chassis.
  
> if I already have a plugged storage device<br>
  on the internal USB connector or MicroSD<br>
  card slot before, then when I plug an<br>
  external USB storage device in to an external<br>
  USB connector, on ESXi web console's Storage<br>
  entry > Adapters tag, two USB Storage<br>
  Controllers show up, such as vmhba32, vmhba33<br>
  or 34. and on Devices tag, there are two USB<br>
  devices listed, such as xxx USB xxx,<br>
  Type:Disk, Capacity:xxGB, and so on.

  I have to differentiate the *controller* for<br>
  external connectors from the *controllor* for<br>
  internal connector so I am able to pass<br>
  directly through the external connectors'<br>
  controller to a VM. a convenient method is<br>
  to establish a SSH connection to ESXi CLI,<br>
  like so (on MacOS Terminal):  
````diff
]$ ssh username@domain name/IP address
````
  enter the password, then,
````diff
]$ lspci
````
  PCIe devices inventory should be listed,<br>
  now I can observe adapters' code number<br>
  of Controller #1 and #2.
  
> unplug external USB device(s) refresh<br>
  ESXi web console, and now the only remained<br>
  adapter code number is the internal USB<br>
  controller code number.
  
  based on the previous steps, I am able to decide<br>
  which controller should be dedicated to a VM.<br>
  (of course the hidden one.)
  
  <sup>1</sup> in fact, the internal USB connector<br>
  and MicroSD card slot share the same USB<br>
  controllor

</code></pre>
</details>
May 8, 2022

### RHEL 7 Server, locale error
<details>
  <pre><code>
  状况：<br>

> + 登录账号后，系统提示警告⚠️：<br>
    ”-bash: warning: setlocale: LC_CTYPE:<br>
    cannot change locale (UTF-8): No such<br>
    file or directory“<br>
    根用户若下载或升级packages，会提示警告⚠️：<br>
    "Failed to set locale, defaulting<br>
    to C"<br>

  RedHat官方给出的解决方案：<br>
> + [RHEL 6](https://access.redhat.com/solutions/1267213 "RHEL 6环境")和[RHEL 8](https://access.redhat.com/solutions/4735471 "RHEL 8环境")  

 让人懊恼的是我在RHEL 7.9系统下按照RedHat提出<br>
 的RHEL 6的解决方案进行测试，并没有解决实际问题；<br>
 另外，因为我的系统是RHEL 7.9，所以无法验证RHEL 8<br>
 的方案的可行性，我猜是因为pool不同，我的系统在试<br>
 图列出"glibc-langpack-en"包时，提示搜索没有<br>
 结果，可能的原因是在RHEL 7的池子里并没有这个包，<br>
 而在8的池子里或许有；也可能是我没有attach某个pool，<br>
 或者是我没有安装某个repo，这让我想起了BSD的ports。<br>
 具体原因我不确定。<br>
 总之，这两种解决方案对我来说都没有实际意义。<br>
 尽管如此，实际上在RHEL 7系统下的有效解决方案其实很<br>
 简单，既然这是因为locale引起的问题，那就加上环境变量<br>
 就可以了。
 
````diff
sudo vi /etc/environment
# 系统缺省的environment文件是空的
````

  输入:<br>
````diff
# setup globle environment as en_US
LANG=en_US.utf-8
LC_ALL=en_US.utf-8
````

  处女座强迫症从此缓解许多 .. 其实这个问题并不严重，<br>
  在7上并不影响升级和安装各种包，只是有提示而已 ..
  
</code></pre>
</details>
good night guys<br>
May 9, 2022 21:57 UTC+8

### install RVM and set up Passenger for nginx
<details>
  <pre><code>
  issue:
  
> error occured with a hint: failed connect<br>
  raw_dot_githubxxx_dot_com 443 connection<br>
  refused.<br>
  LOL. I am not judging this but what misconduct<br>
  of behaviours github is to that our authority<br>
  has to ban this good tech site?

  solution:
  
> anyway, assign an ip address such as<br>
  [185 199 108(109,110,111) 153]
  [192 30 252 153(154)]
  to raw_dot_githubxxx_dot_com<br>
  instead of directly using the url so that I<br>
  am able to cross the damn barrier and<br>
  fetch the rvm package.<br>

  configure passenger.conf for nginx<br>
````diff
sudo vi /etc/nginx/conf.d/passenger.conf # edit or create with:<br>
````

  input:
````diff
passenger_root /usr/share/ruby/vendor_ruby/phusion_passenger/locations.ini;
passenger_ruby /home/hli/.rvm/rubies/ruby-2.7.2/bin/ruby;
passenger_instance_registry_dir /var/run/passenger-instreg;
````

</code></pre>
  </details>
good morning guys<br>
May 15, 2022

### error occured when start nginx
<details>
  <pre><code>
issue:<br>
> nginx: [emerg] bind() to 0.0.0.0:80<br>
  faild (98: Address already in use).<br>
  obviously that is because some<br>
  app occupied 0.0.0.0:80. I should<br>
  find it and kill it.

````diff
# this command lists all active programs with their pid, protocol,
# ip address and port, and so on
sudo netstat -ntlp
sudo kill xxxx # kill the one occupied 0.0.0.0:80
````

restart nginx service:
````diff
sudo service nginx restart
````

check nginx's status:
````diff
sudo systemctl status nginx.service
````

check passenger configuration status:
````diff
sudo passenger-config validate-install
````

check memory status:
````diff
sudo passenger-memory-stats
````

</code></pre>
  </details>
good noon<br>
May 15, 2022
