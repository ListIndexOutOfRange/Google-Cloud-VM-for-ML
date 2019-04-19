# Google-Cloud-VM-for-ML
A comprehensive guide on how to set up from scratch a Google Cloud VM able to runs a Jupyter Notebook with Keras on TensorFlow, and with a large dataset available. 

As Google Cloud can be a bit hard to begin with, i hope this guide will help you to get where you want quickly and painlessly. 

## Goal: 

At the end of the document, we'll get a superpowerfull Virtual Machine running Ubuntu 18.04 on Google Cloud. 
This VM will be able:  
  -  to execute Python through Anaconda with Machine Learning library such as Tensorflow and Keras. 
  -  to launch and execute a Jupyter Notebook.
  -  to acces a huge database stored onto Google Cloud. 


## Plan of action: 

  (Optional: Requesting a GPU) (ONLY IF FREE TRIAL)
  1. Creating the Virtual Machine 
  2. Installing Anaconda and Setting up Jupyter
  3. Reading a Database from the VM
  4. Writing a Database from the VM 
  
  
## Optional: Requesting a GPU:

If you're using your free trial credits, you need to do this extra step in order to have access to a GPU. 
You can do all the others steps of course and have a working VM aswell, but since you'll only have CPUs and RAM you won't do much.

So let's get started. 

1. From the Google Cloud Platform, click on the upper left menu button and go to IAM & Admins > Quotas. 
2. From there, click on upgrade my account. 
3. Once your account is upgraded, you can request a GPU. Select the quota as follows: 
    -  Quota type: all quotas
    -  Services: Compute Engine API only 
    -  Metric: GPUs (all regions) only 
    -  Location: all locations
4. Once this unique quota is selected you can request an increase for it. It takes about a day to be processed by Google. 
 
If everything went as expected, you should receive an email saying your request has been accepted. 
Now you're ready to build your VM. 
 
 
## 1. Creating the VM: 

1. Click on the upper left menu, select Compute Engine > VM Instances. 
2. As the CPUs and GPUs you can choose depend on the region you select, i suggest you to try some options and select what fits you best.   
   For this example, I chose the following configuration: 
    -  Region: us-west1
    -  Zone: us-west1-b
    -  Cores: 8 vCPUs
    -  Memory: 32 GB
    -  GPUs: 1 Nvidia Teslate v100
3.  Select as Boot Disk Ubuntu 18.04 LTS and select the boot disk you like (I suggest an SSD to be faster, and the size is up to your need). 
4. Then in Identiy and API access select: 
    -  Compute Engine default service account
    -  Allow full access to all Cloud APIs
5. In Firewall, check Allow HTTP traffic & Allow HTTPS traffic
6. Click on Management, security, disks, networking, sole tenancy > Disks. Uncheck Delete boot disk when instance is deleted. 
7. You can now click on create and wait for the setup to complete. 
8. Now we need to set a statip external IP address in order to access a notebook from our home browser. Before doing anything, note somewere the name and the region+ of your VM (instance-1 and us-west1 for me, remember ?).  
Click on the upper left menu, go to VPC Network > External IP addresses. Click on Reserve a static address. 
Then chose a name: you can choose whatever you want. You can attach several static IPs to one VM instance, so that a given IP is allowed to a specific service. In our case for instance, this IP will de dedicated to Jupyter. All I'm saying is that a good name should refer to a service and not to a VM instance. Select the appropriate region and attach it to the instance you just created. 
Click on reserve and that's it for the external IP. 
9. Last step for this part ! We need to  set a firewall rules to authorise the traffic for Jupyter on our VM. 
Still in VPC Network, click on Firewall Rules > Create Firewall Rule. Complete the form as follows: 
    -  Name: as for the IP, this rule should be dedicated to a specific service. So I call it jupyter.
    -  Network: Default
    -  Priority: 1000
    -  Direction: Ingress
    -  Action on match: Allow
    -  Target: All instances in the network
    -  Source filter: IP ranges
    -  Source IP ranges: 0.0.0.0/0
    -  Second source filter: None
    -  Protocols and ports: Specified protocols and ports > tcp:5000
Then click on create. 

Ok ! That's it for this part. Congrats mate, you've successfully create and configurated your VM on Google Cloud. 
Now let's use it ;) 


## 2. Installing Anaconda and Setting up Jupyter 



  


  

