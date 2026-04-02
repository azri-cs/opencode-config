# Django Project Guidelines

**For Python/Django Projects**

---

## Project Structure

```
project/
├── apps/
│   ├── core/
│   ├── users/
│   ├── api/
│   └── (feature apps)
├── core/
│   ├── settings/
│   │   ├── base.py
│   │   ├── local.py
│   │   ├── production.py
│   │   └── test.py
│   ├── urls.py
│   ├── wsgi.py
│   └── asgi.py
├── manage.py
├── requirements/
│   ├── base.txt
│   ├── local.txt
│   ├── production.txt
│   └── test.txt
├── templates/
├── static/
├── media/
├── locale/
├── logs/
└── tests/
```

## Architecture Patterns

### Django Apps Structure
```python
# apps/users/
users/
├── __init__.py
├── admin.py
├── apps.py
├── forms.py
├── managers.py
├── models.py
├── signals.py
├── tasks.py
├── tests/
│   ├── __init__.py
│   ├── test_models.py
│   ├── test_views.py
│   └── test_serializers.py
├── urls.py
└── views/
    ├── __init__.py
    ├── api.py
    └── mixins.py
```

### Model Managers & Querysets
```python
# Use custom managers for reusable query logic
class UserManager(models.Manager):
    def get_queryset(self):
        return super().get_queryset().filter(is_active=True)
    
    def create_user(self, email, password=None, **extra_fields):
        # Custom creation logic
        pass

class User(AbstractUser):
    objects = UserManager()
    active = UserManager()
    
    class Meta:
        verbose_name = 'User'
        verbose_name_plural = 'Users'
```

### Service Layer
```python
# services/user_service.py
class UserService:
    def __init__(self, user_repository):
        self.repository = user_repository
    
    def register_user(self, data: dict) -> User:
        # Business logic
        pass
    
    def update_profile(self, user: User, data: dict) -> User:
        # Validation and update logic
        pass
```

### API Viewsets (Django REST Framework)
```python
# views/api/user_viewset.py
from rest_framework import viewsets, permissions
from .serializers import UserSerializer
from apps.users.models import User

class UserViewSet(viewsets.ModelViewSet):
    queryset = User.objects.all()
    serializer_class = UserSerializer
    permission_classes = [permissions.IsAuthenticated]
    
    def get_queryset(self):
        queryset = super().get_queryset()
        
        # Filter by active status
        is_active = self.request.query_params.get('active')
        if is_active is not None:
            queryset = queryset.filter(is_active=is_active.lower() == 'true')
        
        return queryset
    
    def perform_create(self, serializer):
        serializer.save(created_by=self.request.user)
```

## Code Style & Standards

### PEP 8 Compliance
- Follow PEP 8 style guide
- Use Black for formatting
- Use isort for import sorting
- Enable type hints throughout

### Type Hints
```python
from typing import Optional, List
from django.db.models import QuerySet

def get_active_users() -> QuerySet[User]:
    return User.objects.filter(is_active=True)

def create_user(email: str, name: str, password: str) -> User:
    user = User(email=email, name=name)
    user.set_password(password)
    user.save()
    return user
```

### Docstring Standards
```python
class UserService:
    """Service class for user-related business logic."""
    
    def get_user_by_email(self, email: str) -> Optional[User]:
        """
        Retrieve a user by their email address.
        
        Args:
            email: The user's email address
            
        Returns:
            User instance if found, None otherwise
            
        Raises:
            ValidationError: If email format is invalid
        """
        pass
```

## Database & Models

### Model Best Practices
```python
# models/user.py
from django.db import models
from django.contrib.auth.models import AbstractUser
from django.utils.translation import gettext_lazy as _

class User(AbstractUser):
    email = models.EmailField(_('email address'), unique=True)
    phone = models.CharField(max_length=20, blank=True)
    bio = models.TextField(max_length=500, blank=True)
    birth_date = models.DateField(null=True, blank=True)
    is_active = models.BooleanField(default=True)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    USERNAME_FIELD = 'email'
    REQUIRED_FIELDS = ['name']
    
    class Meta:
        verbose_name = _('User')
        verbose_name_plural = _('Users')
        indexes = [
            models.Index(fields=['email']),
            models.Index(fields=['created_at']),
        ]
    
    def __str__(self):
        return self.email
    
    def get_full_name(self) -> str:
        return self.name or self.email
```

### Migrations
```python
# migrations/0001_initial.py
from django.db import migrations, models
import django.utils.timezone

class Migration(migrations.Migration):
    atomic = False
    
    operations = [
        migrations.CreateModel(
            name='User',
            fields=[
                ('id', models.BigAutoField(auto_created=True, primary_key=True, serialize=False, verbose_name='ID')),
                ('password', models.CharField(max_length=128)),
                ('last_login', models.DateTimeField(blank=True, null=True)),
                ('is_superuser', models.BooleanField(default=False)),
                ('email', models.EmailField(max_length=254, unique=True)),
                ('name', models.CharField(max_length=150)),
                ('is_active', models.BooleanField(default=True)),
                ('created_at', models.DateTimeField(auto_now_add=True)),
                ('updated_at', models.DateTimeField(auto_now=True)),
            ],
            options={
                'abstract': False,
            },
        ),
    ]
```

