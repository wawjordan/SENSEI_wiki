# Setting Up SENSEI With Docker
## What is Docker?
Docker is a software platform similar to a virtual machine, which allows a simple way to separate development from infrastructure, and quickly develop, ship, and deploy. It creates a bare-bones instance of a standardized environment that strips away the need for integration with different hardware, these instances are called **containers**. Containers are great, as everybody has their own unique one which has the same exact packages installed (which means the code will run the same every time). Then they are able to customize it in the future to meet their particular preferences. Another crucial component of the Docker structure is an **image**, which is basically a read-only template with instructions for creating a Docker container.

## Installing Docker
To get started we need to install Docker onto your machine. To do this, visit the following link and install whatever version of Docker Desktop corresponds to your current OS: https://docs.docker.com/get-docker/. Follow the default setup steps and install the software. Launch it once it is completely installed and you're already halfway there!

## Getting The Docker Image
Our next step is to get the image for the SENSEI container onto your machine. This can either be done by building the Dockerfile that is included on the repo here manually, or by pulling the pre-built version from our Docker Hub repository (recommended). Both ways are outlined below and will get you the same result (unless you choose to modify the Dockerfile for any reason such as wanting to use an alternative Linux distribution), so feel free to do whichever one suits you!

Note: Make sure that Docker Desktop is running prior to moving on to further steps, if you do not have it running you may receive errors running any Docker commands.

### Building Image Manually
In order to build the image manually, you first need to have the SENSEI repository cloned locally onto your machine, this is in order to access the Dockerfile. Once you have this done, navigate into the main directory for SENSEI and run the following command:

`docker build -t sensei:latest .`

This will start the build process for the SENSEI image which will take around 3-5 minutes. Once it completes your image has been created and you should be able to navigate over to the Docker Desktop and see it under the "Images" tab.
### Pulling Image From Docker Hub
A direct approach that does not require you to wait for the image to build locally is to pull our pre-built version from Docker Hub. This can be done by running the following command within a command line:

`docker pull nrand/sensei:latest`

This should pull the latest SENSEI image from our repository on Docker Hub, and when you navigate into Docker Desktop you should see the image there ready for you to use!

## Creating Your Container
In order to actually utilize our new SENSEI image, we are going to have to create a container based on it. This can be done either directly from the command line, or through Docker Desktop: both approaches will be outlined below:

### Command Line Generation
To generate your Docker container through the command line, we just have to run the following command:

`docker create -i -t --name Sensei nrand/sensei`

This will create your docker container and it will now appear on Docker Desktop. If you did not pull the image from Docker Hub make sure to replace "nrand/sensei" with whatever you called your locally built image. The name of your container can also be modified by replacing "Sensei" with your desired name. Further customization and options are available and can be found at the following link: https://docs.docker.com/engine/reference/commandline/create/

### Docker Desktop Generation
This is my preferred method as it is very straightforward and simple. Just navigate into the "Images" page within Docker Desktop where you should see your new SENSEI image listed. Hover over it and click the "Run" button and follow the quick steps regarding manual customization such as custom naming that follow, and then your container will be created! You can go check it out on the "Containers" page to ensure that it went through!

