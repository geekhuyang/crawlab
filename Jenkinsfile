pipeline {
    agent {
        node {
            label 'crawlab'
        }
    }

    environment {
        HOME = '/home/yeqing'
        NODE_HOME = '/home/yeqing/.nvm/versions/node/v8.12.0'
        ROOT_DIR = "/home/yeqing/jenkins_home/workspace/crawlab_${GIT_BRANCH}" 
        PYTHON_HOME = '/home/yeqing/.pyenv/shims'
    }

    stages {
        stage('Setup') {
            steps {
                echo "Running Setup..."
                // sh '. /home/yeqing/.profile'
                script {
                    if (env.GIT_BRANCH == 'develop') {
                        env.MODE = 'test'
                    } else if (env.GIT_BRANCH == 'master') {
                        env.MODE = 'production'
                    } else {
                        env.MODE = 'test'
                    }
                }
            }
        }
        stage('Build Frontend') {
            steps {
                echo "Building frontend..."
                // sh "${NODE_HOME}/bin/node ${NODE_HOME}/bin/npm install -g yarn pm2 --registry=http://registry.npm.taobao.org/"
                sh "cd ${ROOT_DIR}/frontend && ${NODE_HOME}/bin/node ${NODE_HOME}/bin/yarn install --registry=http://registry.npm.taobao.org/ --scripts-prepend-node-path=${NODE_HOME}/bin/node"
                sh "cd ${ROOT_DIR}/frontend && ${NODE_HOME}/bin/node ${ROOT_DIR}/frontend/node_modules/.bin/vue-cli-service build --mode=${MODE}"
            }
        }
        stage('Build Backend') {
            steps {
                echo "Building backend..."
                sh "${PYTHON_HOME}/pip install -r ${ROOT_DIR}/crawlab/requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple"
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
                sh "${NODE_HOME}/bin/node ${NODE_HOME}/bin/pm2 reload all"
            }
        }
    }
}