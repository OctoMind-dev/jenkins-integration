pipeline {
    agent any
    environment {
        AUTOMAGICALLY_TOKEN = credentials('AUTOMAGICALLY_TOKEN')
    }
    stages {
        stage("Execute Automagically") {
            steps {
                script {
                    final String baseUrl = "https://preview.octomind.dev" // this should be updated with production url and production test target id, but when?
                    final String url = "${baseUrl}/api/v2/execute"
                    String[] split_url = "${env.GIT_URL}".split('/')
                    int number_of_elements = split_url.size()
                    final String repo = split_url[number_of_elements - 1]
                    final String owner = split_url[number_of_elements - 2]

                    // publicly accessible url to your deployment
                    final String testTargetUrl = "https://preview.octomind.dev/testresults/c09d0c97-20f6-452a-aadd-086f627716f8"
                    
                    // your testTargetId that you also get from us
                    final String testTargetId = "2eed7f27-dfef-4062-8594-1b8f49ca0d26"

                    final String header = """{
                        "Content-Type": "application/json", 
                        "x-api-key" : "${AUTOMAGICALLY_TOKEN}"
                    }"""

                    final String data = """{
                        "url": "$testTargetUrl",
                        "testTargetId": "$testTargetId",
                        "context": { 
                            "source": "github",
                            "repo": "$repo",
                            "owner": "$owner",
                            "sha": "${env.GIT_COMMIT}",
                            "ref": "${env.GIT_BRANCH}"
                            }
                    }"""
                    final def (String response, String code) = sh(script: "curl -s -w '\\n%{response_code}' $url --header '$header' --data '$data'", returnStdout: true).trim().tokenize("\n")
                    echo response
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
