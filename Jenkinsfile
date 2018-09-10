pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        addBadge(icon: 'Build', text: 'Build Stage')
      }
    }
    stage('Stage1') {
      parallel {
        stage('Stage1') {
          steps {
            echo 'Stage1 - Resize Primary'
          }
        }
        stage('Resize') {
          steps {
            build(job: 'canada-resize-primary', propagate: true, quietPeriod: 120, wait: true)
            sleep(time: 10, unit: 'MINUTES')
            build(job: 'canada-resize-backup', propagate: true, quietPeriod: 120, wait: true)
            sleep(unit: 'MINUTES', time: 5)
          }
        }
      }
    }
    stage('Stage2') {
      parallel {
        stage('Stage2') {
          steps {
            addBadge(icon: 'Replace Gw', text: 'Replace primary transit gateway')
          }
        }
        stage('Replace') {
          steps {
            build(job: 'canada-replace-primary', propagate: true, quietPeriod: 120, wait: true)
            sleep(unit: 'MINUTES', time: 5)
            build(job: 'canada-replace-backup', propagate: true, quietPeriod: 120, wait: true)
          }
        }
      }
    }
    stage('Report') {
      steps {
        mail(subject: 'canada resize-replace transit gateway', body: 'canada resize-replace transit gateway', to: 'edsel@aviatrix.com')
      }
    }
  }
}