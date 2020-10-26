##  Steps for configuring PostgreSQL
1. ##### Setting up DigitalOcean droplet  
   * Log in into Digital Ocean Account  
   * Go to the following link and create a postgres droplet 
   https://marketplace.digitalocean.com/apps/supabase-postgres 
   * You can also create a postgres droplet by clicking the green Create button from right side.  
   After clicking Droplet item from list choose Marketplace and search for ***supabase-postgres*** keyword in the *Search keyword* text box.
    ![Create](essentialprogramming-api/src/main/resources/img/createDroplet.png)
 
2. ##### Setting up the droplet
   * Once the Droplet has been created, it will have an IP address.  
    Copy the IP address into your clipboard, and open a terminal.  
    We're going to use SSH to connect to our Droplet.
    
   * Write inside the terminal the following line: 
       ```
       ssh root@46.101.88.57
        ```
       > **_NOTE:_**  Your IP address will be different than mine, so make sure you use the one that you copied from the DigitalOcean Droplets panel..

   * In addition to the package installation, this droplet also enables the UFW firewall to allow port 22 for SSH and port 5432 for access to PostgreSQL.  
   Once you have your droplet up and running, you would first need to set up the password for your DB superuser postgres.  
     Once you have SSH into your instance, type in the following:  
     ```$ sudo -u postgres psql postgres```  
     Then, in the psql terminal, type in the following:
     ```$ \password postgres```  
   * Enter your desired password and reconfirm. You can disconnect from your SSH afterwards. Now, with any SQL client of your choice, you can now connect to the database with the following credentials:
     
     Host: **insert droplet ip address**
     
     Port: **5432**
     
     User: **postgres**
     
     Password: **insert your chosen password**
     
     Database: **postgres**  
   * At any point wherein you would want to check whether your database is up, you can type this into your host terminal:  
    ```$ pg_isready -h <insert droplet ip address> -p 5432```

3. ##### Access the DB from terminal
    * In order to be able to interact with the Database from the terminal execute the following command (in the droplet terminal) :  
    ```sudo -i -u postgres psql```
    * After this command is executed you will be able to execute commands on psql which is a terminal-based front-end to PostgreSQL.  
    It enables you to type in queries interactively, issue them to PostgreSQL, and see the query results. 
    * Some useful psql commands :  
        *  **Enlisting the available databases** : ```\l```  
        *  **Enlisting the available tables in the current database** : ```\dt```  
        *  **Switching to another database** : ```\c <database_name>```  
        *  **Knowing the version of PostgreSQL** : ```SELECT version();```
        * **Enlisting all the available commands (list of all the available psql commands)** : ```\?```  

4. ##### Setting up the pgAgent service 
    
    * Execute the following commands in the newly spawned droplet terminal :  
    ```sudo apt-get update```  
    ```sudo apt-get install pgagent```  
    * **pgAgent** runs as a daemon on Unix systems, and a service on Windows systems.  
    In most cases it will run on the database server itself - for this reason, pgAgent is not automatically configured when pgAdmin is installed.
    
    * Before using pgAdmin to manage pgAgent, you must create the pgAgent extension in the maintenance database registered with pgAdmin. To install pgAgent on a PostgreSQL host, connect to the postgres database, and navigate through the Tools menu to open the Query tool.  
    Execute the following command :  
    ```CREATE EXTENSION pgagent;```  
    
         *This command will create a number of tables and other objects in a schema called ‘pgagent’*  
    * The database must also have the pl/pgsql procedural language installed - use the PostgreSQL CREATE LANGUAGE command to install pl/pgsql if necessary.  
    Execute the following command in terminal :  
    ```CREATE LANGUAGE plpgsql;```
    
    * The **pgagent** service needs to be started in order to do that we need to navigate to the /home directory from terminal.  
    Execute the following commands :  
        * ```cd ..```  
        * ```cd home```
        * ```mkdir postgres``` this file should have the name of the DB superuser
        * ```vi .pgpass``` this file needs to be populated as follows :  
        ```HOST:PORT:NAME_OF_DATABASE:USER:PASS```  
        Example : ```46.101.102.33:5432:Play:postgres:password123```
        * Set the permissions for ```.pgpass``` file : ```sudo chmod 600 .pgpass```  
        * Set the file owner as the same user using which you logged in :  
        ```sudo chown login_username:login_username .pgpass```  
            *In our example : ```sudo chown postgres:postgres```*  
        * Set PGPASSFILE environment variable : ```export PGPASSFILE='/home/user/.pgpass'```
    * If the scheduled Job is already created you can test it by right-clicking it and selecting **Run now** option. 
      Check if the actions specified in the job were executed.                                                

                                                 


        


   