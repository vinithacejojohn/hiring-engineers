# DataDog Challenge
## Contents
* [Introduction](https://github.com/vinithacejojohn/hiring-engineers/blob/dev/answers.md#introduction)
* [Questions](https://github.com/vinithacejojohn/hiring-engineers/blob/dev/answers.md#questions)
* [Level 0 (optional) - Setup an Ubuntu VM](https://github.com/vinithacejojohn/hiring-engineers/blob/dev/answers.md#level-0-optional---setup-an-ubuntu-vm)
* [Level 1 - Collecting your Data](https://github.com/vinithacejojohn/hiring-engineers/blob/dev/answers.md#level-1---collecting-your-data)
    * [What is an agent](https://github.com/vinithacejojohn/hiring-engineers/blob/dev/answers.md#what-is-an-agent)
    * [Tagging in agent config](https://github.com/vinithacejojohn/hiring-engineers/blob/dev/answers.md#tagging-in-agent-config)
    * [Mysql Integration](https://github.com/vinithacejojohn/hiring-engineers/blob/dev/answers.md#mysql-integration)
    * [Agent Check](https://github.com/vinithacejojohn/hiring-engineers/blob/dev/answers.md#agent-check)
* [Level 2 - Visualizing your Data](https://github.com/vinithacejojohn/hiring-engineers/blob/dev/answers.md#level-2---visualizing-your-data)
    
* [Level 3 - Alerting on your Data](https://github.com/vinithacejojohn/hiring-engineers/blob/dev/answers.md#level-3---alerting-on-your-data)


## Introduction
Datadog is a monitoring and analytics tool that can be used for collecting and gathering data from cloud-scale applications, servers, databases, tools and services. DataDog is a SaaS-based analytics platform which helps to render view of an entire stack of our infrastucture.
   
## Questions
* This documentation contains the solutions to the DataDog challenge questions for [Support Engineers](https://github.com/DataDog/hiring-engineers/tree/support-engineer#the-challenge)

## Level 0 (optional) - Setup an Ubuntu VM
   * For this I have launched an Ubuntu instance in AWS. 
   
## Level 1 - Collecting your Data
   
### What is an agent
  * In order to use DataDog, we have to install an agent in our host. It is a piece of software that is capable of collecting  and presenting data to DataDog so that we are able to monitor and analyse our data.
  
 * Main parts of the agents are the collector, dogstatsd, and the forwarder.

* ```` collector```` runs checks on the current machine for whatever integrations you have and it will capture system metrics like memory and CPU.```` Dogstatsd````is a statsd backend server you can send custom metrics to, from an application.```` forwarder````retrieves data from both dogstatsd and the collector and then queues it up to be sent to DataDog.
  
     
##### Installation of an agent
   * First of all we have to take an account with the DataDog. You will get an API key with which you will be able to install the agent. After creating the account we'll have to select the type of OS we use, here I selected Ubuntu. On the next page we'll get the command for the installation along with the secret API Key. The command for ubuntu is given below ( *YOUR_API_KEY* will be replaced by our secret API Key )
   ```
   DD_API_KEY=[YOUR_API_KEY] bash -c "$(curl -L https://raw.githubusercontent.com/DataDog/dd-agent/master/packaging/datadog-agent/source/install_agent.sh)"
   ```
##### Basic commands for agent usage
````
sudo /etc/init.d/datadog-agent start

sudo /etc/init.d/datadog-agent stop

sudo /etc/init.d/datadog-agent restart
````
##### Status and Information

To check if the Agent is running: (since 3.8.0)
````
sudo /etc/init.d/datadog-agent status
````
To receive information about the Agent’s state:
```
sudo /etc/init.d/datadog-agent info
```
Tracebacks for errors can be retrieved by setting the -v flag: (since 3.8.0)
````
sudo /etc/init.d/datadog-agent info -v
````

##### Configuration Files

* All the configuration files for the agent are located at ```` /etc/dd-agent/datadog.conf ```` and Configuration files for integrations are located in  `/etc/dd-agent/conf.d/`

##### Troubleshooting

* Try running the info command to see the state of the Agent. Logs for the subsystems are in the following files:

``` 
/var/log/datadog/supervisord.log (since 3.8.0)
/var/log/datadog/collector.log
/var/log/datadog/dogstatsd.log
/var/log/datadog/forwarder.log
```

### Tagging in agent config 

  * Tags can be used to add extra dimensions to the metics so that we can aggregate, compare and render data across different hosts on the front end. You can have a look on [DataDog Tags](https://docs.datadoghq.com/guides/tagging/) for more  information on Tagging. We can put tag either via configuration file or from the datadog interface. Here I am going to show  how to tag using the configuration file.
  * Tags are actually a key: Value pair. In order to tag, open the datadog.conf file ``sudo vim /etc/dd-agent/datadog.conf `` and insert the line ``tags:DatadogCandidate Name:Vinitha `` on the tagging section. This will add a tag to your host.
  
  ###### This is my Screenshot after tagging 
 

![Tagging Screenshot](/images/tag.png)


### Mysql Integration

* The Datadog Agent can collect many metrics from MySQL databases. The MySQL check is included in the Datadog Agent package, so simply install the Agent on your MySQL servers, we dont need to install anything extra for this. After installing the Mysql create a user for datadog agent and give permissions. I have followed the instruction provided in the doc [DataDog Mysql Integration](https://docs.datadoghq.com/integrations/mysql/) . After completing all the steps, database metrics  will be reported to DataDog.

### Agent Check
* Agent check is a python plugin to the datadog agent which can be used for creating customised checks. Agent Checks are a great way to collect metrics from custom applications or unique systems. For more information regarding the agent check visit [DataDog Agent Check](https://docs.datadoghq.com/guides/agent_checks/#overview)

* For Each check we will have to write two files, one configuration file (yaml) and a check module file (.py). Configuration file should be placed in the `conf.d` directory and the module file in `check.d` directory. Configuration is written using YAML and the file name should match the name of the check module.

Here, I have written an agent check for the metric ```test.support.random``` and which samples a random value. 

**/etc/dd-agent/checks.d/test.py**
```
from checks import AgentCheck
import random
    class TestCheck(AgentCheck):
        def check(self, instance):
            random_value=random.random()
            self.gauge('test.support.random', random_value)
 ```

**/etc/dd-agent/conf.d/test.yaml**
```
init_config:

instances:
[{}]

```




## Level 2 - Visualizing your Data

* I have created one dashboard by cloning the Mysql database and added the ``test.suppport.random`` metric along with some other mysql metrics.
 ![Tagging Screenshot](/images/dashboard.png)
 * Difference between a timeboard and a screenboard?
   * There are two type of dashboards in DataDog, Timeboard and Screenboard.
    * **Timeboard:** All graphs are scoped at the same time and graphs will always be in the grid-like fashion. We are able to share the graphs individualy and its better for troubleshooting and correlation.
    * **Screenboard:** They are created with drag-and-drop widgets, which can each have a different time frame. These are flexible, far more customizable and are great for getting a high-level look into a system. ScreenBoards can be shared as a whole live and as a read-only entity, whereas TimeBoards cannot.

 * Snapshot of ```test.support.random``` metric graph that shows it going above 0.90
 ![Tagging Screenshot](/images/matricvalue_0.94.png)








## Level 3 - Alerting on your Data

* **_Set up a monitor on this metric that alerts you when it goes above 0.90 at least once during the last 5 minutes_**

![Tagging Screenshot](/images/alertcondition.png)




* **_Bonus: Multi-alert monitor_**

![Tagging Screenshot](/images/multialert.png)




* **_Monitor alert obtained via mail_**

![Tagging Screenshot](/images/alert.png)




* **_Bonus: Downtime for monitor alert set up_**

![Tagging Screenshot](/images/downtime.png)




* **_Mail notification after Downtime has started_**

![Tagging Screenshot](/images/downtimemail.png)
