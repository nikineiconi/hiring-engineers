### Prerequistes
> You can utilize any OS/host that you would like to complete this exercise. However, we recommend one of the following approaches:

>You can spin up a fresh linux VM via Vagrant or other tools so that you don’t run into any OS or dependency issues. Here are instructions for setting up a Vagrant Ubuntu VM. We strongly recommend using minimum v. 16.04 to avoid dependency issues.
You can utilize a Containerized approach with Docker for Linux and our dockerized Datadog Agent image.
Once this is ready, sign up for a trial Datadog at https://www.datadoghq.com/

>Please make sure to use “Datadog Recruiting Candidate” in the “Company” field

>Then, get the Agent reporting metrics from your local machine and move on to the next section...

<p>First, I read the Datadog References guide before beginning the Hiring Challenge. </p>
<p> Then, I decided to create a Virtual Machine on my laptop to avoid dependency issues. To do this, I read the Vagrant web page and followed along with their instructions at https://learn.hashicorp.com/collections/vagrant/getting-started. Additionally, I installed Virtual Box to run Vagrant properly. </p>
<img width="280" alt="vagrant_version" src="https://user-images.githubusercontent.com/102263800/160946443-86801dee-e3e5-4578-9e73-19ec63e13840.png">
<p> After setting up Vagrant and Virtual Box, I created a trial acount with Datadog at https://www.datadoghq.com/. </p>
<img width="1430" alt="datadog_account" src="https://user-images.githubusercontent.com/102263800/160946577-1be3304c-0cbb-41d2-8a82-097f615ad7cc.png">
<p> Next, I set up the Datadog agent using the https://docs.datadoghq.com/ link and also the https://docs.datadoghq.com/agent/ link. After reading through the instructions and used the CLI commands to configure it.
<img width="932" alt="agent_status" src="https://user-images.githubusercontent.com/102263800/160947139-3073e070-34bd-4d07-8a78-d4f88c2bf089.png">
<p>  'sudo service datadog-agent status' </p>

### Collecting Metrics
> Add tags in the Agent config file and show us a screenshot of your host and its tags on the Host Map page in Datadog.
<img width="1232" alt="agent_tags" src="https://user-images.githubusercontent.com/102263800/160947792-5eaf0a5d-cca5-4673-a96e-f0509c2b973e.png">
I added the tags using the Datadog GUI. Once you select your host, you can enter the tags in the field on the right hand side of the screen, labeled, "Add new comma-separated tags here".

> Install a database on your machine (MongoDB, MySQL, or PostgreSQL) and then install the respective Datadog integration for that database.
<img width="926" alt="sql running" src="https://user-images.githubusercontent.com/102263800/160947962-3e943d61-48eb-4fb9-b00b-1457618ba599.png">
<p>With the help of this website, https://phoenixnap.com/kb/install-mysql-ubuntu-20-04, I then ran the command 'sudo apt install mysql-server'. To then check the version, I ran the 'mysql --version' command. </p>
<p>After I installed mySQL, the next step was to prepare the server by adding a datadog user and password.</p>


'''
mysql> CREATE USER 'datadog'@'%' IDENTIFIED WITH mysql_native_password by 'datadog';

mysql -u datadog --password=datadog -e "show status" | \
grep Uptime && echo -e "\033[0;32mMySQL user - OK\033[0m" || \
echo -e "\033[0;31mCannot connect to MySQL\033[0m"

mysql -u datadog --password=datadog -e "show slave status" && \
echo -e "\033[0;32mMySQL grant - OK\033[0m" || \
echo -e "\033[0;31mMissing REPLICATION CLIENT grant\033[0m"
'''

> Create a custom Agent check that submits a metric named my_metric with a random value between 0 and 1000.


'''
#the following try/except block will make the custom check compatible with any Agent version
import random
try:
    #first, try to import the base class from new versions of the Agent...
    from datadog_checks.base import AgentCheck
except ImportError:
    #...if the above failed, the check is running in Agent version < 6.6.0
    from checks import AgentCheck
#content of the special variable __version__ will be shown in the Agent status page
__version__ = "1.0.0"
class NikiCheck(AgentCheck):
    def check(self, instance):
        self.gauge('niki.check', random.randint(1,1000))
'''


<p>To do this, I went into the '/etc/datadog-agent/conf.d' directory and created the configuration file, custom_nikicheck.yaml (which needs to match the name of custom_nikicheck.py).</p>
<p>Then, I went into the '/checks.d' directory and created the Python script for custom_nikicheck.py using the guide, https://docs.datadoghq.com/developers/write_agent_check/?tab=agentv6v7. I had to import the 'random' method to submit that random value inbetween 0 and 1000.</p>
<p>You can check to see that your check is running by entering the command, 'sudo -u dd-agent -- datadog-agent check my_metric'. Then, you can see the metric in the Metrics Dashboard on the Datadog GUI.</p>
<img width="224" alt="metric" src="https://user-images.githubusercontent.com/102263800/160954688-f217ace3-b5e2-4350-9245-ceeee7d1b81e.png">
  
