pipeline {
    agent any
    tools {
        maven 'maven3'
    }
    options {
        buildDiscarder logRotator(daysToKeepStr: '5', numToKeepStr: '7')
    }
    stages{
        stage('Build'){
            steps{
                 sh script: 'mvn clean package'
                 archiveArtifacts artifacts: 'target/*.war', onlyIfSuccessful: true
            }
        }
        stage('Upload War To Nexus'){
            steps{
                script{

                    def mavenPom = readMavenPom file: 'pom.xml'
                    def nexusRepoName = mavenPom.version.endsWith("SNAPSHOT") ? "myapp-snapshots" : "myapp-releases"
                   
				   
				   nexusArtifactUploader artifacts: [
					[artifactId: 'myapp_project-', 
					classifier: '', file: 'target/myapp_project-1.0.0.war', 
					type: 'war']], 
					credentialsId: 'f5dd286c-5b40-37d7-bd2c-9fef78d26b2c', 
					groupId: 'in.javahome', 
					nexusUrl: '192.168.56.13:8081', 
					nexusVersion: 'nexus3',
					protocol: 'http', 
					repository: 'http://192.168.56.13:8081/repository/myapp-release/',
					version: '1.0.0'
                    }
            }
        }
    }
}
