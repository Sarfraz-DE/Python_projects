Outcome BTN History maintains the call level information while taking outcomes data into account to extract features and understand the pattern of particular PhoneNumber and Account Number.
Introduction:
Outcome BTN History maintains the call level information while taking outcomes data into account to extract features and understand the pattern of particular BTN.


**Main Columns and their Understanding:**


BTN
Billing Telephone Number

calcdate
Date for which btnh was generated
 
lastcall_*
There can be multiple outcomes. Last call outcome variables deal with the most recent outcome

recent_*
recent_* variables - are binomial variables ,calculated for outcome optimization variable list to check if there was any outcome within specified duration or not

first_calldate
first call date

lastdeptsplit
queue in last call

first_call
first call flag

lastcalldate
Last call date of call

handletime_3_lastcall
binning of aht based on normalization variables

handletime_4_lastcall
binning of aht based on normalization variables

handletime_5_lastcall
binning of aht based on normalization variables

days_since_last_call
Days passed since last call

weeks_since_last_call
Weeks passed since last call

months_since_last_call
Months passed since last call

days_since_first_call
Days passed since first call

weeks_since_first_call
Weeks passed since first call

months_since_first_call
Months passed since first call	
 
Ncalls
Number of calls for that btn

ncalls_24hours
Number of calls within 24 hours for that btn

ncalls_48hours
Number of calls within 48 hours for that btn

ncalls_1week
Number of calls within 1 week for that btn

ncalls_1month
Number of calls within 1 month for that btn

ncalls_60days
Number of calls within 60 days for that btn

first_call_*
first call calculated queue wise

lastcalldate_*
Last call date computed queue wise

handletime_3_*
Handletime_3 calculated queue wise

handletime_4_*
Handletime_4 calculated queue wise

handletime_5_*
Handletime_5 calculated queue wise

days_since_last_call_*
days since last call calculated queue wise

weeks_since_last_call_*
weeks since last call calculated queue wise

months_since_last_call_*
months since last call calculated queue wise

days_since_first_call_*
days since first call calculated queue wise

weeks_since_first_call_*
days since first call calculated queue wise

months_since_first_call_*
days since first call calculated queue wise

ncalls_*
Number of calls computed queue wise

 

**Helping Tables List:**
btnh.btn_history	This table contains all historical information of BTN's group by calcdate. This is the one we used to join with SME

btnh.btn_history_backup	Backup table for btn_history use in rollback operation

btnh.btn_history_stagging	This table contains current crux call date btn's

btnh.btn_lookup_backup	Backup table for Lookup use in rollback operation

btnh.btn_lookup_stagging	This table contains current crux call date btn's

btnh.btnhistory_lookup	This table contains latest call information for all BTN's. This table is used in production for runtime lookup

btnh.current_btns	This table contains all btn's for the current call date

btnh.daywaise_n_calls_backup	Backup table for daywise_n_calls_table in rollback operation

btnh.daywise_n_calls_table	This table contains total number of calls per btn and calldate

btnh.future_n_calls_table	This table contains total number of calls per btn and programid(queues)

btnh.future_n_calls_backup	Backup table for future_n_calls_table in rollback operation

btnh.historical_lookup	This table contains historical snapshot for the Lookup table.

btnh.historical_lookup_backup	Backup table for historical_lookup in rollback operation

btnh.previous_ncalls	This table helps in updating ncalls in Lookup table

btnh.sme_btns	This table contain all those btn's that need to be inserted in btn_history table

Setup Outcome BTN History (MySQL):

**Steps:**

BTNH source table should be prepared in MySQL (you can either use view or create separate table with proper optimization)
smedata_configurations table must be filled out according to requirements by AI team
smedata_configurations creation table statement: smedata_configuration.sql
Python and required libraries must be installed needed to run crux code, these are the libraries and their desired versions
Python 3.9.1
Pandas 1.3.3
SQLAlchemy 1.4.25
mysqlclient

mysql-connector-python
 Above packages or other versions which do not suit to the python version you have. Please download these from https://pypi.org
Note: For linux os, make sure you are proper rights for reading and writing files or proper rights to access python and its libraries
After that get crux code and get it uploaded to desired server.


**How to fill smedata.configurations table:**

