#!/usr/bin/env groovy

import groovy.json.JsonOutput

def slackNotificationChannel = '#jenkins-notifications'  // ex: = "builds"


def notifySlack(text, channel, status) {
    def slackURL = 'https://hooks.slack.com/services/T....../B........./m.........................'
    def jenkinsIcon = 'https://wiki.jenkins-ci.org/download/attachments/2916393/logo.png'

    def payload = JsonOutput.toJson([
        channel: channel,
        username: "jenkins",
        icon_url: jenkinsIcon,
        attachments: [[
                         "title" : "${env.JOB_NAME}, build #${env.BUILD_NUMBER}",
                         "title_link": "${env.BUILD_URL}",
                         "text": "Jenkins Build for ${product} is " + status,
                         "color" : "#36a64f",
                         "footer" : "Devops",
                         "footer_icon": "https://platform.slack-edge.com/img/default_application_icon.png"
                         
                         ]]
    ])

    sh "curl -X POST --data-urlencode \'payload=${payload}\' ${slackURL}"
}


pipeline {
   
    agent { label 'kube-agent1' }
    tools { 
        maven 'maven3.5.0' 
        jdk 'jdk-10.0.2' 
          }
    
    stages {
        stage ('Build Started Slack Notificaion')
        {
            steps {
            script {
                  notifySlack("Success!", slackNotificationChannel, "Started")
            }
            }
        }
        stage ('SCM Checkout') {
            steps {
                git url: "${repo}", branch: "${branch}", credentialsId: 'f7fa1261-261c-4b61-9531-fce17361ec2d'
            }
        }
        
        stage ('Build and Test') {
            steps {
            script {
                    currentBuild.displayName = "$product-${env.BUILD_NUMBER}"
                  sh "mvn -U clean install docker:build -DpushImage | tee $product"
                }
            }
        }
        
        stage ('Kube Deployment ') {
            steps {
         
                 sh '''
                  cat $product | grep -i qaregistry.yatra.com | grep Building | cut -d/ -f2>image
                  imagename=`cat image`
                  
                ''' 
                
            }
        }                         
      
    }
    post {
        always {
          
          script { 
                 
              deleteDir()
              
  notifySlack("Success!", slackNotificationChannel, "Executed") 
                }
        }
        
            
      }
}
