<h3>Prerequistes</h1>
<p>First, I read the Datadog References guide before beginning the Hiring Challenge. </p>
<p> Then, I decided to create a Virtual Machine on my laptop to avoid dependency issues. To do this, I read the Vagrant web page and followed along with their instructions at https://learn.hashicorp.com/collections/vagrant/getting-started. Additionally, I installed Virtual Box to run Vagrant properly. </p>
<img width="280" alt="vagrant_version" src="https://user-images.githubusercontent.com/102263800/160946443-86801dee-e3e5-4578-9e73-19ec63e13840.png">
<p> After setting up Vagrant and Virtual Box, I created a trial acount with Datadog at https://www.datadoghq.com/. </p>
<img width="1430" alt="datadog_account" src="https://user-images.githubusercontent.com/102263800/160946577-1be3304c-0cbb-41d2-8a82-097f615ad7cc.png">
<p> Next, I set up the Datadog agent using the https://docs.datadoghq.com/ link and also the https://docs.datadoghq.com/agent/ link. After reading through the instructions and used the CLI commands to configure it.
<img width="932" alt="agent_status" src="https://user-images.githubusercontent.com/102263800/160947139-3073e070-34bd-4d07-8a78-d4f88c2bf089.png">

<h3>Collecting Metrics</h3>
<p>#Add tags in the Agent config file and show us a screenshot of your host and its tags on the Host Map page in Datadog.</p>
<img width="1232" alt="agent_tags" src="https://user-images.githubusercontent.com/102263800/160947792-5eaf0a5d-cca5-4673-a96e-f0509c2b973e.png">
<p>I added the tags using the Datadog GUI. Once you select your host, you can enter the tags in the field on the right hand side of the screen, labeled, "Add new comma-separated tags here".</p>

<p>#Install a database on your machine (MongoDB, MySQL, or PostgreSQL) and then install the respective Datadog integration for that database.</p>
<img width="926" alt="sql running" src="https://user-images.githubusercontent.com/102263800/160947962-3e943d61-48eb-4fb9-b00b-1457618ba599.png">
<p>With the help of this website, https://phoenixnap.com/kb/install-mysql-ubuntu-20-04, I then ran the command 'sudo apt install mysql-server'. To then check the version, I ran the 'mysql --version' command. </p>
<p>After I installed mySQL, the next step was to prepare the server by adding a datadog user and password.</p>
<img width="570" alt="create_SQL_user" src="https://user-images.githubusercontent.com/102263800/160952875-b10d5151-7323-41b8-8a2c-b522d96582ff.png">

<p>#Create a custom Agent check that submits a metric named my_metric with a random value between 0 and 1000.</p>
<img width="694" alt="random_interval" src="https://user-images.githubusercontent.com/102263800/160948667-764f9749-4b85-47c0-9dbf-5c2b64048374.png">
<p>To do this, I went into the '/etc/datadog-agent/conf.d' directory and created the configuration file, custom_nikicheck.yaml (which needs to match the name of custom_nikicheck.py).</p>
<p>Then, I went into the '/checks.d' directory and created the Python script for custom_nikicheck.py using the guide, https://docs.datadoghq.com/developers/write_agent_check/?tab=agentv6v7. I had to import the 'random' method to submit that random value inbetween 0 and 1000.</p>
<p>You can check to see that your check is running by entering the command, 'sudo -u dd-agent -- datadog-agent check my_metric'. Then, you can see the metric in the Metrics Dashboard on the Datadog GUI.</p>
<img width="224" alt="metric" src="https://user-images.githubusercontent.com/102263800/160954688-f217ace3-b5e2-4350-9245-ceeee7d1b81e.png">
  
<p>#Change your check's collection interval so that it only submits the metric once every 45 seconds.</p>
<img width="391" alt="interval" src="https://user-images.githubusercontent.com/102263800/160948324-a28982b1-7be6-43c0-9e87-da3231624dbe.png">
<p>Finally, I went back into the 'conf.d' directory and opened my 'custom_nikicheck.yaml' file, and added the interval collection lines of code from the same website above.</p>

<img width="247" alt="metric_screenshot" src="https://user-images.githubusercontent.com/102263800/160955310-1ad7e852-8a7a-4d73-b157-88952a43b9ae.png">


<p>#Bonus Question Can you change the collection interval without modifying the Python check file you created?</p>
<p>Yes</p>

<h3> Visualizing Data </h3>
<p>Utilize the Datadog API to create a Timeboard that contains:

<p>Your custom metric scoped over your host.

Any metric from the Integration on your Database with the anomaly function applied.

Your custom metric with the rollup function applied to sum up all the points for the past hour into one bucket.
<img width="1278" alt="dashboard_screenshot" src="https://user-images.githubusercontent.com/102263800/160955613-0b34613f-fb14-409a-a6f3-da6162406da6.png">

Please be sure, when submitting your hiring challenge, to include the script that you've used to create this Timeboard.</p>

Once this is created, access the Dashboard from your Dashboard List in the UI:

Set the Timeboard's timeframe to the past 5 minutes
Take a snapshot of this graph and use the @ notation to send it to yourself.
<img width="778" alt="timeboard_snapshot" src="https://user-images.githubusercontent.com/102263800/160955892-38a57d90-c621-4b0a-853d-8db01421ba0c.png">

Bonus Question: What is the Anomaly graph displaying?
