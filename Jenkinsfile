pipeline {
  agent {
    node {
      label 'master'
    }

  }
  stages {
    stage('Input Change No') {
      steps {
        sh "echo ${params.Change} : ${params.Spoc} > /home/agd_user/.web/apache/htdocs/AIRTEL/AES_TUX_MAIL/tuxmaildetails.txt"
      }
    }

    stage('Head\'s Up Mail') {
      steps {
        sh '/home/agd_user/.web/apache/htdocs/AIRTEL/AES_TUX_MAIL/tux_bounce_mail.sh'
      }
    }

    stage('Proceed to send mail to get Tux HA freeze') {
      steps {
        input 'Proceed to send mail to get Tux HA Unfreeze'
      }
    }

    stage('Mail to freeze AES Tux HA') {
      steps {
        sh '/home/agd_user/.web/apache/htdocs/AIRTEL/AES_TUX_MAIL/tux_bounce_mail1.sh'
      }
    }

    stage('Proceed to Tux bounce') {
      steps {
        input 'Proceed to Tux bounce'
      }
    }

    stage('WAS-Weblogic Stop') {
      parallel {
        stage('iB2B Stop') {
          steps {
            sh '''ssh -q wasadmin@10.5.131.62 \'/home/wasadmin/SCRIPTS/jenkins/stopAppCluster.sh\'
ssh -q wasadmin@10.5.131.64 \'/home/wasadmin/SCRIPTS/jenkins/stopRPTCluster.sh\'
ssh -q wasadmin@10.5.131.64 \'/home/wasadmin/SCRIPTS/jenkins/stopSCHCluster.sh\''''
          }
        }

        stage('AES NCore stop') {
          steps {
            sh 'ssh -q weblogic@10.14.7.138 \'/home/weblogic/SCRIPTS/stop_bounce.sh\''
          }
        }

        stage('AES OFS Stop') {
          steps {
            sh 'ssh -q wasadmin@10.14.5.179 \'/home/wasadmin/SCRIPTS/stop_cluster.sh\''
          }
        }

      }
    }

    stage('Stop Tuxedo') {
      steps {
        sh 'ssh -q arbor@10.14.7.154 \'/arborhome/arbor/FXSecServer-1.3.0.5/server/tux_stop\''
      }
    }

    stage('AES TUX/APP Status_Stop') {
      steps {
        sh '''/home/agd_user/JENKINS_SCRIPTS/health_Check/aes_health_monitor.ksh
cat /home/agd_user/JENKINS_SCRIPTS/health_Check/AES_Backend_Running_File.txt'''
      }
    }

    stage('Proceed action Tux Start') {
      steps {
        input 'Proceed to Start Tux and App'
      }
    }

    stage('Start Tuxedo') {
      steps {
        sh 'ssh -q arbor@10.14.7.154 \'/arborhome/arbor/FXSecServer-1.3.0.5/server/tux_start\''
      }
    }

    stage('Tux Bounce Mid mail') {
      steps {
        sh '/home/agd_user/.web/apache/htdocs/AIRTEL/AES_TUX_MAIL/tux_bounce_mail2.sh'
      }
    }

    stage('WAS-Weblogic Start') {
      parallel {
        stage('iB2B start') {
          steps {
            sh '''ssh -q wasadmin@10.5.131.62 \'/home/wasadmin/SCRIPTS/jenkins/startAppCluster.sh\'
ssh -q wasadmin@10.5.131.64 \'/home/wasadmin/SCRIPTS/jenkins/startRPTCluster.sh\'
ssh -q wasadmin@10.5.131.64 \'/home/wasadmin/SCRIPTS/jenkins/startSCHCluster.sh\''''
          }
        }

        stage('AES NCore start') {
          steps {
            sh 'ssh -q weblogic@10.14.7.138 \'/home/weblogic/SCRIPTS/start_bounce.sh\''
          }
        }

        stage('AES OFS start') {
          steps {
            sh 'ssh -q wasadmin@10.14.5.179 \'/home/wasadmin/SCRIPTS/start_cluster.sh\''
          }
        }

      }
    }

    stage('AES Tux/App Status_Start') {
      steps {
        sh '''/home/agd_user/JENKINS_SCRIPTS/health_Check/aes_health_monitor.ksh
cat /home/agd_user/JENKINS_SCRIPTS/health_Check/AES_Backend_Running_File.txt'''
      }
    }

    stage('Proceed to send closure mail') {
      steps {
        input 'Proceed to send closure mail'
      }
    }

    stage('Closure mail') {
      steps {
        sh '/home/agd_user/.web/apache/htdocs/AIRTEL/AES_TUX_MAIL/tux_bounce_mail3.sh'
      }
    }

  }
  parameters {
    string(name: 'Change', description: 'Enter Change number')
    string(name: 'Spoc', description: 'Enter SPOC name')
  }
}