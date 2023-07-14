<h1>Configuring a firewall</h1>

<h2>Description</h2>

I will configure a basic firewall rule on a Linux server to block port 80 (HTTP) traffic. Next I will reconfigure the server to accept HTTP connections, and then I will configure iptables logging for all traffic. Finally, I will display log file traffic.
<br />

<h2>Environments Used </h2>

- <b>Windows 10</b>
- <b>Kali Linux<b>

<h2>Program walk-through:</h2>

<p align="center">

Sign in to the PT1-Kali virtual machine as root.

Open a terminal using the menu at the top of the screen.

Run the following command to start the Apache web service:

systemctl start apache2: <br/>
<img src="https://i.imgur.com/JXblAyr.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<img src="https://i.imgur.com/u64kDK0.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Switch to the LX1 virtual machine. This virtual machine will act as the HTTP client for this task.

Sign on.

From the Applications menu, select Firefox.

In the Firefox address bar, enter http://10.1.0.192 to connect to the 515 support web site.

The connection attempt succeeds. iptables currently accepts all inbound connection attempts.

The default Apache web page.:  <br/>
<img src="https://i.imgur.com/RXmHeAz.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Switch to the PT1-Kali virtual machine, and then configure the iptables service to DROP inbound HTTP connections by port number 80 by using the following command:

iptables -I INPUT 1 -p tcp --destination-port 80 -j DROP

Display the iptables rules and observe that the HTTP service is specified by port number 80.

iptables -S

Run the following command to redirect the output of the iptables -S command to a text file:

iptables -S > ~/iptables.txt    : <br/>
<img src="https://i.imgur.com/ymZHOvX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Switch to the LX1 virtual machine, launch Firefox, and then attempt to connect to the Kali http://10.1.0.192 test site again.

Firefox spends several minutes attempting to connect. Eventually, the connection attempt fails.:  <br/>
<img src="https://i.imgur.com/Ops36kI.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
I will now reverse the firewall configuration, once again permitting inbound traffic over port 80. I will then configure iptables to log all inbound connections.

Select the PT1-Kali virtual machine.

Insert a new iptables rule at line 1 so that connections for port 80 are accepted:

iptables -I INPUT 1 -p tcp --destination-port 80 -j ACCEPT
The easiest way is to use the up arrow key to recall recent commands, and then backspace over DROP and type in ACCEPT.

Enable iptables logging.

iptables -I INPUT 1 -j LOG
Observe that I am now placing the LOG rule at the top of the list. All traffic will be logged before being processed by the rules that follow after the LOG rule. LOG is a non-terminating target, so rules following it are still processed.

Display the iptables rules and observe that the destination port 80 ACCEPT and LOG rules are listed above the DROP rule. Firewall rules are processed in order.

iptables -S:  <br/>
<img src="https://i.imgur.com/rT1yGK3.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Switch back to the LX1 VM, and attempt or refresh the http://10.1.0.192 web connection again with Firefox.

Switch to the PT1-Kali VM.

Display destination port 80 traffic in the /var/log/kern.log log file by using the tail command.

tail /var/log/kern.log | grep "80":  <br/>
<img src="https://i.imgur.com/6nfpTVa.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />

</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
