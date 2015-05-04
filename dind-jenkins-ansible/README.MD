# dind-jenkins-ansible

Not currently published on docker hub.  This Dockerfile basically follows the advice published here: http://www.jayway.com/2015/03/14/docker-in-docker-with-jenkins-and-supervisord/

Main differences:
* Added ansible install
* Running jenkins as root user
* For amazon linux ami, need to kill internal docker gracefully using `start-stop-daemon --stop --pidfile "/var/run/docker.pid"` (not in this repo, but important to know when you stop the container.)  Found from this article: http://paislee.io/how-to-build-and-deploy-docker-images-with-drone/
