pipeline {
    agent any

    environment {
        VERACODE_API_ID = credentials('veracode-credentials-id') ?: ''
        VERACODE_API_SECRET = credentials('veracode-credentials-secret') ?: ''
        CI_TIMEOUT = '20'
        JOB_NAME = 'Jenkins_pipeline'
        filepath = 'app/target/verademo.war'
        VeracodeProfile = 'VeraDemo'
    }

    stages {
        stage('Veracode Policy from Wrapper') {
            steps {
                script {
                    docker.image('veracode/api-wrapper-java').inside {
                        withCredentials([usernamePassword(credentialsId: 'veracode-credentials-id', passwordVariable: 'VERACODE_API_KEY', usernameVariable: 'VERACODE_API_ID')]) {
                            sh """
                                java -jar /opt/veracode/api-wrapper.jar 
                                    -vid ${VERACODE_API_ID}
                                    -vkey ${VERACODE_API_KEY} 
                                    -action UploadAndScan
                                    -createprofile false
                                    -appname "Verademo"
                                    -version \${BUILD_NUMBER}
                                    -filepath 'app/target/verademo.war'
                                    -scantimeout 60
                            """
                            sh """
                                java -jar /opt/veracode/api-wrapper.jar 
                                    -vid ${VERACODE_API_ID}
                                    -vkey ${VERACODE_API_KEY} 
                                    -action getapplist
                            """
                            sh """
                                java -jar /opt/veracode/api-wrapper.jar 
                                    -vid ${VERACODE_API_ID}
                                    -vkey ${VERACODE_API_KEY} 
                                    -action getbuildlist
                                    -appid <the_app_id>
                            """
                            sh """
                                java -jar /opt/veracode/api-wrapper.jar 
                                    -vid ${VERACODE_API_ID}
                                    -vkey ${VERACODE_API_KEY} 
                                    -action detailedreport
                                    -buildid <build_id>
                                    -format pdf
                            """
                        }
                    }
                }
            }
        }
    }
}