Below given are the important table columns which crux utility uses:
client
vaca_date_filter_column
duration
btnh_sourcetable
btnh_filtercriteria
btnh_department_split
btnh_normalized_split
btnh_lastcall_outcome
btnh_opt_variables
Given is the one possible sample of what to fill in this table, fill accordingly to your client:
Column Variable	Description	Sample
client	Unique client name used in script to fetch configuration	test
vaca_date_filter_column	Main calltime column in source that can be used to fetch records	calltime
duration	is used for optimization variables	15
btnh_sourcetable	source table or view with schema	etl_dev.btnh_source
btnh_filtercriteria	filter criteria to filter out records based on some condition or keep it empty	and btn <> ''
btnh_department_split	lob or programid	programid
btnh_normalized_split	column used to normalize aht for binning. Possible values are vdn, skill	vdn
btnh_lastcall_outcome

column list of outcome variables for last call outcome identification	issale,iscredit,creditamount
btnh_opt_variables	column list of outcome variables for optimization purpose	issale,iscredit,creditamount
For Next Steps, go in the code directory where crux script is and fill credentials.json file

**Fill credentials.json file**

host	Server IP	
port	Server Port	
user	Server User	
database	db name where btnhistory source table exits	"satmap"
password	Server password (try to avoid special characters in password)	
FinalTable	Databse+tablename where you need to create crux tables	"etl_dev.`salman_dgs.btnhistory_sme`"
ConfigTable	smedata.configuration db and table name	 "etl_dev.`smedata.configurations_test`"
SourceTable	btnhistory sourcetable db and table name	"etl_dev.`btnhistory.sourcetable`"
ClientName	Value of client field from smedata.configuration	test
programID	Comma separated list of all desired queues	"Mobile,Other,Sales"
LagDays_Utility	Tells if you want to on Lag days	True/False
LagDays	no of lag days	1/2
LagDays_units	units in days/hours	Days/Hours
In case you want to turn off lag, you just need to set LagDays_Utility to False
Make sure, creation and other rights are given to user mentioned used in script
Make sure, you set client value from smedata.configurations table in ClientName correctly to get that configuration

**Setup Outcome BTN History (Greenplum):
Steps:**

BTNH source table should be prepared in Greenplum
smedata_configurations table must be filled out according to requirements by AI team
smedata_configurations creation table statement: sme_config_gp_create_stmt.txt
Python and required libraries must be installed needed to run crux code, these are the libraries and their desired versions
Python 3.9.1
Pandas 1.3.3
Psycopg2 2.9.1
SQLAlchemy 1.4.25
 Above packages or other versions which do not suit to the python version you have. Please download these from https://pypi.org
After that get crux code and get it uploaded to desired server.
Note: For linux os, make sure you are proper rights for reading and writing files or proper rights to access python and its libraries
How to fill smedata_configurations table:

Below given are the important table columns which crux utility uses:
client
vaca_date_filter_column
duration
btnh_sourcetable
btnh_filtercriteria
btnh_department_split
btnh_normalized_split
btnh_lastcall_outcome
btnh_opt_variables
Given is the one possible sample of what to fill in this table, fill accordingly to your client


host	Server IP	
port	Server Port	
user	Server User	"aai_user"
database	db name where btnhistory source table exits	"satmap"
password	Server password (try to avoid special characters in password)	
FinalTable	Databse+tablename where you need to create crux tables	"aai_afiniti.salman_dgs.btnhistory_sme"
finalSchema	schema name	"aai_afiniti"
finalDatabase	Database name	"gpafiniti"
ConfigTable	smedata.configuration db and table name	 "aai_afiniti.\"smedata_configurations\""
SourceTable	btnhistory sourcetable db and table name	"aai_afiniti.btnhistory_sourcetable"
ClientName	Value of client field from smedata.configuration	"test"
programID	Comma separated list of all desired queues	"mobile,other,sales"
LagDays_Utility	Tells if you want to on Lag days	True/False
LagDays	no of lag days	1/2
LagDays_units	units in days/hours	Days/Hours
role	db user value same as passed in user	"aai_user"
In case you want to turn off lag, you just need to set LagDays_Utility to False
Make sure, creation rights are given are user mentioned in role
Make sure, you set client value from smedata_configurations table in ClientName correctly to get that configuration

