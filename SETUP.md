# Task 2 - Jenkins Remoting setup

1. Start the controller:
   docker compose up -d jenkins
   docker exec jenkins-controller cat /var/jenkins_home/secrets/initialAdminPassword

2. Open http://localhost:8080, paste the password, install suggested plugins.

3. Create the node:
   Manage Jenkins -> Nodes -> New Node
   - Name: remote-agent
   - Type: Permanent Agent
   - Remote root directory: /home/jenkins/agent
   - Labels: remote-agent
   - Launch method: "Launch agent by connecting it to the controller"
   Save, open the node page, and copy the secret value.

4. Set the controller URL: Manage Jenkins -> System -> Jenkins URL = http://jenkins:8080/
   (needed so the WebSocket agent resolves the controller inside the Docker network)

5. Start the agent:
   echo "AGENT_SECRET=<paste-secret>" > .env
   docker compose --profile agent up -d --build agent

6. Verify: the node shows as connected on the Nodes page.

7. Create a Pipeline job (Pipeline script from SCM) pointing at your Task 3 repo.
   The Jenkinsfile runs on label `remote-agent`, so the build executes remotely.
