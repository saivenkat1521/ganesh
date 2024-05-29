pipeline {
    agent any
    
    stages {
        stage('Delete Old Branches') {
            steps {
                script {
                    def gitRepo = checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '0a6011eb-9b16-4f3c-9f11-d32b82460219', url: 'https://github.com/venkatseetha/ganesh.git']]])

                    // Get current timestamp
                    def currentTime = new Date().getTime()

                    gitRepo.git('fetch --prune')

                    gitRepo.git('branch -r --format="%(refname:short) %(committerdate:raw)"').eachLine { branch ->
                        def parts = branch.split(" ")
                        def branchName = parts[0]
                        def commitTimestamp = parts[1].toLong() * 1000

                        // Calculate difference in days
                        def daysDiff = (currentTime - commitTimestamp) / (1000 * 60 * 60 * 24)

                        if (daysDiff > 30 && !branchName.contains('/main')) {
                            echo "Deleting branch: ${branchName}"
                            gitRepo.git("push origin --delete ${branchName}")
                        }
                    }
                }
            }
        }
    }
}
