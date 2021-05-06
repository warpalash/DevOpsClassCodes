pipeline{
    agent any
    tools{
        maven 'Tool_Maven'
    }
    environment {
        dockertag = getVersion()
    }
    stages{
            stage('Pull code from git'){
                steps{
                       git credentialsId: 'warpalashforGit', url: 'https://github.com/warpalash/DevOpsClassCodes.git'
                }
            }
            stage('Build package from Maven'){
                steps{
                    sh "mvn clean package"
                }
            }
            stage('Build docker images'){
                steps{
                    sh "docker build . -t babban27/addbook:$dockertag"
                }
            }
            stage('Push docker images'){
                
                steps{
                    withCredentials([string(credentialsId: 'dockerPasswd', variable: 'dockpass')]) {
                    sh "docker login -u babban27 -p ${dockpass}"
}
                    sh "docker push babban27/addbook:$dockertag"
                }
            }
             stage('Deploy via ansible'){
                steps{
                    ansiblePlaybook credentialsId: 'EC2test_ubuntu', disableHostKeyChecking: true, extras: "-e dockertag=${dockertag}", installation: 'Tool_ansible', inventory: 'dev.inv', playbook: 'docker-deploy.yml'
                }
            }
            
        }
    }

def getVersion(){
    def comid = sh returnStdout: true, script: 'git rev-parse --short HEAD'
    return comid
}
