# docker-cheatsheet

## dockerDeploy.sh
    cd /home/asd/webApplicationn
    git pull origin Snapshot
    npm install
    grunt -v
    echo "asd" | sudo -S docker build -t repo.asd.com:8083/solarpulse-web:$1 .
    echo "asd" | sudo -S docker stop `echo "asd" | sudo -S docker ps | grep "nodejs app.js pstg" | awk '{print $1}'`
    echo "asd" | sudo -S docker run -p 5000:5000 -d repo.asd.com:8083/solarpulse-web:$1
    echo "asd" | sudo -S docker ps
    echo "asd" | sudo -S docker push repo.asd.com:8083/solarpulse-web:$1

## dockerRMI.sh
    sudo docker images | awk '{print $3}' > ~/logs/shellLogs/dockerId.log
    while IFS='' read -r serviceProp || [[ -n "$serviceProp" ]]; do
    sudo docker rmi -f $serviceProp
    done < ~/logs/shellLogs/dockerId.log

## Start MongoDB docker container with authentication and port set to non default port
    docker run --restart always --name Mongo -p 0.0.0.0:12345:27017 -v dump:/home/root/dump/  -e MONGO_INITDB_ROOT_USERNAME=asd -e MONGO_INITDB_ROOT_PASSWORD=asd -m 500m -d mongo:3.2

## Example dockerfile for a node app with port 5000 exposed
    FROM node:carbon

    # Create app directory
    WORKDIR /usr/src/app

    # Install app dependencies
    # A wildcard is used to ensure both package.json AND package-lock.json are copied
    # where available (npm@5+)
    COPY package*.json ./

    RUN npm install
    # If you are building your code for production
    # RUN npm install --only=production

    # Bundle app source
    COPY . .

    EXPOSE 8080
    CMD [ "nodejs", "app.js" , "pstg" ]

# To run a container in the background use
	docker run -d redis
    
# docker-compose.yml


	version: '3'								--> always include
	services: 									--> for the various docker containers
 		redis-server:								--> Name given to docker container
 		 image:'muzammilmomin/redis'				--> Image to be used
		node-app:									--> Second docker container
  		 build: .									--> Build from current directory
  		 ports:									--> Port mapping for this docker container
   	 	 - "4001:8081"							


>To use the redis server , replace localhost with Docker container name within the code.

	docker run myimage ---> docker-compose up											
------------------------------------------------------------------------------------																					
>To include the latest changes we would execute 


	docker build .																
	docker run myimage          
	
>We can instead execute


	docker-compose up --Build	
	


>Run docker-compose in the background

	docker-compose up -d
	
>To stop

	docker-compose down
_____________
docker run 
	docker create 
	docker start
	docker start -a
		"show run log"
	docker logs
		does not start a container , just outputs logs
	
	
docker run vs docker create and start
	docker run busybox ping google.com
		(OR)
	docker start `docker create busybox ping google.com`

		In the first one the control is not returned back to the terminal
		In the second one control will be returned back. The ping will continue to run.
		docker run is basically docker create + docker start -and
		
	

docker stop is SIGTERM 15 ; ie do cleanup . limited to 10 secs after which docker kill will be executed
docker kill is SIGTERM 9 ; ie force kill

docker exec -it <containerid> command
	-it --> accepts input from keyboard
		-i --> attach stdin to my terminal
		-t --> format the output

Also possible 
	>docker run -it alpine sh

Steps to create a docker image
	> Specify a base image. (FROM)
	> Run some commands to install required programs. (RUN)
	> Specify a startup command. (CMD)

Alternate (Not recommended)
	>docker run -it alpine sh
		>apk add --update redis
	>docker commit -c 'CMD ["redis-server"]' da89bc5731d1

Dockerfile
	>FROM <baseimage>
	>RUN <execute some command inside the container>
	>CMD <Command to be executed when starting the container>
	>COPY ./ ./
		   |  | container folder
		   |
		   | host folder aka current folder with files   

		>If file name is given then itll overwrite 
			Example: COPY test.txt /app/test.txt
	>WORKDIR /usr/app	      
		If it does not exist it will be created.

	>EXPOSE 80
		Exposes  the specified port and makes it available for inter-container communication.


Docker volumes
	>To prevent building of docker containers everytime we update source code , we use docker volumes.
	> docker run -v /app/node_modules -v $(pwd):/app <image id>
							|				|
							| 				|
											|
											|Map host dir into container dir
							If colon is not present , means dont map this folder. Instead use the folder within the container



