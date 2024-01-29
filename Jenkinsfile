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
            when {
                branch 'release/*'
            }
            environment {
                GITHUB = credentials('JENKINS_GITHUB_TOKEN')
            }
            steps {
                script {
                    // Get the latest version number from a file (or any other source)
                    def currentVersion = readFile('version.txt').trim()

                    // Split the version number into major, minor, and patch parts
                    def (major, minor, patch) = currentVersion.tokenize('.')

                    // Increment the minor version by 1
                    def newMinor = minor.toInteger() + 1

                    // Create the new version number
                    def newVersion = "${major}.${newMinor}.${patch}"

                    // Print the new version
                    echo "New version: ${newVersion}"

                    // Write the new version back to the file
                    writeFile(file: 'version.txt', text: newVersion)

                    // Create the changelog by running Git log
                    def changelog = sh(script: "git log --no-merges --pretty=format:'%h %s' ${currentVersion}..HEAD", returnStdout: true).trim()

                    // Write the changelog to a changelog.txt file
                    writeFile(file: 'changelog.txt', text: changelog)

                    // Commit and push the updated version and changelog
                    sh "git add version.txt changelog.txt"
                    sh "git commit -m 'Bump version to ${newVersion}'"
                    sh "git push origin HEAD:release/${newVersion}"
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
