﻿## IntroduceThis program is for web server.Designed for compute-web-split mode, where web-server computer supplies limited computing power.Compute program: [Click here](https://github.com/oysw/dslocal.git)## Install### Step 1First, download the program:`git@github.com:oysw/dsweb.git`And open the django settings.py file in `dsweb/dsweb/settings.py`.Edit these:```DATABASES = {    'default': {        "ENGINE": 'django.db.backends.mysql',        "HOST": 'ip address(like 192.168.1.1)',        "PORT": 'port',        "NAME": 'data_cloud(whatever you like)',        "USER": 'root',        "PASSWORD": 'password',    }}```Note that this config refers to your compute-server where you install dslocal and mysql. HOST is your compute-server's ip address. PORT is the accessing port of mysql. NAME is the database of mysql you want to use (Empty database is recommanded).USER and PASSWORD is the remote mysql account you created which is accessible for dsweb.```DATA_CENTER = {    'hostname': 'ip address',    'username': 'username',    'port': 22,    'password': 'password'}```Simply because two programs use ssh to transport file. So compute-server required to install ssh server and this config refers to it.'hostname': The same of config above'username': Accessible remote ssh account for logining in.'port': Default 22.'password': Remote ssh account's password.### Step 2Now install python environment.Install mysql client first:`sudo apt install mysql-client-5.7 libmysqlclient-dev`Install python and pip:`sudo apt install python3 python3-pip virtualenv`Create a new environment and activate it:```cd /path/to/your/work/dirvirtualenv venv --python=python3source venv/bin/activate```Install required package:`pip install -r requirements.txt`### Step 3Start the django program in debug mode.Create database's tables:```python manage.py makemigrationspython manage.py migrate```Then just simply run by:```python manage.py runserver 0.0.0.0:8000```You can also use nginx for high performance.## How to use (Test)Before using, please deploy dslocal first.Enter the address like ip(webserver):8000 in your browser.Register your own account.Login and you will see page for uploading file. (x, y)We can prepare data like this (_Take f(x)=cos(x) function as an example._):```import numpy as npfrom sklearn.model_selection import train_test_splitdef f(x):    return np.cos(x)x = np.linspace(-10, 10, 100)y = f(x)x = x.reshape(-1, 1)X_train, X_test, y_train, y_test = train_test_split(x, y, test_size=0.3, random_state=10)np.save("X_train", X_train)np.save("y_train", y_train)np.save("X_test", X_test)```The size of training set x should looks like this (**2D!!!**):For example:```In [2]: xOut[2]: array([[1, 1, 1, 1, 1, 1],       [1, 1, 1, 1, 1, 2],       [1, 1, 1, 1, 1, 3],       ...,       [4, 4, 4, 3, 3, 1],       [4, 4, 4, 3, 3, 2],       [4, 4, 4, 3, 3, 3]])```The size of training set y should looks like this:```In [3]: yOut[3]: array([1, 1, 1, ..., 1, 3, 4])```Then submit `X_train.npy` and `y_train.npy`.Just few minutes later, you can find your result in result page (Enter this page by clicking result tag above the web page).The blue button shows the highest ranking model. You can click it and plot your own graph.Options detail is listed here:* Ttile: The title of graph you want to plot.* x_label: X axis's label (Invalid in 3D graph)* y_label: Y axis's label (Invalid in 3D graph)* Raw\_color: The scatter's color of original data (Like X_train.npy and y_train.npy)* Predict\_color: The scatter's color of data predicted by model.* Data for graph: You can either choose the data you uploaded or uploading new data for prediction.* Feature_N: Number of features you want to plot. Plot 2D graph if you choose 1, Plot 3D graph if you choose 2.* feature\_1 or feature\_2: Choose the column in your X\_train.npy or X\_test.npy you want to plot. Options would generate automatically.Click plot to generate you own graph!