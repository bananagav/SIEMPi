
Getting the new wifi bricked everything, so we're starting from scratch :) 

Shouldn't take as long as it used to before, trying to get this up and running asap

Username - **REDACTED**
Password - **REDACTED**
Hostname - siem.local

Going to be working off of this for now

https://logz.io/blog/elk-stack-raspberry-pi/

nvm not working

https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-22-04




curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg

echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list

sudo apt update

sudo apt install elasticsearch

sudo apt install kibana

Some configuration to be done on both, 

sudo nano /etc/kibana/kibana.yml
	setting server.port to 5601
	setting server.host to <ip>
sudo nano /etc/elasticsearch/elasticsearch.yml
	setting network.host: localhost
		we do this to allow the server to request only to itself and not allow any outside parties to query the elastic api

Now I need to open up the kibana dashboard for access from the outside... aka my pc

sudo ufw allow ssh
sudo ufw allow 5601 (kibana dashboard)


after that, time to start up elastic and kibana

systemctl start elasticsearch
systemctl start kibana

and to enable them to start on reboot...

systemctl enable elasticsearch && systemctl enable kibana

and a quick check to make sure it's working... 

![[Pasted image 20240611045928.png]]

It's live!
\
Now, this is an older version, but Kibana has an upgrade assistant for upgrading to 8.x

https://www.elastic.co/guide/en/elasticsearch/reference/8.0/deb.html

This guide works flawlessly (didn't work before... weird)

either way, it's working now. 



Next day, not working

Need to set up security .

Before, to be able to get it running I set xpack.security.enrollment.enabled to false, now it's time to set it all up.







Full Restart again, this time with the newest version to start off. Auto enables security which gives me less of a headache

--------------------------- Security autoconfiguration information ------------------------------

Authentication and authorization are enabled.
TLS for the transport and HTTP layers is enabled and configured.

The generated password for the elastic built-in superuser is : **REDACTED**

If this node should join an existing cluster, you can reconfigure this with
'/usr/share/elasticsearch/bin/elasticsearch-reconfigure-node --enrollment-token <token-here>'
after creating an enrollment token on your existing cluster.

You can complete the following actions at any time:

Reset the password of the elastic built-in superuser with
'/usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic'.

Generate an enrollment token for Kibana instances with
 '/usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana'.

Generate an enrollment token for Elasticsearch nodes with
'/usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s node'.

-------------------------------------------------------------------------------------------------

Above is my autoconfigs. leaving for when I need to log in with kibana

Added the IP to the kibana.yml file,

server.address: "<ip>"

added a firewall rule to allow 5601 and 22

started kibana, creating elasticsearch token to set up

sudo ./elasticsearch-create-enrollment-token --scope **kibana**

**REDACTED**

Above is my enrollment token



Not going to use the default elastic superuser, creating a new user to auth as

**REDACTED**

Adding the Elastic Agent to my computer... 

$ProgressPreference = 'SilentlyContinue' Invoke-WebRequest -Uri https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.14.0-windows-x86_64.zip -OutFile elastic-agent-8.14.0-windows-x86_64.zip Expand-Archive .elastic-agent-8.14.0-windows-x86_64.zip -DestinationPath . cd elastic-agent-8.14.0-windows-x86_64 .\elastic-agent.exe install

Install Script, made directory Elastic 


Now I have to uninstall my last version of elastic agent on my computer, and it's being a headache of course

Have to uninstall from the C:\Programs Files\Elastic\ directory using .\elastic-endpoint.exe uninstall

Didn't work, and out of rage deleted the Program Files\Elastic directory and now it's not working even more lol


https://discuss.elastic.co/t/elastic-agent-is-installed-but-currently-broken-service-exists-but-installation-path-is-missing/334610/3

Uninstalled, just to properly reinstall, will see if this works

reinstalling with fleet server tokens, wireshark is showing that it's still working but this is wild it's taking so long. 

After waiting and waiting, I retried with -f, showing some errors but it installed the service. Will have to see if that worked or if its going to be tempermental again. 


Working so far, getting data into my elastic instance. Time to set up some more integrations, like the Threat IP module, and some other things like sysmon etc. 



Adding Alienvault OTX threat intel ingestion as well. 

Quick lil API key

the command "kibana-encryption-keys" creates some for us, which works well, added those and kibana is not yelling at me anymore 


My SiemPi is back up and running! 







