
# 📞 Contact List API – Django REST Framework

This is a simple, fully functional Django REST API that allows users to **Create, Read, Update, and Delete (CRUD)** contact records. It includes **token-based authentication**, so only authenticated users can create, update, or delete contacts, while anyone can view them.

---

## 📁 Features

- ✅ Full CRUD support
- ✅ Token Authentication
- ✅ Public `GET` access (anyone can view)
- ✅ Private `POST`, `PUT`, `PATCH`, `DELETE` (only logged-in users)
- ✅ Django REST Framework-powered

---

## 🔧 STEP 1: Create the Django Project and App

### 1. Create and activate a virtual environment

```bash
python3 -m venv contact_venv
source contact_venv/bin/activate
```

### 2. Install Django and Django REST Framework

```bash
pip install django djangorestframework
```

### 3. Create the Django project and app

```bash
django-admin startproject contact_project .
python manage.py startapp contacts
```

---

## 🧩 STEP 2: Configure Installed Apps

In `contact_project/settings.py`, add:

```python
INSTALLED_APPS = [
    ...
    'rest_framework',
    'rest_framework.authtoken',
    'contacts',
]
```

---

## 🔑 STEP 3: Set Up the Contact Model

In `contacts/models.py`:

```python
from django.db import models

class Contact(models.Model):
    first_name = models.CharField(max_length=100)
    last_name = models.CharField(max_length=100)
    email = models.EmailField()
    phone_number = models.CharField(max_length=20)
    address = models.TextField()

    def __str__(self):
        return f"{self.first_name} {self.last_name}"
```

Then run migrations:

```bash
python manage.py makemigrations
python manage.py migrate
```

---

## ⚙️ STEP 4: Create Serializers

In `contacts/serializers.py`:

```python
from rest_framework import serializers
from .models import Contact

class ContactSerializer(serializers.ModelSerializer):
    class Meta:
        model = Contact
        fields = '__all__'
```

---

## 🌐 STEP 5: Create Views and URLs

### In `contacts/views.py`:

```python
from rest_framework import viewsets, permissions
from .models import Contact
from .serializers import ContactSerializer

class ContactViewSet(viewsets.ModelViewSet):
    queryset = Contact.objects.all()
    serializer_class = ContactSerializer

    def get_permissions(self):
        if self.request.method in ['POST', 'PUT', 'PATCH', 'DELETE']:
            return [permissions.IsAuthenticated()]
        return [permissions.AllowAny()]
```

### In `contacts/urls.py`:

```python
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from .views import ContactViewSet

router = DefaultRouter()
router.register(r'contacts', ContactViewSet)

urlpatterns = [
    path('', include(router.urls)),
]
```

---

## 🛠️ STEP 6: Configure Main URLs and Token Auth

In `contact_project/urls.py`:

```python
from django.contrib import admin
from django.urls import path, include
from rest_framework.authtoken.views import obtain_auth_token

urlpatterns = [
    path('admin/', admin.site.urls),
    path('api/', include('contacts.urls')),
    path('api-token-auth/', obtain_auth_token),
]
```

---

## 🧪 STEP 7: Create a Superuser

```bash
python manage.py createsuperuser
```

Follow the prompt and remember your credentials.

---

## 🔐 STEP 8: Test the API Using Postman

### Get Token

`POST http://127.0.0.1:8000/api-token-auth/`

```json
{
  "username": "your_username",
  "password": "your_password"
}
```

Use the returned token in headers:

```
Authorization: Token your_token_here
```

Then you can:
- **GET**: `/api/contacts/`
- **POST**: `/api/contacts/`
- **PUT**/**PATCH**/**DELETE**: `/api/contacts/<id>/`

---

## 🚀 STEP 9: Push Project to GitHub

### 1. Create `.gitignore`

```bash
touch .gitignore
```

```gitignore
__pycache__/
*.pyc
*.sqlite3
contact_venv/
.env
.vscode/
```

### 2. Initialize Git and Push

```bash
git init
git add .
git commit -m "Initial commit: contact list API"
git remote add origin https://github.com/akumobi/contact-django-api.git
git branch -M main
git push -u origin main
```

---

## 📘 Optional Improvements

- ✅ Add JWT auth with SimpleJWT
- ✅ Register models in Django Admin
- ✅ Add pagination and filtering
- ✅ Deploy to Render, Railway, or Heroku

---

## 💡 Author

Built with 💙 by **Kingsley Akumobi**  
GitHub: [@akumobi](https://github.com/akumobi)

---

## 🧠 License

MIT — free to use, modify, and share.
