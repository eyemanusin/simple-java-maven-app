pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
        stage('Release') {
            when {
                branch 'release/*'
            }
            environment {
                GITHUB = credentials('JENKINS_GITHUB_TOKEN')
            }
            steps {
                script {
                    sh '''
                    ./gradlew githubRelease --stacktrace --console plain \
                      -PgitHubToken=${GITHUB} \
                      -PgitHubCommit=${GIT_COMMIT}
                    '''
                }
            }
            post {
                always {
                    script {
                        ontrackCliValidate(stamp: 'GITHUB.RELEASE')
                    }
                }
            }
        }
    }
}
