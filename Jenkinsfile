//Jenkinsfile 
// Fran Leon Eejercicios Modulo 2 - Tema 4 
// Experto DevOps & cloud UNIR 
// 12-22

pipeline {
    agent any

    stages {
        stage('Get code') {
            steps {
                // Traer el codigo del repo
                git 'https://github.com/franlov/helloworld.git'
            }
       }
       
        stage('Build') {
            steps {
                // No hace nada
                echo 'Muestra un mensaje'
                echo WORKSPACE
                sh 'ls'
            }
       }
       
    stage ('Test') 
   {
        parallel
        {
            stage('Unit') {
              steps {
               catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
               sh '''
               export PYTHONPATH=.
               pytest --junitxml=result-unit.xml test//unit
               '''
            }
       }
    }
    
            stage('Rest'){
              steps {
               catchError(buildResult: 'UNSTABLE', stageResult: 'FAILURE') {
               sh '''
               export FLASK_APP=app//api.py
               python -m flask run &
               wget https://repo1.maven.org/maven2/com/github/tomakehurst/wiremock-jre8-standalone/2.35.0/wiremock-jre8-standalone-2.35.0.jar
               java -jar wiremock-jre8-standalone-2.35.0.jar --port 9090 --root-dir test//wiremock &
               sleep 5
               pytest --junitxml=result-unit.xml test//rest
               rm -rf wiremock-jre8-standalone-2.35.0.jar*
               '''
          }
       }
    }
  }
}    

   stage('Results') {
            steps {
               junit 'result-unit.xml'
          }
}
}
}
