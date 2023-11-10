pipeline{
    agent any



    stages {


        stage('Getting project from Git') {
            steps{
      			checkout([$class: 'GitSCM', branches: [[name: '*/main']],
			extensions: [],
			userRemoteConfigs: [[url: 'https://github.com/mariemBoughanmni/DevOps_Project.git']]])
            }
        }


       stage('Cleaning the project') {
            steps{
                	sh "mvn -B -DskipTests clean  "
            }
        }



        stage('Artifact Construction') {
            steps{
                	sh "mvn -B -DskipTests package "
            }
        }



         stage('JUnit / Mockito') {
            steps{
		    echo " test "
               		 //sh "mvn test " 
            }
        }



        stage('Code Quality Check via SonarQube') {
                   steps{

                    		sh "  mvn clean verify sonar:sonar -Dsonar.projectKey=cicd -Dsonar.projectName='cicd' -Dsonar.host.url=http://172.10.0.140:9000 -Dsonar.token=sqp_2d9c32825b84fd25555d9230e8b94561bfb89119 "

                   }
               }


        stage('Publish to Nexus') {
            steps {


  sh 'mvn deploy'


            }
        }

stage('Build Backend Docker Image') {
                      steps {
                          script {
                           sh 'docker build -t amirovvv/spring-app:second .'
                          }
                      }
                  }

                  stage('login dockerhub') {
                                        steps {
				sh 'docker login -u amirovvv --password dckr_pat_LAIjui5cw-3dOSsdt8AoUuVNZ5o'
                                            }
		  }

	                      stage('Push Backend Docker Image') {
                                        steps {
                                   sh 'docker push amirovvv/spring-app:second'
                                            }
		  }

/*  stage('clone frontend'){
         steps{
             script{
                   checkout([$class: 'GitSCM', branches: [[name: '*//* main']], extensions: [], userRemoteConfigs: [[url:"https://github.com/housseml17/front.git"


]]])
             }
         }

 } */

 /* stage("build and push frontend docker image") {
        
         
            steps {
                script {
                     
             sh 'docker login -u toumi15 --password dckr_pat_0iaom9peVjYUg0VIvUkeT-5V4bg'
         
             sh "docker push toumi15/front-app:Toumi"
           
                }
            }
                
                
             
                
            }


     
	    stage('Build Frontend Docker Image') {
                      steps {
                          script {
                            sh 'docker build -t toumi15/front-app:Toumi .'
                          }
                      }
                  }

 stage("Push Frontend Docker image") {


            steps {
                script {

             sh 'docker login -u toumi15 --password dckr_pat_0iaom9peVjYUg0VIvUkeT-5V4bg'

             sh "docker push toumi15/front-app:Toumi"

                }
            }




            } */


stage('Run Spring && MySQL Containers') {
                                steps {
                                    script {
                                      echo "MySql"
					    sh 'docker-compose up -d'
                                    }
                                }
                            }


}




}

