# DevOps-Project
DevOps Project Spring 2018 NC State University

#### Team members

1. Navjot Singh; nsingh9@ncsu.edu
2. Khelan Patel; kjpatel4@ncsu.edu
3. Khantil Choksi; khchoksi@ncsu.edu
4. Pavithra Iyer; piyer3@ncsu.edu


### Continuous Integration/ Continuous Deployment Pipeline Project
[Link to the overview video](https://www.youtube.com/watch?v=ycDcyfz1-ec&feature=youtu.be)


#### Milestone 1: Configuration Management and Build Milestone
[Link to the milestone](https://github.com/navisingh14/DevOps/tree/milestone1)

In this milestone, we completed the following tasks:

1. *Provisioned* and *configured* a Jenkins server, automatically using Ansible.
2. Using jenkins-job-builder + ansible, setup build jobs for two applications:
  * A nodejs web application [checkbox.io](https://github.com/chrisparnin/checkbox.io).
  * A software "enterprise" Java system [iTrust](https://github.ncsu.edu/engr-csc326-staff/iTrust2-v2)
3. Demonstrated a passing build for each job.
4. Setup up a `post-build action` that runs ansible scripts to provision and configure an independent Amazon EC2 instance for running each application.


#### Milestone 2: Test + Analysis Milestone
[Link to the milestone](https://github.com/navisingh14/DevOps/tree/milestone2)

For this milestone, we leveraged techniques related to fuzzing, test case priorization, and automated test generation to improve the quality of checkbox.io and iTrust.

##### Coverage/Jenkins Support

Added a plugin to Jenkins to measure coverage and display a report within Jenkins on every commit.

##### Automated Commit Generation - Commit Fuzzer

Developed a tool that automatically commits new random changes to source code which will trigger a build and run of the test suite.

###### Fuzzing operations:

   - changed content of "strings" in code.
   - swap "<" with ">"
   - swap "==" with "!="
   - swap 0 with 1

To support the commit fuzzer, we created a new branch for iTrust, called `fuzzer`.
Created a corresponding Jenkins job which enabled us to run the test suite against this branch. Finally, we handled rollback (reverting the commit/reseting to head in git) after submitting to Jenkins. Created an Ansible playbook to run the fuzzer.

Using your automated commit fuzzer, generated 100 random commits (that still compiled) and test runs. 


##### Test prioritization analysis

Wrote a test priorization analysis that examined the results of the 100 commit fuzzer runs and test suite runs. 

Generated a report that displays the test cases in sorted order, based on time to execute and number of failed tests discovered.

##### Automated Test Generation

Using esprima and AST visitor patterns, wrote an automated test generation algorithm to analyze checkbox.io's server-side code and automatically generated test cases for the API routes.


#### Milestone 3: Deployment and Infrastructure Upgrade Milestone
[Link to the milestone](https://github.com/navisingh14/DevOps/tree/milestone3)

Previously, we've focused on building, testing, and analysis of our software in a continuous deployment pipeline. Now, we're ready to start deploying software into production environments.

##### Components

Our production infrastructure and deployment pipeline supports the following properties.

* **Deployment:** Deployed iTrust and checkbox.io to a production environment. Created a git hook on our Jenkins server that will trigger a deployment when doing a git push to "production". The deployment occurs on actual remote machine (AWS). The deployment creates and configures the production environment using Ansible.

* **Infrastructure Upgrade** Made improvements to the infrastructure of checkbox.io.
  - Automatically, creates a [kubernetes cluster](https://kubernetes.io/docs/getting-started-guides/scratch/) (with 3 slave nodes) for running a Dockerized version of `server.js`. Service is still available after turning off nodes.
  - Created a Redis feature flagserver that can be used to turn off/on features on checkbox.io. The redis properties server mirrored locally to each instance.
 
* **Canary Release**. Implemented the ability to perform a canary release for checkbox.io: Using a proxy/load balancer server, routed a percentage of traffic to a newly staged version of software and remaining traffic to a stable version of software. Stopped routing traffic to canary if alert is raised. Maintained a single instance of mongodb.

* **Rolling Update**. Implemented a rolling update deployment strategy for iTrust, where 5 instances of iTrust have been deployed, and each instance of iTrust was reployed while 4 instances remain operational. Maintained a single instance of MySQL server. At least four instances are up during rolling update with a monitoring dashboard/ or simple heartbeat mechanism.


#### Milestone 4: Test + Analysis Milestone
[Link to the milestone](https://github.com/navisingh14/DevOps/tree/milestone4)

We tried to monitor the infrastructure and the application service in the final stage to actively monitor the health of the system. Also, we integrated the Jenkins build status onto Slack to get the status of the jobs being built. 
