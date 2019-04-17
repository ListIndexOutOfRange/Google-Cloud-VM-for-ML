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
  
  
## Optional: Requestin a GPU:

If you're using your free trial credits, you need to do this extra step in order to have access to a GPU. 
You can do all the others steps of course and have a working VM aswell, but since you'll only have CPUs and RAM you won't do much.

So let's get started. 

1. From the Google Cloud Platform, click on the upper left menu button and go to IAM & Admins > Quotas. 
2. From there, click on upgrade my account. 
3. Once your account is upgraded, you can request a GPU. Select the quota as follows: 
    -  Quota type: all quotas
    -  


  

