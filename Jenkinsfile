pipeline{
    //Directives
    agent any
    tools {
        maven 'Maven'
        // maven 'Maven 3.9.6' 
    }

    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage 2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'
            }
        }

        // State 3: Publish Artifacts (to Nexus)
        stage ('Publish to Nexus'){
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'JaimeDevOpsLab', classifier: '', file: 'target/JaimeDevOpsLab-0.0.4-SNAPSHOT.war', type: 'war']], credentialsId: '32d5f631-78db-4352-b9c8-752c36442d3d', groupId: 'com.jaimedevopslab', nexusUrl: '172.20.10.162:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'CIDLab-SNAPSHOT', version: '0.0.4-SNAPSHOT'
            }
        }

        // Stage 4 : Deploy
        stage ('deploy'){
            steps {
                echo ' deploying......'

            }
        }
        
    }

}