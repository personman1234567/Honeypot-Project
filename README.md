# Honeypot-Project

## Overview:

Driven by curiosity and the aim of understanding network security and threat detection more deeply, I decided to work on a personal project to deploy and analyze a honeypot environment using the T-Pot platform on a Debian 12 cloud instance. The objective was to capture, analyze, and understand attack patterns to enhance my cybersecurity skills!

## Project Setup and Deployment:

Upon establishing what I wanted to do, I faced the inevitable barrage of initial challenges that come with starting any ambitious/technical project. Questions swirled around like a hive of curious bees: Where will I host this honeypot? How will I create it? And let's be honest... what exactly is a honeypot?

<details>
<summary>Cloud Service Provider</summary>

### VULTR Cloud Service

With all of that in mind, I decided that the best first step was to determine where to host the project, as using my own computer didn't seem like the most... practical option. After some research, I discovered [Vultr](https://www.vultr.com/), a promising and cost-efficient cloud service provider. Upon signing up, I was pleasantly surprised to receive $100 in free credits, allowing me to run my machine and experiment freely for the next two months without any cost!

When deploying and configuring my new machine, I was presented with a variety of operating system options. After CAREFUL consideration (I chose a random one), I chose to use the Debian 12 operating system.
![image](https://github.com/user-attachments/assets/39f467cc-ac06-4afb-8d5c-a37e3e4edc1a)

And just like that, the EpicHoneypot server was all ready to go—except for the small detail that I needed to setup the actual honeypot.
![image](https://github.com/user-attachments/assets/922360a5-9d7d-4eab-b66b-8c59fb8a5809)
</details>

<details>
<summary>Honeypot</summary>
  
### The Honeypot

Now that the host is set up, I needed to find out which honeypot I wanted to utilize. Looking online I found many different options all with their own pros and cons, making the decision challenging. That was until I came across a popular honeypot framework called T-Pot that integrates several of the honeypots into one cohesive system. The idea of deploying multiple honeypots sounded far more productive than just one, so I decided to deploy T-Pot on my system.
</details>

<details>
<summary>T-Pot Setup Guide</summary>
  
### Setting Up T-Pot (Walkthrough)
Reffering to the T-Pot Documentation [here](https://github.security.telekom.com/2024/04/honeypot-tpot-24.04-released.html#t-pot-data-folder), I found that I chose the PERFECT time to figure out how to set this up, because as of April 2024, the T-Pot ISO image is no longer provided! Which is super convenient because every video tutorial I came across used that ISO image, so I had to figure out this installation with just myself and the documentation. But I was determined, and I will now show you how it's done.

* For the purpose of documentation, I am repeating the setup on a Test machine, separate from my actual project. We begin by logging into the machine for the first time with the default credentials provided by Vultr:
![image](https://github.com/user-attachments/assets/4d68be93-fad4-48df-bbf0-a00a49d9d77a)

* We don't want to use the root user during this process, so we are going to make a new user with the following commands:

  Create a new user:
  ```
  adduser newusername
  ```
  Grant the new user administrative priveledges:
  ```
  usermod -aG sudo newusername
  ```
  Log in as the new user:
  ```
  su - newusername
  ```

* Git may not be initially installed on the machine, but you will need that for this next step. Clone the repository with the following command:
  ```
  git clone https://github.com/telekom-security/tpotce
  ```
  Upon completion you will notice a new directory on your system called "tpotce"
  ![image](https://github.com/user-attachments/assets/293043a1-030a-4607-8205-96f93e1af2c0)

* Next you want to navigate into that folder and execute the file named "install.sh". You will be prompted to confirm if you would like to install, enter "y" to proceed
  ![image](https://github.com/user-attachments/assets/ed11634c-62a8-4aee-8203-7a337fd09ba3)

* During the installation process you will be prompted with this message, asking what type you would like to install. I just selected the standard type, entering "h". It will also have you create a new login, and this login is what you will use to access the T-Pot Dashboard once everything is setup

  ![image](https://github.com/user-attachments/assets/c42ac37c-21d3-41d0-8084-066ae6d590d2)

* Once the process is done you will see this table of Active Internet Connections. It is VERY important that you verify that there are no ports conflicting with T-Pot. During my initial setup I spent hours trying to figure out why T-Pot cannot interact with the docker correctly, and that is because the default settings of my machine were utilizing port 123, even though T-Pot was trying to use that port. It was a very long process to narrow down that issue, so save yourself some time and double check your ports!
  ![image](https://github.com/user-attachments/assets/e4b1e325-e84e-47cf-976f-3522dba5f703)

* Finally, reboot your system, log back into the user you created, and check the status of T-Pot to make sure it's running with the following command:
  ```
  sudo systemctl status tpot.service
  ```
  ![image](https://github.com/user-attachments/assets/ed3db370-47f8-4e38-8a0c-a165569ba5f4)

* Now that you've confirmed that T-Pot has been deployed, you can access it by opening any web browser, and vising https://<your.machines.ip>:64297, you will be prompted to login with the website credentials you created a couple steps ago, and by logging in you will officially be presented with the BEAUTIFUL dashboard that is T-Pot!
  ![image](https://github.com/user-attachments/assets/1cdc5952-1826-4fc2-b010-80af10fbb0b9)
</details>

## First Impressions
<details>
<summary>Attack Map</summary>

### Attack Map

#### Main Page
When I first accessed the T-Pot Dashboard, I immediately wanted to check out the attack map. The pictures and videos I had seen of it were fascinating, and getting to see a live feed of events in real time on my own machine was something I was really excited for.

On my first look at the attack map, I was very impressed by the beautiful user interface, the seamless animations, and the way the data was presented so cleanly and intuitively. The design and functionality made it easy to grasp the information at a glance. 
![image](https://github.com/user-attachments/assets/daa47475-fdfe-4b96-89a4-3840ef1b84d6)

Attackers were immediately being displayed on the map, so I decided to explore further to see what information I could uncover with this tool. Each attack point revealed detailed insights about the origin and nature of the attacks. I noticed they all also displayed the reputation of each particular source IP. I wanted to look further into where that reputation was being scored, so that's something I'll investigate in further detail later in my project.

![image](https://github.com/user-attachments/assets/aa277f1c-77e3-4013-b891-0820996cc308)

#### Attack Statistics
![image](https://github.com/user-attachments/assets/d3488b96-8647-4867-87bc-a8882560e80c)

1. **Service Type:** Starting at the left I noticed that the map categorizes attacks by the service, which is represented by different colors. I noticed that DNS seems to be the most common just from looking at the map since most of the attack points are green
2. **Attack Volume:** The next section seems to be data that reveals the unique IPs that have the highest volume of attacks on my machine. 187.137.210.115 just won't leave my machine alone it seems!
3. **Geographic Distribution:** Similarly to the previous section, this one seems to be measuring attack volume, but from each country rather than from each IP. The US takes the cake with this one with 7903 hits!
4. **Event Logs:** This last section seems to be the most important, because it kind of combines the previous section, while also including a timestamp, and specifying which honeypot was hit
</details>

<details>
<summary>Kibana</summary>

The other tool I wanted to explore was the Kibana tool. From my initial understanding, Kibana is the tool that provides visualizations like charts, graphs, and maps, which would help in analyzing these large volumes of data more effectively than just the attack map.

![image](https://github.com/user-attachments/assets/5b05cc05-2e0c-4207-8086-38cf53972dde)
> The first look at the dashboard shows a list of all of the different honeypots T-Pot utilizes

After exploring some of them, I noticed some of them didn't seem to be doing anything. But the one I noticed the most activity in by far was the Cowrie honeypot. Get ready for a very large screenshot!

![image](https://github.com/user-attachments/assets/67f4c436-c787-455d-82fd-edbb378e9a65)
> My initial thoughts from seeing this can be put simply into two words... Information Overload!!!

All the charts, graphs, acronyms, colors, and numbers were overwhelming at first glance. To make sense of it all, I decided to narrow my focus and examine each section individually rather than trying to connect everything at once. This approach proved to be very effective! So here was my thought process:

* The top section makes complete sense. You have Attacks, Unique source IPs, and Unique HASSHs, along with 2 different visualizations, one showing the count, and the other showing the count over a range of time. And then there is also a smaller version of an attack map, which already makes sense because I'm an attack map expert already.
* Moving down I noticed that a lot of these charts are heavily coorelated with eachother, but give different visualizations that can help you understand trends better, so you can identify any anomoly. There is a graph for IP Reputation, Attacks from countries, attacks by port numbers, and ssh versions. Then the two bigger graphs below that combines all of that info in a much more detailed and readable graph. One random thing I noticed was that Australia in particular seems to only use ssh, whereas all other countries at least sometimes use telnet. I enjoy the random statistics.
* The next part of the page was pretty easy to understand what was going on. I saw a display of the most common username login attempts, and then adjacent to that, a display of the most common password attempts. I noticed that the most common of each seem to be the default credentials on most systems, so the attackers are probably trying to find systems that never ended up changing their default credentials, since it would be easy to gain access. And a side note: I found it funny that some of the password attempts were very rude or explicit.
* The bottom section of the page is the part I found the most interesting. You have a list of the top attacker ASNs, the top attacker IPs, and then a list of the top commands that were run on my machine. Initially I realized that the fact that commands were executed on my machine, implied that there are many attackers that were SUCCESSFUL in getting into my system. I saw that they were running all sorts of commands that would give them information about the versions and hardware of my machine. While I know that the honeypot was created with intentional vulnerabilities, I was curious as to how I could prevent some of these attacks from happening, which is what I will be playing around with as an experiment in a later section
* By far the organization that I seemed to be attacked by the most was DIGITALOCEAN-ASN. There was a link that I could click from that dashboard [here](https://mxtoolbox.com/SuperTool.aspx?action=asn%3a14061) that would display all of the IPs associated with this ASN, so I was wondering if it would be efficient to just block every single IP/CIDR Range listed on this website so that this organization can't attack me anymore. After researching about what Digital Ocean actually is, I realized that it is a cloud service provider similar to Vultr (the one I'm using). So every single IP address isn't going to be a malicious one, so it would by WAY overkill to restrict access from that entire organization. I will go further into this in my experiment I do in a later section.

</details>

## Deep Dive Activities

<details>
<summary>Experiment 1</summary>
  
### IP Reputation

After exploring the Cisco [Talos](https://www.talosintelligence.com/) website that the Kibana dashboard directs you to when clicking on an IP address, I discovered that there are databases that maintain information about the reputation of each IP address. This inspired me to create a script that leverages these resources to determine the reputation of a given IP address. And based off of those values, I wanted to make the script block the IPs with poor reputations by updating the firewall rules via iptables.

While this approach wasn't the most practical, my initial plan was to implement a web scraping tool within my script to extract the reputation value from the Talos website. Typically, the IP's reputation was simply graded as either "Poor" or "Neutral," as shown in the screenshot below:
![image](https://github.com/user-attachments/assets/fea1f8e0-ca8c-4233-accf-bb2d48731e39)

<details>
<summary>Attempt 1</summary>
  
#### Python Script Attempt 1
![image](https://github.com/user-attachments/assets/fd9e30e4-2f3f-46f9-ba45-2040371887cd)
> This here was my first attempt for this experiment. No, I did not write it all in one try, but after a lot of work and fixing confusing errors I finally got it to execute!

* First things first, the main function. I wanted to get the skeleton of this code to run before I added more dynamic inputs, so I just made it to where it can take in a simple list of IP addresses, and index through and execute the "get_ip_reputation" function for each of the IPs in the list.
* Moving on, we are brought to the "get_ip_reputation". This is where the "magic" happens! It's just a shame that magic isn't real, because this function did not work how I wanted it to. The thought process was to utilize a tool I discovered called [BeautifulSoup](https://github.com/wention/BeautifulSoup4) (A tool used for pulling data out of HTML and XML files). I wanted to use this tool to grab the reputation score straight from the website. I found the class name always started with "rep-" and then was proceeded by either "Poor" or "Neutral".
* This method did not end up working for a few reasons:
  * Web scraping is sometimes not permitted by websites, so to prevent it, the website's structure changes, which is something my script cannot handle
  * Even if it did work, this method would be very resource intensive when you consider the scale, and the amount of IPs I would be checking the reputation for, so this was just not a good method
* The output of this script unfortunately displayed "None" in the spots that were supposed to display the reputation score
</details>

<details>
<summary>Attempt 2</summary>

#### Python Script Attempt 2
![image](https://github.com/user-attachments/assets/00704c3e-43d9-43d8-ba45-622e2b4bf989)
> This attempt was honestly just a lost cause. I was trying to implement too many different tools to work with eachother

* Unfortunately this attampt was not that much closer to the solution than the last. My mind was still set on web scraping from Talos, so I decided to try a more advanced tool called [selenium](https://github.com/SeleniumHQ/selenium) to accomplish this.
* The only alteration in my script was in the "get_ip_reputation" function to make it work with this new selenium tool.
* This process involved initializing a web driver to open the url, and that aspect alone brought up so many issues that I could not manage to debug.
* In the end, the outcome was the same as the last, where the output displayed "None" instead of the reputation score it was supposed to grab.
</details>

<details>
<summary>Successful Attempt</summary>

#### Python Script Success

Third times a charm right? The universe confirmed that's right, becuase this time around I did it! In this approach, I had to take a step back. No more web scraping. I decided to try and find if there was just a way to get this information directly, maybe through an API. After a lot of searching, I came to realize that there was no publically available API for Talos... But what if their was a publically available API for another IP Reputation source? (There is. And I used it)

Looking around at all the different IP Reputation websites, I found one called [AbuseIPDB](https://www.abuseipdb.com/). Just by registering to their website, they give you access to a free option to make API calls. 1,000 calls a day was plenty enough for me to get this script working the way it should.

![GetIPReputation_Function](https://github.com/user-attachments/assets/173e021b-bdc5-49c4-a3ce-7469ea197fdc)
> Had to scribble out my API key since I'm putting this on the internet

* Finally! a "get_ip_reputation" function that works! Using the API made the process much simpler. I just simply passed in the IP address, and from that I am able to access the value of every detail associated with that IP.
* This website also seemed to have a more detailed measurement on the reputation. Instead of Talos displaying "Poor" or "Neutral", AbuseIPDB goes a step forward and measures their reputation on a scale of 0-100, with 100 meaning very malicious.
* Furthermore, with this API I got to access all of the other values instead of just the reputation, so I wanted to make my script output the score as well as the ASN, just so I could confirm that it all worked.

![AbuseIPDB_Script_Output](https://github.com/user-attachments/assets/e5545f6f-6860-4be2-86f0-051cdc194b25)
>This is the output after running my script

* As you can see, the output indicates that things are working perfectly!
* I fed 3 different IPs to the script that would all give me different outputs. There is a very malicious one from Digital Ocean, there is my own IP, and finally another random one that was only slightly malicious.
* The cooresponding reputation scores all line up with the AbuseIPDB website, and to check even further I double checked these same IPs on Talos and the reputations were consistent on both websites.

![Block_IP](https://github.com/user-attachments/assets/6aaabea3-5213-46b7-a804-ef875326b330)
> The main aspect of the experiment

* And last but not least, the whole reason behind creating this script is illustrated by this function.
* The "get_ip_reputation" returns the numerical reputation score back to main, which them moves on to a conditional if statement. If the score that was returned comes back as more than 75, then we're going to consider that malicious and block it.
* In this function, the IP is sent over, and a couple of commands are run using that value.
* To be put simply, these lines are adding a rule for the incomming traffic, and outgoing traffic to that IP address, all packets are dropped.
* Then finally the updated IP Table is saved so that these rules will persist.

</details>
</details>

<details>
<summary>Experiment 2</summary>
  
### Automation
I finished this, but it has yet to be documented here. I will add the documentation soon!
</details>
