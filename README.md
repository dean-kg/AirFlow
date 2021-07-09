# AirFlow
airflow 설치부터 운영까지 이모저모


## 셋업 postgre    
sudo apt-get install postgresql postgresql-contrib -y    


## 초기 셋업 db   
https://jungwoon.github.io/airflow/2019/02/26/Airflow.html 
sudo -u postgres psql


## 도커용 
https://dorumugs.tistory.com/entry/AirFlow-Manual-on-Docker-stage-install    


## 초기 유저설정

airflow users create \
 --username admin \
 --firstname FIRST_NAME \
 --lastname LAST_NAME \
 --role Admin \
 --password admin \
 --email pbj00812@gmail.com
 
 
 ##
