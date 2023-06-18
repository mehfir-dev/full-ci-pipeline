pipeline {
    
    stage('clone the repo'){
        
        git credentialsId: 'GIT_CRED', url: 'https://github.com/wome06/full-ci-pipeline.git'
    }
    stage('Maven Build'){
        def mavenHome = tool name: "Maven-3.9.1", type: "maven"
        def mavenCMD = "${mavenHome}/bin/mvn"
        sh "${mavenCMD} clean package"
    }
    
    stage('Sonarqube analysis'){
        withSonarQubeEnv('Sonar-server-9.0'){
            def mavenHome = tool name: "Maven-3.9.1", type: "maven"
            def mavenCMD = "${mavenHome}/bin/mvn"
            sh "${mavenCMD} sonar:sonar"
        }
    }
    
    stage('upload Build Artifact'){
        nexusArtifactUploader artifacts: [[artifactId: 'junit', classifier: '', file: 'target/01-maven-web-app.war', type: 'war']], credentialsId: 'NEXUS_CRED', groupId: 'in.ashokit', nexusUrl: '184.73.131.234:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '3.0-SNAPSHOT'
    }

}
