pipeline{
agent any
    stages{
        stage('read the version'){
        steps{
        script{
            def packageJson = readJSON file: 'package.json'
            appVersion= packageJson.version
            echo "applicationVersion: $appVersion"
        }
        }
        }
      stage('build'){
        steps{
            sh """
            zip -q -r backend-${appVersion}.zip * -x Jenkinsfile -x backend-${appVersion}.zip
            ls -ltr
            """
        }
      }
        stage('nexus artifact upload'){
          steps{
              script{
                 nexusArtifactUploader(
                            nexusVersion: 'nexus3',
                            protocol: 'http',
                            nexusUrl: 'localhost:8081',
                            groupId: 'com.expensee',
                            version:  "    ${appVersion}",
                            repository: 'backend',
                            credentialsId: 'nexus_cred',
                            artifacts: [
                                [artifactId: projectName,
                                 classifier: '',
                                 file: 'backend-' +" ${appVersion}" + '.jar',
                                 type: 'jar']
        ]
     )

              }
          }
        }
        
    }



}
