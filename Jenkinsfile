pipeline {
    agent any

    stages {
        stage('First stage') {
            steps {
                script {
                    currentBuild.description = "1.0.${env.BUILD_NUMBER}"
                }
                sh 'rm -rf the_artifact'
                sh 'uname'
                echo "Our build number is ${env.BUILD_NUMBER}"
                sh "echo ${env.BUILD_NUMBER} >> the_artifact"
                stash 'the_artifact'
            }
        }
        stage('Second stage') {
            steps {
                sh 'cat the_file.txt'
                echo "Our build number is ${env.BUILD_NUMBER}"
                unstash 'the_artifact'
                sh 'cat the_artifact'
                sh 'rm this_will_fail'
            }
        }
    }
    options {
        preserveStashes()
        buildDiscarder logRotator(numToKeepStr: '2')
    }
}
