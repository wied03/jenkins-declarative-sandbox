pipeline {
    agent any

    environment {
        VCORES = '0.1'
        VERSION_TRACKER = 'the_artifact'
    }

    stages {
        stage('Build') {
            steps {
                script {
                    currentBuild.description = "1.0.${env.BUILD_NUMBER} for vCores ${env.vCores}"
                }
                sh 'uname'
                echo "Our build number is ${env.BUILD_NUMBER}"
                writeFile file: env.VERSION_TRACKER,
                          text: env.BUILD_NUMBER
                stash env.VERSION_TRACKER
            }
        }
        stage('Deploy to DEV') {
            input {
                message 'Is this OK?'
            }
            steps {
                unstash env.VERSION_TRACKER
                script {
                    env.theVersion = readFile(env.VERSION_TRACKER)
                    def isRestart = env.BUILD_NUMBER.toString() != env.theVersion.trim()
                    if (isRestart) {
                        currentBuild.description = "Restarted build for ${env.theVersion}"
                    }
                }
                sh 'cat the_file.txt'
                echo "Our build number is ${env.theVersion}"
                sh 'cat the_artifact'
                sh 'rm this_will_fail'
            }
        }
    }
    options {
        preserveStashes()
        buildDiscarder logRotator(numToKeepStr: '2')
    }

    post {
        always {
            cleanWs()
        }
    }
}
