pipeline {
    agent any

    parameters {
        string(name: 'VCENTER_HOST', defaultValue: '', description: 'vCenter IP or hostname')
        string(name: 'DATACENTER_NAME', defaultValue: '', description: 'Datacenter name')
        string(name: 'VM_NAME', defaultValue: '', description: 'Name of the VM')
        string(name: 'SNAPSHOT_NAME', defaultValue: '', description: 'Snapshot name to delete')
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-credentials',
                    url: 'https://github.com/bkumbk/jenkins-pipeline-demo'
            }
        }

        stage('Validate Inputs') {
            steps {
                script {
                    if (!params.VM_NAME || !params.SNAPSHOT_NAME || !params.VCENTER_HOST || !params.DATACENTER_NAME) {
                        error 'All parameters are required!'
                    }
                }
                echo "VM Name      : ${params.VM_NAME}"
                echo "Snapshot     : ${params.SNAPSHOT_NAME}"
                echo "vCenter Host : ${params.VCENTER_HOST}"
                echo "Datacenter   : ${params.DATACENTER_NAME}"
            }
        }

        stage('Delete Snapshot') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'vcenter-credentials',
                    usernameVariable: 'VCENTER_USER',
                    passwordVariable: 'VCENTER_PASS'
                )]) {
                    ansiblePlaybook(
                        playbook: 'delete_snapshot.yml',
                        installation: 'ansible',
                        colorized: true,
                        extras: '-v',
                        extraVars: [
                            vcenter_hostname: params.VCENTER_HOST,
                            vcenter_username: env.VCENTER_USER,
                            vcenter_password: env.VCENTER_PASS,
                            datacenter_name: params.DATACENTER_NAME,
                            vm_name: params.VM_NAME,
                            snapshot_name: params.SNAPSHOT_NAME
                        ]
                    )
                }
            }
        }

    }

    post {
        success {
            echo "✅ Snapshot '${params.SNAPSHOT_NAME}' deleted from '${params.VM_NAME}' successfully!"
        }
        failure {
            echo "❌ Failed to delete snapshot. Check console output for details."
        }
    }
}
