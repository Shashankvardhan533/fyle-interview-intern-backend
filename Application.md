## Application 

There are 5 resources:
- Users
- Principal
- Students
- Teachers
- Assignments

5 Users (1 Principal, 2 students and 2 teachers) have already been created for you in the db fixture

- A principal can view all the teachers
- A principal can view all the assignments submitted and/or graded by teachers.
- A principal can re-grade the assignments already graded by the teacher.
- A student can create and edit a draft assignment
- A student can list all his created assignments
- A student can submit a draft assignment to a teacher
- A teacher can list all assignments submitted to him
- A teacher can grade an assignment submitted to him

## Challenge

Please fork the repository into your account and continue the development in your fork.

Your tasks
- Add missing APIs mentioned here and get the automated tests to pass
- Add tests for grading API
- Please be aware that intentional bugs have been incorporated into the application, leading to test failures. Kindly address and rectify these issues as part of the assignment.
- All tests should pass
- Get the test coverage to 94% or above
- There are certain SQL tests present inside `tests/SQL/`. You have to write SQL in following files:
    - count_grade_A_assignments_by_teacher_with_max_grading.sql
    - number_of_graded_assignments_for_each_student.sql
- Optionally, Dockerize your application by creating a Dockerfile and a docker-compose.yml file, providing clear documentation on building and running the application with Docker, to stand out in your submission

