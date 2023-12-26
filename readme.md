![](BHP_website.PNG)

This data science project series walks through step by step process of how to build a real estate price prediction website. i first built a model using sklearn and linear regression using banglore home prices dataset from kaggle.com. Second step was to write a python flask server that uses the saved model to serve http requests. Third component was to built website in html, css and javascript that allows user to enter home square ft area, bedrooms etc and it will call python flask server to retrieve the predicted price. During this project i covered almost all data science concepts such as data load and cleaning, outlier detection and removal, feature engineering, dimensionality reduction, gridsearchcv for hyperparameter tunning, k fold cross validation, software development covered front and backend, utilization of AWS services to deploy the model etc. Technology and  tools wise this project covers,

1. Python
2. Numpy and Pandas for data cleaning
3. Matplotlib for data visualization
4. Sklearn for model building
5. Jupyter notebook, visual studio code and pycharm as IDE
6. Python flask for http server
7. HTML/CSS/Javascript for UI

# Deploy this app to cloud (AWS EC2)

1. Created EC2 instance using amazon console, also in security group add a rule to allow HTTP incoming traffic
2. Connected to the instance using a command like this,
```
ssh -i "C:\Users\Viral\.ssh\Banglore.pem" ubuntu@ec2-3-133-88-210.us-east-2.compute.amazonaws.com
```
3. nginx setup
   1. Install nginx on EC2 instance using these commands,
   ```
   sudo apt-get update
   sudo apt-get install nginx
   ```
   2. Above commands installed nginx as well as run it. Check status of nginx using
   ```
   sudo service nginx status
   ```
   3. Here are the commands to start/stop/restart nginx
   ```
   sudo service nginx start
   sudo service nginx stop
   sudo service nginx restart
   ```
   4. In browser a message saying "welcome to nginx" This means nginx is setup and running.
4. Then i needed to copy all my code to EC2 instance. I can do this either using git or copy files using winscp. I used winscp. Download winscp from here: https://winscp.net/eng/download.php
5. Once i get connect to EC2 instance from winscp, i copied all code files into /home/ubuntu/ folder. The full path of my root folder is now: **/home/ubuntu/BangloreHomePrices**
6.  After copyied code on EC2 server then i pointed nginx to load property website by default. For below steps,
    1. Created this file /etc/nginx/sites-available/bhp.conf. The file content looks like this,
    ```
    server {
	    listen 80;
            server_name bhp;
            root /home/ubuntu/BangloreHomePrices/client;
            index app.html;
            location /api/ {
                 rewrite ^/api(.*) $1 break;
                 proxy_pass http://127.0.0.1:5000;
            }
    }
    ```
    2. Created symlink for this file in /etc/nginx/sites-enabled by running this command,
    ```
    sudo ln -v -s /etc/nginx/sites-available/bhp.conf
    ```
    3. Removed symlink for default file in /etc/nginx/sites-enabled directory,
    ```
    sudo unlink default
    ```
    4. Restarted nginx,
    ```
    sudo service nginx restart
    ```
7. Installed python packages and start flask server
```
sudo apt-get install python3-pip
sudo pip3 install -r /home/ubuntu/BangloreHomePrices/requirements.txt
python3 /home/ubuntu/BangloreHomePrices/client/server.py
```
Running last command above prompt that server is running on port 5000.
8. load cloud url in browser and got fully functional website running in production cloud environment.



