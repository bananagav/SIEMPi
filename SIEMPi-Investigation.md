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


