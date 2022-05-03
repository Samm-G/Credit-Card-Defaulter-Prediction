# Credit Card Deaulters Prediction

## Problem Statement
    
    To build a classification methodology to determine whether a person defaults the credit card payment for the next month.
    
## Data Description
    
    The client will send data in multiple sets of files in batches at a given location. The data has been extracted from the census bureau. 
    The data contains 32561 instances with the following attributes:

| Feature Name               | Data Type   | Description                                                                                          |
|----------------------------|-------------|------------------------------------------------------------------------------------------------------|
| LIMIT_BAL                  | continuous  | Credit Limit of the person.                                                                          |
| SEX                        | categorical | 1 = male; 2 = female                                                                                 |
| EDUCATION                  | categorical | 1 = graduate school; 2 = university; 3 = high school; 4 = others                                     |
| MARRIAGE                   | categorical | 1 = married; 2 = single; 3 = others                                                                  |
| AGE                        | num         | Age of the Person                                                                                    |
| PAY_0 to PAY_6             | num         | History of past payment. We tracked the past monthly payment records (from April to September, 2005) |
| BILL_AMT1 to BILL_AMT6     | num         | Amount of bill statements                                                                            |
| PAY_AMT1 to PAY_AMT6       | num         | Amount of previous payments                                                                          |
| default payment next month | Boolean     | Target Label; Yes = 1, No = 0                                                                        |
    
## Data Validation
    
    In This step, we perform different sets of validation on the given set of training files.
    
    Name Validation: We validate the name of the files based on the given name in the schema file. We have 
    created a regex patterg as per the name given in the schema fileto use for validation. After validating 
    the pattern in the name, we check for the length of the date in the file name as well as the length of time 
    in the file name. If all the values are as per requirements, we move such files to "Good_Data_Folder" else
    we move such files to "Bad_Data_Folder."
    
    Number of Columns: We validate the number of columns present in the files, and if it doesn't match with the
    value given in the schema file, then the file id moves to "Bad_Data_Folder."
    
    Name of Columns: The name of the columns is validated and should be the same as given in the schema file. 
    If not, then the file is moved to "Bad_Data_Folder".
    
    The datatype of columns: The datatype of columns is given in the schema file. This is validated when we insert
    the files into Database. If the datatype is wrong, then the file is moved to "Bad_Data_Folder."
    
    Null values in columns: If any of the columns in a file have all the values as NULL or missing, we discard such
    a file and move it to "Bad_Data_Folder".
    
## Data Insertion in Database
     
     Database Creation and Connection: Create a database with the given name passed. If the database is already created,
     open the connection to the database.
     
     Table creation in the database: Table with name - "Good_Data", is created in the database for inserting the files 
     in the "Good_Data_Folder" based on given column names and datatype in the schema file. If the table is already
     present, then the new table is not created and new files are inserted in the already present table as we want 
     training to be done on new as well as old training files.
     
     Insertion of file in the table: All the files in the "Good_Data_Folder" are inserted in the above-created table. If
     any file has invalid data type in any of the columns, the file is not loaded in the table and is moved to 
     "Bad_Data_Folder".
     
## Model Training
    
     Data Export from Db: The data in a stored database is exported as a CSV file to be used for model training.
     
     Data Preprocessing: 
        Check for null values in the columns. If present, impute the null values using the KNN imputer.
        
        Check if any column has zero standard deviation, remove such columns as they don't give any information during 
        model training.
        
     Clustering: KMeans algorithm is used to create clusters in the preprocessed data. The optimum number of clusters 
     is selected

# Local Development Setup

## Clone this Git.
```
git clone https://github.com/Samm-G/Cement-Strength-Prediction.git
```

## Create Conda Environment.
(Git Bash in project folder)
```
conda create -p <path_of_new_conda-venv> python==3.8 -y
```
(Open CMD in the main git-repo folder)
```
conda activate <path_of_new_conda-venv>
```
```
pip install -r requirements.txt
```

## To update requirements.txt
```buildoutcfg
pip freeze > requirements.txt
```

## Initialize Your Own Git Repo
```
rm -rf .git

git init

echo "**/__pycache__/" >> .gitignore

git add .
git commit -m "First Commit"

git branch -M main

git branch dev
git rebase dev



git remote add origin <github_url>
git push -u origin main
```

## To update your Modifications
```
git add .
git commit -m "proper message"
git push 
```

## Remove Py-Cache:
```
find . | grep -E "(__pycache__|\.pyc|\.pyo$)" | xargs rm -rf
```

## Application Entry Point:
```
python main.py
```


# Build Local Docker Image:

## Docker Login:
```
docker login -u $DOCKERHUB_USER -p $DOCKER_HUB_PASSWORD_USER docker.io
```

## Create a file "Dockerfile" with below content

```
FROM python:3.7
COPY . /app
WORKDIR /app
RUN pip install -r requirements.txt
ENTRYPOINT [ "python" ]
CMD [ "main.py" ]
```

## Build Docker Image:
```
docker build -t <docker_image_name>:latest .
```

## See Docker Images:
```
docker images
```

## Run Image on local.:
```
docker run -p 5000:5000 <docker_image_name>
```


# Deployment:

## GCloud Deployment: (TODO)

    
    Create G Cloud Console Project:
        cloud.google.com > Console > IAM & Admin > Manage Resources > Create Project
        Give Project Name > Create

    Dashboard:
        Go to Menu > App Engine > Dashboard
        Select current project in top bar.
        Start Tutorial > Python
        Follow the Tutorial.. Select correct Project Name, and enter given commands.

    Download GCloud CLI:
        Follow Instructions:
            https://cloud.google.com/sdk/docs/install

    Open cmd:
        cd <project_root>
        
        Create new Config and set :
            gloud init
            2
            3
            <Give New account email to to create config>
            <Set Current project in choice to this one.>
        
        gcloud app deploy app.yaml --project <project_name>
        <Select any region to deploy>
        y

        Configure Access:
            Copy Access URL and launch
            Enable API (Enable Billing)
            Add Billing Info.
        
        gcloud app deploy app.yaml --project <project_name>
        <Select any region to deploy>
        y

        <Open Given URL and Access from Browser>


        
    





    
    


