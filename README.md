7
---
- name: Install htop system monitor tool
  hosts: local
  become: true
  
  tasks:
    - name: Install htop package
    ansible.builtin.package:
      name: htop
      state: present

8
---
- name: Deploy JAR
  hosts: local
  become: true
  
  tasks:
    - name: Copy JAR file
      copy:
        src: /mnt/c/ProgramData/Jenkins/.jenkins/workspace/your-job/target/your-app.jar
        dest: /home/your-username/ansible-lab/app.jar
        mode: '0755'
    - name: Run JAR file
      shell: nohup java -jar /home/your-username/ansible-lab/app.jar > app.log 2>&1 &

  sudo apt install-deafult-jdk

mainifest error
<build>
<plugins>
<plugin>
<artifactId>maven-jar-plugin</artifactId>
<version>3.1.0</version>
<configuration>
<archive>
<manifest>
<addClasspath>true</addClasspath>
<mainClass>com.multit.App</mainClass> <!-- Replace with main class
</manifest>
</archive>
</configuration>
</plugin>
</plugins>
</build>

10
trigger:
 - main
pool:
  name: 'MyLocalPool' # Your self-hosted agent pool name
steps:
# Step 1: Checkout the Code from GitHub
- checkout: self
  displayName: 'Checkout Code from GitHub'
# Step 2: Build and Run Unit Tests
- script: mvn clean test
  displayName: 'Build and Run Unit Tests'
# Step 3: Publish Test Results (JUnit)
- task: PublishTestResults@2
  inputs:
    testResultsFiles: '**/target/surefire-reports/TEST-*.xml'
    testResultsFormat: 'JUnit'
    failTaskOnMissingResultsFile: true
displayName: 'Publish Maven Test Results'
# Step 4: Publish Build Artifacts
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: 'target'
    ArtifactName: 'drop'
    publishLocation: 'Container'
  displayName: 'Publish Build Artifacts'
