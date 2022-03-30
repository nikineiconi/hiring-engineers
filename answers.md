<h3>Prerequistes</h1>
<p>First, I read the Datadog References guide before beginning the Hiring Challenge. </p>
<p> Then, I decided to create a Virtual Machine on my laptop to avoid dependency issues. To do this, I read the Vagrant web page and followed along with their instructions at https://learn.hashicorp.com/collections/vagrant/getting-started. Additionally, I installed Virtual Box to run Vagrant properly. </p>
<img width="280" alt="vagrant_version" src="https://user-images.githubusercontent.com/102263800/160946443-86801dee-e3e5-4578-9e73-19ec63e13840.png">
<p> After setting up Vagrant and Virtual Box, I created a trial acount with Datadog at https://www.datadoghq.com/. </p>
<img width="1430" alt="datadog_account" src="https://user-images.githubusercontent.com/102263800/160946577-1be3304c-0cbb-41d2-8a82-097f615ad7cc.png">
<p> Next, I set up the Datadog agent using the https://docs.datadoghq.com/ link and also the https://docs.datadoghq.com/agent/ link. After reading through the instructions and used the CLI commands to configure it.
<img width="932" alt="agent_status" src="https://user-images.githubusercontent.com/102263800/160947139-3073e070-34bd-4d07-8a78-d4f88c2bf089.png">

<h3>Collecting Metrics</h3>
<p>Add tags in the Agent config file and show us a screenshot of your host and its tags on the Host Map page in Datadog.</p>
<img width="1232" alt="agent_tags" src="https://user-images.githubusercontent.com/102263800/160947792-5eaf0a5d-cca5-4673-a96e-f0509c2b973e.png">

<p>Install a database on your machine (MongoDB, MySQL, or PostgreSQL) and then install the respective Datadog integration for that database.</p>
<img width="926" alt="sql running" src="https://user-images.githubusercontent.com/102263800/160947962-3e943d61-48eb-4fb9-b00b-1457618ba599.png">

<p>Create a custom Agent check that submits a metric named my_metric with a random value between 0 and 1000.</p>
<img width="694" alt="random_interval" src="https://user-images.githubusercontent.com/102263800/160948667-764f9749-4b85-47c0-9dbf-5c2b64048374.png">

<p>Change your check's collection interval so that it only submits the metric once every 45 seconds.</p>
<img width="391" alt="interval" src="https://user-images.githubusercontent.com/102263800/160948324-a28982b1-7be6-43c0-9e87-da3231624dbe.png">

<p>Bonus Question Can you change the collection interval without modifying the Python check file you created?</p>
