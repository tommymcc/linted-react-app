pipeline {
  agent any

  stages {

    /*
    stage('Setup') {
      steps {

          sh 'docker network create jenkins_test || true'

          setupDatabase()
        }
      }
    }
    */

    stage('Lint') {
      when { not { branch 'master' } }

      environment {
        PRONTO_TOKEN = credentials('PRONTO_GITHUB_ACCESS_TOKEN')
      }

      steps {
        echo 'Linting...'

        script {

          docker.image('tommymccallig/codelint:0.0.1').inside(
            "-e PRONTO_GITHUB_ACCESS_TOKEN=${env.PRONTO_TOKEN} " +
            "-e PRONTO_PULL_REQUEST_ID=${env.CHANGE_ID} "
          ){
            sh 'pronto run -f github_status github_pr_review -c origin/master'
          }
        }
      }
    }

    /*
    stage('Build Image'){
      steps {
        echo 'Building docker image...'

        script {
          def app = docker.build('jenkins-docker-sample')

          // If the database is still initialising, we wait
          ensureDatabase()

          def testConfig =
            '--network jenkins_test ' +
            '-e "DATABASE_URL=mysql2://root:my-secret-pw@jenkinsmysql/testdb"'

          app.inside(testConfig) {
            sh 'rake db:setup'
            sh 'rake db:migrate'
            sh 'rspec --format progress --format RspecJunitFormatter --out tmp/rspec.xml'
          }
        }
      }

      post {
        always {
          junit 'tmp/rspec.xml'
        }
      }
    }
    */
  }
}
