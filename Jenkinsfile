library 'magic-butler-catalogue'
def PROJECT_NAME = 'jenkins-test'
def commentTrigger = currentBuild.rawBuild.getCause(org.jenkinsci.plugins.pipeline.github.trigger.IssueCommentCause)

pipeline {
    triggers {
        issueCommentTrigger('.*test this please.*')
    }
    agent any
    stages {
        stage('Test') {
            when { expression { !env.CHANGE_FORK || commentTrigger.comment.contains("test this please") } }
            environment {
                CREDS_FILE = credentials('pipeline-e2e-creds')
                LOGDNA_HOST = "logs.use.stage.logdna.net"
            }
            steps {
                script {
                    def creds = readJSON file: CREDS_FILE
                    // Assumes the pipeline-e2e-creds format remains the same. Chase
                    // refer to the e2e tests's README's authorization docs for the
                    // current structure
                    LOGDNA_INGESTION_KEY = creds["packet-stage"]["account"]["ingestionkey"]
                }
                sh """
                    make test
                    echo "This is not a fork!"
                """
            }
        }
    }
}
