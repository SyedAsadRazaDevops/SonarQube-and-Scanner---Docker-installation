Static code inspection, or static source code analysis, as it is fondly called, is a debugging technique adopted during code reviews where the program being debugged is not executed.

There are several code analysis tools available to software engineers, such as SonarQube, Coverity, and Codacy.

# What is SonarQube?
SonarQube is a popular continuous inspection tool for code quality and code security that aims to help development teams ship better software. It functions as an automatic code review tool with support for more than 30 programming languages.

SonarQube easily interfaces with CI pipelines and DevOps builds to make code inspection swift and efficient for engineers. It is also self managed, satisfying the need for developers to ship quality and maintainable code at a fast pace.

Would you like to learn how to install SonarQube using Docker on Ubuntu Linux?
we are going to show you all the steps required to perform the SonarQube installation using Docker on a computer running Ubuntu.

# Installing SonarQube on Docker

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
# Installing SonarScanner

SonarScanner is a CLI tool that is used to run SonarQube tests. To view the test results, SonarScanner sends feedback from the test to the SonarQube server for you to review.

Select your operating system and download the file.

https://docs.sonarqube.org/latest/analyzing-source-code/scanners/sonarscanner/

After extracting the contents of the zip file, navigate to **conf/sonar-scanner.properties** and ensure the default server port is the same as your SonarQube port. This will allow SonarScanner to send analysis results to SonarQube.

**Now, add SonarScanner’s bin folder to your machine’s environment $PATH. On Windows, the procedure involves following these steps:**
- Download the Windows zip file
- Rename the file sonar-scanner
- Move the file to your preferred directory
- Add that directory as a new environment variable in the $PATH

" If you require more information about debugging in the SonarScanner **CLI**, use either of these flags with your commands: **-X**, **--verbose**, or **-Dsonar.verbose=true**."

### you can run SonarScanner from the Docker image with the below command:
```
docker run \\
    --rm \\
    -e SONAR_HOST_URL="<http://$>{SONARQUBE_URL}" \\
    -e SONAR_LOGIN="myAuthenticationToken" \\
    -v "${YOUR_REPO}:/usr/src" \\
    sonarsource/sonar-scanner-cli
```

### Creating a new SonarQube project
click the Projects bar on the homepage, and decide how you want to create a new project. we’ll use the manual mode. After clicking Manual, you’ll see fields displayed for the Project display name and Project key:
![image](https://user-images.githubusercontent.com/71556060/215311962-2a500081-f61d-4ce7-86f0-fbf1acde8510.png)

After completing these fields, click Set Up. In the next window, select Locally. In the window after that, click Generate to generate a token for your project. Then, click Continue to finish up with the tokenization.

### Run the SonarScanner Code Analysis for this project
```
sonar-scanner \
  -Dsonar.projectKey=my_project_key_as_defined_on_sonarqube_web \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://localhost:9000 \
  -Dsonar.login=my_sonarqube_project_token
```

![image](https://user-images.githubusercontent.com/71556060/215470288-6088ba86-96d8-4efd-810d-ffe1589ccc0d.png)




LINK:
- https://gist.github.com/Swiss-Mac-User/8cc5a5e688f1b22d2c17b865649201d8
- https://blog.logrocket.com/inspect-code-docker-sonarqube/
- https://techexpert.tips/sonarqube/sonarqube-docker-installation/
- https://www.sonarsource.com/products/sonarqube/deployment/





