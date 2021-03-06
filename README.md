# ClinicalTrialsViz

### Setting up system environment variables:

To access the clinical trials database, you will be prompted with a username and password to connect. Before using the files in this repository, you must do the following:

* Go to the home directory of your terminal `cd $HOME`

* Create a new file called __.Renviron__ `sudo nano .Renviron`

* Inside the file write your username, password, and host environment variables with your database login information:

``` 
userid = "'your database account username'"
userpass = "'your database account password'"
host = "'your database endpoint'"
```

****Important: for our moravian college username, password, and database endpoint information reference our 1password entry under "AACT AWS Instance" notes**

* Save and exit nano pressing the keys `ctrl+c, ctrl+x, y, enter`

* Restart your R studio application to ensure environment variables are processed

* Now you may use the scripts available in the repository, these environment variables will be referenced later using the syntax Sys.getenv()

### Allowing access to AWS instance

* Connect to the AWS website and click on the RDS section

* Click on the database labeled "clinicaltrialsdatabase"

* Under the "Connectivity & security" tab, find "Security" and click on the blue link directly under "VPC security groups"

* Click on the "inbound" tab and click edit

* Click "Add Rule" and create a new Custom TCP Rule with port range 5432 and for the source drop-down menu click "My IP", then click save

* If this is done correctly, your computer will now be able to pass through security and connect to the database

### Connecting to the database

* First-time startup enter the command (this command will only ever need to be entered one time) `install.packages("RPostgreSQL")`

* Then you must access the RPostgreSQL library to use the commands `library(RPostgreSQL)`

* Initialize database driver `drv <- dbDriver('PostgreSQL')`

* Connect to database with user environment variables

```
con <- dbConnect(drv,    dbname="aact",    host=Sys.getenv("host"),    port=5432,    user=Sys.getenv("userid"),    password=Sys.getenv("userpass"))
```

* Make the database schema public for easier query syntax `dbExecute(con, "SET search_path TO ctgov,public")
`

* Test that your connection is valid with a sample database query command `sample <- dbGetQuery(con, "select distinct study_type from studies")`