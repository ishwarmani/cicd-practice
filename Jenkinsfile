pipeline {
    agent any

    stages {
        // this is first stage or you can say first job with given steps
        stage('testing first jenkinsfile') {
            steps {
                echo 'Hello Jenkins'
                sh 'whoami'
                // using git plugin to clone
                git branch: 'main', url: 'https://github.com/ishwarmani/cicd-practice.git'
                sh 'ls -a'
            }
        }
        // addin SAST in source 
        stage('doing security check in source code') {
            steps {
                echo 'starting with code checking process'
                
            }
        }
        // building above repo code using maven
        stage('mvn build stage') {
            steps {
                echo "building war file using mvn"
                 // build project
                sh '/opt/maven39/bin/mvn  install'
                sh 'ls target'
                sh 'mkdir -p /tmp/ishwarnew/'
                sh 'cp -rf target/*.war /tmp/ishwarnew/'
                sh '/opt/maven39/bin/mvn  clean'
            }
            // post stage for this stage
            post {
                failure {
                    echo 'sending email to Dev team'
                    mail bcc: '', body: '''check your recent push it got some build issues!!!
for more info ask for jenkins current build logs''', cc: '', from: '', replyTo: '', subject: 'Dev ALert', to: 'ishwarmanithapa@gmail.com'
                    
                }
            }
        }
        // pushing war to new git repo in release branch
        stage('pushing war to github') {
            steps {
                echo "pushing only war file to new repo"
                sh 'mkdir -p pushdata'
                dir('pushdata') {
                    git branch: 'main', url: 'https://github.com/ishwarmani/demo-cicd-arti.git'
                }
                
                sh 'ls -a'
                sh '''
                    cd pushdata
                    git checkout release
                    cp -rf /tmp/ishwarnew/*.war .
                    git add .
                    git commit -m "first push war file"
                    git push https://ishwarmani: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@github.com/ishwarmani/demo-cicd-arti.git
                    '''
            }
            
        }
        
        // Deployment stage using ansible
        stage ('ansible based deployment'){
            steps {
                echo 'checking ansible installation'
                sh 'ansible --version'
                sh 'mkdir -p /tmp/ishwar-ansible'
                dir('/tmp/ishwar-ansible'){
                    git branch: 'main', url: 'https://github.com/ishwarmani/demo-cicd-arti.git'
                    sh 'ls '
                    sh 'ansible-playbook -i inventory tomcat.yaml'
                }
                // removing ansible dir
                
            }
        }
    }
        // define post section
        post {
            success {
                echo 'Everything ran successfully'
                sh 'rm -rf /tmp/ishwar-ansible/'
                mail bcc: '', body: '''Hey we did it successfully!!!
all the pipeline with all stages are running successfully''', cc: 'ishwarmanithapa@gmail.com', from: '', replyTo: '', subject: 'congratulations -- we did it ', to: 'ashutoshhsingh93@gmail.com'
           
            }
            failure {
                echo 'some error occured'
            }
            
            always {
                echo 'clearing cache memory'
            }
        }
    
}
