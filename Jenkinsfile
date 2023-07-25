pipeline {
    agent any
    environment {
        AUTOMAGICALLY_TOKEN = credentials('AUTOMAGICALLY_TOKEN')
    }
    

    stages {
        stage("Execute Automagically") {
            steps {
                script {
                    final String baseUrl = "https://preview.octomind.dev"
                    final String url = "${baseUrl}/api/v2/execute"
                    final String header = "Content-Type: application/json"
                    def owner_repo_matches = "https://github.com/OctoMind-dev/jenkins-integration" =~'.*(/|:)(.*)/([^\\.]*)(\\.git)?$'
                    echo owner_repo_matches

                    
                    
    
                    final String data = """{
                        "url": "https://preview.octomind.dev/testresults/c09d0c97-20f6-452a-aadd-086f627716f8", 
                        "token": "${AUTOMAGICALLY_TOKEN}",
                        "testTargetId": "2eed7f27-dfef-4062-8594-1b8f49ca0d26",
                        "context": { 
                            "source": "github",
                            "repo": "automagically",
                            "owner": "OctoMind-dev",
                            "sha": "${env.GIT_COMMIT}",
                            "ref": "${env.GIT_BRANCH}"
                            }
                    }"""

                    final def (String response, String code) = sh(script: "curl -s -w '\\n%{response_code}' $url --header '$header' --data '$data'", returnStdout: true).trim().tokenize("\n")
                    if (code == '202') {
                        def matches = response =~/"id":"(.+?)"/
                        final String testReportId = matches[0][1]
                        final String testReportUrl = "${baseUrl}/testreports/${testReportId}"
    
                        currentBuild.description = """<a href="${testReportUrl}">Link to Test Report</a>"""
                        echo "You can view your Test Report here: ${testReportUrl}"
                    } else {
                        currentBuild.description = "Execution unsuccessful. Got status ${code}"
                        echo "Execution unsuccessful. Got status ${code}"
                    }
                }
            }
        }
    }
}
