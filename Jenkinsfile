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


         sh 'mvn clean package deploy:deploy-file -DgroupId=tn.esprit.devops_project -DartifactId=DevOps_Project -Dversion=1.0 -DgeneratePom=true -Dpackaging=jar -DrepositoryId=maven-releases -Durl=http://172.10.0.140:8081/repository/maven-releases/ -Dfile=target/DevOps_Project-1.2.jar'


                   }
               }

stage('Build Backend Docker Image') {
                      steps {
                          script {
                           sh 'docker build -t mariemmm935/spring-app:mariemdevops .'
                          }
                      }
                  }

                  stage('login dockerhub') {
                                        steps {
				sh 'docker login -u mariemmm935 --password dckr_pat_PWrzv5C_U5p9vuubMxCmZYi7dTY'
                                            }
		  }

	                      stage('Push Backend Docker Image') {
                                        steps {
                                   sh 'docker push mariemmm935/spring-app:mariemdevops'
                                            }
		  }

  stage('clone frontend'){
         steps{
             script{
                   checkout([$class: 'GitSCM', branches: [[name: '*//* main']], extensions: [], userRemoteConfigs: [[url:"https://github.com/mariemmm935/front.git"


]]])
             }
         }

 } 

  stage("build and push frontend docker image") {
        
         
            steps {
                script {
                     
             sh 'docker login -u mariemmm935 --password dckr_pat_0iaom9peVjYUg0VIvUkeT-5V4bg'
         
             sh "docker push mariemmm935/front-app:mariemdevopsfront"
           
                }
            }
                
                
             
                
            }


     
	    stage('Build Frontend Docker Image') {
                      steps {
                          script {
                            sh 'docker build -t mariemmm935/front-app:mariemdevopsfront .'
                          }
                      }
                  }

 stage("Push Frontend Docker image") {


            steps {
                script {

             sh 'docker login -u mariemmm935 --password dckr_pat_0iaom9peVjYUg0VIvUkeT-5V4bg'

             sh "docker push mariemmm935/front-app:mariemdevopsfront"

                }
            }




            } 


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

