
7. by going to Start Menu and start typing `Firewall` and choose `Windows Defender Firewall`, if everything is setup correctly, you should be able to see two rules for `Cockpit` 
![Windows Firewall](_res/Network/Firewall1.jpg)

6. if the rules are missing, go a head and make one new rule as follows:
	- Right click `Inbound Rules > New rule > Program > This program path` browse to select the server build `Cockpit.exe` Click next `Allow the connection` > check all three boxes `Domain, Private & Public`. Click next to name the rule `Cockpit`  and finish