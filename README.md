# docker-cheatsheet


cd /home/asd/webApplicationn
git pull origin Snapshot
npm install
grunt -v
echo "asd" | sudo -S docker build -t repo.asd.com:8083/solarpulse-web:$1 .
echo "asd" | sudo -S docker stop `echo "asd" | sudo -S docker ps | grep "nodejs app.js pstg" | awk '{print $1}'`
echo "asd" | sudo -S docker run -p 5000:5000 -d repo.asd.com:8083/solarpulse-web:$1
echo "asd" | sudo -S docker ps
echo "asd" | sudo -S docker push repo.asd.com:8083/solarpulse-web:$1


