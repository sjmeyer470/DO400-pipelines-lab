pipeline {

    agent any

    parameters {
        booleanParam(name: "RUN_IuNTEGRATION_TESTS", defaultValue: true)
    }

    stages {
        stage('Test'){
            parallel {
                stage('Unit tests'){
                    steps {
                        sh './mvnw test -D testGroups=unit'
                    }
                }

                stage('Build'){
                    steps {
                        script{
                            try{
                                sh './mvnw package -D skipTests'
                            }catch(ex) {
                                echo "Error while generating JAR file"
                                throw ex
                            }
                        }
                    }
                }

                stage('Integration tests'){
                    when {
                        expression { return params.RUN_INTEGRATION_TESTS }
                    }

                    steps {
                        sh './mvnw test -D testGroups=integration'
                    }
                }
            }
        }
    }
}