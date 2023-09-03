# Deploying-Testing-API-To-Hosting-Server

![PythonAnywhere Logo](https://media.licdn.com/dms/image/C561BAQHDnw3jPc3HsA/company-background_10000/0/1588183934551/pythonanywhere_cover?e=1694044800&v=beta&t=dP2V2FL2fjghyXT6naRm1lZ7SbpGdIp6peNjJGneQaE)

## Welcome to PythonAnywhere 🚀

Are you ready to take your Python applications and websites to the clouds? 🌥️ Enter PythonAnywhere, your one-stop destination for hassle-free Python development, deployment, and web hosting. Say goodbye to server management woes and say hello to an integrated Python paradise!

### 🌐 What Exactly Is PythonAnywhere?

PythonAnywhere is not just a hosting platform; it's your launchpad into the Python cosmos. Here's what makes it incredible:

**Effortless Deployment:** Easily deploy Python applications, including dazzling web apps crafted with Flask, Django, and more.

**Web-Based Code Editors:** Edit your Python code directly in your browser, making quick changes a breeze.

**Python Console:** Instant access to a Python console for experimentation and debugging.

**Scheduled Tasks:** Automate Python tasks with scheduled executions – let your scripts work while you sleep!

**Database Magic:** Connect to databases like MySQL, PostgreSQL, and SQLite for seamless data storage and retrieval.

**Package Wonderland:** Expand your Python powers by installing additional packages using the built-in package manager.

### 🌟 Why PythonAnywhere Rocks?

PythonAnywhere supports a multitude of web technologies and frameworks, making it the perfect playground for a wide range of Python web development projects.

## Your Python Journey Starts Here

### Ready to Begin?

1. **Sign Up**: Create your PythonAnywhere account at [pythonanywhere.com](https://www.pythonanywhere.com/).

2. **Explore**: Dive into web-based code editors, the Python console, and other features to unleash your Python prowess.

3. **Deploy**: Launch your Python applications with ease – say hello to cloud-based hosting.

4. **Scale Up**: Focus on building and scaling your projects, leaving server management in the rearview mirror

### Follow below steps!

![PythonAnywhere Interface](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/d715dde0-f7ff-4c11-b1da-ee3a2cd493b0)

![PythonAnywhere Features](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/9457bf21-3f4a-4cd0-87a8-9bdbb1db7d3b)

![PythonAnywhere Database Support](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/f2ba9132-0115-4c83-93dc-4c1c66c1f746)

![PythonAnywhere File System](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/0bce4e90-7964-4d42-a728-8647a8a792d0)

![PythonAnywhere Community](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/a59cd8cc-d878-421f-b292-ee6b337485f8)

![PythonAnywhere Python Console](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/c14ea896-60d2-43ca-83e7-3dcc758985c7)

![PythonAnywhere Databases](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/2fe17db1-6121-42d1-aa0e-deef63c608e9)

![PythonAnywhere Task Scheduler](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/f5c852e3-217c-4c3b-85f3-8df7773c2a56)

![PythonAnywhere Code Editor](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/5e37db1f-5857-4229-8167-62e068e3b592)

![PythonAnywhere Project Management](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/e8328405-c938-4359-80fe-6e8ecac214d2)

![PythonAnywhere Python Packages](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/7b0bd521-3b74-4c2c-bea2-2860a2d964c4)

# Paste this API script into the flask_app.py file, change the database connections with your values, and don't forget to comment app.run() in the last line of the script
```Python
from flask import Flask, request, jsonify
import mysql.connector

app = Flask(__name__)

# MySQL configurations
db = mysql.connector.connect(
    host='[your_mysql_host]',
    user='[your_mysql_user]',
    password='[your_mysql_password]',
    database='[your_mysql_database]'
)

# Create a table in the database if it doesn't exist
def create_table():
    cur = db.cursor()
    cur.execute("CREATE TABLE IF NOT EXISTS temp_emp_3 (ID INT PRIMARY KEY AUTO_INCREMENT, Name VARCHAR(100), Email VARCHAR(100), Address VARCHAR(100))")
    db.commit()
    cur.close()

# API route to get all users
@app.route('/api/users', methods=['GET'])
def get_users():
    cur = db.cursor()
    cur.execute("SELECT * FROM temp_emp_3")
    users = cur.fetchall()
    cur.close()
    user_list = []
    for user in users:
        user_dict = {
            'id': user[0],
            'name': user[1],
            'email': user[2],
            'address': user[3]
        }
        user_list.append(user_dict)
    return jsonify(user_list)

# API route to create a new user
@app.route('/api/users/add', methods=['POST'])
def create_user():
    name = request.json.get('name')
    email = request.json.get('email')
    addr = request.json.get('address')
    cur = db.cursor()
    cur.execute("INSERT INTO temp_emp_3 (Name, Email, Address) VALUES (%s, %s, %s)", (name, email, addr))
    db.commit()
    cur.close()
    return jsonify({'message': 'User created successfully'})

# API route to update an existing user
@app.route('/api/users/update/<string:user_id>', methods=['PUT'])
def update_user(user_id):
    name = request.json.get('name')
    email = request.json.get('email')
    addr = request.json.get('address')
    cur = db.cursor()
    cur.execute("UPDATE temp_emp_3 SET Name = %s, Email = %s, Address = %s WHERE ID = %s", (name, email, addr, user_id))
    db.commit()
    cur.close()
    return jsonify({'message': 'User updated successfully'})

# API route to delete a user
@app.route('/api/users/delete/<int:user_id>', methods=['DELETE'])
def delete_user(user_id):
    cur = db.cursor()
    cur.execute("DELETE FROM temp_emp_3 WHERE ID = %s", (user_id,))
    db.commit()
    cur.close()
    return jsonify({'message': 'User deleted successfully'})

if __name__ == '__main__':
    create_table()
    # app.run()

```

![image](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/d17cfecf-dc38-450d-bc80-9173e0b3257c)
![image](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/6100d08f-3453-4b16-b268-ba95ed0d7669)
![image](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/8ab0c9cf-e1b3-4ee1-b3e1-ea48fe122efe)
![image](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/3ce29301-0294-4190-9270-5828871978f1)
![image](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/51bcd4ef-ce3a-474e-9daa-05548c48a5a1)
![image](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/a75a104a-394f-4b1d-ad78-117a04b02afa)
![image](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/e035777a-5e20-46c8-b825-6bf91af0262c)
![image](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/e56dfe94-30b4-4762-bcd6-86a49ce61fe2)
![image](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/a54f3405-b213-47da-8728-50dac1125681)
![image](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/f4707672-95be-445f-832f-c22642330f34)
![image](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/d6fcb2f2-9c08-4306-ab76-c84e3a09ac7f)
![image](https://github.com/MuhammadRaheelNaseem/Flask-API-Development/assets/63813881/8e20d293-e8e8-4547-a4ac-118861561b3e)


## Join Our Vibrant Community!

Explore the PythonAnywhere community, where Python aficionados unite to share knowledge, solve problems, and celebrate Python's awesomeness together!

## License

This project is licensed under the [MIT License](LICENSE). 

Ready to embark on your Python adventure with PythonAnywhere? Unleash the Python magic now!
