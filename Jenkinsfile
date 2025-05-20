pipeline {
    agent any

    stages {
        stage('Install Nginx on CentOS') {
            steps {
                echo 'Installing Nginx on CentOS...'
                sh '''
                   sudo yum install -y epel-release
                   sudo yum install -y nginx
                   sudo systemctl start nginx
                   sudo systemctl enable nginx
                '''
            }
        }

        stage('Check Nginx status') {
            steps {
                echo 'Checking Nginx service status...'
                sh 'sudo systemctl status nginx'
                echo 'Checking Nginx HTTP response...'
                sh 'curl -I http://localhost'
            }
        }

        stage('Install Grafana on CentOS') {
            steps {
                echo 'Installing Grafana on CentOS...'
                sh '''
                   sudo bash -c 'cat > /etc/yum.repos.d/grafana.repo <<EOF
[grafana]
name=grafana
baseurl=https://packages.grafana.com/oss/rpm
repo_gpgcheck=1
enabled=1
gpgcheck=1
gpgkey=https://packages.grafana.com/gpg.key
sslverify=1
EOF'

                   sudo yum install -y grafana
                   sudo systemctl daemon-reload
                   sudo systemctl start grafana-server
                   sudo systemctl enable grafana-server
                '''
            }
        }

        stage('Check Grafana status') {
            steps {
                echo 'Checking Grafana service status...'
                sh 'sudo systemctl status grafana-server'
                echo 'Checking Grafana HTTP response...'
                sh 'curl -I http://localhost:3000 || true'
            }
        }
    }
}
