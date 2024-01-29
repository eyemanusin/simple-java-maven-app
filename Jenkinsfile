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
        stage('Deliver') {

            environment {
                GITHUB = credentials('bb398646-df06-4260-9372-16161e2e14ea')
            }
            steps {
                script {
                    // Get the latest version number from a file (or any other source)
                    def currentVersion = readFile('version.txt').trim()

                    // Create the new version number
                    def newVersion = "0.0.2"

                    // Print the new version
                    echo "New version: ${newVersion}"

                    // Write the new version back to the file
		    echo newVersion >> version.txt

                    // Create the changelog by running Git log
                    def changelog = sh(script: "git log --no-merges --pretty=format:'%h %s' ${currentVersion}..HEAD", returnStdout: true).trim()

                    // Write the changelog to a changelog.txt file
		    echo changelog >> changelog.txt

                    // Commit and push the updated version and changelog
                    sh "git add version.txt changelog.txt"
                    sh "git commit -m 'Bump version to ${newVersion}'"
                    sh "git push origin HEAD:release/${newVersion}"
                }
            }
            
        }
    }
}
