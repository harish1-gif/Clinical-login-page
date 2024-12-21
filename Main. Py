from flask import Flask, request, jsonify
from werkzeug.security import generate_password_hash, check_password_hash

app = Flask(__name__)

# In-memory database
users = {
    "joel87": {"password": generate_password_hash("pwjoel87"), "role": "Senior"},
    "shiminh": {"password": generate_password_hash("pwshiminh"), "role": "Junior"},
    "rishiaw": {"password": generate_password_hash("pwrishiaw"), "role": "Junior"},
}

cases = [
    {"name": "Jonathan Lim", "description": "A 28-year-old software engineer who is experiencing intense anxiety during team meetings and is struggling to speak up, fearing judgment from colleagues."},
    {"name": "Angela Paolo", "description": "A 42-year-old teacher who is coping with the recent loss of a parent and is finding it difficult to concentrate on work and daily responsibilities."},
    {"name": "Xu Yaoming", "description": "A 16-year-old high school student who is feeling overwhelmed by academic pressure and is struggling to balance schoolwork, extracurriculars, and personal time."}
]

# Routes
@app.route('/register', methods=['POST'])
def register():
    data = request.get_json()
    username = data.get('username')
    password = data.get('password')
    
    if username in users:
        return jsonify({"message": "Username already exists"}), 400
    
    users[username] = {"password": generate_password_hash(password), "role": "Junior"}
    return jsonify({"message": "User registered successfully"}), 201

@app.route('/login', methods=['POST'])
def login():
    data = request.get_json()
    username = data.get('username')
    password = data.get('password')
    
    user = users.get(username)
    if user and check_password_hash(user['password'], password):
        return jsonify({"message": f"Login successful, role: {user['role']}"}), 200
    
    return jsonify({"message": "Invalid username or password"}), 401

@app.route('/promote', methods=['POST'])
def promote():
    data = request.get_json()
    username = data.get('username')
    password = data.get('password')
    
    user = users.get(username)
    if user and check_password_hash(user['password'], password):
        if user['role'] == "Senior":
            return jsonify({"message": "User is already Senior"}), 400
        
        user['role'] = "Senior"
        return jsonify({"message": "User promoted to Senior"}), 200
    
    return jsonify({"message": "Invalid username or password"}), 401

@app.route('/demote', methods=['POST'])
def demote():
    data = request.get_json()
    username = data.get('username')
    password = data.get('password')
    
    user = users.get(username)
    if user and check_password_hash(user['password'], password):
        if user['role'] == "Junior":
            return jsonify({"message": "User is already Junior"}), 400
        
        user['role'] = "Junior"
        return jsonify({"message": "User demoted to Junior"}), 200
    
    return jsonify({"message": "Invalid username or password"}), 401

@app.route('/cases', methods=['GET'])
def get_cases():
    return jsonify({"cases": cases}), 200

@app.route('/case', methods=['POST'])
def add_case():
    data = request.get_json()
    name = data.get('name')
    description = data.get('description')
    
    if not name or not description:
        return jsonify({"message": "Name and description are required"}), 400
    
    cases.append({"name": name, "description": description})
    return jsonify({"message": "Case added successfully"}), 201

# Main entry point
if __name__ == '__main__':
    app.run(debug=True)
