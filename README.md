# Instructions for running this project in the lab environment (Cloud IDE)

You have to carry out the steps and run the terminals in this exact order to ensure that there are no errors.
Start from the very first step of the very first terminal. Do not run the terminals in the wrong order.

Terminal 1 (Mongoose)
1. Clone the repository
2. cd /home/project/xrwvm-fullstack_developer_capstone/server/database
3. docker build . -t nodeapp
4. docker-compose up
5. Go to /xrwvm-fullstack_developer_capstone/server/djangoapp/.env
6. Ensure that backend_url is the correct URL for the current lab session
7. Ensure that there is no "/" at the very end of the backend_url
8. Test the URL port 3030 to see if Mongoose responds correctly

Terminal 2 (Frontend)
1. cd /home/project/xrwvm-fullstack_developer_capstone/server/frontend
2. npm install
3. npm run build

Terminal 3 (Code Engine, Sentiment Analyzer)
1. Start the Code Engine
2. Create a new project in Code Engine
3. Wait for the project creation process to complete then open the Code Engine CLI
4. If you open the Code Engine CLI before the project is properly created, it will show an error message
5. cd /home/project/xrwvm-fullstack_developer_capstone/server/djangoapp/microservices
6. docker build . -t us.icr.io/${SN_ICR_NAMESPACE}/senti_analyzer
7. docker push us.icr.io/${SN_ICR_NAMESPACE}/senti_analyzer
8. ibmcloud ce application create --name sentianalyzer --image us.icr.io/${SN_ICR_NAMESPACE}/senti_analyzer --registry-secret icr-secret --port 5000
9. The terminal should return a new URL for the sentiment analyzer app
10. Copy the URL
11. Go to /xrwvm-fullstack_developer_capstone/server/djangoapp/.env
12. Update the sentiment_analyzer_url with the URL you just copied
13. Ensure that there is a "/" at the very end of the sentiment_analyzer_url

Before running Terminal 4, do the following
1. Delete /xrwvm-fullstack_developer_capstone/server/djangoapp/__pycache__ folder (if exists)
2. Delete /xrwvm-fullstack_developer_capstone/server/db.sqlite3 (if exists)

Terminal 4 (Run server with manage.py)
1. cd /home/project/xrwvm-fullstack_developer_capstone/server
2. pip install virtualenv
3. virtualenv djangoenv
4. source djangoenv/bin/activate
5. python3 -m pip install -U -r requirements.txt
6. python3 manage.py makemigrations
7. python3 manage.py migrate --run-syncdb
8. python3 manage.py runserver

Terminal 4 (Run server with kubectl)
1. cd /home/project/xrwvm-fullstack_developer_capstone/server
2. MY_NAMESPACE=$(ibmcloud cr namespaces | grep sn-labs-)
3. echo $MY_NAMESPACE
4. docker build -t us.icr.io/$MY_NAMESPACE/dealership .
5. docker push us.icr.io/$MY_NAMESPACE/dealership
6. kubectl apply -f deployment.yaml
7. Wait for a short moment, if you run the next step too soon, then it will show an error
8. kubectl port-forward deployment.apps/dealership 8000:8000
