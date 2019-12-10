
## Magento Docker
Magento 2.2.6

For this project, you would need to install Docker. [Here](https://gist.github.com/GaryRojas/652aa66885c9dd26e6f38f86f68fc81c), you have a simple guide where you can set/install step to step 
on Linux 18.x. 

### Installation Instructions

1. Git clone this project

    ```
    git clone git@github.com:garyrojas/docker-magento226.git
    ```

2. From the project root go to clone your project Magento repo with "src" as directory name

    ```
    cd my-project
    ```
    
    ```
    git clone git@github.com:e-commerce/my-project.git src
    ```

       
3. From the project root run
 
    ```
    cp env/db.env.example env/db.env
    ```
   
4. Define a DB access
 
    ```
    env/db.env
    ```

   
5. Copy some files to the containers and install dependencies, then restart the containers.

    ```
    docker-compose up -d
    ```
   
    ```
    bin/copytocontainer --all
    ```
   
6. Install composer dependencies, then copy artifacts back to the host:

    ```
    bin/composer install
    ```
   
   * NOTE: You will need the public and private key from Magento keys to log in. If you no have one then generate it.
    Anyway, here we are sharing generic keys.
    
    ```
   user: 7c5afdbb50efd603fa5baa7c39b845e1
   password: cb7be2c272973e544af7fc7cef58f955
    ```
   
    ```
    bin/copyfromcontainer vendor
    ```
   
7. Put the database to project

    ```
    cp origin_your_path_database/italmod.sql db/data.sql
    ```

8. Import database
 
    ```
    bin/clinotty mysql -hdb -umyuser -pmypassword db_name < db/data.sql
    ```
   
9. From the project root run
 
    ```
    cp src/app/etc/env.php.example src/app/etc/env.php
    ```
   
12. Config your admin url and database connection details
 
    ```
    src/app/etc/env.php
    ```
   
    * NOTE: If you wish config your data by setup file then config and run `bin/setup`


13. Set base URLs to local environment URL

 
    ```
    bin/magento setup:store-config:set --base-url="http://my-project.local/"
    ```
   
    ```
    bin/magento setup:store-config:set --base-url-secure="https://my-project.local/"
    ```
        
   
14. Clean cache and re-index.
    
    ```
    bin/magento cache:flush
    ```   
    
    ```
    bin/magento indexer:reindex
    ```
    
15. Restart

    ```
    bin/restart
    ```
    
16. Create you user admin, set with your data and run this command line
    
    ```
    bin/magento admin:user:create --admin-user='myusername' --admin-password='MyPassword' --admin-email='myusername@myemail.com' --admin-firstname='MyName' --admin-lastname='MyLastname'
    ```     

