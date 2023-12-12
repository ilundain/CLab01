pipeline{
    //Directives
    agent any
    tools {
        maven 'Maven'
        // maven 'Maven 3.9.6' 
    }
    environment{
        ArtifactId = readMavenPom().getArtifactId()
        Version = readMavenPom().getVersion()
        Name = readMavenPom().getName()
        GroupId = readMavenPom().getGroupId()       
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
                script {

                    def NexusRepo = Version.endsWith("SNAPSHOT") ? "CIDLab-SNAPSHOT" : "CIDLab-RELEASE"

                    nexusArtifactUploader artifacts: 
                    [[artifactId: "${ArtifactId}", 
                    classifier: '', 
                    file: "target/${ArtifactId}-${Version}.war", 
                    type: 'war']], 
                    credentialsId: '32d5f631-78db-4352-b9c8-752c36442d3d', 
                    groupId: "${GroupId}", 
                    nexusUrl: '172.20.10.162:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: "${NexusRepo}", 
                    version: "${Version}"     
                }
            }
        }

        // Stage 4: Print Env. Variables Values
        stage ('Print Env Variables') {
            steps {
                echo "Artifact ID is '${ArtifactId}'"
                echo "Version is '${Version}'"
                echo "GroupID is '${GroupId}'"
                echo "Name is '${Name}'"                
            }
        }


        // Stage 5 : Deploy
        stage ('deploy'){
            steps {
                echo ' deploying......'

            }
        }
        
    }

}