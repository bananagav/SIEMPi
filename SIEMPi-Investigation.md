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






