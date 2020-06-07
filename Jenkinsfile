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
        echo 'Head\'s Up mail'
      }
    }

    stage('Proceed to send mail to get Tux HA freeze') {
      steps {
        input 'Proceed to send mail to get Tux HA Unfreeze'
      }
    }

    stage('Mail to freeze AES Tux HA') {
      steps {
        echo 'Mail sent'
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
            echo 'Stop iB2B'
          }
        }

        stage('AES NCore stop') {
          steps {
            echo 'AES NCORE stop'
          }
        }

        stage('AES OFS Stop') {
          steps {
            echo 'AES OFS Stop'
          }
        }

      }
    }

    stage('Stop Tuxedo') {
      steps {
        echo 'Stop Tuxedo'
      }
    }

    stage('AES TUX/APP Status_Stop') {
      steps {
        echo 'AES TUX/APP Status_Stop'
      }
    }

    stage('Proceed action Tux Start') {
      steps {
        input 'Proceed to Start Tux and App'
      }
    }

    stage('Start Tuxedo') {
      steps {
        echo 'Stop Tuxedo'
      }
    }

    stage('Tux Bounce Mid mail') {
      steps {
        echo 'Tux bounce Mid mail'
      }
    }

    stage('WAS-Weblogic Start') {
      parallel {
        stage('iB2B start') {
          steps {
            echo 'iB2B start'
          }
        }

        stage('AES NCore start') {
          steps {
            echo 'AES NCore stop'
          }
        }

        stage('AES OFS start') {
          steps {
            echo 'AES OFS start'
          }
        }

      }
    }

    stage('AES Tux/App Status_Start') {
      steps {
        echo 'AES TUX/APP Status_Start'
      }
    }

    stage('Proceed to send closure mail') {
      steps {
        echo 'Proceed to send final mail'
      }
    }

    stage('Closure mail') {
      steps {
        echo 'Closure mail'
      }
    }

  }
  parameters {
    string(name: 'Change', description: 'Enter Change number')
    string(name: 'Spoc', description: 'Enter SPOC name')
  }
}
