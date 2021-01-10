pipeline {
     agent any
     triggers {
          pollSCM('* * * * *')
     }
     stages {
          stage("Compile") {
               steps {
                    sh "./gradlew compileJava"
               }
          }
          stage("Unit test") {
               steps {
                    sh "./gradlew test"
               }
          }
          stage("Code coverage") {
               steps {
                    sh "./gradlew jacocoTestReport"
                   /* sh "./gradlew jacocoTestCoverageVerification"*/
               }
          }
          stage("Static code analysis") {
               steps {
                    sh "./gradlew checkstyleMain"
               }
          }
          stage("Package") {
               steps {
                    sh "./gradlew build"
               }
          }

          stage("Docker build") {
               steps {
                    sh "docker build -t devops81/calculator:${BUILD_NUMBER} ."
               }
          }

          stage("Docker login") {
               steps {
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub',
                               usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                         sh "docker login --username $USERNAME --password $PASSWORD"
                    }
               }
          }

          stage("Docker push") {
               steps {
                    sh "docker push devops81/calculator:${BUILD_NUMBER}"
               }
          }

          stage("Update version") {
               steps {
                    sh "sed  -i 's/{{VERSION}}/${BUILD_NUMBER}/g' calculator.yaml"
               }
          }
          
         /*  stage("Deploy to staging") {
               steps {
                    sh "kubectl config use-context staging"
                    sh "kubectl apply -f hazelcast.yaml"
                    sh "kubectl apply -f calculator.yaml"
               }
          }*/

          /*stage("Acceptance test") {
               steps {
                    sleep 60
                    sh "chmod +x acceptance-test.sh && ./acceptance-test.sh"
               }
          }*/

          /* stage("Release") {
               steps {
                    sh "kubectl config use-context production"
                    sh "kubectl apply -f hazelcast.yaml"
                    sh "kubectl apply -f calculator.yaml"
               }
          }*/
          /* stage("Smoke test") {
              steps {
                  sleep 60
                  sh "chmod +x smoke-test.sh && ./smoke-test.sh"
              }
          }*/
          stage("Publish HTML Report")
          {
               steps
               {
               publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, includes: '*/*.html,**/*.css', keepAll: false, reportDir: 'build/reports/tests/test', reportFiles: 'index.html', reportName: 'HTML Report', reportTitles: ''])
               }
          }
          
     }   
     post {
          always {
            echo 'One way or another, I have finished'
            
            mail bcc: '', body: "<b>Example</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "devops81@gmail.com";   
        }
        success {
           mail bcc: '', body: "<b>Example</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "devops81@gmail.com";   
           
        }
        unstable {
            mail bcc: '', body: "<b>Example</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "devops81@gmail.com";   
        }
        failure {
             mail bcc: '', body: "<b>Example</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "devops81@gmail.com";   
        }
        changed {
            mail bcc: '', body: "<b>Example</b><br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "devops81@gmail.com";   
        }
          }
}
