# Jenkins-Zero-To-Hero

Are you looking forward to learn Jenkins right from Zero(installation) to Hero(Build end to end pipelines)? then you are at the right place.

## Installation on EC2 Instance

YouTube Video ->
https://www.youtube.com/watch?v=zZfhAXfBvVA&list=RDCMUCnnQ3ybuyFdzvgv2Ky5jnAA&index=1

![Screenshot 2023-02-01 at 5 46 14 PM](https://user-images.githubusercontent.com/43399466/216040281-6c8b89c3-8c22-4620-ad1c-8edd78eb31ae.png)

Install Jenkins, configure Docker as agent, set up cicd, deploy applications to k8s and much more.

## AWS EC2 Instance

- Go to AWS Console
- Instances(running)
- Launch instances

Then SSH into the EC2 instance by writing similar command in powershell

```bash
ssh -i "Jenkins.pem" ubuntu@ec2-52-90-109-70.compute-1.amazonaws.com
```

Jenkins is a JAVA based application. So we need to have JAVA as a prerequisite.

<img width="994" alt="Screenshot 2023-02-01 at 12 37 45 PM" src="https://user-images.githubusercontent.com/43399466/215974891-196abfe9-ace0-407b-abd2-adcffe218e3f.png">

### Install Jenkins.

Pre-Requisites:

- Java (JDK)

### Run the below commands to install Java and Jenkins

Install Java

```bash
sudo apt update
sudo apt install openjdk-11-jre
```

Verify Java is Installed

```bash
java -version
```

Now, you can proceed with installing Jenkins

```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins
```

**Note: ** By default, Jenkins will not be accessible to the external world due to the inbound traffic restriction by AWS. Port 8080 is not accessible to the external world. AWS is blocking access on the specific IP address. Open port 8080 in the inbound traffic rules as show below.

- EC2 > Instances > Click on <Instance-ID>
- In the bottom tabs -> Click on Security
- Security groups
- Add inbound traffic rules as shown in the image(on 4th image) (you can just allow TCP 8080 as well, in my case, I allowed `All traffic`).

![Screenshot 2024-05-08 001950](https://github.com/kaustubhchile/git_practice_test/assets/72078555/08d769e7-55fc-4cb8-a289-42032be514f1)
![Screenshot 2024-05-08 002621](https://github.com/kaustubhchile/git_practice_test/assets/72078555/eb0ea27c-a36e-45ca-a714-2f08aee4bfef)
![Screenshot 2024-05-08 003328](https://github.com/kaustubhchile/git_practice_test/assets/72078555/242f31ab-edfb-4fae-bdd1-0f764b75f402)
<img width="1187" alt="Screenshot 2023-02-01 at 12 42 01 PM" src="https://user-images.githubusercontent.com/43399466/215975712-2fc569cb-9d76-49b4-9345-d8b62187aa22.png">

### Login to Jenkins using the below URL:

http://<ec2-instance-public-ip-address>:8080 [You can get the ec2-instance-public-ip-address from your AWS EC2 console page]
eg: `http://52.90.109.70:8080`

Note: If you are not interested in allowing `All Traffic` to your EC2 instance 1. Delete the inbound traffic rule for your instance 2. Edit the inbound traffic rule to only allow custom TCP port `8080`

After you login to Jenkins, - Run the command to copy the Jenkins Admin Password - `sudo cat /var/lib/jenkins/secrets/initialAdminPassword` - Enter the Administrator password

<img width="1291" alt="Screenshot 2023-02-01 at 10 56 25 AM" src="https://user-images.githubusercontent.com/43399466/215959008-3ebca431-1f14-4d81-9f12-6bb232bfbee3.png">

### Click on Install suggested plugins

<img width="1291" alt="Screenshot 2023-02-01 at 10 58 40 AM" src="https://user-images.githubusercontent.com/43399466/215959294-047eadef-7e64-4795-bd3b-b1efb0375988.png">

Wait for the Jenkins to Install suggested plugins

<img width="1291" alt="Screenshot 2023-02-01 at 10 59 31 AM" src="https://user-images.githubusercontent.com/43399466/215959398-344b5721-28ec-47a5-8908-b698e435608d.png">

Create First Admin User or Skip the step [If you want to use this Jenkins instance for future use-cases as well, better to create admin user]

<img width="990" alt="Screenshot 2023-02-01 at 11 02 09 AM" src="https://user-images.githubusercontent.com/43399466/215959757-403246c8-e739-4103-9265-6bdab418013e.png">

If you choose to skip the step then remember that when logging next time it will ask for username and password

username: admin

password: run the following command-->

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Jenkins Installation is Successful. You can now starting using the Jenkins.

**_Note_**: Ensure that you are in the regular user instead of root user

<img width="990" alt="Screenshot 2023-02-01 at 11 14 13 AM" src="https://user-images.githubusercontent.com/43399466/215961440-3f13f82b-61a2-4117-88bc-0da265a67fa7.png">

## Docker Slave Configuration

Run the below command to Install Docker

```
sudo apt update
sudo apt install docker.io
```

### Grant Jenkins user and Ubuntu user permission to docker deamon.

```
sudo su -
usermod -aG docker jenkins
usermod -aG docker ubuntu
systemctl restart docker
```

After this switch user to jenkins using the following command

```bash
  su - jenkins
```

To ckeck whether docker is installed and user has access to docker run the following command:

```bash
  docker run hello-world
```

Once you are done with the above steps, it is better to restart Jenkins as jenkins sometimes doesnt pick up the changes

```bash
http://<ec2-instance-public-ip>:8080/restart
eg. http://18.206.176.142:8080/restart
```

The docker agent configuration is now successful.

## Install the Docker Pipeline plugin in Jenkins:

- Log in to Jenkins.
- Go to Manage Jenkins > Manage Plugins.
- In the Available tab, search for "Docker Pipeline".
- Select the plugin and click the Install button.
- Restart Jenkins after the plugin is installed.

<img width="1392" alt="Screenshot 2023-02-01 at 12 17 02 PM" src="https://user-images.githubusercontent.com/43399466/215973898-7c366525-15db-4876-bd71-49522ecb267d.png">

![Screenshot 2024-05-08 083712](https://github.com/kaustubhchile/git_practice_test/assets/72078555/b9bd1727-e565-4d7f-8d83-b0c62ee92eb7)

![Screenshot 2024-05-08 084234](https://github.com/kaustubhchile/git_practice_test/assets/72078555/c806506a-b473-45d6-96f0-5abf37ef7518)

Wait for the Jenkins to be restarted.

## Creation of Jenkins Pipeline

- When you click on New Item there are multiple ways to create a Jenkins Pipeline

  ![Screenshot 2024-05-08 090602](https://github.com/kaustubhchile/git_practice_test/assets/72078555/6e046ae8-f563-4193-b702-cc57fd4de329)

- and then choose the pipeline approach:

  ![Screenshot 2024-05-08 092804](https://github.com/kaustubhchile/git_practice_test/assets/72078555/cd72540f-0c5f-439f-9cad-845836fee3e8)

  Name your pipeline `first-jenkins-job`

  In pipeline approach one can write declarative or scripted pipelines here

  ![Screenshot 2024-05-09 101126](https://github.com/kaustubhchile/git_practice_test/assets/72078555/86000387-8daa-46d6-9cfb-fc656dc95b66)  
  Then write the jenkins file code from `my-first-pipeline` here.

  ### How Jenkins works

  - Jenkins is all about picking up the code from the local development or any version control system and delivering it to production while automating all the stages that are involved in between.

    ![image](https://github.com/kaustubhchile/git_practice_test/assets/72078555/54c57b6c-1a1e-4ea4-b125-3136de95aae7)

    Basic JenkinsFile eg.

    ```
    pipeline {
      agent {
        docker {
          image 'node:16-alpine'
        }
      }
    stages {
        stage('Test') {
          steps { -->Steps: commands that you want to execute in a particular stage
            sh 'node --version'
          }
        }
      }
    }
    ```

    There is also an option for pipeline syntax which can help you to write your jenkins pipeline.

  ![Screenshot 2024-05-09 102935](https://github.com/kaustubhchile/git_practice_test/assets/72078555/96348f92-a389-4b8f-af19-80b65ba89f67)  
  Then choose the following option: `pipeline script from SCM` that is pick up a pipeline script from source code

  ![Screenshot 2024-05-09 103256](https://github.com/kaustubhchile/git_practice_test/assets/72078555/07e28125-8392-4761-86e5-9157fe0cc426)

  Then add the following details:

  ![Screenshot 2024-05-09 105039](https://github.com/kaustubhchile/git_practice_test/assets/72078555/76958005-5c4d-438c-a0ba-afabece68c73)  
  Then keep the branch as main as it is the current branch of the github repository and save the changes.

  ![Screenshot 2024-05-09 105246](https://github.com/kaustubhchile/git_practice_test/assets/72078555/719343f4-c475-438c-8e26-825beac156b9)

  Then click on the build now:

  ![Screenshot 2024-05-09 105701](https://github.com/kaustubhchile/git_practice_test/assets/72078555/66fc91a2-ab36-4322-b64c-b622ac24136f)

  To see the console output click here:

  ![Screenshot 2024-05-10 174414](https://github.com/kaustubhchile/git_practice_test/assets/72078555/77c43d22-8df3-4d1f-9d18-bbf9b976eb7c)

  ![Screenshot 2024-05-09 110212](https://github.com/kaustubhchile/git_practice_test/assets/72078555/94faae96-c99f-4979-be5e-1cebed8b775a)  
  In this above simple created pipeline we just want to see whether node is installed or not with the correct version.

## Multi Stage Multi Agent

Set up a multi stage jenkins pipeline where each stage is run on a unique docker agent. This is a very useful approach when you have multi language application or application that has conflicting dependencies.

Click on configure here:

![Screenshot 2024-05-10 181523](https://github.com/kaustubhchile/git_practice_test/assets/72078555/07436527-e42f-4a65-93be-87a26150c61e)

And Just modify this section of code here:

![Screenshot 2024-05-10 182018](https://github.com/kaustubhchile/git_practice_test/assets/72078555/5cb9c7b1-ddd8-4d3b-9610-f49cac344390)

And then execute the pipeline. When you `docker ps` for some time you will see your containers.

![Screenshot 2024-05-10 183740](https://github.com/kaustubhchile/git_practice_test/assets/72078555/173a3ae1-8d1a-405c-a9c5-34d5107dc8b6)
