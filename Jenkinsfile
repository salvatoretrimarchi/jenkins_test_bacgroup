
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'

                parallel (
     "Odoo Argentina": {
         dir('odoo-account'){
             git branch: 't10.0', depth: '1', url: 'http://bitbucket.org/bacgroup/odoo-account.git'
         }
      },
   )
                
                
                
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
