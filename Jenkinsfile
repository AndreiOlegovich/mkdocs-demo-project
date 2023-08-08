pipeline {
    agent {label 'mkdocs_builder'}


    stages {
        stage("Pull Git Repository") {
            steps {
                script {
                    sh 'pwd'
                    sh 'ls -la'
                }
                git(
                    url: "git@github.com:AndreiOlegovich/mkdocs-demo-project.git",
                    branch: "main",
                    changelog: true,
                    poll: true
                )
            }
        }
        
        
        stage('Backup') {
            steps {
                script {
                    sh 'pwd'
                    sh 'ls -la'
                }
                dir(".") {
                    sh 'pwd'
                    sh 'ls -la'
                    script {
                        if (fileExists('site_arj')) {
                            dir("site_arj") {
                                deleteDir()
                            }
                        } else {
                            echo 'No site_arj dir found'
                        }
                    }
                    script {
                        if (fileExists('site')) {
                           sh 'mv site site_arj'
                        } else {
                            echo 'No site dir found'
                        }
                    }
                }
                script {
                    sh 'pwd'
                    sh 'ls -la'
                }
                
                
            }
        }
        stage('Build') {
            steps {
                script {
                    dir('.') {
                        sh 'python -m mkdocs build'
                        sh 'ls -la'
                    }
                }
            }
        }
        stage('Deliver') {
            steps {
                dir('site') {
                    sh 'pwd'
                    sh 'ls -la'
                }
                script {
                    sh 'cp -r site /home/jenkins/mkdocs/documentation/'
                }
            }
        }
    }
    
    post {
        // Clean after build
        always {
            cleanWs(cleanWhenNotBuilt: false,
                    deleteDirs: true,
                    disableDeferredWipeout: true,
                    notFailBuild: true,
                    patterns: [[pattern: '.gitignore', type: 'INCLUDE'],
                               [pattern: '.propsfile', type: 'EXCLUDE']])
        }
    }
}
