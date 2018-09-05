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
