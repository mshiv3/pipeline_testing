node {
     stage('checkout the repo'){
              
	               git url: 'https://github.com/chrishantha/sample-java-programs.git'
		            }
			         
				      stage('SonarQube analysis') {
				          withSonarQubeEnv('sonarqube') {
					        sh 'mvn clean package sonar:sonar'
						     }
						        }
							   
							      stage("Quality Gate"){
							        timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
								    def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
								        if (qg.status != 'OK') {
									      error "Pipeline aborted due to quality gate failure: ${qg.status}"
									          }
										    }
										    }
										       
										            stage('building the artifact')
											          {
												      sh 'mvn clean install'
												             } 
													           stage('uploading the artifacts')
														         {
															     def server = Artifactory.server 'Artifactory-1'
															         
																     def uploadSpec = """{
																       "files": [
																           {
																	         "pattern": "allocations/target/allocations.jar",
																		       "target": "example-repo-local"
																		           }
																			    ]
																			    }"""
																			    server.upload(uploadSpec)
																			          }
																				  }
