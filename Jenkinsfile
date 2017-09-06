node {    
    stage('Build') {
        echo 'Building..'
        parallel (
            "Odoo Honduras": {
                dir('odoo-honduras'){
                    echo 'Hola!'
                    git branch: 't10.0', depth: '1', url: 'http://bitbucket.org/bacgroup/odoo-account.git'
                }
             },
            "Odoo Argentina": {
                dir('odoo-account'){
                    echo 'Hola!'
                    git branch: 't10.0', depth: '1', url: 'http://bitbucket.org/bacgroup/odoo-account.git'
                }
             }
        )
    }
}

