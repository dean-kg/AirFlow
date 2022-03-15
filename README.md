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

도커 권한    
https://velog.io/@jeong3320/dockerdocker-sudo%EA%B6%8C%ED%95%9C%EC%97%86%EC%9D%B4-%EC%8B%A4%ED%96%89%ED%95%98%EA%B8%B0

## 서버 이사가면서 재설정
- docker-compose 2버전 이상설치
- airflow 2.3.2업데이트


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
- 3.5달러 (RAM 500MB) // 5달러 (RAM 1GB) -> 모두 webserver, scheduler 동시실행시 서버터짐 ㅋ  (최소 4gb의 메모리가 필요)

## dags 관련   
- 

## roles 관련   
- user에게 실행 권한 주기 ("can create on DAG Runs")
  
## webhook 관련     
- slack등의 outcome의 기능은 존재     
- 자체 income 기능은 없다     


## api
- client api -> 같은 서버 공간에서의 dags 실행을 위한 api.   
- 외부 서버 REST api https://airflow.apache.org/docs/apache-airflow/stable/stable-rest-api-ref.html.   



## pythonoperator Dags conf 인자 받기
api 상에서    
c = Client(None, None)    
c.trigger_dag(dag_id='dag_id', conf={'target':target}) 으로 api 호출    
dag 상에서 
```{.python}
def print_arguments(**kwargs):   
    table_name = kwargs['dag_run'].conf.get('table')   -> 포인트   
      
        
         
task = PythonOperator(
    task_id="sample_task",
    python_callable=print_arguments,
    provide_context=True,                ## 반드시 해당 옵션을 지정해야 함
    dag=dag
)
```


## variable 익명화   
password,secret,passwd,authorization,api_key,apikey,access_token 의 단어들이 key값으로 들어가면 value값이 익명으로 나타난다.     

## postgresql operator   
- hook을 이용 하여 sql return value를 핸들링 할 수 있다.  
```{.python}
from airflow.providers.postgres.hooks.postgres import PostgresHook


def reorderCheck(**xcompusher):
    hook = PostgresHook(postgres_conn_id='db_conn')
    
    #x_com으로 특정 value 추출
    userpk = xcompusher['t'].xcom_pull(key='v')['v1']

    if userpk =='NotExist':
        reorder = 'response Error'
    else:
    
        #쿼리 날리기
        resultSql = hook.get_records("select exists(select 1 from completedorderlst where userpk='%s');"%userpk)
        reorder =str(resultSql[0][0])
    
    #x_com 으로 데이터 업로드
    xcompusher['t'].xcom_push(key='vv', value={'vv':reorder})


```





## db 에서 pandas 이동

### push
```
def fun1(**params):
    mergeDF = pd.DataFrame()
    params['ti'].xcom_push(key='subdata', value={'data': mergeDF.to_dict()})
    return

def fun2(**params):
    import pandas as pd
    df = pd.DataFrame(params['ti'].xcom_pull(key='subdata')['data'])
    retrun


```


## postgreHook

### check exist
```
from airflow.providers.postgres.hooks.postgres import PostgresHook

hook = PostgresHook(postgres_conn_id='dbconn')
resultSql = hook.get_records(
    "select exists(select 1 from TABLENAME where COLUMNSNALE='%s');" % VALUE)
```

### insert

```
request = "insert into TABLENAME (p1,p2,p3) values ('%s',%d,'%s');" % (
    p1value, p2value, p3value)
pg_hook = PostgresHook(postgres_conn_id='dbconn')
connection = pg_hook.get_conn()
cursor = connection.cursor()
cursor.execute(request)
connection.commit()
cursor.close()
connection.close()
```


## get parameter conf from webserver
```
def function(**parm):

    ## on webserber {"name":"target_paramter"} -> 큰 따옴표로 찍어야함
    parm['dag_run'].conf.get('name')
    return 
```



## 에러발생케이스
- docker로 운영시 Python pakages -> webserver 도커에 설치
- Failed to fetch log file from worker. Unsupported URL protocol '' 에러의 경우 logs 폴더에 777권한
