pipeline{
    agent{
        label "slave"
    }
    stages{
        // stage("Build Started"){
        //     steps{
        //     sh '''
        //             curl -s -X POST https://api.telegram.org/bot5988052752:AAEsBVdnpFaainVkCE_ns5IQOGdsvy9tKTc/sendMessage -d chat_id=-1001804879191 -d parse_mode="HTML" -d text="Build Started ${JOB_NAME} ${BUILD_ID}"
		// 		'''
        //     }
        // }
        stage("Clearning workspace"){
            steps{
            cleanWs()
            }
        }
        stage("Git pull"){
            steps{
            checkout(
                [$class: 'GitSCM', 
                branches: [[name: '*/master']], 
                extensions: [], 
                userRemoteConfigs: [
                    [
                        url: 'https://github.com/vaibhavraj05/recuritmentportal.git'
                    ]
                ]])
            }
        }
        // stage("Docker Notification Started"){
        //     steps{
        //         sh '''
        //             curl -s -X POST https://api.telegram.org/bot5988052752:AAEsBVdnpFaainVkCE_ns5IQOGdsvy9tKTc/sendMessage -d chat_id=-1001804879191 -d parse_mode="HTML" -d text="Image Build Started ${JOB_NAME} ${BUILD_ID}"
		// 		'''

        //     }
        // }
        stage("Docker Push"){
            steps{
                    script{
                       def image1 = docker.build(
                        "vaibhavrajnathchauhan/recuritmentportal:${BUILD_ID}",
                         "--file ./recuritmentportal/Dockerfile ./recuritmentportal"
                         )
	                image1.push()
                    }
                    
                }
                
            }
        // stage("Docker Notification Completed"){
        //     steps{
        //         sh '''
        //             curl -s -X POST https://api.telegram.org/bot5988052752:AAEsBVdnpFaainVkCE_ns5IQOGdsvy9tKTc/sendMessage -d chat_id=-1001804879191 -d parse_mode="HTML" -d text="Image Build Completed ${JOB_NAME} ${BUILD_ID}"
		// 		'''

        //     }
        // }
        stage("Removing Image"){
            steps{
                sh "docker rmi -f vaibhavrajnathchauhan/recuritmentportal:${BUILD_ID}"
                sh '''
                    if [ $(docker images -f "dangling=true" -q) != "" ];
                        then
                            docker rmi $(docker images -f "dangling=true" -q)
                    fi
                '''
            }
        }
        // stage("Docker Notification"){
        //     steps{
        //         sh '''
        //             curl -s -X POST https://api.telegram.org/bot5988052752:AAEsBVdnpFaainVkCE_ns5IQOGdsvy9tKTc/sendMessage -d chat_id=-1001804879191 -d parse_mode="HTML" -d text="Ansible Build ${JOB_NAME} ${BUILD_ID}"
		// 		'''

        //     }
        // }
        stage("Ansible playbook"){
            steps{
                ansiblePlaybook credentialsId: '125c65a4-99ea-48f5-8fd5-50bd6c9ac735', 
                    installation: 'Ansible', 
                    inventory: 'dev.inv' ,
                    playbook: 'main.yml' ,
                    extraVars   : [
                    BUILD_ID: "${ BUILD_ID }",
                    ]
            }
        }
        stage("Removal of old images"){
            steps{
            sh'''
            if [ $(docker images vaibhavrajnathchauhan/recuritmentportal --filter "before=vaibhavrajnathchauhan/recuritmentportal:${BUILD_ID}" -q) != "" ];
                        then
                            docker rmi -f $(docker images vaibhavrajnathchauhan/recuritmentportal --filter "before=vaibhavrajnathchauhan/recuritmentportal:${BUILD_ID}" -q)
            fi
                

            '''
            }
        }
    }
    // post{
    //     success{
    //             sh '''
    //                 curl -s -X POST https://api.telegram.org/bot5988052752:AAEsBVdnpFaainVkCE_ns5IQOGdsvy9tKTc/sendMessage -d chat_id=-1001804879191 -d parse_mode="HTML" -d text="Build Completed ${JOB_NAME} ${BUILD_ID}"
	// 			'''

    //     }
    //     failure{
    //         sh'''
    //             curl -s -X POST https://api.telegram.org/bot5988052752:AAEsBVdnpFaainVkCE_ns5IQOGdsvy9tKTc/sendMessage -d chat_id=-1001804879191 -d parse_mode="HTML" -d text="Build Failed ${JOB_NAME}  ${BUILD_ID}"
    //         '''

    //     }
    // }
}