>#Change your check's collection interval so that it only submits the metric once every 45 seconds.


'''
#init_config: 

#instances: [{}]
init_config:

instances:
  - min_collection_interval: 45
'''
    
    
<p>Finally, I went back into the 'conf.d' directory and opened my 'custom_nikicheck.yaml' file, and added the interval collection lines of code from the same website above.</p>

<img width="247" alt="metric_screenshot" src="https://user-images.githubusercontent.com/102263800/160955310-1ad7e852-8a7a-4d73-b157-88952a43b9ae.png">


> Bonus Question Can you change the collection interval without modifying the Python check file you created?
<p>Yes, by changing the interval in the .yaml file.</p>

### Visualizing Data 
> Utilize the Datadog API to create a Timeboard that contains:
> - Your custom metric scoped over your host.</p>
> - Any metric from the Integration on your Database with the anomaly function applied.</p>
> - Your custom metric with the rollup function applied to sum up all the points for the past hour into one bucket.

<img width="1278" alt="dashboard_screenshot" src="https://user-images.githubusercontent.com/102263800/160955613-0b34613f-fb14-409a-a6f3-da6162406da6.png">


> Please be sure, when submitting your hiring challenge, to include the script that you've used to create this Timeboard.

> Once this is created, access the Dashboard from your Dashboard List in the UI:
> - Set the Timeboard's timeframe to the past 5 minutes
> - Take a snapshot of this graph and use the @ notation to send it to yourself.

<p>To set up the Timeboard, select "Dashboard"-> "New Dashboard" from the drop-down list, name your dashboard and select "New Dashboard". Click on the time drop-down and select "Past 5 minutes". Click on the "Add Widget" button then select "Time Series". Once I did this, I entered in the name of my metric (niki.check), the host, and gave my Timeboard a title of "Avg of My_Metric". </p>
<img width="1054" alt="visualization" src="https://user-images.githubusercontent.com/102263800/161866627-931ce493-9ab2-46d1-9097-89bf886a67ab.png">

<p>Then I added another timeseries for my Database + Anomaly graph and followed the same steps as above.</p>
<img width="1151" alt="anomaly_graph" src="https://user-images.githubusercontent.com/102263800/161866896-0954c580-bdbb-420c-b674-0de7ce23529a.png">
<p>I did the same process as above for the rollup function.</p>
<img width="1207" alt="rollup" src="https://user-images.githubusercontent.com/102263800/161867117-c21c7b9d-4106-4834-a5a4-2b0c26439906.png">

> #Bonus Question: What is the Anomaly graph displaying?
- The anomaly graph shows spikes using historic data to determine if there are any abnormalities being monitored.</p>

### Monitoring Data
> Since you’ve already caught your test metric going above 800 once, you don’t want to have to continually watch this dashboard to be alerted when it goes above 800 again. So let’s make life easier by creating a monitor.
> Create a new Metric Monitor that watches the average of your custom metric (my_metric) and will alert if it’s above the following values over the past 5 minutes:
> - #Warning threshold of 500
> - #Alerting threshold of 800
> - #And also ensure that it will notify you if there is No Data for this query over the past 10m.</ul>
<img width="765" alt="email_warning" src="https://user-images.githubusercontent.com/102263800/161653103-6581acbe-2d30-4f23-9086-ca86a8baa4a1.png">

> Please configure the monitor’s message so that it will:
> - Send you an email whenever the monitor triggers.
> - Create different messages based on whether the monitor is in an Alert, Warning, or No Data state.
> - Include the metric value that caused the monitor to trigger and host ip when the Monitor triggers an Alert state.
> - When this monitor sends you an email notification, take a screenshot of the email that it sends you.
<img width="942" alt="metric_gui" src="https://user-images.githubusercontent.com/102263800/161865678-97ade976-04a8-463b-a482-327706951606.png">
<img width="1277" alt="monitor" src="https://user-images.githubusercontent.com/102263800/161865296-b124eae5-b25e-4a77-9f2c-4468dbb63dc0.png">

<p>To create a new monitor, click the "New Monitor" button on the right-hand side of the page, then select "Metric" on the next page. Then, I entered my metric in the "Define the Metric" field, set the warning and alerting thresholds to 500 and 800, respectively, then sent up a message to notify my team when the monitor is in an Alert, Warning, or No Data state. After clicking "Save", the monitor should show up under the "Manage Monitors" tab.</p>
<img width="753" alt="metric_gui2" src="https://user-images.githubusercontent.com/102263800/161866009-c1a3a7c7-fc55-43d7-85d5-283039ebf91e.png">

> Bonus Question: Since this monitor is going to alert pretty often, you don’t want to be alerted when you are out of the office. Set up two scheduled downtimes for this monitor:
> - One that silences it from 7pm to 9am daily on M-F,
<img width="799" alt="bonus1" src="https://user-images.githubusercontent.com/102263800/161659441-c8ab883f-968a-4558-8274-ccc5c92721e8.png">
<img width="716" alt="bonus1_email" src="https://user-images.githubusercontent.com/102263800/161659449-09fe1ae0-28cb-4ca0-a223-17d3b5ea82cf.png">