***Once you are done with your task, please use [this form](https://forms.gle/TbpVhYojG84cyUD77) to complete your submission.***

You will hear back within 48 hours from us via email. 


## Available APIs

### Auth
- header: "X-Principal"
- value: {"user_id":1, "student_id":1}

For APIs to work you need a principal header to establish identity and context

### GET /student/assignments

List all assignments created by a student
```
headers:
X-Principal: {"user_id":1, "student_id":1}

response:
{
    "data": [
        {
            "content": "ESSAY T1",
            "created_at": "2021-09-17T02:53:45.028101",
            "grade": null,
            "id": 1,
            "state": "SUBMITTED",
            "student_id": 1,
            "teacher_id": 1,
            "updated_at": "2021-09-17T02:53:45.034289"
        },
        {
            "content": "THESIS T1",
            "created_at": "2021-09-17T02:53:45.028876",
            "grade": null,
            "id": 2,
            "state": "DRAFT",
            "student_id": 1,
            "teacher_id": null,
            "updated_at": "2021-09-17T02:53:45.028882"
        }
    ]
}
```

### POST /student/assignments

Create an assignment
```
headers:
X-Principal: {"user_id":2, "student_id":2}

payload:
{
    "content": "some text"
}

response:
{
    "data": {
        "content": "some text",
        "created_at": "2021-09-17T03:14:08.572545",
        "grade": null,
        "id": 5,
        "state": "DRAFT",
        "student_id": 1,
        "teacher_id": null,
        "updated_at": "2021-09-17T03:14:08.572560"
    }
}
```

### POST /student/assignments

Edit an assignment
```
headers:
X-Principal: {"user_id":2, "student_id":2}

payload:
{
    "id": 5,
    "content": "some updated text"
}

response:
{
    "data": {
        "content": "some updated text",
        "created_at": "2021-09-17T03:14:08.572545",
        "grade": null,
        "id": 5,
        "state": "DRAFT",
        "student_id": 1,
        "teacher_id": null,
        "updated_at": "2021-09-17T03:15:06.349337"
    }
}
```

### POST /student/assignments/submit

Submit an assignment
```
headers:
X-Principal: {"user_id":1, "student_id":1}

payload:
{
    "id": 2,
    "teacher_id": 2
}

response:
{
    "data": {
        "content": "THESIS T1",
        "created_at": "2021-09-17T03:14:01.580467",
        "grade": null,
        "id": 2,
        "state": "SUBMITTED",
        "student_id": 1,
        "teacher_id": 2,
        "updated_at": "2021-09-17T03:17:20.147349"
    }
}

```
### GET /teacher/assignments

List all assignments submitted to this teacher
```
headers:
X-Principal: {"user_id":3, "teacher_id":1}

response:
{
    "data": [
        {
            "content": "ESSAY T1",
            "created_at": "2021-09-17T03:14:01.580126",
            "grade": null,
            "id": 1,
            "state": "SUBMITTED",
            "student_id": 1,
            "teacher_id": 1,
            "updated_at": "2021-09-17T03:14:01.584644"
        }
    ]
}
```

### POST /teacher/assignments/grade

Grade an assignment
```
headers:
X-Principal: {"user_id":3, "teacher_id":1}

payload:
{
    "id":  1,
    "grade": "A"
}

response:
{
    "data": {
        "content": "ESSAY T1",
        "created_at": "2021-09-17T03:14:01.580126",
        "grade": "A",
        "id": 1,
        "state": "GRADED",
        "student_id": 1,
        "teacher_id": 1,
        "updated_at": "2021-09-17T03:20:42.896947"
    }
}
```

## Missing APIs

You'll need to implement these APIs

### GET /principal/assignments

List all submitted and graded assignments
```
headers:
X-Principal: {"user_id":5, "principal_id":1}

response:
{
    "data": [
        {
            "content": "ESSAY T1",
            "created_at": "2021-09-17T03:14:01.580126",
            "grade": null,
            "id": 1,
            "state": "SUBMITTED",
            "student_id": 1,
            "teacher_id": 1,
            "updated_at": "2021-09-17T03:14:01.584644"
        }
    ]
}
```

### GET /principal/teachers

List all the teachers
```
headers:
X-Principal: {"user_id":5, "principal_id":1}


response:
{
    "data": [
        {
            "created_at": "2024-01-08T07:58:53.131970",
            "id": 1,
            "updated_at": "2024-01-08T07:58:53.131972",
            "user_id": 3
        }
    ]
}
```

### POST /principal/assignments/grade

Grade or re-grade an assignment
```
headers:
X-Principal: {"user_id":5, "principal_id":1}

payload:
{
    "id":  1,
    "grade": "A"
}

response:
{
    "data": {
        "content": "ESSAY T1",
        "created_at": "2021-09-17T03:14:01.580126",
        "grade": "A",
        "id": 1,
        "state": "GRADED",
        "student_id": 1,
        "teacher_id": 1,
        "updated_at": "2021-09-17T03:20:42.896947"
    }
}
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------
In models.py, define the necessary models for users and assignments.

python
# models.py
from flask_sqlalchemy import SQLAlchemy
from datetime import datetime

db = SQLAlchemy()

class User(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    username = db.Column(db.String(50), unique=True, nullable=False)
    role = db.Column(db.String(20))  # 'principal', 'student', 'teacher'
    created_at = db.Column(db.DateTime, default=datetime.utcnow)
    updated_at = db.Column(db.DateTime, default=datetime.utcnow, onupdate=datetime.utcnow)

class Assignment(db.Model):
    id = db.Column(db.Integer, primary_key=True)
    student_id = db.Column(db.Integer, db.ForeignKey('user.id'))
    teacher_id = db.Column(db.Integer, db.ForeignKey('user.id'))
    content = db.Column(db.Text)
    state = db.Column(db.String(20), default='DRAFT')  # DRAFT, SUBMITTED, GRADED
    grade = db.Column(db.String(2), nullable=True)  # e.g., 'A', 'B', etc.
    created_at = db.Column(db.DateTime, default=datetime.utcnow)
    updated_at = db.Column(db.DateTime, default=datetime.utcnow, onupdate=datetime.utcnow)
Step 3: Define Routes
In routes.py, implement the necessary APIs.

python
# routes.py
from flask import Flask, request, jsonify
from models import db, User, Assignment

app = Flask(__name__)
app.config['SQLALCHEMY_DATABASE_URI'] = 'sqlite:///school.db'  # Use your preferred database
db.init_app(app)

@app.route('/principal/teachers', methods=['GET'])
def get_teachers():
    teachers = User.query.filter_by(role='teacher').all()
    return jsonify({'data': [{'id': teacher.id, 'created_at': teacher.created_at, 'updated_at': teacher.updated_at, 'user_id': teacher.id} for teacher in teachers]})

@app.route('/principal/assignments', methods=['GET'])
def get_assignments():
    assignments = Assignment.query.all()
    return jsonify({'data': [{
        'id': a.id,
        'content': a.content,
        'created_at': a.created_at,
        'updated_at': a.updated_at,
        'state': a.state,
        'student_id': a.student_id,
        'teacher_id': a.teacher_id,
        'grade': a.grade
    } for a in assignments]})

@app.route('/principal/assignments/grade', methods=['POST'])
def grade_assignment_by_principal():
    data = request.json
    assignment = Assignment.query.get(data['id'])
    if not assignment:
        return jsonify({'message': 'Assignment not found!'}), 404
    
    assignment.grade = data['grade']
    assignment.state = 'GRADED'
    db.session.commit()
    
    return jsonify({'data': {
        'content': assignment.content,
        'created_at': assignment.created_at,
        'grade': assignment.grade,
        'id': assignment.id,
        'state': assignment.state,
        'student_id': assignment.student_id,
        'teacher_id': assignment.teacher_id,
        'updated_at': assignment.updated_at
    }})

@app.route('/student/assignments', methods=['GET'])
def list_student_assignments():
    principal_data = request.headers.get('X-Principal')
    student_id = principal_data['student_id']
    assignments = Assignment.query.filter_by(student_id=student_id).all()
    return jsonify({'data': [{'id': a.id, 'content': a.content, 'created_at': a.created_at, 'state': a.state, 'grade': a.grade} for a in assignments]})

@app.route('/student/assignments', methods=['POST'])
def create_assignment():
    principal_data = request.headers.get('X-Principal')
    student_id = principal_data['student_id']
    
    data = request.json
    new_assignment = Assignment(
        student_id=student_id,
        content=data['content'],
    )
    db.session.add(new_assignment)
    db.session.commit()
    
    return jsonify({'data': {
        'id': new_assignment.id,
        'content': new_assignment.content,
        'created_at': new_assignment.created_at,
        'state': new_assignment.state,
        'grade': new_assignment.grade,
        'student_id': student_id
    }}), 201

@app.route('/student/assignments/submit', methods=['POST'])
def submit_assignment():
    principal_data = request.headers.get('X-Principal')
    student_id = principal_data['student_id']
    
    data = request.json
    assignment = Assignment.query.get(data['id'])
    if not assignment or assignment.student_id != student_id:
        return jsonify({'message': 'Assignment not found or does not belong to student!'}), 404
    
    assignment.teacher_id = data['teacher_id']
    assignment.state = 'SUBMITTED'
    db.session.commit()
    
    return jsonify({'data': {
        'id': assignment.id,
        'content': assignment.content,
        'created_at': assignment.created_at,
        'state': assignment.state,
        'grade': assignment.grade,
        'student_id': assignment.student_id,
        'teacher_id': assignment.teacher_id
    }})

@app.route('/teacher/assignments', methods=['GET'])
def list_teacher_assignments():
    principal_data = request.headers.get('X-Principal')
    teacher_id = principal_data['teacher_id']
    
    assignments = Assignment.query.filter_by(teacher_id=teacher_id).all()
    return jsonify({'data': [{'id': a.id, 'content': a.content, 'created_at': a.created_at, 'state': a.state, 'grade': a.grade} for a in assignments]})

@app.route('/teacher/assignments/grade', methods=['POST'])
def grade_assignment():
    principal_data = request.headers.get('X-Principal')
    teacher_id = principal_data['teacher_id']
    
    data = request.json
    assignment = Assignment.query.get(data['id'])
    if not assignment or assignment.teacher_id != teacher_id:
        return jsonify({'message': 'Assignment not found or does not belong to teacher!'}), 404
    
    assignment.grade = data['grade']
    assignment.state = 'GRADED'
    db.session.commit()
    
    return jsonify({'data': {
        'content': assignment.content,
        'created_at': assignment.created_at,
        'grade': assignment.grade,
        'id': assignment.id,
        'state': assignment.state,
        'student_id': assignment.student_id,
        'teacher_id': assignment.teacher_id,
        'updated_at': assignment.updated_at
    }})
Step 4: SQL Queries
Create SQL files for the required queries in tests/SQL/.

count_grade_A_assignments_by_teacher_with_max_grading.sql
sql
SELECT teacher_id, COUNT(*) AS grade_A_count
FROM assignments
WHERE grade = 'A'
GROUP BY teacher_id
ORDER BY grade_A_count DESC;
number_of_graded_assignments_for_each_student.sql
sql
SELECT student_id, COUNT(*) AS graded_count
FROM assignments
WHERE grade IS NOT NULL
GROUP BY student_id;
Step 5: Testing
In tests/test_api.py, write tests for the APIs.

python
# tests/test_api.py
import pytest
from app import app, db, User, Assignment

@pytest.fixture
def client():
    app.config['TESTING'] = True
    with app.test_client() as client:
        with app.app_context():
            db.create_all()
            # Seed data
            principal = User(username='Principal', role='principal')
            student = User(username='Student', role='student')
            teacher = User(username='Teacher', role='teacher')
            db.session.add(principal)
            db.session.add(student)
            db.session.add(teacher)
            db.session.commit()
            yield client
            db.session.remove()
            db.drop_all()

def test_create_assignment(client):
    headers = {"X-Principal": '{"user_id": 2, "student_id": 1}'}
    response = client.post('/student/assignments', json={
        'content': 'New Assignment'
    }, headers=headers)
    assert response.status_code == 201
    assert b'New Assignment' in response.data

def test_grade_assignment(client):
    headers = {"X-Principal": '{"user_id": 3, "teacher_id": 1}'}
    response = client.post('/student/assignments', json={
        'content': 'New Assignment'
    }, headers={"X-Principal": '{"user_id": 2, "student_id": 1}'})
    assignment_id = response.get_json()['data']['id']

    response = client.post('/teacher/assignments/grade', json={
        'id': assignment_id,
        'grade': 'A'
    }, headers=headers)
    assert response.status_code == 200
    assert b'"grade": "A"' in response.data
Step 6: Dockerization
Dockerfile
dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt
COPY . .

CMD ["python", "app.py"]
docker-compose.yml
yaml
version: '3.8'
services:
  app:
    build: .
    ports:
      - "5000:5000"
    environment:
      - DATABASE_URL=sqlite:///school.db
Step 7: Documentation
In your README file, include instructions on how to build and run your application using Docker, as well as how to run tests.

Step 8: Running the Application
Build and run your application:

bash
docker-compose up --build
Run tests:

bash
pytest tests/
