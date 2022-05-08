pipeline{
    agent any
    environment{
        ArtifactId= 'FlaskAPP'
        Version = '2'
        Name = 'FlaskApp'
        GroupId = 'PythonFlask'
    }

    stages {

        stage ('Build'){
            steps{
                sh 'zip -r app.zip app.py'
            }
           
        }

        stage ('Test'){
            steps {
                echo 'Testing ...'
            }
        }

        stage ('Send to nexus'){
            steps{
                nexusArtifactUploader artifacts: [[artifactId: 'app', classifier: '', file: 'app.zip', type: 'zip']], credentialsId: '', 
                groupId: 'FlaskAPP', nexusUrl: '192.168.1.206:8081', nexusVersion: 'nexus2', protocol: 'http', 
                repository: 'jenkins-flask-app', version: '2'
            }
        }

        stage ('Deploy docker'){
            steps{
                sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible', transfers: [sshTransfer(cleanRemote: false, excludes: '', 
                execCommand: 'ansible-playbook /home/ansibleadmin/deploydocker.yaml -i /etc/ansible/hosts', execTimeout: 120000, flatten: false, 
                makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', 
                remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
            }
        }
    }
}