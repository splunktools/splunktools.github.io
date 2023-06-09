---
layout: default
---

# **SEC1332C:** Level up your Response Actions: 
## Hands-on Building Splunk SOAR Apps using the SOAR App Wizard ##

**Session Abstract:**   Do you want to increase your capabilities in Splunk® SOAR? Get connections with more tools than ever before. Join us for an interactive workshop where we demonstrate how to build a custom app in Splunk SOAR using the SOAR App Wizard. We will demonstrate how to use the Splunk App Wizard to take care of authentication and write custom Python code for interacting with REST application programming interface (APIs).

## Introduction and Purpose 

*   Build custom SOAR app
*   Ingest paginated data from an authenticated REST API source
*   POST data to an authenticated REST API source
*   Understand the basics of app development inside SOAR

## Splunk SOAR Architecture 

**Playbooks** trigger **Actions** on the App

![Process](/assets/images/soar-process.png)

* * *

## Build a development environment 

It is best to develop using a development environment with an on-prem instance of Splunk SOAR.

User can either download the Splunk SOAR OVA or download the tar.gz file and install on Red Hat Enterprise Linux 7.x or CentOS 7.x.

See resources below on setting up a dev environment:

*   [SOAR Installation](https://docs.splunk.com/Documentation/SOARonprem/latest/Install/Overview)
*   [Dev Environment Setup Instructions](https://docs.splunk.com/Documentation/SOAR/current/DevelopApps/SetUpADevEnvironment)

* * *
## Important Notes

* Some versions of Splunk SOAR (Phantom) use different versions of Python (2.7, 3.7, 3.9, etc) so please make sure that your dev environment matches your prod environment. The latest on-prem version will match cloud.
* In order to use dependencies such as pypi repo items, the servers you are developing on and installing on must have access to the repos.
* If you wish to ingest data into SOAR (such as polling for new events), a manual edit of the JSON file must be done to enable on-poll as (of the writing of this document) the App Wizard does not support that action OOB.
* **Do not hard code credentials in apps, playbooks, or custom functions** -- even when just testing. Splunk SOAR uses a local git repo to store all changes so you may risk commiting sensitive data in your local repo.
* Some functions are different in Phantom app builder then in custom functions -- for example while trying debug to the console otherwise you have to look at log files:
  - phantom.debug("") -> self.save_progress("")   **don't leave this in your code once you are done**
  - [List of App API functions](https://docs.splunk.com/Documentation/SOAR/current/DevelopApps/AppDevAPIRef)


* * *

## Resources 

*   [Splunk Official SOAR Development Documentation](https://docs.splunk.com/Documentation/SOAR/current/DevelopApps/Overview)
*   [Ingest Actions](https://docs.splunk.com/Documentation/SOAR/current/DevelopApps/Connector#Ingestion)
*   [App Github Repository](https://github.com/splunktools/sample_soar_app)
*   [FastAPI GitHub Repository](https://github.com/splunktools/fast_api_server)
*   [Splunk SOAR Repositories](https://github.com/orgs/splunk-soar-connectors/repositories)
*   [Splunk SOAR Contribution Guide](https://github.com/splunk-soar-connectors/.github/blob/main/.github/CONTRIBUTING.md)
*   [Splunk Slack Usergroups](https://splk.it/slack)
*   [Splunk SOAR VSCode Extension](https://marketplace.visualstudio.com/items?itemName=Splunk.vscode-splunk-soar)

You can use this logo for your test app:

![Logo](/assets/images/applogo.png)

## Related .Conf Talks


### SEC1592C - Tools of the Trade: Advancing App Development for Splunk® SOAR

David Riddle, Sr. Security Eng. & Software Developer, University of Illinois at Urbana-Champaign

Daniel Federschmidt, Senior Solution Engineer, Security, Splunk

The app ecosystem surrounding the Splunk® SOAR platform is integral to its automation capabilities. Let’s peek behind the curtain and learn what old and new tools are available to build, extend and test apps for SOAR. Join us for the first showcase of the Visual Studio Code Extension for Splunk SOAR, designed to make app development a breeze. Beyond that, we’ll dive deeper and study University of Illinois’ approach to building, testing and deploying SOAR apps.

**.conf23**


* * *

### SEC1104C - Jump to Hyperspace - Publish Apps at Lightspeed with Open SOARce

Matt Sayar, Senior Product Manager, Splunk

Dan Nunes, VP, Technical Strategy & Program Management, DomainTools

Stop wasting months waiting for app updates to go live! Making changes to Splunk® SOAR apps has never been faster for partners and our community. Join Splunk and DomainTools to find out how we shortened the app update cycle, put the power in the hands of partners, and are equipping users to make it easier than ever to integrate your security tools.

[SEC1104C Video](https://conf.splunk.com/files/2022/recordings/SEC1104C_1080.mp4)

[SEC1104C Slides](https://conf.splunk.com/files/2022/slides/SEC1104C.pdf)

**.conf22**


* * *

### SEC1700C - Ready, Set, SOAR: How Utility Apps Can Up Level Your Playbooks!

Erica Pescio, Forward Deployed Software Engineer, Splunk

Daniel Federschmidt, Forward Deployed Software Engineer, Splunk

You don't need a Super Mushroom to 1-up your automation and move to the next level! Powering your capabilities has never been so easy with ready-made Splunk® SOAR Utility Apps. Parse indicators of compromise. Accept HTTP data. Create requests to any API. Run playbooks on an interval. Let's explore apps that help you climb up walls and reach the bonus level. Are you ready? Join Splunk's forward-deployed software engineers on the quest to save the SOAR kingdom!

[SEC1700C Video](https://conf.splunk.com/files/2022/recordings/SEC1700C_1080.mp4)

[SEC1700C Slides](https://conf.splunk.com/files/2022/slides/SEC1700C.pdf)

**.conf22**


* * *
