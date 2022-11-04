pipeline {
    agent any

    stages {
        stage('Preparation') {
            steps {
                checkout scm
            }
        }
        stage('Assemble') {
            steps {
                sh './gradlew clean assemble'
            }
            post {
                always {
                    archiveArtifacts '**/build/libs/*.jar'
                }
            }
        }
        stage('Unit tests') {
            steps {
                sh './gradlew test'
            }
            post {
                always {
                    junit '**/build/test-results/test/*.xml'
                }
            }
        }
        stage('Static code analyses') {
            steps {
                sh './gradlew build'
            }
            post {
                always {
                    dry canComputeNew: false, defaultEncoding: '', healthy: '', pattern: 'build/reports/cpd/*.xml', unHealthy: ''
                    findbugs defaultEncoding: '', excludePattern: '**/*Test.java', healthy: '', includePattern: '', pattern: '**/build/reports/spotbugs/*.xml', unHealthy: '', unstableNewAll: '0'
                    jacoco classPattern: '**/build/classes/*/main/', execPattern: '**/build/jacoco/*.exec', sourcePattern: '**/src/main/java'
                    warnings canComputeNew: false, canResolveRelativePaths: false, categoriesPattern: '', consoleParsers: [[parserName: 'Java Compiler (Eclipse)'], [parserName: 'Java Compiler (javac)'], [parserName: 'JavaDoc Tool']], defaultEncoding: 'UTF-8', excludePattern: '', healthy: '', includePattern: '', messagesPattern: '', unHealthy: ''
                }
            }
        }
    }
}

