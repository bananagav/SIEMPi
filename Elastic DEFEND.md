Now that I have my Agent connected for the time being, I'm starting to get to work on my Defence monitoring and setting up my SIEM. 


11/13/2023

I've added all of the Elastic Rules that they recommend on their security Dashboard, including a lot of them for services I don't use (it was an *add all* button type of thing, and their selection system is pretty weird. 

Regardless, SIEM rules are up, no alerts quite yet which I'm not sure is a good thing, given that I've been SSH'ing, running PowerShell scripts, etc. It'll be interesting to see what comes in from just regular PC use
(Given that the rules and log intake are working correctly)

11/29/2023

![image](https://github.com/bananagav/SIEMPi/assets/117794258/b5bd95ac-31d1-4624-84a7-9abb8718f8de)


Added tons of rules and removed all of the Failed and Warning for one reason or another, either I don't have the integration, I don't have the necessary data stream, or the rule doesn't apply

Sitting at 630 Total Rules. Way too many, but it's not creating much noise which is nice. The Pi is still holding up with it so I'm not concerned for now. Might be something I'll have to tune more when more endpoints are added on. 

![image](https://github.com/bananagav/SIEMPi/assets/117794258/d30395e5-44ba-4557-9607-b897152f2e08)

