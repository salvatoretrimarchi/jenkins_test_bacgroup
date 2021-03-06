node {
    PROJECT_NAME="${JOB_BASE_NAME}-${BUILD_NUMBER}"
    echo "${PROJECT_NAME}"
    stage('Integrate Addons') {
        deleteDir()
        parallel (
            "Odoo Account": {
                dir('extra-addons/odoo-account'){
                    git branch: '10.0', depth: '1', url: 'http://bitbucket.org/bacgroup/odoo-account.git'
                }
             },
             "Odoo Fleet": {
                dir('extra-addons/odoo-fleet'){
                    git branch: '10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-fleet'
                }
             },
             "Odoo Purchase": {
                dir('extra-addons/odoo-purchaset'){
                    git branch: '10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-purchase'
                }
             },
             "Odoo Base": {
                dir('extra-addons/odoo-base'){
                    git branch: '10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-base'
                }
             },
             "Odoo Sale": {
                dir('extra-addons/odoo-sale'){
                    git branch: '10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-sale'
                }
             },
             "Odoo Panama Fiscal Printer": {
                dir('extra-addons/odoo-panama-fiscal-printer'){
                    //git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-panama-fiscal-printer'
                }
             },
             /* "Odoo fiscal printer": {
                dir('odoo-fiscal-printer'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-fiscal-printer'
                }
             },*/
             "Odoo Honduras": {
                dir('extra-addons/odoo-honduras'){
                    git branch: '10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-honduras'
                }
             },
             "Odoo POS": {
                dir('extra-addons/odoo-pos'){
                    git branch: '10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-pos'
                }
             },
             "Odoo OCA": {
                dir('extra-addons/odoo-oca'){
                    git branch: '10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-oca'
                }
             },
             "Odoo Panama": {
                dir('extra-addons/odoo-panama'){
                    //git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-panama'
                }
             },
             "Odoo Eyl": {
                dir('extra-addons/odoo-eyl'){
                    git branch: '10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-eyl'
                }
             },
             "Odoo Product": {
                dir('extra-addons/odoo-product'){
                    git branch: '10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-product'
                }
             },
             "Odoo Stock": {
                dir('extra-addons/odoo-stock'){
                    git branch: '10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-stock'
                }
             },
             "Odoo Avis": {
                dir('extra-addons/odoo-avis'){
                    git branch: '10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-avis'
                }
             },
             "Odoo DrugStore": {
                dir('extra-addons/odoo-drugstore'){
                    git branch: '10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-drugstore'
                }
             },
             "Odoo Partner": {
                dir('extra-addons/odoo-partner'){
                    git branch: '10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-partner'
                }
             },
             "Odoo CRM": {
                dir('extra-addons/odoo-crm'){
                    git branch: '10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-crm'
                }
             }
        )
        }
        stage('Integrate Odoo Enterprise') {
            
             parallel (
            "Odoo Core": {
                dir('odoo'){
                    sh 'git clone -b 10.0 --depth 1 https://github.com/bacgroup/odoo.git'
                }
             },
             "Odoo Enterprise": {
                dir('extra-addons/enterprise'){
                    git branch: '10.0', depth: '1', url: 'https://github.com/bacgroup/enterprise.git'
                }
             },
             )
            
                /* sh 'mkdir -p extra-addons'
                sh 'find . -maxdepth 1 | grep -v extra-addons| xargs -i mv {} ./extra-addons' */
        }
    stage('Configure Container') {
        
        sh 'sudo lxc-create -t download -n "${JOB_BASE_NAME}-${BUILD_NUMBER}" -- -d ubuntu -r xenial -a amd64'
        sh 'sudo lxc-start -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -d'
        sh 'sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- ls -l'

        sh 'sudo mkdir -p /var/lib/lxc/${JOB_BASE_NAME}-${BUILD_NUMBER}/rootfs/home/cust'
        sh 'sudo rsync -avP $HOME/wkhtmltox-0.12.1_linux-trusty-amd64.deb /var/lib/lxc/${JOB_BASE_NAME}-${BUILD_NUMBER}/rootfs/opt/'
        sh 'sudo rsync -avP odoo/odoo /var/lib/lxc/${JOB_BASE_NAME}-${BUILD_NUMBER}/rootfs/home/cust/'
        sh 'sudo rsync -avP extra-addons /var/lib/lxc/${JOB_BASE_NAME}-${BUILD_NUMBER}/rootfs/home/cust/'
        
        PORT=sh (
        script: 'port=1025; while netstat -atn | grep -q :$port; do port=$(expr $port + 1); done; echo $port',
        returnStdout: true
        ).trim()
            
        LXC_IP=sh (
        script: 'sudo lxc-ls --fancy --filter=${JOB_BASE_NAME}-${BUILD_NUMBER} --fancy-format IPV4 | tail -n1',
        returnStdout: true
        ).trim()
        
        echo "Esta IP se muestra desde Jenkins: ${LXC_IP}"
        
        sh 'cp $HOME/NGINX_Deploy_Template .'
        sh "sed -i \"s/RANDOM/${PORT}/g\" NGINX_Deploy_Template"
        sh "sed -i \"s/BUILD_NUMBER/${JOB_BASE_NAME}-${BUILD_NUMBER}/g\" NGINX_Deploy_Template"
        sh "sed -i \"s/LXC_IP/${LXC_IP}/g\" NGINX_Deploy_Template"
        sh "sudo mv NGINX_Deploy_Template /etc/nginx/sites-available/${JOB_BASE_NAME}-${BUILD_NUMBER}"
        sh "sudo ln -s /etc/nginx/sites-available/${JOB_BASE_NAME}-${BUILD_NUMBER} /etc/nginx/sites-enabled/${JOB_BASE_NAME}-${BUILD_NUMBER}"
        
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- adduser --system --quiet --shell=/bin/bash --home=/home/odoo --gecos 'ODOO' --group odoo"
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- adduser odoo sudo"
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- mkdir /var/log/odoo"
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- chown odoo:odoo /var/log/odoo -R"
        sh 'sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- chown odoo:odoo /home/cust -R'
        
    }
    stage('Configure Odoo') {
     
        ADDONSPATH=sh (
        script: "echo `sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- su -c \"ls -d -1 -m /home/cust/extra-addons/*\"` | sed 's/ //g'",
        returnStdout: true
        ).trim()
                
        echo "${ADDONSPATH}"

        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- apt-get update -y"
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- apt-get upgrade -y"
        
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- apt-get install gdebi-core -y"
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- gdebi /opt/wkhtmltox-0.12.1_linux-trusty-amd64.deb -n"
        
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- apt-get install postgresql -y"
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- su - postgres -c \"createuser -s odoo\" 2> /dev/null || true"
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- apt-get install wget git python-pip gdebi-core -y"
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- apt-get install python-dateutil python-feedparser python-ldap python-libxslt1 python-lxml python-mako python-openid python-psycopg2 python-pybabel python-pychart python-pydot python-pyparsing python-reportlab python-simplejson python-tz python-vatnumber python-vobject python-webdav python-werkzeug python-xlwt python-yaml python-zsi python-docutils python-psutil python-mock python-unittest2 python-jinja2 python-pypdf python-decorator python-requests python-passlib python-pil -y python-suds"
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- pip install gdata psycogreen ofxparse XlsxWriter xlrd"
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- apt-get install node-clean-css -y"
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- apt-get install node-less -y"
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- apt-get install python-gevent -y"
                
        sh 'sudo rsync -avP $HOME/ODOO_SYSTEMD_TEMPLATE /var/lib/lxc/${JOB_BASE_NAME}-${BUILD_NUMBER}/rootfs/etc/systemd/system/odoo.service'
        sh 'sudo rsync -avP $HOME/ODOO_Deploy_Template /var/lib/lxc/${JOB_BASE_NAME}-${BUILD_NUMBER}/rootfs/etc/odoo-server.conf'
        sh "sudo sed -i \"s&ODOO_MODULES&${ADDONSPATH}&g\" /var/lib/lxc/${JOB_BASE_NAME}-${BUILD_NUMBER}/rootfs/etc/odoo-server.conf"

        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- chown odoo:odoo /etc/odoo-server.conf -R"
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- chown odoo:odoo /etc/systemd/system/odoo.service -R"
        
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- systemctl daemon-reload"
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- systemctl enable odoo.service"
        
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- systemctl start odoo.service"
        sh "sudo /etc/init.d/nginx reload"
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- systemctl status odoo.service"
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- apt-get -y install nginx"
        
        sh "sudo rsync -avP $HOME/NGINX_Container_Template /var/lib/lxc/${JOB_BASE_NAME}-${BUILD_NUMBER}/rootfs/etc/nginx/sites-available/odoo"
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- chown root:root /etc/nginx/sites-available/odoo"
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- touch /etc/ssl/certs/commercial.crt"
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- touch /etc/ssl/certs/commercial.key"
    }
    stage('Prepare Artifact')
    {  
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- systemctl stop odoo.service"
        sh "sudo lxc-attach -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -- shutdown -h now"
    }
    stage('Archive Artifact') {
        sh "sudo tar -ccvf /home/jenkins/workspace/${JOB_NAME}/${JOB_BASE_NAME}-${BUILD_NUMBER}.tar /var/lib/lxc/${JOB_BASE_NAME}-${BUILD_NUMBER} && sudo pbzip2 -9f /home/jenkins/workspace/${JOB_NAME}/${JOB_BASE_NAME}-${BUILD_NUMBER}.tar"
        archiveArtifacts '*.bz2'
    }
    stage('Odoo is Ready!')
    {
        sh "sudo lxc-start -n ${JOB_BASE_NAME}-${BUILD_NUMBER} -d"
        echo "Your Odoo Instance is Ready :) Please use ${PORT} to Test"
    }
/*     
    stage('Create Hotcopy') {
        sh 'sudo mkdir -p /${JOB_BASE_NAME}-${BUILD_NUMBER}'
        sh "sudo /usr/bin/dbdctl setup-snapshot /dev/mapper/manvhost042--vg-root /.datto ${BUILD_NUMBER}"
        sh "sudo /bin/mount -o ro,noexec,noload /dev/datto${BUILD_NUMBER} /${JOB_BASE_NAME}-${BUILD_NUMBER}"
    }
    
    stage('Archive Artifact') {
        sh "sudo tar -ccvf /home/jenkins/workspace/${JOB_NAME}/${JOB_BASE_NAME}-${BUILD_NUMBER}.tar /hotcopy/var/lib/lxc/${JOB_BASE_NAME}-${BUILD_NUMBER} && sudo pbzip2 -9f /home/jenkins/workspace/${JOB_NAME}/${JOB_BASE_NAME}-${BUILD_NUMBER}.tar"
        archiveArtifacts '*.bz2'
    }
    stage('Destoy Hotcopy') {
        sh "sudo /bin/umount /${JOB_BASE_NAME}-${BUILD_NUMBER}"
        sh "sudo /usr/bin/dbdctl destroy ${BUILD_NUMBER}"
        sh "rm -rf /${JOB_BASE_NAME}-${BUILD_NUMBER}"
    } */
    
}

