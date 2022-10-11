@Library('pipeline-library-demo')_

def mymethod(String name = 'human') {
  echo "Hello, ${name}."
  echo "Hello, ${name}."
}

pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }

    stages {
        stage('Build') {
            steps {
                mymethod "socgen"
                sayHello "DevOps"
                // Get some code from a GitHub repository
                git branch: 'main', url: 'https://github.com/socgenapp/socgapp.git'
                // Run Maven on a Unix agent.
                sh "mvn -gs settings.xml -Dmaven.test.failure.ignore=true clean deploy sonar:sonar -Dsonar.projectKey=socgenapp \
  -Dsonar.host.url=http://13.233.200.83:9000 \
  -Dsonar.login=sqp_0a47ff2d571925d042fe0120fa60699a9c3e015f"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}
