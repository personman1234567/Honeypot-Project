# Honeypot-Project

### Overview:

Driven by curiosity and the aim of understanding network security and threat detection more deeply, I decided to work on a personal project to deploy and analyze a honeypot environment using the T-Pot platform on a Debian 12 cloud instance. The objective was to capture, analyze, and understand attack patterns to enhance my cybersecurity skills!

### Project Setup and Deployment:

Upon establishing what I wanted to do, I faced the inevitable barrage of initial challenges that come with starting any ambitious/technical project. Questions swirled around like a hive of curious bees: Where will I host this honeypot? How will I create it? And let's be honest... what exactly is a honeypot?

#### VULTR Cloud Service

With all of that in mind, I decided that the best first step was to determine where to host the project, as using my own computer didn't seem like the most... practical option. After some research, I discovered [Vultr](https://www.vultr.com/), a promising and cost-efficient cloud service provider. Upon signing up, I was pleasantly surprised to receive $100 in free credits, allowing me to run my machine and experiment freely for the next two months without any cost!

When deploying and configuring my new machine, I was presented with a variety of operating system options. After CAREFUL consideration (I chose a random one), I chose to use the Debian 12 operating system.
![image](https://github.com/user-attachments/assets/39f467cc-ac06-4afb-8d5c-a37e3e4edc1a)

And just like that, the EpicHoneypot server was all ready to go—except for the small detail that I needed to setup the actual honeypot.
![image](https://github.com/user-attachments/assets/922360a5-9d7d-4eab-b66b-8c59fb8a5809)

#### The Honeypot

Now that the host is set up, I needed to find out which honeypot I wanted to utilize. Looking online I found many different options all with their own pros and cons, making the decision challenging. That was until I came across a popular honeypot framework called T-Pot that integrates several of the honeypots into one cohesive system. The idea of deploying multiple honeypots sounded far more productive than just one, so I decided to deploy T-Pot on my system.

<details>
<summary>T-Pot Setup Guide</summary>
  
#### Setting Up T-Pot (Walkthrough)
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



