## Activity File: Exploring Kibana

* You are a DevOps professional and have set up monitoring for one of your web servers. You are collecting all sorts of web log data and it is your job to review the data regularly to make sure everything is running smoothly. 

* Today, you notice something strange in the logs and you want to take a closer look.

* Your task: Explore the web server logs to see if there's anything unusual. Specifically, you will:

:warning: **Heads Up**: These sample logs are specific to the time you view them. As such, your answers will be different from the answers provided in the solution file. 

---

### Instructions

1. Add the sample web log data to Kibana.

2. Answer the following questions:

    - In the last 7 days, how many unique visitors were located in India?
		- 223 Unique Visitors
    - In the last 24 hours, of the visitors from China, how many were using Mac OSX?
		- 9 Unique Visitors
    - In the last 2 days, what percentage of visitors received 404 errors? How about 503 errors?
		- 0% of users recieved 404 errors
		- 5% of users recieved 503 errors
    - In the last 7 days, what country produced the majority of the traffic on the website?
		- China with 19.2% of all traffic
    - Of the traffic that's coming from that country, what time of day had the highest amount of activity?
		- 9:00am, 12:00pm, and 1:00pm are the most active times for China
    - List all the types of downloaded files that have been identified for the last 7 days, along with a short description of each file type (use Google if you aren't sure about a particular file type).
		- gz: These are compressed archives created by using the GNU (gzip) compression algorithm. This is a more complex version of zip files.
		- css: Cascading Style Sheets, these fileks are used with HTML that describe how HTML will display on screen. This is useful for large projects because css files can be referenced and edited independantly without compromising the rest of the project.
		- zip: This is an archive that holds files and directories. Compression can be added to the zip file to save on ZIP file size though this is not required. Most often is you use the native compression option on a PC you will create a zip file.
		- deb: Debian binary pachage file. This is meant to work with Linux OS software
		- rpm: This is a Red Hat Linux operating system pachage for installations of programs on Linux systems, Red Hat Pachage Manager uses these files.

3. Now that you have a feel for the data, Let's dive a bit deeper. Look at the chart that shows Unique Visitors Vs. Average Bytes.
     - Locate the time frame in the last 7 days with the most amount of bytes (activity).
	 	- 4:34am on 12-18-2021, using 19732 bytes. This user is from China
     - In your own words, is there anything that seems potentially strange about this activity?
	 	This user was downloading a deb file. They were also using a Windows xp computer. It is also strange that this user was using roughly 10,000 mor bytes that any other user in that time range.

4. Filter the data by this event.
     - What is the timestamp for this event?
	 	- 4:34am on 12-18-2021
     - What kind of file was downloaded?
	 	- deb
     - From what country did this activity originate?
	 	- China
     - What HTTP response codes were encountered by this visitor?
	 	- 200, meaning that the even was a success

5. Switch to the Kibana Discover page to see more details about this activity.
     - What is the source IP address of this activity?
	 	- 65.129.211.245
     - What are the geo coordinates of this activity?
	 	- lat: 59.43264556
		- lon: -154.8027058
     - What OS was the source machine running?
	 	- Windows xp
     - What is the full URL that was accessed?
	 	- https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-6.3.2-amd64.deb
     - From what website did the visitor's traffic originate?
	 	- http://facebook.com/success/akihiko-hoshide

6. Finish your investigation with a short overview of your insights. 

     - What do you think the user was doing?
	 	- I believe the user was downloading the program files for metricbeat with the intention of analyzing or installing metricbeat software on a device.
     - Was the file they downloaded malicious? If not, what is the file used for?
	 	- No, it is used for monitoring virtual machines and the url matches that of metricbeat download.
     - Is there anything that seems suspicious about this activity?
	 	- The only odd thing is the referral to facebook.
     - Is any of the traffic you inspected potentially outside of compliance guidlines?
	 	- Usage of Facebook a referral, but not with the actual file.
