pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Clone'){
            
            steps{
                
                script{
                    
                    git branch: 'main', credentialsId: 'git_credentials', url: 'https://github.com/riyaz1121/sonar-task.git'
                }
            }
        }
    stage('Maven Build'){
        
        steps{
            
            script{
                def mavenHome = tool name: "Maven-3.9.0", type: "maven"
                def mavenCMD = "${mavenHome}/bin/mvn"
                sh "${mavenCMD} clean package"
            }
        }
      }
    stage('SonarQube analysis'){
        
        steps{
                script{
                     withSonarQubeEnv(credentialsId: 'sonar-token') {
                     def mavenHome = tool name: "Maven-3.9.0", type: "maven"
                     def mavenCMD = "${mavenHome}/bin/mvn"
                     sh "${mavenCMD} sonar:sonar"
                    } 
                }
           }
    }
       stage('upload war to nexus'){ 
           
           steps{
                script{
                    
                   nexusArtifactUploader artifacts: [
                       [
                           artifactId: 'sidgs', 
                           classifier: '', 
                           file: 'target/sidgs-3.9.0.jar', 
                           type: 'jar'
                           ]
                           ], 
                           credentialsId: 'nexus-credentials', 
                           groupId: 'name', 
                           nexusUrl: '3.145.57.237:8081/', 
                           nexusVersion: 'nexus3', 
                           protocol: 'http', 
                           repository: 'mahas-repo', 
                           version: '3.9.0'
               } 
           }
        }
        
    }
}
