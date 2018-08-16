node('docker-slave') {
    properties([
        parameters([
            string(defaultValue: '3.9.32-1', description: 'Origin version', name: 'VERSION'),
            string(defaultValue: 'getupcloud/origin-ansible', description: 'Repositoty name', name: 'PREFIX'),
            string(defaultValue: 'v3.9', description: 'Local tag name', name: 'OS_TAG'),
            string(defaultValue: 'v3.9', description: 'Remote tag name', name: 'OS_PUSH_TAG'),
        ]),
    ])
    ansiColor('xterm') {
        timestamps {
            stage('Build Image') {
                sh """
                    curl -OL https://github.com/openshift/openshift-ansible/archive/openshift-ansible-${VERSION}.tar.gz
                    tar -xzvf openshift-ansible-${VERSION}.tar.gz
                    cd openshift-ansible-openshift-ansible-${VERSION}/
                    ./hack/build-images.sh
                """
            }

            stage('Push Image') {
                withDockerRegistry([credentialsId: 'dockerhub', url: 'https://index.docker.io/v1/']) {
                    sh """
                        cd openshift-ansible-openshift-ansible-${VERSION}/
                        ./hack/push-release.sh
                    """
                }
            }
        }
    }
}
