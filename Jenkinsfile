import groovy.json.JsonSlurper
pipeline {
    agent any
    tools {    
        maven "(Default)"
        jdk '1.8.0_144_x64' 
    }

    stages {
        stage('Build from branch') {
            when {
                environment name: 'CHANGE_ID', value: null
            }

            steps {
                timestamps {
                    withMaven(maven: '(Default)', mavenLocalRepo: '.repository', mavenOpts: '-Xmx6g -Xms2g -XX:-UseGCOverheadLimit') {
                        bat "mvn -Dsonar.branch.name=$BRANCH_NAME clean verify"
                    }
                }
            }
        }

        stage('Build from pull-request') {
            when {
                not { environment name: 'CHANGE_ID', value: null }
            }

            steps {
                timestamps {
                    withMaven(maven: '(Default)', mavenLocalRepo: '.repository', mavenOpts: '-Xmx6g -Xms2g -XX:-UseGCOverheadLimit') {
                        bat "mvn -Dsonar.branch.name=$CHANGE_BRANCH -Dsonar.branch.target=$CHANGE_TARGET clean verify"
                    }
                }
            }
        }

        stage('Test') {
            steps {
                junit '**/target/surefire-reports/*.xml' 
            }
        }

        stage('Deploy to first Repository') {
            steps {
                timestamps {
                    withMaven(maven: '(Default)', mavenLocalRepo: '.repository', mavenOpts: '-Xmx6g -Xms2g -XX:-UseGCOverheadLimit') {
                        bat "mvn javadoc:jar source:jar deploy -DskipTests"
                    }
                }
            }
        }

        stage('Deploy to second Repository') {
            when {
                environment name: 'BRANCH_NAME', value: master
            }

            steps {
                timestamps {
                    withMaven(maven: '(Default)', mavenLocalRepo: '.repository', mavenOpts: '-Xmx6g -Xms2g -XX:-UseGCOverheadLimit') {
                        bat "mvn javadoc:jar source:jar deploy -DskipTests -Pgod-release-deploy"
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                currentBuild.result = currentBuild.result ?: 'SUCCESS'
                notifyBitbucket()
            }
        }
    }
}
