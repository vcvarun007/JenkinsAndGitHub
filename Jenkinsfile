pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo "Building ..."
            }
            post{
                success{
                    mail to: "vcvarun007@gmail.com"
                    subject: "Build Status Email"
                    body: "Build was successful!"
                }
            }
        }     
    }
}
