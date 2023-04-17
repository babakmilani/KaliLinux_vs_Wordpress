# KaliLinux_vs_Wordpress
Unit 7 &amp; 8 Project: WordPress vs. Kali

Write-Up:
1. Install Docker Desktop on the Host machine from www.docker.com.
2. Open a terminal and clone the Wordpress repo to desired folder : git clone https://github.com/0xrutvij/wpVSkali.git.
3. goto new folder with git repo and make a new director for docker compose to use as source : mkdir wpFolder
4. run >> docker compose up -d to start the containers. 
5. check containers are running >> docker ps -a

Challenge: upon the first install and command executions everything worked as it was supposed to, but after a host machine shutdown and restart the docker container db-1 din't run. after many attempts to solve the issue, I discovered the reason
why the mySQL db-1 container didnt run. It occurred to me that the mySQL server was already running after I had opened the mySQL Workbench application on my host machine for other projects. I ran services.msc in the ' Run' search box and shutdown mySQL. Then I restarted Docker Desktop and activated the Wordpress containers and was able to access wordpress admin console in the browser. 

![Screenshot (49)](https://user-images.githubusercontent.com/55906428/232388995-5f72815c-9bf9-4bc7-a65a-30f550977655.png)

A: Opening an Attack Surface
1. in the Wordpress admin console I added a new plugin (Reflex Gallery previous vs. 3.1.3)
![image](https://user-images.githubusercontent.com/55906428/232391057-10111e4f-e0b8-4dba-8bf3-ed708208a289.png)
2. made sure the website url is accessible from the host OS and Kali interactive mode at 127.0.0.1:8080 I ran wpscan against the url >> wpscan --url http://127.0.0.1:8080 --random-agent/
3. in Kali Linux interactive mode I started Metasploit database.

![Kali Linux vs Wordpress](https://user-images.githubusercontent.com/55906428/232394941-4d8acac6-fec9-4ffd-974b-392c6d1a968c.gif)
4. after searching for a Wordpress plugin Reflex payload in metasploit its time to set it up and exploit or website container in Docker Desktop. 
![Meterpreter](https://user-images.githubusercontent.com/55906428/232397319-4107bc5a-bb80-452d-9214-c15d1ef442fa.gif)
