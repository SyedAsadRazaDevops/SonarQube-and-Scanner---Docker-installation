Would you like to learn how to install SonarQube using Docker on Ubuntu Linux?
we are going to show you all the steps required to perform the SonarQube installation using Docker on a computer running Ubuntu.

### Install the Docker service.

```
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - 
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" 
apt-cache policy docker-ce
sudo apt install docker-ce -y![image](https://user-images.githubusercontent.com/71556060/215311082-5c75bb69-6e29-4b0d-9c7b-004763db235e.png)
```
### Download the SonarQube Docker image from the online repository.
```
docker pull sonarqube
```
### Create Docker volumes to store the SonarQube persistent data.
```
docker volume create sonarqube-conf 
docker volume create sonarqube-data
docker volume create sonarqube-logs
docker volume create sonarqube-extensions
```
Verify the persistent data directories.
```
docker volume inspect sonarqube-conf 
docker volume inspect sonarqube-data
docker volume inspect sonarqube-logs
docker volume inspect sonarqube-extensions
```
Optionally, create symbolic links to an easier access location.
```
mkdir /sonarqube
ln -s /var/lib/docker/volumes/sonarqube-conf/_data /sonarqube/conf
ln -s /var/lib/docker/volumes/sonarqube-data/_data /sonarqube/data
ln -s /var/lib/docker/volumes/sonarqube-logs/_data /sonarqube/logs
ln -s /var/lib/docker/volumes/sonarqube-extensions/_data /sonarqube/extensions
```
### Start a SonarQube container with persistent data storage.
```
docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 -v sonarqube-conf:/opt/sonarqube/conf -v sonarqube-data:/opt/sonarqube/data -v sonarqube-logs:/opt/sonarqube/logs -v sonarqube-extensions:/opt/sonarqube/extensions sonarqube
```
**Open your browser and enter the IP address of your web server plus :9000**

In our example, the following URL was entered in the Browser:
```
http://192.168.15.10:9000
```
### The SonarQube dashboard will be presented.
Click on the Login button and use the Sonarqube default username and password.

• Default Username: admin
• Default Password: admin

### Verify the containers status using the following command:
```
docker ps -a
```
### Verify the status of a container.
```
docker ps -a -f name=sonarqube
```
### To stop a container, use the following command:
```
docker container stop sonarqube
```
### To start a container, use the following command:
```
docker container start sonarqube
```
### To restart a container, use the following command:
```
docker container restart sonarqube
```
### In case of error, use the following command to verify the container logs.
```
docker logs sonarqube
```













