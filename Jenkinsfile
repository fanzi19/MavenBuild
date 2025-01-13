node(){
    stage('Code Checkout'){
        checkout changelog: false, poll: false, scm: scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/fanzi19/MavenBuild']])
    }
    
    stage('Build Automation'){
        sh """
            ls -lart
            mvn clean install
            ls -lart target
        """
        archiveArtifacts artifacts: 'target/*.war', fingerprint: true
    }
    
    stage('Code Scan'){
        //withSonarQubeEnv(credentialsId: 'SonarQubeCreds') {
        //    sh "${sonarHome}/bin/sonar-scanner"
        //}
    }
    
    stage('Code Coverage ') {
        //sh "curl -o coverage.json 'http://35.154.151.174:9000/sonar/api/measures/component?componentKey=com.java.example:java-example&metricKeys=coverage';sonarCoverage=`jq '.component.measures[].value' coverage.json`;if [ 1 -eq '\$(echo '\${sonarCoverage} >= 50'| bc)' ]; then echo 'Failed' ;exit 1;else echo 'Passed'; fi"
    }
    
    stage('Code Deployment'){
        deploy adapters: [tomcat9(credentialsId: 'TomcatCreds', path: '', url: 'http://localhost:8080/')], 
               contextPath: 'Planview', 
               onFailure: false, 
               war: 'target/java-example.war'
    }
}
