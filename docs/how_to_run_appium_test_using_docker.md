## How to run appium test using docker and device emulator inside a container


#### Install Docker, Docker-compose and Docker-machine


#### Clone this docker-android project

          $ git clone https://github.com/budtmo/docker-android

#### Launch the containers using docker-compose
          $ cd docker-android
          $ docker-compose up -d

#### Dissecting contents of docker-compose.yml 
		  version: "3.2"

		   services:
		     selenium_hub: 
		       image: selenium/hub:3.14.0-curium
		       ports:
		         - 4444:4444

selenium_hub is the name/title assigned to a service, the name selenium_hub must be used when connecting from other containers or services
image: selenium/hub:3.14.0-curium is stored on docker hub and is downloaded locally, this means it won't build it locally
ports - 4444: 4444 the number left side of the colon (:) is the port on the host machine and the number on the right side is the port inside docker container, both numbers can be same, incase a stand alone version of selenium is running on the host machine, the number on the left side must be modified to any number that is not in use ex: - 4411 : 4444 To connect from host machine get the ip address of the container/service and suffix 4411 ex http://127.18.0.3:4411

		  real_device:
		    image: appium/appium
		    depends_on:
		      - selenium_hub
		    networks:
		      - appium_net
		    privileged: true
		    volumes:
		      - /dev/bus/usb:/dev/bus/usb
		      - ~/.android:/root/.android
		      - $PWD/example/sample_apk:/root/tmp
		    environment:
		      - CONNECT_TO_GRID=true
		      - SELENIUM_HOST=selenium_hub
		      # Enable it for msite testing
		      #- BROWSER_NAME=chrome

Once again the image is downloaded from docker hub
depends_on: Indicates that the service real_deviec depends on selenium_hub without this parameter, connection between the two services will not be established
networks:
 - appium_net
 This is a title for the network so that it can be shared across all the services
 volumes:
- /dev/bus/usb:/dev/bus/usb
- ~/.android:/root/.android
- $PWD/example/sample_apk:/root/tmp

All values left side of the colon (:) are folders on the host or local machine. All values right side of the colon (:) are folders inside the container. This means values on the left side get mapped to a different name inside the container, ex: $PWD or present working directory/example/sample_apk is accessible inside the container with a different name i.e /root/tmp
If a file called docker-android/example/sample_apk/test.apk is present in your local system, the same file can be accessed from inside the container using with a different path/name i.e /root/tmp/test.apk
This avoids the need to copy files back and forth, similary if a file is created inside the container /root/tmp/test.log that file will be accessible in the host/local system via docker-android/example/sample_apk/test.log



#### How to run the appium test using python

Identify the ipaddress of the device emualtor container          

		  $ IP=$(docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' nexus7_emulator)
		  $ echo $IP

          $ git cd example/android/python
          $ virtualenv -p python3 venv  #this must be executed only once during the setup
          $ source venv/bin/activate
          $ python chess.py


You should see a response similar to 172.21.0.3

To view the test inside the docker container, point your browser to 172.21.0.3:6080
