# Week 2 — Distributed Tracing

For this week 2, I have decided to **put the code for the homework tasks in the journal**, and write down and document how I completed all the tasks watching Andrew's Videos 2-3x.

I wrote down code for each task on the notepad, and went over the videos again to check and then executed them.

Did commit them to GitHub as and when neccessary.


# Homework Review

Followed instructions from https://github.com/omenking/aws-bootcamp-cruddur-2023/blob/week-2/journal/week2.md, watched videos, took notes carefully on the notepad, went over the videos again, compared them to my notes, and finally executed all the tasks step-wise.


## Videos

- [Week 2 -  AWS Cloud Project Bootcamp - Distributed Tracing - honeycomb.io set up](https://www.youtube.com/watch?v=2GD9xCzRId4)

- [Week 2 - Instrument XRay](https://www.youtube.com/watch?v=n2DTsuBrD_A)

- [Week 2 - CloudWatch Logs](https://www.youtube.com/watch?v=ipdFizZjOF4)

- [Week 2 - Rollbar](https://www.youtube.com/watch?v=xMBDAb5SEU4)

- [Week 2 - X-Ray Subsegments Solved](https://www.youtube.com/watch?v=4SGTW0Db5y0)

- [Week 2 - Observability vs Monitoring Explained in AWS](https://www.youtube.com/watch?v=bOf4ITxAcXc&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=31)

- [Week 2 - Pick the right cloud role: A beginners guide!](https://www.youtube.com/watch?v=E0haz6mymxY&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=35)

- [Week 2 - Honeycomb, Rollbar, AWS X-Ray and AWS Cloudwatch Logs pricing considerations](https://www.youtube.com/watch?v=2W3KeqCjtDY&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=38)

- [Week 2 - Github Codespaces Crash Course](https://www.youtube.com/watch?v=L9KKBXgKopA&list=PLBfufR7vyJJ7k25byhRXJldB5AiwgNnWv&index=36)

### 1. Week 2 - AWS Cloud Project Bootcamp - Distributed Tracing - honeycomb.io set up

**Honeycomb** is an opentelemetry standard service. By integrating Honeycomb into our application, we can observe and improve our code by tracing (monitor and analyze the behavior and performance of a system) issues that may occur while ruuning the application. It supports a wide range of data sources, including logs, metrics, and traces, and provides powerful visualization and analysis tools for exploring and understanding this data. 

Traces are a way of tracking the flow of requests through a distributed system. A trace is essentially a record of the interactions between the different components of the system as the request is processed. It includes information about the timing and duration of each interaction, as well as any errors or exceptions that occurred along the way.

**1. Created an anvironment for Cruddur in honeycomb.io to get the API_KEY**

**2. Setting environment variables for Gitpod with API Key for start up**

```
export HONEYCOMB_API_KEY=<"API KEY">
gp env HONEYCOMB_API_KEY=<"API KEY">
```

- Check if the environment variables have been set

```
env | grep HONEY
```

-  Jessica advised not to set honeycomb service name globally, instead to set it up for each service in docker-compose file. If you need to unset the service name,

run this command

```
unset SERVICE_NAME
```
 
**3.  OTEL (open telemetry) variables were added to docker compose for back-end service**

OpenTelemetry (OTEL) is an open source observability framework that assist in generating and capturing telemetry data and send it honeycomb.io

Add the following to the docker compose file for back-end service

```
OTEL_SERVICE_NAME: 'backend-flask'
OTEL_EXPORTER_OTLP_ENDPOINT: "https://api.honeycomb.io"
OTEL_EXPORTER_OTLP_HEADERS: "x-honeycomb-team=${HONEYCOMB_API_KEY}"
```

**4. Add open telemetry libraries (packages) to requirements.txt**

- This is to ensure that these packages are installed when deploying the application, and the backend application will have the necessary instrumentation to generate and export telemetry data to a tracing and monitoring backend service like Honeycomb.io

```
opentelemetry-api 
opentelemetry-sdk 
opentelemetry-exporter-otlp-proto-http 
opentelemetry-instrumentation-flask 
opentelemetry-instrumentation-requests
```

- cd into backend-flask and run the command

```
pip install opentelemetry-api 
```

```
pip install -r requirements.txt
```

**5. Importing required modules from the opentelemetry package for tracing and instrumentation**

- backend-flask/app.py, add

```
# Honeycomb -------
from opentelemetry import trace
from opentelemetry.instrumentation.flask import FlaskInstrumentor
from opentelemetry.instrumentation.requests import RequestsInstrumentor
from opentelemetry.exporter.otlp.proto.http.trace_exporter import OTLPSpanExporter
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor
from opentelemetry.sdk.trace.export import ConsoleSpanExporter, SimpleSpanProcessor


# Honeycomb -------
# Honeycomb - Initialize tracing and an exporter that can send data to Honeycomb
provider = TracerProvider()
processor = BatchSpanProcessor(OTLPSpanExporter())
provider.add_span_processor(processor)

# Honeycomb - Show spans in console standard output
simple_processor = SimpleSpanProcessor(ConsoleSpanExporter())
provider.add_span_processor(simple_processor)


trace.set_tracer_provider(provider)
tracer = trace.get_tracer(__name__)


# Create a flask and initialize automatic instrumentation with Flask
app = Flask(__name__)
FlaskInstrumentor().instrument_app(app)
RequestsInstrumentor().instrument()
```



[commit link]() shows adding the instrument honeycomb to app.py


- git commit -m "instrument honeycomb"

- git push 

**6. cd ../front-end**

```
npm i
```

**7. from root directory** 

```
docker compose up
```


**8. Update .gitpod.yml file to open ports**

```
ports:
  - name: frontend
    port: 3000
    onOpen: open-browser
    visibility: public
  - name: backend
    port: 4567
    visibility: public
  - name: xray-daemon
    port: 2000
    visibility: public
```

- git commit -m "adding ports to .gitpod.yml"

- git push 


- from backend URL, you will see this Raw Data, before you create a span

```
[
  {
    "created_at": "2023-02-27T22:15:03.577610+00:00",
    "expires_at": "2023-03-06T22:15:03.577610+00:00",
    "handle": "andrew Brown",
    "likes_count": 5,
    "message": "cloud is very fun!",
    "replies": [
      {
        "created_at": "2023-02-27T22:15:03.577610+00:00",
        "handle": "worf",
        "likes_count": 0,
        "message": "this post has no honor!",
        "replies_count": 0,
        "reply_to_activity_uuid": "68f126b0-1ceb-4a33-88be-d90fa7109eee",
        "reposts_count": 0,
        "uuid": "26e12864-1c26-5c3a-9658-97a10f8fea67"
      }
    ],
    "replies_count": 1,
    "reposts_count": 0,
    "uuid": "68f126b0-1ceb-4a33-88be-d90fa7109eee"
  },
  {
    "created_at": "2023-02-22T22:15:03.577610+00:00",
    "expires_at": "2023-03-10T22:15:03.577610+00:00",
    "handle": "worf",
    "likes": 0,
    "message": "i am out of prune juice",
    "replies": [],
    "uuid": "66e12864-8c26-4c3a-9658-95a10f8fea67"
  },
  {
    "created_at": "2023-03-01T21:15:03.577610+00:00",
    "expires_at": "2023-03-02T10:15:03.577610+00:00",
    "handle": "garek",
    "likes": 0,
    "message": "my dear doctor, I am just simple tailor",
    "replies": [],
    "uuid": "248959df-3079-4947-b847-9e0892d1bab4"
  }
]
```


**9. Create a custom span, added a tracer for this new span for home activities**

- Done as per [HoneyComb documentation](https://docs.honeycomb.io/getting-data-in/opentelemetry/python/)

- check to see your API Key

```
env|grep HONEY
```

- update - backend-flask/services/home_activities.py, with setting an attribute

- code demonstrates how OpenTelemetry can be used to trace and monitor a specific operation within an application by creating and updating spans with attributes and events

```
from opentelemetry import trace

tracer = trace.get_tracer("home.activities")

with tracer.start_as_current_span("home-activities-mock-data"):
      span = trace.get_current_span()
      now = datetime.now(timezone.utc).astimezone()
      span.set_attribute("app.now", now.isoformat())
```


- you will this Raw Data from the backend URL after you create a span

```
class HomeActivities:
  def run():
    with tracer.start_as_current_span("home-activities-mock-data"):
      span = trace.get_current_span()
      now = datetime.now(timezone.utc).astimezone()
      span.set_attribute("app.now", now.isoformat())
      results = [{
        'uuid': '68f126b0-1ceb-4a33-88be-d90fa7109eee',
        'handle':  'andrew Brown',
        'message': 'cloud is very fun!',
        'created_at': (now - timedelta(days=2)).isoformat(),
        'expires_at': (now + timedelta(days=5)).isoformat(),
        'likes_count': 5,
        'replies_count': 1,
        'reposts_count': 0,
        'replies': [{
          'uuid': '26e12864-1c26-5c3a-9658-97a10f8fea67',
          'reply_to_activity_uuid': '68f126b0-1ceb-4a33-88be-d90fa7109eee',
          'handle':  'worf',
          'message': 'this post has no honor!',
          'likes_count': 0,
          'replies_count': 0,
          'reposts_count': 0,
          'created_at': (now - timedelta(days=2)).isoformat()
      }],
    },
    {
      'uuid': '66e12864-8c26-4c3a-9658-95a10f8fea67',
      'handle':  'worf',
      'message': 'i am out of prune juice',
      'created_at': (now - timedelta(days=7)).isoformat(),
      'expires_at': (now + timedelta(days=9)).isoformat(),
      'likes': 0,
      'replies': []
    },
    {
      'uuid': '248959df-3079-4947-b847-9e0892d1bab4',
      'handle':  'garek',
      'message': 'my dear doctor, I am just simple tailor',
      'created_at': (now - timedelta(hours=1)).isoformat(),
      'expires_at': (now + timedelta(hours=12)).isoformat(),
      'likes': 0,
      'replies': []
    }
    ]
    span.set_attribute("app.result_length", len(results))    
    return results
```

- Next I ran custom query grouped by trace.trace_id and chose a trace fron a span and see the expected results.


#### Images

- [1-honeycomb-setup](https://github.com/omenking/aws-bootcamp-cruddur-2023/blob/week-2/journal/week2.md)

- [1-honeycomb-setup](https://github.com/omenking/aws-bootcamp-cruddur-2023/blob/week-2/journal/week2.md)

- [2-honeycomb-span](https://github.com/omenking/aws-bootcamp-cruddur-2023/blob/week-2/journal/week2.md)

- [3-honeycomb-trace](https://github.com/omenking/aws-bootcamp-cruddur-2023/blob/week-2/journal/week2.md)

- [4-honeycomb-home-activities-mock-data-trace]()

- [5-honeycomb-app-result_length]()

        
### 2. Week 2 Instrument XRay
        
**AWS Xray** is similar to Honeycomb. It uses distributed tracing to your applications in order for you to gain insights on latency and performance of your applications. 

- [Github - AWS X-Ray SDK Python](https://github.com/aws/aws-xray-sdk-python)

 **1. Installing AWS X-RAY SDK to the backend-end flask**
 
 - From root directory, cd to backend-flask and add this to the requirents.txt file (to install python dependencies)

```
aws-xray-sdk
```

- then run from backend-flask

```
pip install -r requirements.txt
```

**2. Add to app.py**

- backend-flask/app.py

```
# import X-Ray modules for running the application
from aws_xray_sdk.core import xray_recorder
from aws_xray_sdk.ext.flask.middleware import XRayMiddleware

# configures the X-Ray SDK with the retrieved X-Ray URL for service "backend-flask"
xray_url = os.getenv("AWS_XRAY_URL")
xray_recorder.configure(service='backend-flask', dynamic_naming=xray_url)

# Adding X-Ray middleware
XRayMiddleware(app, xray_recorder)
```

**3. Setup AWS X-Ray Resources - xray.json**
     
- aws/json/xray.json - Create a file xray.json 

```
{
    "SamplingRule": {
        "RuleName": "Cruddur",
        "ResourceARN": "*",
        "Priority": 9000,
        "FixedRate": 0.1,
        "ReservoirSize": 5,
        "ServiceName": "backend-flask",
        "ServiceType": "*",
        "Host": "*",
        "HTTPMethod": "*",
        "URLPath": "*",
        "Version": 1
    }
  }
```

**4. Configuring AWS X-Ray to create a group for the "Cruddur" and filtering traces for the "backend-flask" service**

- from backend-flask, run cli command

```
aws xray create-group \
   --group-name "Cruddur" \
   --filter-expression "service(\"backend-flask\")"
```

- not used

```
FLASK_ADDRESS="https://4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}" 
```    
  
- **Note: The X-Ray Trace Groups are under X-Ray > New Console > CloudWatch > Settings > Traces > View Settings > Groups**
- 
- PHOTO from AWS took it

**5. Configure the X-Ray sampling rules for the application with cli**

- run from aws/json

```
aws xray create-sampling-rule --cli-input-json file://xray.json
```

- OR

```
aws xray create-sampling-rule --cli-input-json file://aws/json/xray.json
```

- **Note: The Sampling rules are under CloudWatch > Settings > Traces > View Settings > Sampling rules**


- PHOTO took



**6. Adding X-Ray Daemon Container to Docker Compose**

- need to set up a container to host the xray daemon that will be tracing our application

- add to docker-compose.yml

```
# AWS X-Ray Daemon Container
# https://hub.docker.com/r/amazon/aws-xray-daemon
  xray-daemon:
      image: "amazon/aws-xray-daemon"
      environment:
        AWS_ACCESS_KEY_ID: "${AWS_ACCESS_KEY_ID}"
        AWS_SECRET_ACCESS_KEY: "${AWS_SECRET_ACCESS_KEY}"
        AWS_REGION: ""us-east-1"
      command:
        - xray -o -b "xray-daemon:2000"
      ports:
        - 2000:2000/udp
```

- add these two env vars to our backend-flask in our docker-compose.yml file

```
AWS_XRAY_URL: "*4567-${GITPOD_WORKSPACE_ID}.${GITPOD_WORKSPACE_CLUSTER_HOST}*"
AWS_XRAY_DAEMON_ADDRESS: "xray-daemon:2000"
```

- git commit -m "instrument x-ray"

- git push

[commit link]() shows adding the instrument x-ray to docker-compose.yml
     
**7. ran docker compose up -d**

- Before you can see the x-ray trace 

- Connect to the Back-end URL from the Gitpod in the ports tab by appending /api/activities/home

- take you to the API page that is hosting the backend for our home_activities.py

- Refresh it few times

- **then Go AWS Console, X-Ray > New Console > CloudWatch > Traces > Run Query**

- X-Ray trace appeared in AWS X-Ray console 
- 
- PHOTO not taken

- Click on a trace 


- PHOTO not taken


 
 ### 3. Week 2 CloudWatch Logs
 
**AWS Cloudwatch** logs our application's processes. In order for it to work with our application, we have to implemnt Watchtower. 

**Watchtower** has libraries for python applications which provide a handler that can be used to log events in your Python code, and then send those logs to CloudWatch for storage, analysis, and visualization.

**1.  Install WatchTower**

- in the backend-flask/requirements.txt, add

```
watchtower
```

- cd to backend-flask, run cli

```
pip install -r requirements.txt
```

**2. import the watchtower handler along with the following code**

-backend-flask/app.py - add

```
# CloudWatch Logs -----
import watchtower
import logging
from time import strftime


# CloudWatch --------
# Configuring Logger to Use CloudWatch
LOGGER = logging.getLogger(__name__)
LOGGER.setLevel(logging.DEBUG)
console_handler = logging.StreamHandler()
cw_handler = watchtower.CloudWatchLogHandler(log_group='cruddur')
LOGGER.addHandler(console_handler)
LOGGER.addHandler(cw_handler)
LOGGER.info("cruddur log")


# to collect logs for Cloudwatch errors 
@app.after_request to collect logs for errors
def after_request(response):
    timestamp = strftime('[%Y-%b-%d %H:%M]')
    LOGGER.error('%s %s %s %s %s %s', timestamp, request.remote_addr, request.method, request.scheme, request.full_path, response.status)
    return response



```


   
    
   

 
 
 
 
 
 
 ### 4. [Week 2 - Rollbar](https://www.youtube.com/watch?v=xMBDAb5SEU4)
 
 
 
      
      
      
      