![image](https://user-images.githubusercontent.com/56923966/193469697-4035a365-f19e-47f2-8cd7-aca990b5b53e.png)

## Running & Accessing Your Container
Now that you have your container created you can finally access and begin using it! There are 3 main ways that I am aware of to enter the container environment, and I will outline each of them below. Any will work, so choose what you think will fit your workflow and preferences best! I personally prefer the VS Code integrated environment, but both other command line options are also good assuming you like development in the command line.

### Command Line Access
We can run and access our Docker container entirely from the command line. First, we need to get the container up and running, which we can do with the following command (replace Sensei with your container name):

`docker start Sensei`

Now that our container is running we just have to "attach" our command line to it (which basically places us inside the container environment). This can be done by running this next command:

`docker attach Sensei`

Now your command line should shift over to within the Container, and you can do any normal commands and actions just like you would on your local machine!

### Docker Desktop Command Line Access

Running and accessing your container from the Docker Desktop GUI is very straightforward. First, navigate to the Containers page where you should see your recently created container. In order to start the container hover over the container and click the play button:

![image](https://user-images.githubusercontent.com/56923966/193578630-35a79f24-3e09-4038-ae6e-caf1baf45200.png)

Then, just click the command line interface button that appears once the container has started:

![image](https://user-images.githubusercontent.com/56923966/193578826-7ea42d96-6090-4015-a3d7-8a28d7d1445f.png)

With that, a Terminal window should appear that is within the Container environment! This does not by default open the bash window, so you have to run the following command in the command line in order to get bash open:

`/bin/bash`

Now you can do any normal commands and actions just like you would on your local machine, but it is isolated within the Docker container.

### VS Code Integrated Remote Access
Our first step for accessing our container through VS Code is to actually download it, which can be done at the following link: https://code.visualstudio.com/download. Once it is downloaded just open it up and you are ready to go on to the next step.

Now we want to start our container, this can be done from within VS Code or within the Docker Desktop. In order to do it within VS Code create a new terminal within VS Code or just on your local machine and type the following command (replace Sensei with your container name):

`docker start Sensei`

If you want to start it through Docker Desktop, just launch the application, navigate to the containers page, and click the play button on the container you just created. Both of these methods will get your container up and running.

All we have left to do now is to attach our VS Code to the container, kind of like remote access to another computer (but the other computer is our container). We do this by running a VS Code command from the Command Palette which can be accessed by the shortcut `Ctrl+Shift+P`. Once this is open, search for the "Remote-Containers: Attach to Running Container..." option and select it. This should pop up a list of currently running containers on your machine. Go ahead and select the container you just created for running Sensei. 

This will open up a new instance of VS Code that is remotely connected to your container! All files and changes within this instance of VS Code are encapsulated within the container, including your OS and packages.

## Cloning SENSEI
There are a variety of authentication methods supported by Git that can be accessed from the command line, however, the method discussed here can be set up once to remove the need for passwords and further authentication in the future. This method will utilize a Personal Access Token (PAT) that is encrypted and stored locally within your container to be utilized in cases where authentication is needed (such as contributing to the SENSEI repository).

The first step is to create the local stores within your container, which we will do with the help of the `gpg` and `pass` packages. First, we need to create a GPG Key Pair on our system by running the following command and filling out the given prompts:

`gpg --gen-key`

This should produce a lot of output, which at the end shows a long string of characters which is your GPG ID, we just need to take the last 8 characters of this key, append "0x" to the front, and use it in our next command. For example, the bottom of the output when I produced my key looked like this:

![image](https://user-images.githubusercontent.com/56923966/194775376-4e4c18da-92aa-4302-81fe-77c2cacdd6b7.png)

I extracted the 8 characters and appended the "0x" to the front with left me with the proper id: "0xEB39D262". We then use that in the next command which helps us initialize our key pair:

`pass init 0xEB39D262`

Finally, we can tell our Git Credential Manager (GCM) that we intend to use gpg as our storage method by running the following command:

`git config --global credential.credentialStore gpg`

Next, we have to actually generate our Personal Access Token, which can be done through the GitHub website by navigating to "Settings" in the top right expandable menu within your profile, then to "Developer Settings", and finally to "Personal access tokens". Within here we click to generate a new PAT, call it whatever you like, and give it full access to everything (or whatever your preferences are for this). Finally, generate the token and make sure to copy it into your clipboard as once you navigate away from this page you will not be able to get it back!

Now we can finally clone the SENSEI repository through HTTPS by running the following command in the directory in which you desire SENSEI to live in your container:

`git clone https://github.com/cjroy/SENSEI.git`

This should ask if you want to use OAuth Apps or a PAT, select PAT, and paste it from your clipboard. This should then prompt you to type in the gpg password that you just previously set up, which you can enter, and then SENSEI should begin the cloning process! After this, you will not need your PAT anymore as it is stored and encrypted within your container, and you no longer need to worry about authentication with GitHub! (You may need to enter the gpg password once or twice more to get everything running smoothly, so make sure to remember this in the meantime though)!

## Conclusion
With that, you are all ready to go with utilizing SENSEI within your brand-new Docker container! Your next steps are to build and test SENSEI, which can be done through the steps documented on the "[Building Sensei](https://github.com/cjroy/SENSEI/wiki/Setting-Up-SENSEI#building-sensei)" portion of the wiki!

If you have any questions at all regarding this guide please feel free to reach out to me at nrand02@vt.edu and I would be more than happy to help you get things up and running!


