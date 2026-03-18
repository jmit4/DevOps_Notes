# Jenkins

Notes on Jenkins pipelines and configuration.

---

## Key Concepts

| Term | Description |
|---|---|
| **Job / Project** | A configured task (Freestyle or Pipeline) |
| **Pipeline** | A series of automated steps defined in a Jenkinsfile |
| **Stage** | A logical phase of the pipeline (Build, Test, Deploy) |
| **Step** | A single command or action within a stage |
| **Agent** | Node/machine that executes the pipeline |
| **Artifact** | File produced by the build that can be archived |
| **Credential** | Securely stored secret (username/password, SSH key, token) |

---

## Declarative Pipeline (Jenkinsfile)

The recommended modern approach:

```groovy
pipeline {
    agent any

    environment {
        APP_NAME = 'my-app'
        DOCKER_IMAGE = "myrepo/${APP_NAME}:${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'npm ci'
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
            post {
                always {
                    junit 'test-results/**/*.xml'
                }
            }
        }

        stage('Docker Build & Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-hub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker build -t "$DOCKER_IMAGE" .
                        docker push "$DOCKER_IMAGE"
                    '''
                }
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                sh './scripts/deploy.sh'
            }
        }
    }

    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
            // mail to: 'team@example.com', subject: 'Build Failed', body: "Check ${env.BUILD_URL}"
        }
        always {
            cleanWs()    // Clean workspace after run
        }
    }
}
```

---

## Agent Options

```groovy
agent any                          // Any available agent
agent none                         // No global agent; each stage defines its own
agent { label 'linux' }            // Agent with specific label
agent {
    docker {
        image 'node:20-alpine'
        args '-v /tmp:/tmp'
    }
}
```

---

## Parallel Stages

```groovy
stage('Test') {
    parallel {
        stage('Unit Tests') {
            steps { sh 'npm run test:unit' }
        }
        stage('Integration Tests') {
            steps { sh 'npm run test:integration' }
        }
    }
}
```

---

## Parameters (Manual Input)

```groovy
parameters {
    string(name: 'BRANCH', defaultValue: 'main', description: 'Branch to build')
    booleanParam(name: 'DEPLOY', defaultValue: false, description: 'Deploy after build?')
    choice(name: 'ENV', choices: ['staging', 'production'], description: 'Target environment')
}

// Usage:
sh "git checkout ${params.BRANCH}"
if (params.DEPLOY) {
    sh './deploy.sh'
}
```

---

## Credentials & Secrets

```groovy
// Username & Password
withCredentials([usernamePassword(credentialsId: 'my-cred', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
    sh 'curl -u "$USER:$PASS" https://api.example.com'
}

// Secret text
withCredentials([string(credentialsId: 'api-token', variable: 'TOKEN')]) {
    sh 'curl -H "Authorization: Bearer $TOKEN" https://api.example.com'
}

// SSH key
withCredentials([sshUserPrivateKey(credentialsId: 'ssh-key', keyFileVariable: 'SSH_KEY')]) {
    sh 'ssh -i "$SSH_KEY" user@server'
}
```

---

## Useful Built-in Variables

| Variable | Description |
|---|---|
| `env.BUILD_NUMBER` | Current build number |
| `env.BUILD_URL` | URL to the build in Jenkins |
| `env.JOB_NAME` | Name of the job |
| `env.BRANCH_NAME` | Branch being built (Multibranch) |
| `env.GIT_COMMIT` | Full git commit SHA |
| `env.WORKSPACE` | Workspace directory path |

---

## Scripted Pipeline vs Declarative

| | Declarative | Scripted |
|---|---|---|
| Syntax | Structured, opinionated | Groovy DSL, flexible |
| Error handling | `post` blocks | try/catch |
| Best for | Most pipelines | Complex logic |

Always prefer **Declarative** unless you need advanced Groovy logic.
