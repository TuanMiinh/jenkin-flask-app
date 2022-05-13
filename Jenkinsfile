pipeline{
    agent any
    environment{
        ArtifactId= 'FlaskAPP'
        Version = '2'
        Name = 'jenkins-flask-app'
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
                nexusArtifactUploader artifacts: [[artifactId: 'app', classifier: '', file: 'app.zip', type: 'zip']], 
                credentialsId: '604cf559-1668-400f-9c30-7f89813b8415', groupId: 'jenkins-flask-app', 
                nexusUrl: '10.13.12.112:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'jenkins-flask-app', version: '2'
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