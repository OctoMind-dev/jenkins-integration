pipeline {
    agent any
    environment {
        AUTOMAGICALLY_TOKEN = credentials('AUTOMAGICALLY_TOKEN')
    }
    stages {
        stage("Execute Automagically") {
            steps {
                script {
                    final String baseUrl = "https://app.octomind.dev"
                    final String url = "${baseUrl}/api/v2/execute"
                    final String header = "Content-Type: application/json"
                    String[] split_url = "${env.GIT_URL}".split('/')
                    int number_of_elements = split_url.size()
                    final String repo = split_url[number_of_elements - 1]
                    final String owner = split_url[number_of_elements - 2]

                    // publicly accessible url to your deployment
                    final String testTargetUrl = "https://storage.googleapis.com/mocktopus/index.html"
                    
                    // your testTargetId that you also get from us
                    final String testTargetId = "35c8bfca-48d2-4eb2-8042-4ee50707a295"

                    final String data = """{
                        "url": "$testTargetUrl",
                        "token": "${AUTOMAGICALLY_TOKEN}",
                        "testTargetId": "$testTargetId",
                        "context": { 
                            "source": "github",
                            "repo": "$repo",
                            "owner": "$owner",
                            "sha": "${env.GIT_COMMIT}",
                            "ref": "${env.GIT_BRANCH}"
                            }
                    }"""
                    final def (String response, String code) = sh(script: "curl -s -w '\\n%{response_code}' $url --header '$header' --header 'x-api-key: $AUTOMAGICALLY_TOKEN' --data '$data'", returnStdout: true).trim().tokenize("\n")
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
