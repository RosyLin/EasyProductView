pipeline {
    agent any

    tools {
	    jdk 'JDK1.8'
	  }
       
    stages {
        stage('Build and Test') {
            steps {
                echo 'Building...'
                sh './gradlew clean build -Dlabel=${GIT_BRANCH}.${BUILD_NUMBER} --stacktrace'
                echo 'Build finish ok'
            }
        }
        stage('Staging') {
            steps {
                sh 'mkdir -p /scmshared/CE/buildsvr/staging/Retail/${GIT_BRANCH}/${GIT_BRANCH}.${BUILD_NUMBER}/EasyProductViewEAR'
                            
                sh 'cp -r EasyProductViewEAR/build/libs/* \
                            /scmshared/CE/buildsvr/staging/Retail/${GIT_BRANCH}/${GIT_BRANCH}.${BUILD_NUMBER}/EasyProductViewEAR'
                            
                sh 'chmod -Rf 2775 /scmshared/CE/buildsvr/staging/Retail/${GIT_BRANCH}'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying...'
                echo "Moving EAR to: /scmshared/CE/buildsvr/staging/Retail/${env.GIT_BRANCH}/${env.GIT_BRANCH}.${env.BUILD_NUMBER}/EasyProductViewEAR/"
                        step([$class: 'UCDeployPublisher',
                              component: [componentName: 'EasyProductViewEAR_Component',
                              componentTag: '',
                              delivery: [$class: 'Push', baseDir: "/scmshared/CE/buildsvr/staging/Retail/${env.GIT_BRANCH}/${env.GIT_BRANCH}.${env.BUILD_NUMBER}/EasyProductViewEAR/",
                              fileExcludePatterns: '',
                              fileIncludePatterns: '**/*',
                              pushDescription: 'Pushed to UCD from Jenkins',
                              pushIncremental: false, pushProperties: '',
                              pushVersion: "${env.GIT_BRANCH}.${env.BUILD_NUMBER}"]],
                              siteName: 'udeploy-server'
                          ])
                        // echo 'Moving Property files...'     
                        // step([$class: 'UCDeployPublisher',
                        //       component: [componentName: 'EasyProductViewEARProperties_Component',
                        //       componentTag: '',
                        //       delivery: [$class: 'Push', baseDir: "/scmshared/CE/buildsvr/staging/Retail/${env.GIT_BRANCH}/${env.GIT_BRANCH}.${env.BUILD_NUMBER}/EasyProductViewEARProperties/",
                        //       fileExcludePatterns: '',
                        //       fileIncludePatterns: '**/*',
                        //       pushDescription: 'PROPS Pushed to UCD from Jenkins',
                        //       pushIncremental: false, pushProperties: '',
                        //       pushVersion: "${env.GIT_BRANCH}.${env.BUILD_NUMBER}"]],
                        //       siteName: 'udeploy-server'
                        // ])
                        echo 'Deploying Latest EAR to WAS TEST...'
                        step([$class: 'UCDeployPublisher',
                          deploy: [
                            $class: 'com.urbancode.jenkins.plugins.ucdeploy.DeployHelper$DeployBlock',
                            deployApp: 'EasyProductViewEAR',
                            deployEnv: 'TEST',
                            deployVersions: "EasyProductViewEAR_Component:${env.GIT_BRANCH}.${env.BUILD_NUMBER}",
                            deployProc: 'DeployEAROnly',
                            deployOnlyChanged: false
                          ]
                        ])
            }
        }
    }
    post {
    
    success {
         emailext (
          to: 'ConfigurationEngineers@llbean.com',
          subject: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
          body: """SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':
            Check console output at ${env.BUILD_URL}consoleFull """,
          recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider'], 
          [$class: 'CulpritsRecipientProvider']]
        )
    }

    failure {
       emailext (
          to: 'ConfigurationEngineers@llbean.com',
          subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
          body: """FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]':
            Check console output at ${env.BUILD_URL}consoleFull """,
          recipientProviders: [[$class: 'DevelopersRecipientProvider'], [$class: 'RequesterRecipientProvider'], 
          [$class: 'CulpritsRecipientProvider']]
        )
    }
  }
}
//how to --stacktrace parameter to your grade build in the Jenkinsfile?