pipeline {
   agent any
   stages{
	
     stage("Git checkout"){
     steps{

         git credentialsId: 'jenkins', url: 'git@github.com:praveshjangra/jenkins-kubernetes.git'
      }
     
     }
     stage("Destroying docker containers"){
       steps{
        sh "sudo  docker rm -f \$(sudo docker ps -aq)|| echo 'All Good !!'"
       }
     }
     stage("Running create Set up"){
		 steps{
      sh ''' sudo bash /home/pjangra/code/axonchefms/examples/consul_vault/setup -o create; 
	     sudo bash /home/pjangra/code/axonchefms/examples/consul_vault/setup-o cookbookconfig -p ../../axon_core/.kitchen
		 '''
              
    
     }
	 }
	 stage("Destroying existing Kitchen docker"){
		 steps{
                sh "cd /home/pjangra/code/axonchefms/axon_core;sudo kitchen destroy all"
	 }
	 }
	 stage("Running kitchen Craete"){
		 steps{
		 sh "cd /home/pjangra/code/axonchefms/axon_core;sudo kitchen create"
	 }
	 }
	 stage("Runing MS in parralel"){
		
	    options{
			retry (2)
		}
        parallel{
		   
			stage ("postgresql"){
				steps{
				sh "cd /home/pjangra/code/axonchefms/axon_core;sudo kitchen converge postgresql-axcentos-7"
			}
			}
             stage ("rabbitmq"){
				steps{
				sh "cd /home/pjangra/code/axonchefms/axon_core;sudo kitchen converge rabbitmq-axcentos-7"
			}
			}
			 stage ("orientdb"){
				steps{
				sh "cd /home/pjangra/code/axonchefms/axon_core;sudo kitchen converge orientdb-axcentos-7"
			}
			}
			 stage ("Odbc"){
				steps{
				sh "cd /home/pjangra/code/axonchefms/axon_core;sudo kitchen converge odbconsumer-axcentos-7"
			}
			}
			 stage ("changequest"){
				steps{
				sh "cd /home/pjangra/code/axonchefms/axon_core;sudo kitchen converge changequest-axcentos-7"
			}
			}

			 stage ("docupload"){
				steps{
				sh "cd /home/pjangra/code/axonchefms/axon_core;sudo kitchen converge docupload-axcentos-7"
			}
			}

		}
	 }

			 stage ("axoncore"){
				 
				steps{
					retry(2)
				sh "cd /home/pjangra/code/axonchefms/axon_core;sudo kitchen converge axon-core-axcentos-7"
			}
			}

	 

   }
}