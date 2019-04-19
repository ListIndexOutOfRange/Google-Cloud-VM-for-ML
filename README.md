# Google-Cloud-Virtual-Machine-for-Machine-Learning
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

    -  Click on the upper left menu, go to VPC Network > External IP addresses. Click on Reserve a static address. 

    -  Then chose a name: you can choose whatever you want. You can attach several static IPs to one VM instance, so that a given     IP is allowed to a specific service. In our case for instance, this IP will de dedicated to Jupyter. All I'm saying is that a good name should refer to a service and not to a VM instance.

    -  Select the appropriate region and attach it to the instance you just created. 
    -  Click on reserve and that's it for the external IP. 

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


1. Let's start the VM. Click on the main menu > Compute Engine API > VM instances. At this point, note somewhere the external IP address. Click on the three vertical dots on the right, then start. Once it's started, click on SSH. 

2.  In your browser, go to https://repo.continuum.io/archive/. Look for the latest Anaconda that fits your need. You need to check 3 things:
    -  the OS: nothing to explain right ? 
    -  the CPU type: powerPC or x_86 (32 bits) or x_86_64 (64 bits)
    -  the Anaconda version: do you prefer Python 2 or 3 ? Well actually even if you somehow prefer Python 2, consider trying Python 3 buddy. 
For instance, for me, what i want it named "Anaconda3-2019.03-Linux-x86_64.sh".  
So in your vm type:  
  ```
  wget http://repo.continuum.io/archive/Anaconda3-2019.03-Linux-x86_64.sh
  ```
Obviously you need to change the Anaconda version with the one you just checked. 

3. Launch install by typing 
  ```
  bash Anacond3-2019.03-Linux-x86_64.sh
  ```
Again change the Anaconda filename as needed. 
Follow the steps (press space to pass the License). Accept to initialize Anaconda3 by running conda init at the end of the installation process. 

4. In order to use Anaconda right now let's refresh the bash:
```
source ~/.bashrc
```

5. If everything went fine, you are now in an environment called (base). You can now install any Python library you want. 
For instance let's do: 
```
pip install tensorflow
pip install keras
```

6. Now that everything is ready "Python-side", all we need to do is setting up Jupyter. Type
```
jupyter notebook --generate-config
```

7. Then we'll modify the config file we juste created. I use Vim for this tutorial. I know it can be a bit confusing but just remember those few things: 
    -  to actually write something in the file, you need yo be in insert mode, which you can do by pressing the "i" key. 
    -  to save and quit a file you need to quit the insert mode first by pressing the "Echap" key. 
    -  when you're not in insert mode, you can save & quit by typing ":wq".
So let's go. Type: 
```
vim ~/.jupyter/jupyter_notebook_config.py
```
Then, carefully: 
    -  press "i"
    -  in the notebookApp Configuration section, type: 
    ```
    c = get_config()
    c.NotebookApp.ip = '0.0.0.0'
    c.NotebookApp.open_browser = False
    c.NotebookApp.port = 5000
    ```
    _The port value should be equal to the one you chose while creating the Firewall rule_
    -  press "Echap"
    -  type ":wqw
    
We're almost done ! Everything is ready. All we have to do is launch jupyter one type to setup a password. (We could have done it in the jupyter config file but this way in more user-friendly imo.)  
So type 
``` 
jupyter notebook --no-browser --port=5000
```
You'll see that a authentification token is given to you. Copy it. 
Then in your browser go to 
```
http://<external_IP_address>:<port>
```
Where of course <external_IP_address> is the IP we defined before, attached to our VM instance, and the port is the one of the Firewall rule. 

You should arrive to a Jupyter Notebook login page.   
From there you can paste the token and use it to define a permanent password.  

That's it you've done it ! 

BE CAREFULL !! 
FROM NOW ON, YOU'LL PAY FOR EVERY HOUR YOUR VM RUNS. SO REMEMBER NOT ONLY TO QUIT YOUR SSH WINDOW BUT ALSO TO GO TO THE VM INSTANCES PANEL AND SHUT DOWN YOUR VM. 

From now on, all you will have to do is starting up your VM, launch a jupyter notebook, go to it through your home browser, type your password and it is on ;) 


That's pretty nice but in order to properly do machine learning on this VM we need to access potentially huge datasets. 
Let's see how to do that. 


## 3. Reading a database from the VM 


Let's say you have a huge dataset on your computer. The idea here is to import it in a disk stored on the cloud (a "bucket" that is in Google Cloud), and then mount this disk in your VM. That will allow you to read this disk, though you won't be able to write on it because of security settings. We'll get on that later. For now, follow me: 

1. Click on the main menu button > Storage > Create Bucket. Just name it as you want and let the form as it is. 
Remeber the name of your bucket, we'll need it to mount it on our VM. 
2. Then you can upload anything you want on your bucket. 
3. Now let's mount the bucket on our VM. But first we'll need our Project ID. Go to the main menu > Home > Project Info. Note the project ID.  
4. Get back to your VM instance. Type 
``` 
gcloud init
```
Follow carefullyt the installation process. Be particulary focused when choosing the region. You need to set it accordingly to your VM zone (for me it's us-west1-b for instance). 
5. Then type 
``` 
lsb_release -c -s
```
And remember the output ! Let's call it <release>. (If you have followed my instruction strictly your VM is under Ubuntu 18.04 LTS so the output should be "bionic". 
  
6. Take your time. Any mispelled word will lead to an error so just relax, take a coffe and your time. Type 
```
echo "deb http://packages.cloud.google.com/apt gcsfuse-<release> main" | sudo tee /etc/apt/sources.list.d/gcsfuse.list
```
Of course you have to replace <release> by what you got previousky. For instance: 
```
echo "deb http://packages.cloud.google.com/apt gcsfuse-bionic main" | sudo tee /etc/apt/sources.list.d/gcsfuse.list
```
  
7. Then: 
```
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
```

8. Finally:
```
sudo apt-get update
sudo apt-get install gcsfuse
```

I hope at this point you have installed gcsfuse without any trouble. We're almost done ;) 

9. We'll create a folder to get the bucket: 
```
sudo mkdir /mnt/gcs-buclet
```

_Note: it is possible that the sudo get rejected because we didn't actually setted up a sudo user. You can do it by typing "sudo passwd" and chose a password._

10. Finally we can mount the bucket: 
```
sudo gcsfuse <bucket-name> /mnt/gcs-bucket
```

Okaaaaay ! That's it. You now have a superpowerfull VM in the cloud able to access any dataset you want and use it through a Jupyter Notebook. 

One last point: you may want to store data in your bucket(s). If you try a this point you'll get an error saying "permission denied". Let's fix that. 


## 4. Writing a database from the VM


1. On the Google Cloud Platform, go to main menu > IAM & Admin > Service Accounts. 

2. Click on the three vertical dots > create key, and create a key in JSON format. 

3. Import the key in your VM. You can do it by putting it on a bucket and mounting it or simply on the VM SSH window you can click on the upper right menu > import file. 

4. Type (carefully as always :p ):
```
gcsfuse -o allow_other --gid 0 --uid 0 --file-mode 777 --dir-mode 777 --key-file /path/to/json/key/file <bucket-name> /path/of/mounted/bucket
```


That's it ! I hope this tutorial was clear enough and helped you as you wanted to. 
If you have any questions or comments feel free to contact me. 




  


  
  
  
  
  
  
  
  
  
  
  
  
  



































  


  

