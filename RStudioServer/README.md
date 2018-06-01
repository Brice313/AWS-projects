# Install RStudio Server on a EC2 instance 

source: https://aws.amazon.com/blogs/big-data/running-r-on-aws/

This tutorial will install the following on a AWS EC2 instance of your choice:

* R
* RServer
* Shiny
* Shiny Server


When you launch an EC2 instance, you can pass in user data that can be used to perform common automated configuration tasks. 
The tasks can even run scripts for installation after the instance starts. 
In the EC2 launch wizard, you can add this at the Configure Instance Details step by expanding the Advanced Details pane.
Use the **Advanced_details_User_Data.txt**.

Before running the following script to install R, RStudio Server, the Shiny package, and Shiny Server, 
visit https://www.rstudio.com/products/rstudio/download-server/ to check for the latest versions of RStudio Server. 
Modify the script to download and install the most recent version. This script also adds a **user** and **password** that you use for logging in later to RStudio.

In the EC2 launch wizard, you define a security group, which acts as a virtual firewall that controls the traffic for one or more instances. 
For your R-based analysis environment, you have to open up 
* port 8787 for RStudio Server 
* port 3838 for Shiny Server


To see what has happened at launch, you can check the user data output in the following file:
**/var/log/cloud-init-output.log**

##### Using Rstudio server
Using the public IP of your EC2 you can access RStudio server if you specify the right door: ***EC2_IP*:8787**in your browser
Then login using the credentials specified in the user data.
If you have massive datasets put them on S3 and import them in R using RCurl package;

```R
> install.packages("RCurl")
> library("RCurl") 
> data <- read.table(textConnection(getURL("https://cgiardata.s3-us-west-2.amazonaws.com/ccafs/amzn.csv")),
					 sep=",",
					 header=FALSE)
> head(data)
```

##### Configuring the Shiny Server
Once the Server Shiny is set up, a little configuration is needed. Connect to the instance and launch the following command lines:

```
mkdir ~/ShinyApps
sudo /opt/shiny-server/bin/deploy-example user-dirs
cp -R /opt/shiny-server/samples/sample-apps/hello ~/ShinyApps/
```

Finally you can check that it works using the following:

http://YOU_IP:3838/ec2-user/hello/