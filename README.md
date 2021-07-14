# AirFlow
airflow 설치부터 운영까지 이모저모


## 셋업 postgre    
sudo apt-get install postgresql postgresql-contrib -y    


## 초기 셋업 db   
mysql, sqlite의 경우 multiprocess불가능 -> postgresql 이동필요     


https://jungwoon.github.io/airflow/2019/02/26/Airflow.html   
sudo -u postgres psql    
- db셋업 동작 :http://sanghun.xyz/2017/12/airflow-3.-localexecutor-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0/

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
 
 
## /home/admin/airflow/python_runtime/bin/gunicorn 에러 대처    
 
$ export PATH=$PATH:~/.local/bin   

## 실행 
airflow webserver -p port_num &!
airflow schduler &!

## lightsail vm 실패    
- 3.5달러 (RAM 500MB) // 5달러 (RAM 1GB) -> 모두 webserver, scheduler 동시실행시 서버터짐 ㅋ    

## dags 관련   
- 

## roles 관련   
- user에게 실행 권한 주기 ("can create on DAG Runs")
  
## webhook 관련     
- slack등의 outcome의 기능은 존재     
- 자체 income 기능은 없다     
