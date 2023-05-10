pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo '*** No need to build the project, this is mostly JavaScript and HTML'
            }
        }
        stage ('SCA') {
            steps {
                echo '*** Checking dependencies...'
                dependencyCheck additionalArguments: ''' 
                    -o "./"
                    -s "./" 
                    -f ALL 
                    --prettyPrint''', odcInstallation: 'SCA: Dependency-Check'
                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }
        stage('SAST') {
            environment {
                SCANNER_HOME = tool 'SonarQube Scanner'
                PROJECT_KEY = 'owl-carousel2'
            }
            steps {
                echo '*** Scanning the code...'
                // test command here
                withSonarQubeEnv('SonarQube server') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner \
                    -Dsonar.projectKey=$PROJECT_KEY \
                    -Dsonar.projectName='Project analyzed by SonarQube' \
                    -Dsonar.projectVersion=1.0 \
                    -Dsonar.sources=src/js \
                    -Dsonar.language=javascript \
                    -Dsonar.sourceEncoding=UTF-8 \
                    '''
                }
            }
        }
    }
}
