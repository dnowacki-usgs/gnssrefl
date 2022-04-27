# Assistance on deploying gnssrefl [docker image from dockerhub](https://hub.docker.com/repository/docker/unavdocker/gnssrefl)
April 2022

(Feedback on these instructions is appreciated!)

for jupyter notebook version, please see [gnssrefl_jupyter instructions](https://www.unavco.org/gitlab/gnss_reflectometry/gnssrefl_jupyter)
## Install Docker
&ensp;&ensp; Pick your system and follow instructions on the Docker website. 
* **Mac** - https://docs.docker.com/docker-for-mac/install/ 
* **Windows** - https://docs.docker.com/docker-for-windows/install/ 
* **Ubuntu** - https://docs.docker.com/install/linux/docker-ce/ubuntu/ 

*Once installed, type `docker run hello-world` in terminal to check if installed correctly.*

More information on [getting started, testing your installation, and developing.](https://docs.docker.com/get-started/) 

Useful tool to use is [Docker Desktop](https://www.docker.com/products/docker-desktop)

## Run gnssrefl Docker
* cd into the local directory that you wish to keep your processed results
* <code>docker run -it -v $(pwd)/refl_code:/etc/gnssrefl/refl_code/ -v $(pwd)/refl_code/Files:/etc/gnssrefl/refl_code/Files unavdocker/gnssrefl:latest /bin/bash</code>

(Description of the commands used:  <code>-it</code> calls interactive process (bin/bash shell); <code>-v</code> mounts external volumes to allow the user to keep their processing results and figures)

* Start your [gnssrefl processing!](https://github.com/kristinemlarson/gnssrefl#iv-rinex2snr---extracting-snr-data-from-rinex-files-)

### notes:
* docker has vim for editing text files (ie .json station config file)
* if you want to process rinex files already on your local machine, you can copy them into <code>/refl_code/</code> local directory that is already mounted to the container given the previous run command.  If you have a lot of rinex and want to keep organized, you can copy into refl_code/rinex/station/cyyy/, where station is the 4char ID and cyyy is the 4char year, and then mount that directory in the docker run command as follows: <code> docker run -it -v $(pwd)/refl_code:/etc/gnssrefl/refl_code/ -v $(pwd)/refl_code/Files:/etc/gnssrefl/refl_code/Files/ -v $(pwd)/refl_code/rinex/station/cyyy:/etc/gnssrefl/refl_code/rinex/station/cyyy/ unavdocker/gnssrefl:latest /bin/bash </code>


### Shutdown Docker <a name="Shutdown"></a>
To Shut down the container from the terminal just use `ctrl+c`

To shut down the docker container run `docker stop [container name]`
If you need to see the container(s) you have running you can use `docker ps`

### Update Docker Image to newest version <a name="Update Docker"></a>
To update your Image from our DockerHub. Run `docker pull unavdocker/gnssrefl`


## For WINDOWS USERS:
(thank you Paul Wu and James Monaco @ Univ. of CO for this)
* install [Docker for Windows](https://docs.docker.com/desktop/windows/install/)
	* Problem: <code>WSL2 Installation is incomplete</code>.  
		* Solution: Need to download and install [from step 4](https://docs.microsoft.com/en-us/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package)
	* Problem: Docker stuck at initial stage
		* Solution: restart the computer after docker installation
	* Problem: need to convert existing WSL environment into WSL 2 and associate with Docker
	 	* Solution: follow [this documentation](https://docs.docker.com/desktop/windows/wsl/)


* execute docker run command (see above) in terminal window
* Feeback from jupyter notebook user:
	* About folder permission: In the notebook environment test, the error prompted that the program could not write to the file.  This is remedied by changing the permissions of the folder from the command line.

## additional references:
* [gnssrefl base image dockerfile](https://gitlab.com/gnss_reflectometry/gnssrefl_docker_base_img/-/blob/master/Dockerfile)
* [gnssrefl docker file](https://github.com/kristinemlarson/gnssrefl/blob/master/Dockerfile)

