pipeline {
    agent any

    environment {
        AUTOMAGICALLY_TOKEN = credentials('AUTOMAGICALLY_TOKEN')
    }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
        stage("Using curl example") {
            steps {
                script {
                    final String url = "https://preview.octomind.dev/api/v2/execute"
                    final String header = "Content-Type: application/json"
                    // def matches = ("git@github.com:OctoMind-dev/jenkins-test.git" =~ /((git@|https:\/\/)([\w\.@]+)(\/|:))([\w,\-,\_]+)\/([\w,\-,\_]+)(.git){0,1}((\/){0,1})/)
                    final String owner = "test" // matches[0][4]
                    final String repo = "test" //matches[0][5]
                    // final String data = '''{
                    // "url": "https://preview.octomind.dev/testresults/c09d0c97-20f6-452a-aadd-086f627716f8", 
                    // "token": "d791a2d0603f7863951575ca05e1ffcfddaaf17f4d2b5d0998212ea093ae78d13b836d8ece1172719ed73199d89a5ba2",
                    // "testTargetId": "2eed7f27-dfef-4062-8594-1b8f49ca0d26",
                    // "context": { 
                    //     "source": "github",
                    //     "repo": "${repo}",
                    //     "owner": ${owner}",
                    //     "sha": "${env.GIT_COMMIT}",
                    //     "ref": "${env.GIT_BRANCH}"
                    //     }
                    // }'''
                    final String data = """{
                    "url": "https://preview.octomind.dev/testresults/c09d0c97-20f6-452a-aadd-086f627716f8", 
                    "token": "$AUTOMAGICALLY_TOKEN",
                    "testTargetId": "2eed7f27-dfef-4062-8594-1b8f49ca0d26",
                    "context": { 
                        "source": "github",
                        "repo": "automagically",
                        "owner": "OctoMind-dev",
                        "sha": "${env.GIT_COMMIT}",
                        "ref": "${env.GIT_BRANCH}"
                        }
                    }"""


                    final String response = sh(script: "curl $url --header '$header' --data '$data'", returnStdout: true)

                    echo response
                }
            }
        }
    }
}
