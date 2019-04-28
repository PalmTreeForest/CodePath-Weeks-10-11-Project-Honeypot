# Weeks 10 & 11 Project: Honeypot

Time spent: **10** hours spent in total
> Objective: Setup a honeypot and intercept some attempted attacks in the wild.
###########################################

## Honeypots Deployed

1. Dionaea over HTTP

This was the command used to deploy the Dionaea Honeypot on an AWS VM and connect it back to my MHN Admin VM:

	$wget "http://54.213.101.245/api/script/?text=true&script_id=5" -O deploy.sh && sudo bash deploy.sh http://54.213.101.245 oJ2NwYrx

<img src="https://github.com/PalmTreeForest/Weeks-10-11-Project-Honeypot/blob/master/Dionaea.png" width="800">

Here are the results of the Nmap OS and Version scan I ran against the Dionaea Honeypot:

<img src="https://github.com/PalmTreeForest/Weeks-10-11-Project-Honeypot/blob/master/nmap_Dionaea.png" width="800">


2. Ubuntu - Cowrie

This was the command used to deploy the Cowrie Honeypot on an AWS VM and connect it back to my MHN Admin VM:

	$wget "http://54.213.101.245/api/script/?text=true&script_id=5" -O deploy.sh && sudo bash deploy.sh http://54.213.101.245 oJ2NwYrx

<img src="https://github.com/PalmTreeForest/Weeks-10-11-Project-Honeypot/blob/master/Cowrie.png" width="800">

Here are the results of the basic Nmap scan I ran against the Cowrie Honeypot:

<img src="https://github.com/PalmTreeForest/Weeks-10-11-Project-Honeypot/blob/master/nmap_Cowrie.png" width="800">

Here are the results of the Nmap OS and Version scan I ran against the Cowrie Honeypot:

<img src="https://github.com/PalmTreeForest/Weeks-10-11-Project-Honeypot/blob/master/nmap_OS_Cowrie.png" width="800">

3. Ubuntu - Shockpot Sinkhole

This was the command used to deploy the Shockpot Sinkhole Honeypot on an AWS VM and connect it back to my MHN Admin VM:

	$wget "http://54.213.101.245/api/script/?text=true&script_id=1" -O deploy.sh && sudo bash deploy.sh http://54.213.101.245 oJ2NwYrx

<img src="https://github.com/PalmTreeForest/Weeks-10-11-Project-Honeypot/blob/master/Shockpot_Sinkhole.png" width="800">

Here are the results of the basic Nmap scan I ran against the Shockpot Sinkhole Honeypot:

<img src="https://github.com/PalmTreeForest/Weeks-10-11-Project-Honeypot/blob/master/nmap_Shockpot_Sinkhole.png" width="800">

Here are the results of the Nmap OS and Version scan I ran against the Shockpot Sinkhole Honeypot:

<img src="https://github.com/PalmTreeForest/Weeks-10-11-Project-Honeypot/blob/master/nmap_OS_Shockpot_SInkhole.png" width="800">


## Issues Encountered

I used AWS instead of Google Cloud, and so had to modify some of the steps in the instructions given to adapt to this platform. I am relatively familiar with how to launch and manage instances in AWS, and so did not encounter issues on that front.

<img src="https://github.com/PalmTreeForest/Weeks-10-11-Project-Honeypot/blob/master/AWS_Console.png" width="800">

What I was less familiar with was using SCP to transfer files. I initially tried to establish an SSH connection with the admin VM and then use netcat to transfer the file. I created a listener on port 50505 and directed the session.json file into the input of this listener using this command:

	$nc -l -p 50505 < /home/ubuntu/session.json

In another terminal on my host system, I created a netcat client that connected to the listener to try and get the file and write it to a local folder using this command:

	$nc 54.213.101.245 50505 > /home/Desktop/session.json

The session.json file failed to transfer using netcat. At this point I tried my hand at adapting the gcloud scp command provided in the instructions to one that would work with my AWS Ubuntu VM. This is the command that ended up working:

	$scp -i "CodePath-Week-10.pem" ubuntu@ec2-54-213-101-245.us-west-2.compute.amazonaws.com:~/session.json ./session.json

For the Cowrie Honeypot, I initially deployed the honeypot on Ubuntu 14.04. After installation and connecting it back to the admin server, I attempted to run Nmap against it. When I checked the sensor feed, the activity was not logged. After checking the MHN troubleshooting guide, I found this instruction listed: "The script provided supports installation on Ubuntu 16.04 systems. Ubuntu 14.04 is currently failing to install using the provided script."

I deleted the Ubuntu 14.04 instance, and relaunched with 16.04. This did not solve the issue, and decided to launch a different type of honeypot (Shockpot Sinkhole) rather than continue with Cowrie. I left Cowrie running in case I had time to circle back and troubleshoot the remaining issues.


## Summary of the data collected: 

The session.json file which contains all of the data collected by the honeypots can be reviewed here: https://github.com/PalmTreeForest/Weeks-10-11-Project-Honeypot/blob/master/session.json

1. Number of attacks

	Total Attacks: 3243

	Attacks against Dionaea:  3224

	Attacks against Shockpot Sinkhole: 9

	Attacks against Cowrie: 0

<img src="https://github.com/PalmTreeForest/Weeks-10-11-Project-Honeypot/blob/master/Sensors.png" width="800">

To my surprise, IP addresses from Iran were by far the most active in attacking my poor little defenseless honeypot. The runner up was Lithuania. Coming in third was The Netherlands. All in all, this was not who I was expecting to make the top three.

<img src="https://github.com/PalmTreeForest/Weeks-10-11-Project-Honeypot/blob/master/Attack_Stats.png" width="800">

2. Number of malware samples = 0
    

## Any unresolved questions raised by the data collected

Given the number of attacks on the Dionaea Honeypot, I should be seeing malware payloads through the Admin VM. This indicates that there may be a configuration issue in the script provided by MHN, and will require further troubleshooting to resolve. 

