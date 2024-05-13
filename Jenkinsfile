pipeline {
    agent any

    environment {
        VERACODE_API_ID = '9c42b4bd8abf978e9dc1667451778c58'
        VERACODE_API_SECRET = '568e7bd345890259c3e869c8da3d42597b815e6a780c5d72ce76eaf3045a1736d80eb0ced53eb61956b3f0410546b2ac6e681829eaa3c9c621fcd722cee60b14'
        CI_TIMEOUT = '20'
        JOB_NAME = 'Jenkins_pipeline'
        VeracodeProfile = 'VeraDemo'
    }

    stages {
        stage('Maven Build') {
            steps {
                dir('app') {
                    sh 'mvn clean package'
                    pipeline {
    agent any

    environment {
        VERACODE_API_ID = '9c42b4bd8abf978e9dc1667451778c58'
        VERACODE_API_SECRET = '568e7bd345890259c3e869c8da3d42597b815e6a780c5d72ce76eaf3045a1736d80eb0ced53eb61956b3f0410546b2ac6e681829eaa3c9c621fcd722cee60b14'
        CI_TIMEOUT = '20'
        JOB_NAME = 'Jenkins_pipeline'
        VeracodeProfile = 'VeraDemo'
    }

    stages {
        stage('Maven Build') {
            steps {
                dir('app') {
                    sh 'mvn clean package'
                    stash includes: 'target/*.war', name: 'warFile'
                }
            }
        }

        stage('Veracode Policy from Wrapper') {
            agent {
                docker { 
                    image 'veracode/api-wrapper-java' 
                }
            }
            steps {
                script {
                    def warFilePath = sh(script: 'find . -name "*.war"', returnStdout: true).trim()
                    if (warFilePath) {
                        echo "Found WAR file: $warFilePath"
                        stash name: 'warFile', includes: warFilePath
                    } else {
                        error "No WAR file found"
                    }
                }

                unstash 'warFile'
                sh '''
                cp /opt/veracode/app/target/*.war /opt/veracode/verademo.war
                java -jar /opt/veracode/api-wrapper.jar \
                    -vid '9c42b4bd8abf978e9dc1667451778c58' \
                    -vkey '568e7bd345890259c3e869c8da3d42597b815e6a780c5d72ce76eaf3045a1736d80eb0ced53eb61956b3f0410546b2ac6e681829eaa3c9c621fcd722cee60b14' \
                    -action UploadAndScan \
                    -createprofile false \
                    -appname "Verademo" \
                    -version ${BUILD_NUMBER} \
                    -filepath /opt/veracode/verademo.war \
                    -scantimeout 60
                '''
            }
        }
    }
}

                }
            }
        }

        stage('Veracode Policy from Wrapper') {
            agent {
                docker { 
                    image 'veracode/api-wrapper-java' 
                }
            }
            steps {
                script {
                    def warFilePath = sh(script: 'find . -name "*.war"', returnStdout: true).trim()
                    if (warFilePath) {
                        echo "Found WAR file: $warFilePath"
                        stash name: 'warFile', includes: warFilePath
                    } else {
                        error "No WAR file found"
                    }
                }

                unstash 'warFile'
                sh '''
                cp /opt/veracode/app/target/*.war /opt/veracode/verademo.war
                java -jar /opt/veracode/api-wrapper.jar \
                    -vid '9c42b4bd8abf978e9dc1667451778c58' \
                    -vkey '568e7bd345890259c3e869c8da3d42597b815e6a780c5d72ce76eaf3045a1736d80eb0ced53eb61956b3f0410546b2ac6e681829eaa3c9c621fcd722cee60b14' \
                    -action UploadAndScan \
                    -createprofile false \
                    -appname "Verademo" \
                    -version ${BUILD_NUMBER} \
                    -filepath /opt/veracode/verademo.war \
                    -scantimeout 60
                '''
            }
        }
    }
}
