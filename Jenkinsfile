node {
    PROJECT_NAME="${JOB_BASE_NAME}-${BUILD_NUMBER}"
    echo "${PROJECT_NAME}"
    stage('Integrate Addons') {
        deleteDir()
        parallel (
            "Odoo Account": {
                dir('extra-addons/odoo-account'){
                    git branch: 't10.0', depth: '1', url: 'http://bitbucket.org/bacgroup/odoo-account.git'
                }
             },
             "Odoo Fleet": {
                dir('extra-addons/odoo-fleet'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-fleet'
                }
             },
             "Odoo Purchase": {
                dir('extra-addons/odoo-purchaset'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-purchase'
                }
             },
             "Odoo Base": {
                dir('extra-addons/odoo-base'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-base'
                }
             },
             "Odoo Sale": {
                dir('extra-addons/odoo-sale'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-sale'
                }
             },
             "Odoo Panama Fiscal Printer": {
                dir('extra-addons/odoo-panama-fiscal-printer'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-panama-fiscal-printer'
                }
             },
             /* "Odoo fiscal printer": {
                dir('odoo-fiscal-printer'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-fiscal-printer'
                }
             },*/
             "Odoo Honduras": {
                dir('extra-addons/odoo-honduras'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-honduras'
                }
             },
             "Odoo POS": {
                dir('extra-addons/odoo-pos'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-pos'
                }
             },
             "Odoo OCA": {
                dir('extra-addons/odoo-oca'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-oca'
                }
             },
             "Odoo Panama": {
                dir('extra-addons/odoo-panama'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-panama'
                }
             },
             "Odoo Eyl": {
                dir('extra-addons/odoo-eyl'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-eyl'
                }
             },
             "Odoo Product": {
                dir('extra-addons/odoo-product'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-product'
                }
             },
             "Odoo Stock": {
                dir('extra-addons/odoo-stock'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-stock'
                }
             },
             "Odoo Avis": {
                dir('extra-addons/odoo-avis'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-avis'
                }
             },
             "Odoo DrugStore": {
                dir('extra-addons/odoo-drugstore'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-drugstore'
                }
             },
             /* "VITT Icons": {
                dir('vitt-icons'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/vitt_icons'
                }
             },*/
             "Odoo Partner": {
                dir('extra-addons/odoo-partner'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-partner'
                }
             },
             "Odoo CRM": {
                dir('extra-addons/odoo-crm'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-crm'
                }
             }
            /*  "VITT FiscalSeq Stock": {
                dir('vitt_fiscalseq_stock'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/vitt_fiscalseq_stock'
                }
             },*/
             /* "VITT CRM Selectphone": {
                dir('odoo-honduras'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/vitt_crm_lead_selectphone'
                }
             }*/
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
        
    }
    stage('Configure Odoo') {
     
        ADDONSPATH=sh (
        script: "sudo su -c \"ls -d -1 -m /var/lib/lxc/VITT_HN_BETA-55/rootfs/home/cust/extra-addons/*\"",
        returnStdout: true
        ).trim()
        
        echo "${ADDONSPATH}"
        
    }
    
}

