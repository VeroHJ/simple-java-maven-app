pipeline {
    agent {
      label 'slave-node'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean install'
                sh 'mvn test'
                sh 'mvn sonar:sonar -Dsonar.projectKey=java_app -Dsonar.host.url=http://192.168.33.37:9000 -Dsonar.login=88116bd4aaa7c13e911c90c37596e6c3f8c05e85'
            }
        }
        stage('Pack') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Deploy') {
            steps {
                sshPublisher(publishers: [
                    sshPublisherDesc(
                        configName: '192.168.33.33',
                        transfers: [
                            sshTransfer(
                                cleanRemote: false,
                                excludes: '',
                                execCommand: 'java -jar /vagrant/declarative/my-app-1.0-SNAPSHOT.jar',
                                execTimeout: 120000,
                                flatten: false,
                                makeEmptyDirs: false,
                                noDefaultExcludes: false,
                                patternSeparator: '[, ]+',
                                remoteDirectory: '/vagrant/declarative',
                                remoteDirectorySDF: false,
                                removePrefix: 'target',
                                sourceFiles: 'target/*.jar'
                            )
                        ],
                        usePromotionTimestamp: false,
                        useWorkspaceInPromotion: false,
                        verbose: true
                    )
                ])
            }
        }
    }
}