#!usr/bin/env groovy
node{
    stage('Git checkout'){
        git 'https://github.com/warpalash/DevOpsClassCodes.git'
    }
    stage('Compile'){
        withMaven(maven: 'TrainingMaven') {
        sh 'mvn compile'
    }
    }
    stage('Test'){
        try{
            withMaven(maven:'TrainingMaven')
           { sh 'mvn test'}
        }
        finally{
            junit 'target/surefire-reports/*.xml'
        }
    }
    stage('package'){
        withMaven(maven:'TrainingMaven')
        {sh 'mvn package'}
    }
}