### Query Optimization
```python
# Use select_related for ForeignKey
users = User.objects.select_related('profile', 'department').all()

# Use prefetch_related for ManyToMany
users = User.objects.prefetch_related('roles', 'permissions').all()

# Use only() to limit fields
users = User.objects.only('id', 'email', 'name')

# Use defer() to exclude large fields
users = User.objects.defer('bio', 'settings')

# Use annotate() for aggregations
from django.db.models import Count
users = User.objects.annotate(post_count=Count('posts'))
```

## Testing

### Pytest Configuration
```python
# pytest.ini
[pytest]
DJANGO_SETTINGS_MODULE = core.settings.test
python_files = test_*.py
python_classes = Test*
python_functions = test_*
addopts = -v --tb=short
filterwarnings =
    ignore::DeprecationWarning
    ignore::PendingDeprecationWarning
```

### Test Examples
```python
# tests/test_models.py
import pytest
from django.urls import reverse
from apps.users.models import User

@pytest.mark.django_db
class TestUserModel:
    def test_create_user(self):
        user = User.objects.create_user(
            email='test@example.com',
            password='testpass123',
            name='Test User'
        )
        assert user.email == 'test@example.com'
        assert user.name == 'Test User'
        assert user.check_password('testpass123')
    
    def test_create_user_without_email_raises_error(self):
        with pytest.raises(ValueError):
            User.objects.create_user(email='')
    
    def test_user_str_representation(self):
        user = User.objects.create_user(
            email='test@example.com',
            password='testpass123',
            name='Test User'
        )
        assert str(user) == 'test@example.com'
```

### Test Coverage Goals
- Minimum 80% on business logic
- 100% on Models, Serializers, Views
- Integration tests for API endpoints
- Coverage reports with pytest-cov

## Django Management Commands

### Common Commands
```bash
# Development
python manage.py runserver
python manage.py shell
python manage.py shell_plus

# Database
python manage.py migrate
python manage.py migrate --fake-initial
python manage.py makemigrations
python manage.py showmigrations
python manage.py dbshell

# Testing
python manage.py test
python manage.py test --coverage
pytest

# Code Quality
python manage.py check
flake8 .
black --check .
isort --check-only .
mypy .
```

## Security Best Practices

### Django Security Settings
```python
# settings/production.py
SECURITY_MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

# Enable security features
SECURE_SSL_REDIRECT = True
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True
SECURE_BROWSER_XSS_FILTER = True
X_FRAME_OPTIONS = 'DENY'

# Password hashing
PASSWORD_HASHERS = [
    'django.contrib.auth.hashers.Argon2PasswordHasher',
    'django.contrib.auth.hashers.PBKDF2PasswordHasher',
]
```

### SQL Injection Prevention
- Use Django ORM (automatically sanitized)
- Never use `extra()` with user input
- Use parameterized queries in raw SQL

### XSS Prevention
- Use template escaping ({{ var }})
- Use `mark_safe()` only for trusted content
- Enable Django's XSS middleware

### Authentication
- Use Django REST Framework authentication classes
- Implement proper session management
- Use token authentication for APIs
- Implement rate limiting

## Performance Optimization

### Database Optimization
```python
# Use database-level optimizations
from django.db import connection

# Monitor query count
print(connection.queries)

# Use select_related/prefetch_related
# Implement database indexing
# Use read replicas for read-heavy operations
```

### Caching
```python
# settings/base.py
CACHES = {
    'default': {
        'BACKEND': 'django.core.cache.backends.redis.RedisCache',
        'LOCATION': 'redis://127.0.0.1:6379/1',
        'KEY_PREFIX': 'myproject',
    }
}

# Usage in code
from django.core.cache import cache

def get_user(user_id):
    key = f'user_{user_id}'
    user = cache.get(key)
    if user is None:
        user = User.objects.get(id=user_id)
        cache.set(key, user, 3600)  # 1 hour
    return user
```

### Async Tasks with Celery
```python
# tasks.py
from celery import shared_task
from django.core.mail import send_mail

@shared_task
def send_registration_email(user_id):
    user = User.objects.get(id=user_id)
    send_mail(
        'Welcome',
        'Thank you for registering.',
        'noreply@example.com',
        [user.email],
        fail_silently=False,
    )
```

## API Design

### Django REST Framework
```python
# serializers.py
from rest_framework import serializers
from apps.users.models import User

class UserSerializer(serializers.ModelSerializer):
    class Meta:
        model = User
        fields = ['id', 'email', 'name', 'created_at']
        read_only_fields = ['id', 'created_at']
    
    def validate_email(self, value):
        if User.objects.filter(email=value).exists():
            raise serializers.ValidationError("Email already exists")
        return value
```

### Response Format
```python
# Standard response wrapper
def success_response(data=None, message=None, status_code=200):
    response_data = {
        'success': True,
        'data': data,
        'message': message,
    }
    return Response(response_data, status=status_code)
```

### Error Response Format
```python
{
    "success": false,
    "error": {
        "code": "VALIDATION_ERROR",
        "message": "The given data was invalid",
        "details": {
            "email": ["This field is required."]
        }
    }
}
```

## Additional Resources

- [Django Documentation](https://docs.djangoproject.com/)
- [Django REST Framework](https://www.django-rest-framework.org/)
- [PEP 8 Style Guide](https://pep8.org/)
- [Django Best Practices](https://docs.djangoproject.com/en/stable/topics/)
- [OWASP Django Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Django_Cheat_Sheet.html)
