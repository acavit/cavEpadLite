pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Building containers and starting..'
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
                echo "workspace : ${env.WORKSPACE}"
                //sh 'cd /home/epad/thick_test_v4_october_26_plugintest/epad_lite_dist'
                //sh 'ls -l'
                dir("${env.WORKSPACE}/testFolder"){
                    sh "pwd"
                    sh 'wget ftp://epad-public.stanford.edu/epadLite/epad-dist-0.4.zip'
                    sh 'ls -l'
                    sh 'unzip  epad-dist-0.4.zip'
                    sh ''
                }
                dir("${env.WORKSPACE}/testFolder/"){
                    sh 'mkdir couchdbloc'
                    sh 'mkdir mariadbloc'
                    sh 'mkdir pluginData'
                    sh 'mkdir tmp'
                    sh 'chmod 777 couchdbloc'
                    sh 'chmod 777 mariadbloc'
                    sh 'chmod 777 pluginData'
                    sh 'chmod 777 tmp'
                   
                }
                dir("${env.WORKSPACE}/testFolder/epad-dist-master/"){
                    sh 'ls -l'
                    sh 'pwd'
                    sh 'cp /home/epad/epad-dist-master/epad.yml ./'
                    sh 'cat epad.yml'
                    sh './configure_epad.sh ../epad_lite_dist ./epad.yml'
                   
                }
                dir("${env.WORKSPACE}/testFolder/epad_lite_dist/"){
                    withEnv(['COMPOSE_HOME=/usr/local/bin']) {
                        sh '$COMPOSE_HOME/docker-compose up -d'
                     }
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh 'printenv'
                echo "using the branch ${env.BRANCH_NAME}"
                echo "using the commit ${env.GIT_COMMIT}"
                echo "using local branch ${env.GIT_LOCAL_BRANCH}"
             
                
            }
               // post {
                //  always {
                 //       dir("${env.WORKSPACE}/testFolder/epad_lite_dist/"){
                  //          withEnv(['COMPOSE_HOME=/usr/local/bin']) {
                   //             sh '$COMPOSE_HOME/docker-compose down'
                    //         }
                    //    }
                 // }

                  //success {
                  //    githubNotify status: "SUCCESSFUL" , description: "building succeed for "
                  //}

                  //failure {
                  //    githubNotify status: "FAILED" , description: "building failed for "
                  // }
               // }
        }
        stage('Deploy') {
            steps {
                 waitUntil {
                    script{
                        output = sh(returnStdout: true, script: 'docker ps -a --filter health=healthy | wc -l')
                    
                    echo "${output}"
                     if (output.toInteger() > 4)
                        echo "passed"
                     else
                        echo "no container"
                    }
                 }
                echo 'Deploying....'
                   sh 'docker ps -a'
                echo 'finished....'
                //dir("${env.WORKSPACE}/"){
                //        sh 'sudo rm -rf ./*'
                //}
            }
        }
    }
}
