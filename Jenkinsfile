pipeline {
  agent any
   stages {
    stage('Git checkout') {
      steps {
         echo 'This is for cloning the gitrepo'
         git branch: 'master', url: 'https://github.com/sowjanyaparupally/star-agile-health-care'
                          }
            }
    stage('Create a Package') {
      steps {
         echo 'This will create a package using maven'
         sh 'mvn package'
                             }
            }

    stage('Publish the HTML Reports') {
      steps {
          publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '/var/lib/jenkins/workspace/health-job/target/surefire-reports', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                        }
            }
    stage('Create a Docker image') {
      steps {
        sh 'docker build -t sowjanyaparupally/healthcare20:1.0 .'
                    }
            }
    stage('Login to Dockerhub') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockercode', passwordVariable: 'dockerpasswd', usernameVariable: 'dockeruser')]) {
        sh 'docker login -u sowjanyaparupally -p ${dockerpasswd}'
                                                                    }
                                }
            }
    stage('Push the Docker image') {
      steps {
        sh 'docker push sowjanyaparupally/healthcare20:1.0'
                                }
            }
    stage('Ansible Playbook') {
      steps {
        ansiblePlaybook credentialsId: 'sshkey', disableHostKeyChecking: true, installation: 'ansible', inventory: '/etc/ansible/hosts', playbook: 'deploy.yml', vaultTmpPath: ''
            }
        }

    }
}
