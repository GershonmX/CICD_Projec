pipeline {
    agent any

    parameters { string(name: 'IMG_URL', defaultValue: '', description: '') }

    stages {
        stage('Update YAML') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    sh '''
                    printenv
                    if [[ "$IMG_URL" == *"-polybot"* ]]; then
                        YAML_FILE="k8s/dev/polybot.yaml"
                        PWD
                    elif [[ "$IMG_URL" == *"-yolo5"* ]]; then
                        YAML_FILE="k8s/dev/yolo5.yaml"
                        PWD
                    else
                        exit 7
                    fi

                    git config --global user.email "Jenkins@ip-10.0.0.216"
                    git config --global user.name "gershonmx"
                    git checkout releases
                    git config pull.rebase false
                    git pull
                    git diff
                    git merge origin/dev
                    sed -i "s|image: .*|image: ${IMG_URL}|g" $YAML_FILE
                    git add $YAML_FILE
                    git commit -m "$IMG_URL"
                    git push https://gershonmx:$PASSWORD@github.com/gershonmx/CICD_Projec.git releases
                    '''
                }
            }
        }
    }
}