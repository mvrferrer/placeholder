
# # Flask Cookbook Project

## Overview
This project is a **Flask-based backend** designed to support a **cookbook application**. It is deployed on a **VPS** and integrated with a **Cloudflare Pages frontend**. The backend serves API requests and is built to handle future AI enhancements, such as **Mistral 7B**.

## Features
- Flask API for handling requests
- Hosted on a VPS for full control
- Integrated with Cloudflare Pages
- Future AI integration planned

## Deployment Setup
### **1. VPS Setup**
Ensure you have a VPS with SSH access. Connect via:
```sh
ssh root@your-vps-ip
```
Update system packages:
```sh
sudo apt update && sudo apt upgrade -y
```
Install dependencies:
```sh
sudo apt install python3 python3-venv python3-pip nginx git ufw -y
```

### **2. Clone the Repository**
Move to the appropriate directory and clone the project:
```sh
cd /var/www
git clone https://github.com/your-username/your-repo.git flask-app
cd flask-app
```

### **3. Virtual Environment & Dependencies**
Create and activate a virtual environment:
```sh
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

### **4. Running Flask Locally for Testing**
Modify `app.py` to allow external connections:
```python
if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```
Run Flask:
```sh
python app.py
```
Test in your browser:
```
http://your-vps-ip:5000
```

### **5. Deploying Flask with Gunicorn & Nginx**
Install Gunicorn:
```sh
pip install gunicorn
```
Run Flask using Gunicorn:
```sh
gunicorn --bind 0.0.0.0:5000 app:app
```

Set up Nginx as a reverse proxy:
```sh
sudo nano /etc/nginx/sites-available/flask
```
Paste this:
```
server {
    listen 80;
    server_name your-vps-ip;
    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```
Save and exit. Enable the configuration:
```sh
sudo ln -s /etc/nginx/sites-available/flask /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```
Now, Flask should be accessible at:
```
http://your-vps-ip
```

### **6. Cloudflare Integration**
- Set up a domain in **Cloudflare Pages**.
- Update **DNS settings** to point your domain to your VPS.
- Optionally, set up **SSL (HTTPS)** with Cloudflare.

## Future Enhancements
- AI-powered recipe recommendations
- User authentication system
- Database integration for recipe storage

## Contributing
Pull requests are welcome! Please ensure your code is well-documented and adheres to best practices.

## License
This project is open-source and licensed under the MIT License.

