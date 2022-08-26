pipeline {
    
    agent any
    stages {
        stage('Test') {
            echo "Clean test starting"
            steps {
                sh 'mvn clean test'
            }
        }
        stage('Build') {
            echo "Build starting"
            steps {
                sh '''
                mvn clean install
                mkdir -p /home/jenkins/project-wars
                mv ./target/*.war /home/jenkins/project-wars/project-${BUILD_NUMBER}.war
                '''
            }
        }
        stage('Deploy') {
            echo "Build starting"
            steps {
                sh '''

                build_num=${BUILD_NUMBER}
                echo '[Unit]
Description=My SpringBoot App
[Service]

User=jenkins
Type=simple

ExecStart=/usr/bin/java -jar /home/jenkins/project-wars/project-'$build_num'.war

[Install]
WantedBy=multi-user.target' > /home/jenkins/MyApp.service
                sudo mv /home/jenkins/MyApp.service /etc/systemd/system/MyApp.service

                sudo systemctl daemon-reload
                sudo systemctl restart MyApp

                '''
            }
        }
    }
}