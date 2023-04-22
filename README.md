# KaliLinux_vs_Wordpress
Unit 7 &amp; 8 Project: WordPress vs. Kali

Write-Up:
1. Install Docker Desktop on the Host machine from www.docker.com.
2. Open a terminal and clone the Wordpress repo to desired folder : git clone https://github.com/0xrutvij/wpVSkali.git.
3. goto new folder with git repo and make a new director for docker compose to use as source : mkdir wpFolder
4. run >> docker compose up -d to start the containers. 
5. start kali linux interactive mode in powershell >> docker exec -it [containerID_kaliCP] bash
6. check containers are running >> docker ps -a

Challenge: upon the first install and command executions everything worked as it was supposed to, but after a host machine shutdown and restart the docker container db-1 din't run. after many attempts to solve the issue, I discovered the reason
why the mySQL db-1 container didnt run. It occurred to me that the mySQL server was already running after I had opened the mySQL Workbench application on my host machine for other projects. I ran services.msc in the ' Run' search box and shutdown mySQL. Then I restarted Docker Desktop and activated the Wordpress containers and was able to access wordpress admin console in the browser. 


Exploit #1: Opening an Attack Surface 
a. in the Wordpress admin console I added a new plugin (Reflex Gallery previous vs. 3.1.3)
![image](https://user-images.githubusercontent.com/55906428/232391057-10111e4f-e0b8-4dba-8bf3-ed708208a289.png)
b. made sure the website url is accessible from the host OS and Kali interactive mode at 127.0.0.1:8080 I ran wpscan against the url >> wpscan --url http://127.0.0.1:8080 --random-agent/
c. in Kali Linux interactive mode I started Metasploit database.

![Kali Linux vs Wordpress](https://user-images.githubusercontent.com/55906428/232394941-4d8acac6-fec9-4ffd-974b-392c6d1a968c.gif)
d. after searching for a Wordpress plugin Reflex payload in metasploit its time to set it up and exploit or website container in Docker Desktop. 
![Meterpreter](https://user-images.githubusercontent.com/55906428/232397319-4107bc5a-bb80-452d-9214-c15d1ef442fa.gif)
e. after opening the channel to exploit the vulnerable web application in metasploit I created a text document to test if I have really accessed the server. 
![Exploit 1](https://user-images.githubusercontent.com/55906428/232566954-2d8ce4eb-2f18-49f2-ae13-4554990e8c0c.gif)

Exploit #2: XSS
a. edited index.php in /wp-content with a simple php pop alert
![Exploit 2](https://user-images.githubusercontent.com/55906428/232601883-ecbb35e7-81b3-4f56-8960-5edf43cb2c34.gif)

Exploit #3: SQLi (plugin: olimometer )
1. Searched the plugins database from the wordpress admin portal 127.0.0.1/wp-admin/ and couldnt find the plugin)
![image](https://user-images.githubusercontent.com/55906428/233787314-4cdc8c44-c88e-4ce3-af29-96988f3469db.png)
3. 2. searched the web for the plugin and found it in https://www.exploit-db.com/
![image](https://user-images.githubusercontent.com/55906428/233787354-b98e308b-415d-4173-8f1f-4a0da8a97e6c.png)
5. 3. downloaded the zip file and installed it in my wordpress website. 
6. 4. opened powershell and executed the command >> sqlmap -u "http://127.0.0.1:8080/wp-content/plugins/olimometer/thermometer.php?olimometer_id=1" --dbs --threads=5 --random-agent --no-cast
-u : specify target URL
--random-agent : use tandomly selected HTTP User-agent header value
--threads: Maximum number of concurrent HTTP(s) request(default 1)
--no-cast: Turn off payload casting mechanism
--dbs: retrieve info about database server name. 
![image](https://user-images.githubusercontent.com/55906428/233787432-7d581325-a37f-4b3f-9679-aba087e7a3a1.png)
6. sqlmap was able to find vulnerabilities 
![image](https://user-images.githubusercontent.com/55906428/233787477-e280d208-d178-44c6-8d68-92c0f4bf6b5d.png)
![image](https://user-images.githubusercontent.com/55906428/233787515-ef738829-1e22-415c-b2d4-f62d54434b1c.png)
