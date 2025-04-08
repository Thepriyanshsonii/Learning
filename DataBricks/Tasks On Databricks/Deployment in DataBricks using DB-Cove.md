# Deployment in DataBricks using DB-Coves(Kenvue)
*Here we are deploying the notebook in snowflake from databricks using DB-coves for Kenvue.*

# *Deployment from DataBricks to Snowflake*

1.) Create Feature branch 

![git.png](attachment:ff27f7f3-028a-4922-b0fb-91d57f547078:39c96106-fecc-423f-a73b-7edfbdc867a1.png)

2.) Do following Steps in Feature Branch

## Steps to be Followed Here:-

### Step 1:- Databricks to Snowflake Raw (Core Raw)

                   command:-

               

### Step 2:- Insert Core Raw from Snowflake to DB-Coves

                    command:- 

                    `*dbt-coves generate sources --database <db name>*`

### Step 3:- Core Raw to Core Integration in DB-Coves

             1.) The file which is created in Core Raw copy paste it to in core Integration and rename it using ‘***int’*** in the beginning.

### Step 4:- Transfer Core Integration File to Core Access

             1.) Create  *“.sql”  file* 

                       → copy integration file and paste it 

             2.) create *“.yml”* file  and change reference in it 

                       → Reference of integration folder file 

### Step 5:- Run the command to push the files in Snowflake

                   command:- 

                    `*dbt run -model “model_name”*` 

                               Example : Run dbt for a specific model:

                                                 *`dbt-coves dbt --run -s int_formula_vw`*