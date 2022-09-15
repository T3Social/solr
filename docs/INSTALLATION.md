# Installation

## Apache Solr Installation

This guide covers only the basic installation and configuration steps of Apache Solr. More advanced configuration will be required, such as firewall security settings or individual performance tuning.

> Note: This documentation was tested at last with Apache Solr 8.4.1 and should be compatible with all Apache Solr versions 8.0+.

1. Install [Apache Solr](https://lucene.apache.org/solr/downloads.html) as described in the [Solr manual](https://lucene.apache.org/solr/guide/). 

    As quick start: Download Apache Solr and extract it to any folder (e.g. /opt/solr). After successfully extracting the file, Solr can be installed as a service in a Linux system by calling solr/bin/install_solr_service.sh.

2. Start SOLR. e.g. 

    ```
    cd /opt/solr
    ./bin/solr start
    ```
  
3. Add a new core for HumHub. e.g.

    ```
    cd /opt/solr
    ./bin/solr create -c humhubcore
    ```

4. Configure the core. Make sure that this step happens before the first use of the core.

    Stop the Solr Service:
    
    ```bash
    cd /opt/solr
    ./bin/solr stop
    ```
    
    Copy configuration provided by HumHub SOLR module.
    
     ```bash
     cd /opt/solr/server/solr/humhubcore/conf
  
     cp /path-to-humhub/protected/modules/solr/docs/schema.xml .
     cp /path-to-humhub/protected/modules/solr/docs/solrconfig.xml .  
     rm managed-schema
     ```

5. Restart Apache Solr after this change!

    ```
    cd /opt/solr
    ./bin/solr restart
    ``` 

## HumHub Configuration

1. Make sure the Solr Search module is installed and enabled.

2. Add following part into the `components` section of your configuration file `protected/config/common.php`.

    ```php
    <?php
    return [
        // ...
        'components' => [
            // ...
            'search' => [
                'class' => 'humhub\modules\solr\engine\SolrSearch',
                'host' => 'localhost',
                'port' => 8983,
                'path' => '/solr/humhubcore'
            ],
            // ...
        ]
        // ...
    ];
    ```

    More information about the individual configurations can be found here: [docs.humhub.org](https://docs.humhub.org)

3. Rebuild the search index. 

    ```bash
    cd /path-to-humhub-here/protected
    php yii search/rebuild
    ```