> - And one that silences it all day on Sat-Sun.
<img width="842" alt="bonus2" src="https://user-images.githubusercontent.com/102263800/161659461-6765e650-0968-46b2-ab38-b5ab4423ff72.png">
<img width="717" alt="bonus2_email" src="https://user-images.githubusercontent.com/102263800/161659479-532d292a-b1cb-4af5-91ca-97d7d4498b2d.png">

> - Make sure that your email is notified when you schedule the downtime and take a screenshot of that notification.
<img width="710" alt="downtime_email" src="https://user-images.githubusercontent.com/102263800/161660178-a8792adc-2029-41bc-a073-642b0398aebb.png">
<p>If you're like me, you'll want to make sure you can attain some level of work-life balance. :happy: Thankfully, Datadog's downtime management tool allows you to schedule downtimes when you're back home and enjoying your afternoons and weekends. </p>
<p>To manage and schedule downtimes, click the "Monitors"->"Manage Monitors" tab on the left-hand dropdown Datadog page, select the "Manage Downtimes" tab, and then the "Schedule Downtimes" icon. After clicking the icon, you determine what you want to silence, what the schedule of the downtime looks like, and if you want to add a message for the downtime. In this message box, I used the @ notation to @ myself (and my email) whenever there is a scheduled downtime. </p>
<p> The above screenshot is what an email notification for a downtime looks like.</p>
  
### APM Data
> Given the following Flask app (or any Python/Ruby/Go app of your choice) instrument this using Datadog’s APM solution:

'''
from flask import Flask
import logging
import sys

# Have flask use stdout as the logger
main_logger = logging.getLogger()
main_logger.setLevel(logging.DEBUG)
c = logging.StreamHandler(sys.stdout)
formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
c.setFormatter(formatter)
main_logger.addHandler(c)

app = Flask(__name__)

@app.route('/')
def api_entry():
    return 'Entrypoint to the Application'

@app.route('/api/apm')
def apm_endpoint():
    return 'Getting APM Started'

@app.route('/api/trace')
def trace_endpoint():
    return 'Posting Traces'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port='5050')
'''

<p>I downloaded flask and used https://docs.datadoghq.com/getting_started/tracing/ to make sure that everything was set up properly before tracing. Then, I followed this page https://docs.datadoghq.com/tracing/setup_overview/setup/python/?tab=otherenvironments to install the ddtrace module which connects to the Datadog Agent. I set multiple tags, using this in my CLI: "DD_SERVICE="sample_app" DD_ENV="dev" DD_LOGS_INJECTION=true DD_TRACE_SAMPLE_RATE="1" DD_PROFILING_ENABLED=true ddtrace-run python sample_app.py --port=4999". </p>
  
> Bonus Question: What is the difference between a Service and a Resource?
<p>Datadog provides an APM glossary page, at the website: https://docs.datadoghq.com/tracing/visualization/. A service is a building block of modern microservice architectures - broadly a service groups together endpoints, queries, or jobs for the purposes of building your application, and a resource is a particular domain of a customer application - they are typically an instrumented web endpoint, database query, or background job.</p>

> Provide a link and a screenshot of a Dashboard with both APM and Infrastructure Metrics.
> Please include your fully instrumented app in your submission, as well.
<img width="744" alt="services_page" src="https://user-images.githubusercontent.com/102263800/161863969-f8c38bb4-385e-48da-96d8-708bdcabba8b.png">
<p>Even though the APM service I set up did not show on the Datadog dashboard, you can see that the env variable was correctly set up on the CLI and is reflected on the Datadog GUI. I got to the "Services" page by clicking the "APM" tab on the left-hand side of my page. I used the https://docs.datadoghq.com/agent/guide/agent-log-files/?tab=agentv6v7 page to look up commands  to help troubleshoot the connectivity problem and ran the agent.log / trace-agent.log command which did not show any errors. </p>
<img width="960" alt="logs-screenshot" src="https://user-images.githubusercontent.com/102263800/161864647-ef55b5c1-72c6-4935-a204-1dc6b1406db2.png">
![giphy](https://user-images.githubusercontent.com/102263800/163732999-cd28f1f0-ce20-46ff-94db-12dc144fa1bc.gif)


### Final Data
> Datadog has been used in a lot of creative ways in the past. We’ve written some blog posts about using Datadog to monitor the NYC Subway System, Pokemon Go, and even office restroom availability! Is there anything creative you would use Datadog for?

<p>As a musician and music lover, I have come to greatly appreciate the process of creating music and listening to it. However, it can be nerve-wracking to constantly watch your social media or Spotify for Artists application to try to determine the listening patterns of your listeners. I would use Datadog in association with the Spotify for Artists app to create monitors for how long a user is listening to my music, and try to send out email lists for people who listen over 30 minutes (or any set amount of time). This could allow for my fans who greatly appreciate my music to be a part of my touring schedule and promotional updates. </p>


