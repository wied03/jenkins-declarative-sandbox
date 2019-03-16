pipeline {
    agent any
    stages {
        stage('First stage') {
            steps {
                script {
                    weAreBuilding = "1.0.${env.BUILD_NUMBER}"
                    currentBuild.description = weAreBuilding
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
                script {
                    currentBuild.description = weAreBuilding
                }
                echo "Our build number is ${env.BUILD_NUMBER}"
                unstash 'the_artifact'
                sh 'cat the_artifact'
                sh 'rm this_will_fail'
            }
        }
    }
    options {
        preserveStashes()
    }
}
