echo "declarative-git-pipeline"

pipeline {
    agent any
    
    stages {
        stage("checkout") {
            steps {
                git branch:"main", url:"https://github.com/gabokc/course3-jenkins-gs-spring-petclinic"
            }
        }
        stage("build") {
            steps {
                sh "./mvnw package"
            }
        }
        stage("capture") {
            steps {
                archiveArtifacts '**/target/*.jar'
                junit '**/target/surefire-reports/TEST*.xml'
                jacoco()
            }
        }
    }
    
    post {
        always {
            emailext body: "${env.BUILD_URL}\n${currentBuild.absoluteUrl}",
                to: "gabriel.munera@kinandcarta.com",
                recipientProviders: [previous()],
                subject: "${currentBuild.currentResult}: Job ${env.JOB_NAME} [${env.BUILD_NUMBER}]"
        }
    }
}
