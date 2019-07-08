node ('ansible'){
    deleteDir()
    stage'Build'
        checkout poll: false, scm: [$class: 'GitSCM', branches: [[name: '*/master']], 
        doGenerateSubmoduleConfigurations: false, extensions: [], 
        submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', 
        url: 'https://github.com/jembim/spring-framework-petclinic.git']]]

        sh "mvn clean package"

    stage'Unit Test'
        sh "mvn test"

    stage'Code Quality'
        def scannerHome = tool 'SonarScanner';
        sh "${scannerHome}/bin/sonar-scanner"

    stage'Publish'
        sh "mv /workspace/pipeline/target/petclinic.war /workspace/pipeline/target/petclinic-${env.BUILD_ID}.war"
        sh "curl -v -u admin:admin --upload-file /workspace/pipeline/target/petclinic-${env.BUILD_ID}.war http://wdcdmzyz22033182.ibmcloud.dst.ibm.com/nexus/content/repositories/PETCLINIC/petclinic-${env.BUILD_ID}.war"

    stage'Deploy'
        sh "echo 'Deploy'"
}