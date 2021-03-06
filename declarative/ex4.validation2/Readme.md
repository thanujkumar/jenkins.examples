
```javascript
pipeline {
    agent {
        label 'master'
    }
    
    tools {
      maven 'maven-3.5.4'
      jdk 'jdk1.8.0_181'
    }
    
    stages{
        stage('Git Checkout') {
            steps {
                git 'https://github.com/thanujkumar/bean-validation2.git'
            }
        }
        stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }
        stage ('Test and Jacoco Report') {
            steps{
                sh 'mvn jacoco:prepare-agent test jacoco:report'
            }
        }
     }
   
    post {
        success {
           junit '**/*/target/surefire-reports/**/*.xml'
           //https://jenkins.io/blog/2018/08/17/code-coverage-api-plugin-1/
           publishCoverage adapters: [jacocoAdapter('**/*/target/site/jacoco/jacoco.xml')],sourceFileResolver: sourceFiles('STORE_LAST_BUILD')

       }
    }

}
```