FLEET server is conncected, but now it's showing as offline in my Kibana Dashboard with a pop up showing that my server is not healthy

![image](https://github.com/bananagav/ELKPi/assets/117794258/391d4cec-6e06-4843-ac68-ce6787365a15)



Time to start troubleshooting, first I've enabled a firewall rule allowing traffic through the FLEET server port. 


11/29/2023 - An Update

Fleet seems to want to stop working whenever the server restarted before, which is what I mentioned above, but after tooling with the permissions on my elastic, kibana, and the download file folders I've gotten it to stop being weird and it will now stay alive even after a restart. 
