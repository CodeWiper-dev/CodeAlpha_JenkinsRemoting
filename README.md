# CodeAlpha_JenkinsRemoting

Jenkins Remoting project for the CodeAlpha DevOps internship (Task 2): a Jenkins controller and a remote agent, each in its own Docker container, connected over WebSocket.

## Files
- docker-compose.yml - defines the Jenkins controller and the remote agent
- Dockerfile.agent - builds the agent image, running as the non-root jenkins user
- Jenkinsfile - pipeline that runs on the remote agent
- SETUP.md - manual setup steps

## How it works
1. The controller runs in Docker on port 8080.
2. A permanent node named remote-agent is created in Jenkins with the label remote-agent.
3. The agent container connects to the controller using the node's secret (stored in .env, not committed).
4. The pipeline uses agent { label 'remote-agent' }, so its steps run on the remote node instead of the controller.

## Run it
docker compose up -d jenkins
(create the node in the Jenkins UI and put its secret in .env as AGENT_SECRET=...)
docker compose --profile agent up -d --build agent

## Security and isolation
- The agent runs as an unprivileged user
- No volumes are shared between the controller and the agent
- Builds run on the agent's own workspace
- The secret is kept in .env, which is git-ignored

## What I learned
- How Jenkins controllers and agents communicate through Remoting
- Distributing builds to remote nodes using labels
- Running Jenkins components in Docker Compose


