11/08/2023

Initially, I tried using a Raspberry Pi OS distro for the project but found issues trying to install Elasticsearch. I've since created a new instance with Ubuntu 22.04

11/09/2023

After setting up SSH to my Raspberry Pi for Headless operation, I've continued with my plan to get my elasticstack install completed

After initial download and configuration, I've run into an error with starting the service

After running "systemctl start elasticsearch.service"

The Service fails to run, and displays Exit Code 1.

After looking through the Elastic Log file, the Error is

fatal exception while booting Elasticsearch java.lang.IllegalArgumentException: setting [cluster.initial_master_nodes] is not allowed when [discovery.type] is set to [single-node]

After commenting out that line, the service runs! Time to continue with trying to get everything running

New problem, when I try to Curl the service to test connectivity, it shows

{"error":{"root_cause":[{"type":"security_exception","reason":"unable to authenticate user [gav] for REST request [/]","header":{"WWW-Authenticate":["Basic realm="security" charset="UTF-8"","Bearer realm="security"","ApiKey"]}}],"type":"security_exception","reason":"unable to authenticate user [gav] for REST request [/]","header":{"WWW-Authenticate":["Basic realm="security" charset="UTF-8"","Bearer realm="security"","ApiKey"]}},"status":401}

Which looks to me like the security features are enabled, causing me to need to provide credentials(Which I haven't set up yet)

Created auto-gen password with

./bin/elasticsearch-setup-passwords -u elastic

Installing Kibana

https://www.elastic.co/guide/en/kibana/8.11/deb.html

Given that the initial keyring and repo update added kibana, install went smoothly

Logstash is now installed as well.

A few hours later, now 10:03.

Working on linking Kibana and Elastic, after some troubleshooting I've found a forum post about how to configure these after I ran into an issue with my user/pass linking up along with other things

The exact error I was getting was this

[ERROR][elasticsearch-service] Unable to retrieve version information from Elasticsearch nodes. self-signed certificate in certificate chain

https://discuss.elastic.co/t/kibana-unable-to-retrieve-version-information-from-elasticsearch-nodes-self-signed-certificate-in-certificate-chain/299159/3

Now after working with the .ymls for elastic and kibana, I've created an enrollment token to sync the two.

![image](https://github.com/bananagav/ELKPi/assets/117794258/a252a545-d8e9-4301-9ba0-8e527d77cb95)


11/10/2023 - Elastic Agents Deployment

Now that I have my dashboard set up, it's time to start getting some data into Elastic.

Starting with my personal computer, it's time to figure out how to install the Elastic Agent

![image](https://github.com/bananagav/ELKPi/assets/117794258/013ab8b9-8d6b-489e-b7e5-285b872a7b85)


After a few hours, and trying to install a standalone agent, to no avail, I've switched to trying to use Fleet...

After a bit of troubleshooting getting the old agent uninstalled and reinstalling the new Fleet server, enrolling, and connecting to Kibana, they are now communicating

![image](https://github.com/bananagav/ELKPi/assets/117794258/412299cf-be98-48b0-855c-4a4c6c4e6968)


And with that, I've added my personal desktop to my ShererMon Policy, with Elastic Defend, SysMon, etc.

![image](https://github.com/bananagav/ELKPi/assets/117794258/562328f9-57e9-4ed4-a7a6-89377be95fa6)

