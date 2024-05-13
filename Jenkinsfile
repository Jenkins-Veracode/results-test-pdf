pipeline {
    agent any

    environment {
        VERACODE_API_ID = '9c42b4bd8abf978e9dc1667451778c58'
        VERACODE_API_SECRET = '568e7bd345890259c3e869c8da3d42597b815e6a780c5d72ce76eaf3045a1736d80eb0ced53eb61956b3f0410546b2ac6e681829eaa3c9c621fcd722cee60b14'
        CI_TIMEOUT = '20'
        JOB_NAME = 'Jenkins_pipeline'
        filepath = 'app/target/verademo.war'
        VeracodeProfile = 'VeraDemo'
    }

    stages {
        stage('Veracode Policy from Wrapper') {
            agent {
                docker { 
                    image 'veracode/api-wrapper-java' 
                }
            }
            steps {
                sh '''java -jar /opt/veracode/api-wrapper.jar 
                        -vid ${VERACODE_API_ID}
                        -vkey ${VERACODE_API_SECRET} 
                        -action UploadAndScan
                        -createprofile false
                        -appname "Verademo"
                        -version ${BUILD_NUMBER}
                        -filepath 'app/target/verademo.war'
                        -scantimeout 60'''

                sh '''java -jar /opt/veracode/api-wrapper.jar 
                    -vid ${VERACODE_API_ID}
                    -vkey ${VERACODE_API_SECRET} 
                    -action getapplist'''

                sh '''java -jar /opt/veracode/api-wrapper.jar 
                    -vid ${VERACODE_API_ID}
                    -vkey ${VERACODE_API_SECRET} 
                    -action getbuildlist
                    -appid <the_app_id>'''

                sh '''java -jar /opt/veracode/api-wrapper.jar 
                    -vid ${VERACODE_API_ID}
                    -vkey ${VERACODE_API_SECRET} 
                    -action detailedreport
                    -buildid <build_id>
                    -format pdf'''
            }
        }
    }
}
