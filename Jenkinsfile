pipeline {
    
    agent any
    
    stages {
        
        stage("checkout") {
            steps {
                sh "ls"
                git branch:"main", url : 'https://github.com/gintokid/spring-petclinic'
                sh "ls"
            }
        }
        
        stage("build") {
            steps {
                sh "./mvnw package"   
            }
        }
        
        stage("capture") {
            steps {
                archiveArtifacts artifacts: 'target\\*.jar'
                junit 'target/surefire-reports/TEST-*.xml'
                jacoco()
            }
        }
    }
    
    post {
        
        always {
            emailext body: "${env.BUILD_URL}\n${currentBuild.absoluteUrl}", 
            to: 'mad@foo.bar',
            recipientProviders: [previous()], 
            subject: "${currentBuild.currentResult}: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]"
        }
    }
    
}
