Investigations from alerts found on SIEMPi





11/21/2023 - Learning how to use the Cases function in Kibana. 

![image](https://github.com/bananagav/SIEMPi/assets/117794258/3a5bf0a8-3802-4d0f-8ffc-1a4f3d7f4cf0)

I've set up some alerts and this is the start of my investigations into the alerts I receive. I think I'll start with the critical alerts I've received, all of them being Threat Intel IP address matches

11/29/2023 - Learning How to Use Kibana as a SIEM.

[https://www.elastic.co/guide/en/kibana/current/manage-cases.html](https://www.elastic.co/guide/en/kibana/current/xpack-siem.html)

This is the guide I've been using, the documentation on Kibana's site seems to be pretty helpful so far. 

This describes the expected workflow using ELK

![image](https://github.com/bananagav/SIEMPi/assets/117794258/95d548f3-1a22-43d9-9140-2ac91f7807c3)

As a SOC analyst, I would like to learn how to properly work through alerts, investigate, and open cases to document findings and remediation.


Now that I have my rules set up and alerts coming in, it's time to learn how to investigate. 

From this diagram, the place to start looks to be the Timelines tab.

https://www.elastic.co/guide/en/security/8.11/timelines-ui.html

To begin, the alert I will be investigating will be *Threat Intel IP Address Indicator Match* Which uses IOCs imported from AbuseCH using their integration. 

![image](https://github.com/bananagav/SIEMPi/assets/117794258/4391e799-33ae-427e-8ef2-ba61ae4ab07b)

I currently have 44 Matches over the past 15 days. 


To begin the investigation I'll be creating a timeline

When you create a timeline, you add fields below the time range selection in order to pull alerts to view. 

Using the filter ( kibana.alert.rule.name:"Threat Intel IP Address Indicator Match" ) I was able to pull all of the alerts to investigate. 

![image](https://github.com/bananagav/SIEMPi/assets/117794258/050e7eb8-dd0d-478b-9409-ea3a5a150140)

Here's a sample of the information with the alert

![image](https://github.com/bananagav/SIEMPi/assets/117794258/42bbd67d-b6d4-4efa-b877-cac88c30658f)

You can see the event action, Source, Destination, Ports, etc. 

In this case, since I'm investigating a threat intel IP Address match, we're gonna focus on Destination IP and Process. 


![image](https://github.com/bananagav/SIEMPi/assets/117794258/8dc79f6e-4a50-4053-8144-53032be4ef5e)


Using the view details tab you can open up the event and see more information 


![image](https://github.com/bananagav/SIEMPi/assets/117794258/96e99949-9d42-4696-a4d6-bf9e2d534ad8)

There is an overview showing the status, Severity, Rule, Risk Score, the Reasons for the event, you can view all of the fields in table, get risk data and it even comes with an Investigation Guide, which is a built in playbook for what the event means, why it's happening, fields to investigate, Remediation, What to do if it's a false positive and more. It's incredibly in-depth. 

![image](https://github.com/bananagav/SIEMPi/assets/117794258/d8fab2ef-24af-4fd5-b8ae-a0054fa1953b)

Moving on from that, using the *Analyze Event* Button, ![image](https://github.com/bananagav/SIEMPi/assets/117794258/3b608796-4d9b-4d76-a56c-cc035c909271) You can open the Analyzer Tab

Which looks like this, Showing the Running processes that created the event. In this case, Chrome.

![image](https://github.com/bananagav/SIEMPi/assets/117794258/e5e1809c-2d77-4f48-bf6d-75d52198ea59)

12/2/2023 - Continuing the Investigation - Threat Intel IP Match

For this specific investigation, the threat IP indicator my SIEMPi is alerting on is (destination.ip: "204.79.197.200") on port 443. 


By bringing up the details tab, you can see that this event flags as a Critical Alert with a 99 Risk score.


![image](https://github.com/bananagav/SIEMPi/assets/117794258/94714459-5f2c-4988-a67d-779b26144964)


The process creating this activity is Chrome, which appears as process.name: "chrome.exe"

![image](https://github.com/bananagav/SIEMPi/assets/117794258/0f26a0f8-a993-4eaa-806b-90693161f1ed)

The Investigation guide gives me a few points to investigate, including Context about the matching field, Investigating the IP Address's Reputation using VirusTotal, Hybrid Analysis, Talos, etc. Also it advises excuting a Reverse DNS Lookup to see the hostnames associated with the IP. 

![image](https://github.com/bananagav/SIEMPi/assets/117794258/e1d255e3-d366-46e2-9732-d61977c015c6)

I'll start with VirusTotal, and move forward from there, using other tools like PolySwarm, MXToolBox, and Brightcloud to check the reputation of the IP Address. 

![image](https://github.com/bananagav/SIEMPi/assets/117794258/203b4f53-4cdd-4daf-b286-2cf4a61985b5)

https://www.virustotal.com/gui/ip-address/204.79.197.200

For VirusTotal, It shows a 4/88, from CyRadar, CRDF, Criminal IP, VIPRE and as a Mixed IOC from ArcSight Threat Intel for "IT-Dienstleister". 

Looking at the community score, It's way down and the community page shows entries for Phishing Campaigns 5 months ago, to a C2 and SSH Bruteforce 3 Years ago. 

MXToolBbox Reverse DNS shows the IP as a-0001[.]a-msedge[.]net and wasnt convicted by any of the blacklists there...

![image](https://github.com/bananagav/SIEMPi/assets/117794258/0069905c-f64c-48e9-9345-8e166bc14aa1)

![image](https://github.com/bananagav/SIEMPi/assets/117794258/c3a52352-1a3a-4a6e-a2bb-8d31d7bb8e6c)

Searching for the IP on PolySwarm shows as a 1/4 Engines reporting malicious, with Criminal IP

https://polyswarm.network/scan/results/url/a67bebaf28610074396e820a63098e4d27b1038896d8a70a8d8501acdabd302c

![image](https://github.com/bananagav/SIEMPi/assets/117794258/05da5cf6-0ba9-4ed7-828d-a61136adc77a)

The Talos IP reputation lookup shows the reputation as good, with no FWD/REV DNS Match.


