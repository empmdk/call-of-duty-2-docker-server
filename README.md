# Call of Duty 2 server meet docker

Create a containarized [Call of Duty 2](https://en.wikipedia.org/wiki/Call_of_Duty_2) multiplayer game server.

## Prerequisite
You will need the following things :
1. The linux dedicated server binary, which can be found in this repository in the `backups` folder;
2. The orginal game, as it's contents are used by the dedicated server ;
3. A host machine of your choice with x86_64 architecture ;
4. [Docker](https://www.docker.com/) installed and configured on your host machine, obviously.

## Usage on unix
Clone or download the repository and follow theses steps to get the server up and running :
1. Copy the required data from the game to the server :
   1. Go in the `main` folder of your original game (install directory or retail DVD);
   2. Copy all the `iw_XX.iwd` from 00 to 15 to the `cod2server/main` folder;
   3. Copy all the localizations `localized_english_iwXX.iwd` to the `cod2server/main` (it might be another language).
2. Edit the config file located in `cod2server/main/config.cfg` to suits your needs:
   * **Set the RCON password to something strong !**
   * Tweak the rest as you see fit.
3. From the project root, Launch the server:
   ``` bash
   docker-compose up -d
   ```
4. Depending on your setup, you might have some port-forwarding and firewalling to do in order to make your server publicly available.
5. And "voila" !

## Troobleshooting
* For `docker-compose` to "see" potential changes in the Dockerfile, force the image build: 
  ``` bash
  docker-compose up --build
  ```
* If you choose another server binary, make sure to make it executable:
  ``` bash
  cd cod2server && chmod +x cod2_lnxded
  ```

## Notes
* There is a similar repository on github proposing a Call of Duty 2 server based on CentOS : [hberntsen/docker-cod2](https://github.com/hberntsen/docker-cod2)
* This setup was tested on an ubuntu server 14.04.3 LTS x86_64 architecture.
* You might want to use a separated host user to launch your docker container. In this case do not forget to add him to the docker group : `sudo gpasswd -a USER_NAME docker`, and restart docker dameon : `sudo service docker restart`.
* You will find original and modified `cod2_lnxded` dedicated server binaries in the `backup` folder. It might be usefull if you want to create a cracked server to play on without a CD key. But you won't need this as you did buy the game, didn't you ? 
  > if you didn't, [this link](http://killtube.org/showthread.php?1337-CoD2-Tutorial-How-to-make-your-cracked-server-show-up-in-the-master-list) might help your pervert mind create a masterlist-visible-pirate-uberawesome server.

* The gcc3-libs in the `cod2server` folder was used as a workaround before finding a proper solution to add it using official repositories (32 / 64 bit gcc library issues as dicussed [here](http://askubuntu.com/questions/454253/how-to-run-32-bit-app-in-ubuntu-64-bit/454254#454254)). It is not used anymore but will stay here as a backup, just in case it would not be supported anymore.

## Docker
If you are not familiar with docker (docker engine, docker hub, dockerfiles, docker images and containers), I suggest you have a look at the [documentation](https://docs.docker.com/) (especially the [dockerfiles](https://docs.docker.com/reference/builder/) part).

I also strongly recommend reading the [best practices for writing dockerfiles](https://docs.docker.com/articles/dockerfile_best-practices/) for a better understanding and a cleaner writing of dockerfiles.

**[Docker](https://www.docker.com/) is awesome**, give it a try ;-)

## Roadmap
- [ ] Extract the hard-coded rcon password from the server configuration (In an env var or in a secret)
- [ ] Try to minimize the docker image using Scratch or Alpine instead of full featured OSs images (this implies to manually import all libraries and more)
- [ ] Handle server logs (in the container or on a volume )
