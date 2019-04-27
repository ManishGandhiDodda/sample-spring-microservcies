def manish = 'Desktop'
pipeline{
    tools{
        maven 'M3'
    }
    environment{
        sonarHome = '/usr/local/Cellar/sonar-scanner/3.3.0.1492'
        dockerHome = tool name: 'docker', type: 'org.jenkinsci.plugins.docker.commons.tools.DockerTool'
    }
    agent any
    parameters{
        choice choices: ['Windows', 'Linux'], description: 'Please choose the environment on which this job need to be executed.', name: 'Environment'
        choice choices: ['account-service', 'customer-service', 'discovery-service', 'gateway-service', 'zipkin-service', 'All microservices'], description: 'Select the service which needs to undergo a CICD.', name: 'Service'
    }
    stages{
        stage('Code Checkout')
        {
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '0becc3d3-39fd-4d03-9209-eec1722f0586', url: 'git@github.com:ManishGandhiDodda/sample-spring-microservcies.git']]])
            }
        }
        stage('Building Microservices')
        {
            when{
                expression{ params.Environmen == 'Linux'
              params.Service == 'account-service' || 'All microservices'}
            }
            steps{
                withMaven(maven: 'M3', tempBinDir: '') {
                    sh 'mvn clean install package'
                }
            }
        }
        stage ('Creating Custom Image of customer-service')
        {
            steps{
                script{
                    echo "Docker image of customer-service is created"
                    echo "${manish}"
                //    sh '''sudo docker build -t customer-service:latest ./customer-service'''
                // /   def customer_service_image = docker.build( "customer-service:latest", "./customer-service" )
                  }
            }
        }
        stage('Code Analysis')
        {
            steps{
               withSonarQubeEnv('SonarQube Server') {
            sh "${sonarHome}/bin/sonar-scanner" }             
            }
        }
        stage('Code Deploy')
        {
            steps{
                echo "Code is deployed into Tomcat"
            //    sh 'cp '
            }
        }
    }
    post{
        always{
            echo "Reports will be generated inf junit test cases are present in code"
         //   junit 'junit_reports/*.xml'
        }

    }
}
