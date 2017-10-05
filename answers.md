# DataDog Challenge
* Introduction
* Questions
* Level 0 (optional) - Setup an Ubuntu VM
* Level 1 - Collecting your Data
   
    * What is an agent
    * Tagging in agent config 
    * Mysql Integration 
    * Agent Check
* Level 2 - Visualizing your Data
    * Clone database ingration dashboard
    * Difference between a timeboard and a screenboard?
    * snapshot of your test.support.random graph that shows it going above 0.90. 
    
* Level 3 - Alerting on your Data
    * Create monitor and alert 
    * Scheduling downtime for monitor 

## Introduction
   *  Datadog is a monitoring and analysing tool that can be used for collectig and gathering data from cloud-scale applications,servers, databases, tools, and services. Datadog helps to render view of an entire stack of our infrastucture and datadog is a SaaS-based data analytics platform.
   
## Questions
* This documentation contains the solutions to the DataDog challenge provided https://github.com/DataDog/hiring-engineers/tree/support-engineer#the-challenge

# Level 0 (optional) - Setup an Ubuntu VM
   * For this I have launched one ubuntu VM in AWS. 
   
### Bonus question: What is an agent
  * Inorder to use datadog, we have to install an agent in out host. It is a piece of software that is capable of collecting  and presenting datas to datadog so that we are able to monitor and analyse our data.
  
 * Main parts of the agents are the collector, dogstatsd, and the forwarder.

* ```` collector```` runs checks on the current machine for whatever integrations you have and it will capture system metrics like memory and CPU.```` Dogstatsd````is a statsd backend server you can send custom metrics to from an application.```` forwarder````retrieves data from both dogstatsd and the collector and then queues it up to be sent to Datadog.
  
     
###### Installation of an agent
   * Firts of all we have to take an account with the datadog. You will get an API key with that you are able to install the agent.
###### Basic commands for agent usage
  Starting an agent
````sh
sudo /etc/init.d/datadog-agent start
````
Stopping an agent
````sh
sudo /etc/init.d/datadog-agent stot
````
Restarting an agent
````sh
sudo /etc/init.d/datadog-agent restart
````
###### Status and Information

To check if the Agent is running: (since 3.8.0)
````sh
sudo /etc/init.d/datadog-agent status
````
To receive information about the Agent’s state:
```sh
sudo /etc/init.d/datadog-agent info
```
Tracebacks for errors can be retrieved by setting the -v flag: (since 3.8.0)
````sh
sudo /etc/init.d/datadog-agent info -v
````

###### Configuration Files

* All the configuration file for the agent is loacted at ````sh /etc/dd-agent/datadog.conf ```` and Configuration files for integrations are located in  `sh /etc/dd-agent/conf.d/`

###### Troubleshooting

* Try running the info command to see the state of the Agent.Logs for the subsystems are in the following files:

```sh 
/var/log/datadog/supervisord.log (since 3.8.0)
/var/log/datadog/collector.log
/var/log/datadog/dogstatsd.log
/var/log/datadog/forwarder.log
```

### Tagging 

  * Tags can be used to add extra dimensions to the metics so that we can aggregate, compare and render data across different hosts on the front end. You can have a look on https://docs.datadoghq.com/guides/tagging/ for more tagging informations. We can put tag either via configuration file or from the datadog interface. Here I am going to show to how to tag using the configuration file.
  * Tags are actually a key: Value pair. In order to tag open the datadog.conf file ``sudo vim /etc/dd-agent/datadog.conf `` and insert the line ``tags:DatadogCandidate Name:Vinitha `` on the tagging section. This will add a tag to your host.
  
  ###### This is my Screenshot after tagging 
 

![Tagging Screenshot](/images/Tag.png)


### Mysql Integration

* The Datadog Agent can collect many metrics from MySQL databases. The MySQL check is included in the Datadog Agent package, so simply install the Agent on your MySQL servers, we dont need to install anything extra for this. After installing the Mysql create a user for datadog agent and give permissions. I have followed the instruction provided in this doc https://docs.datadoghq.com/integrations/mysql/ . After completing all the steps database integration will reporte the matrics to datadog.

### Agent Check
* Agent check is a python plugin to the datadog agent. Agent Checks are a great way to collect metrics from custom applications or unique systems. For more information regarding the aget check visit https://docs.datadoghq.com/guides/agent_checks/#overview

* For Each check we will have to write two files, one configuration file (yaml) and a check module file (.py). Configuration file should  placed in the conf.d directory and the module file in check.d directory. Configuration is written using YAML and the file name should match the name of the check module.

Here, I have written an agent check for the matric ```test.support.random``` and which samples a random value. 
# Level 2 - Visualizing your Data

* I have created one dashboard by cloning the Mysql database and added the ``test.suppport.random`` matric along with some othrer mysql matrics.
 ![Tagging Screenshot](/images/dashboard.png)
 * Difference between a timeboard and a screenboard?
 * Snapshot of ```test.support.random``` matric graph and that shows it going above 0.90
 ![Tagging Screenshot](/images/matricvalue_0.94.png)

# Level 3 - Alerting on your Data

* Set up a monitor on this metric that alerts you when it goes above 0.90 at least once during the last 5 minutes
![Tagging Screenshot](/images/alertcondition.png)
* Bonus points: Multi-alert monitor
![Tagging Screenshot](/images/multialert.png)

* Monitor alert obtained via mail 
![Tagging Screenshot](/images/alert.png)

* Bonus: Downtime for monitor alert
![Tagging Screenshot](/images/downtime.png)
