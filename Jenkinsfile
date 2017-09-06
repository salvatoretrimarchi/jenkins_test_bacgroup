node {
    stage('Integrate') {
        deleteDir()
        parallel (
            "Odoo Account": {
                dir('extra-addons/Odoo-Account'){
                    git branch: 't10.0', depth: '1', url: 'http://bitbucket.org/bacgroup/odoo-account.git'
                }
             },
             "Odoo Fleet": {
                dir('odoo-fleet'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-fleet'
                }
             },
             "Odoo Purchase": {
                dir('odoo-purchaset'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-purchase'
                }
             },
             "Odoo Base": {
                dir('odoo-base'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-base'
                }
             },
             "Odoo Sale": {
                dir('odoo-sale'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-sale'
                }
             },
             "Odoo Panama Fiscal Printer": {
                dir('odoo-panama-fiscal-printer'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-panama-fiscal-printer'
                }
             },
             /* "Odoo fiscal printer": {
                dir('odoo-fiscal-printer'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-fiscal-printer'
                }
             },*/
             "Odoo Honduras": {
                dir('odoo-honduras'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-honduras'
                }
             },
             "Odoo POS": {
                dir('odoo-pos'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-pos'
                }
             },
             "Odoo OCA": {
                dir('odoo-oca'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-oca'
                }
             },
             "Odoo Panama": {
                dir('odoo-panama'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-panama'
                }
             },
             "Odoo Eyl": {
                dir('odoo-eyl'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-eyl'
                }
             },
             "Odoo Product": {
                dir('odoo-product'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-product'
                }
             },
             "Odoo Stock": {
                dir('odoo-stock'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-stock'
                }
             },
             "Odoo Avis": {
                dir('odoo-avis'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-avis'
                }
             },
             "Odoo DrugStore": {
                dir('odoo-drugstore'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-drugstore'
                }
             },
             /* "VITT Icons": {
                dir('vitt-icons'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/vitt_icons'
                }
             },*/
             "Odoo Partner": {
                dir('odoo-partner'){
                    git branch: 't10.0', depth: '1', url: 'https://bitbucket.org/bacgroup/odoo-partner'
                }
             },
             "Odoo CRM": {
                dir('odoo-crm'){
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
        stage('Configuration') {
                /* sh 'mkdir -p extra-addons'
                sh 'find . -maxdepth 1 | grep -v extra-addons| xargs -i mv {} ./extra-addons' */
        }
    }
}

