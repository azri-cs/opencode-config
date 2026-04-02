---
description: Migrate code or functionality between frameworks or versions
agent: legacy-modernizer
temperature: 0.2
---

Migrate code between frameworks or versions based on the requirements provided as $ARGUMENTS.

**Current Analysis:**
!`find . -type f \( -name "*.php" -o -name "*.py" -o -name "*.js" -o -name "*.ts" \) ! -path "./vendor/*" ! -path "./node_modules/*" ! -path "./.git/*" | head -20`

!`cat composer.json 2>/dev/null | grep -E "laravel|php" || echo "No Laravel/PHP found"`
!`cat package.json 2>/dev/null | grep -E "react|django|python" || echo "No React/Django found"`

## Migration Plan

### Migration Overview
- **Source**: [current framework/version]
- **Target**: [target framework/version]
- **Scope**: [partial/full migration]
- **Estimated Effort**: [X days/weeks]

### Migration Scope
**In Scope**:
- [Component/feature 1]
- [Component/feature 2]
- [Component/feature 3]

**Out of Scope**:
- [Component/feature 1]
- [Component/feature 2]

---

## Migration Strategy

### Phase 1: Analysis & Planning

#### 1.1 Code Inventory
**Files to Migrate**:
| File | Complexity | Dependencies | Priority |
|------|------------|--------------|----------|
| [file1] | [High/Med/Low] | [deps] | [P0/P1/P2] |
| [file2] | [High/Med/Low] | [deps] | [P0/P1/P2] |
| [file3] | [High/Med/Low] | [deps] | [P0/P1/P2] |

#### 1.2 Dependency Mapping
| Source Package | Target Package | Notes |
|----------------|----------------|-------|
| [package1] | [target1] | [notes] |
| [package2] | [target2] | [notes] |
| [package3] | [target3] | [notes] |

#### 1.3 Pattern Conversion
**Laravel to Django Mapping**:
| Laravel Pattern | Django Equivalent |
|-----------------|-------------------|
| Eloquent Models | Django Models |
| Blade Templates | Django Templates |
| Controllers | Views (Class-based) |
| Routes | URL Configuration |
| Middleware | Decorators |
| Artisan Commands | Management Commands |
| Migrations | Migrations |
| Service Container | Dependency Injection (manual) |

**React to Vue Mapping**:
| React Pattern | Vue Equivalent |
|---------------|----------------|
| useState | ref, reactive |
| useEffect | watch, onMounted |
| useContext | provide/inject |
| useCallback | computed |
| useMemo | computed |
| useReducer | redux (with vuex) |
| Components | Single File Components |
| Props | Props |
| State Management | Vuex/Pinia |

---

## Detailed Migration Steps

### Step 1: Project Setup

**Target Project Structure**:
```
[target-project]/
├── [app]/
│   ├── [module1]/
│   │   ├── models/
│   │   ├── views/
│   │   ├── urls.py
│   │   └── admin.py
│   └── [module2]/
├── core/
│   ├── settings/
│   ├── urls.py
│   └── wsgi.py
├── tests/
├── manage.py
└── requirements.txt
```

**Configuration**:
```python
# settings/base.py
import os
from pathlib import Path

BASE_DIR = Path(__file__).resolve().parent.parent

SECRET_KEY = os.environ.get('DJANGO_SECRET_KEY')

DEBUG = os.environ.get('DEBUG', 'False') == 'True'

ALLOWED_HOSTS = ['*']

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    '[app_name]',
]

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

### Step 2: Data Model Migration

**Laravel Model**:
```php
// Laravel Model
namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Post extends Model
{
    protected $fillable = [
        'title',
        'content',
        'user_id',
        'status',
    ];
    
    protected $casts = [
        'published_at' => 'datetime',
    ];
    
    public function user()
    {
        return $this->belongsTo(User::class);
    }
    
    public function comments()
    {
        return $this->hasMany(Comment::class);
    }
    
    public function scopePublished($query)
    {
        return $query->where('status', 'published');
    }
}
```

**Django Model**:
```python
# Django Model
from django.db import models
from django.conf import settings
from django.utils import timezone

class Post(models.Model):
    STATUS_CHOICES = [
        ('draft', 'Draft'),
        ('published', 'Published'),
    ]
    
    title = models.CharField(max_length=255)
    content = models.TextField()
    user = models.ForeignKey(
        settings.AUTH_USER_MODEL,
        on_delete=models.CASCADE,
        related_name='posts'
    )
    status = models.CharField(
        max_length=20,
        choices=STATUS_CHOICES,
        default='draft'
    )
    published_at = models.DateTimeField(null=True, blank=True)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    
    class Meta:
        ordering = ['-created_at']
        indexes = [
            models.Index(fields=['status', 'created_at']),
        ]
    
    def __str__(self):
        return self.title
    
    @property
    def is_published(self):
        return self.status == 'published'
    
    def publish(self):
        self.status = 'published'
        self.published_at = timezone.now()
        self.save()
    
    @classmethod
    def published(cls):
        return cls.objects.filter(status='published')
```

### Step 3: Controller/View Migration

**Laravel Controller**:
```php
// Laravel Controller
namespace App\Http\Controllers\Api;

use App\Models\Post;
use Illuminate\Http\Request;
use App\Http\Resources\PostResource;

class PostController extends Controller
{
    public function index(Request $request)
    {
        $posts = Post::with('user')
            ->published()
            ->when($request->user_id, function ($query, $userId) {
                $query->where('user_id', $userId);
            })
            ->orderBy('created_at', 'desc')
            ->paginate(10);
        
        return PostResource::collection($posts);
    }
    
    public function store(Request $request)
    {
        $validated = $request->validate([
            'title' => 'required|max:255',
            'content' => 'required',
        ]);
        
        $post = $request->user()
            ->posts()
            ->create($validated);
        
        return new PostResource($post);
    }
    
    public function show(Post $post)
    {
        return new PostResource($post->load('user', 'comments'));
    }
    
    public function update(Request $request, Post $post)
    {
        $this->authorize('update', $post);
        
        $validated = $request->validate([
            'title' => 'sometimes|required|max:255',
            'content' => 'sometimes|required',
        ]);
        
        $post->update($validated);
        
        return new PostResource($post);
    }
    
    public function destroy(Post $post)
    {
        $this->authorize('delete', $post);
        
        $post->delete();
        
        return response()->noContent();
    }
}
```

**Django ViewSet**:
```python
# Django ViewSet
from rest_framework import viewsets, permissions, filters
from django.db.models import Q
from .models import Post
from .serializers import PostSerializer

class PostViewSet(viewsets.ModelViewSet):
    queryset = Post.objects.select_related('user').all()
    serializer_class = PostSerializer
    permission_classes = [permissions.IsAuthenticatedOrReadOnly]
    filter_backends = [filters.OrderingFilter, filters.SearchFilter]
    ordering_fields = ['created_at', 'title']
    search_fields = ['title', 'content']
    
    def get_queryset(self):
        queryset = super().get_queryset()
        
        # Filter by status (published only for unauthenticated)
        if not self.request.user.is_authenticated:
            queryset = queryset.filter(status='published')
        
        # Filter by user if provided
        user_id = self.request.query_params.get('user_id')
        if user_id:
            queryset = queryset.filter(user_id=user_id)
        
        # Custom scope: published
        is_published = self.request.query_params.get('published')
        if is_published and is_published.lower() == 'true':
            queryset = queryset.filter(status='published')
        
        return queryset.order_by('-created_at')
    
    def perform_create(self, serializer):
        serializer.save(user=self.request.user)
    
    def get_serializer_context(self):
        return {'request': self.request}
```

### Step 4: Route Migration

**Laravel Routes**:
```php
// routes/api.php
Route::middleware(['auth:api'])->group(function () {
    Route::apiResources([
        'posts' => PostController::class,
    ]);
    
    Route::get('posts/{post}/comments', function (Post $post) {
        return $post->comments()->paginate(10);
    });
});
```

**Django URLs**:
```python
# urls.py
from django.urls import path, include
from rest_framework.routers import DefaultRouter
from .views import PostViewSet

router = DefaultRouter()
router.register(r'posts', PostViewSet, basename='post')

urlpatterns = [
    path('', include(router.urls)),
]

# urls.py (app-level)
from . import views

app_name = '[app_name]'

urlpatterns = [
    path('', views.PostViewSet.as_view({
        'get': 'list',
        'post': 'create',
    }), name='post-list'),
    
    path('<int:pk>/', views.PostViewSet.as_view({
        'get': 'retrieve',
        'put': 'update',
        'patch': 'partial_update',
        'delete': 'destroy',
    }), name='post-detail'),
]
```

### Step 5: Template/Component Migration

**Laravel Blade**:
```blade
<!-- resources/views/posts/index.blade.php -->
@extends('layouts.app')

@section('content')
<div class="container">
    <h1>Posts</h1>
    
    @foreach($posts as $post)
        <article class="post-card">
            <h2>{{ $post->title }}</h2>
            <p>{{ $post->excerpt }}</p>
            <span>By {{ $post->user->name }}</span>
            <time>{{ $post->published_at->format('F j, Y') }}</time>
        </article>
    @endforeach
    
    {{ $posts->links() }}
</div>
@endsection
```

**React Component**:
```tsx
// components/PostList.tsx
import React from 'react';
import { Link } from 'react-router-dom';
import { formatDate } from '../utils/date';

interface Post {
  id: string;
  title: string;
  excerpt: string;
  user: {
    name: string;
  };
  publishedAt: string;
}

interface PostListProps {
  posts: Post[];
}

export const PostList: React.FC<PostListProps> = ({ posts }) => {
  return (
    <div className="container">
      <h1>Posts</h1>
      
      {posts.map((post) => (
        <article key={post.id} className="post-card">
          <h2>
            <Link to={`/posts/${post.id}`}>
              {post.title}
            </Link>
          </h2>
          <p>{post.excerpt}</p>
          <div className="post-meta">
            <span>By {post.user.name}</span>
            <time>{formatDate(post.publishedAt)}</time>
          </div>
        </article>
      ))}
    </div>
  );
};
```

### Step 6: Test Migration

**Laravel Test**:
```php
// tests/Feature/PostTest.php
namespace Tests\Feature;

use App\Models\User;
use App\Models\Post;
use Tests\TestCase;
use Illuminate\Foundation\Testing\RefreshDatabase;

class PostTest extends TestCase
{
    use RefreshDatabase;
    
    public function test_user_can_create_post()
    {
        $user = User::factory()->create();
        
        $response = $this->actingAs($user)
            ->postJson('/api/posts', [
                'title' => 'Test Post',
                'content' => 'Test content',
            ]);
        
        $response->assertStatus(201)
            ->assertJsonStructure([
                'data' => [
                    'id',
                    'title',
                    'content',
                ],
            ]);
        
        $this->assertDatabaseHas('posts', [
            'title' => 'Test Post',
            'user_id' => $user->id,
        ]);
    }
}
```

**Django Test**:
```python
# tests/test_posts.py
from django.test import TestCase
from django.urls import reverse
from rest_framework.test import APITestCase
from rest_framework import status
from .models import Post
from django.contrib.auth import get_user_model

User = get_user_model()

class PostAPITest(APITestCase):
    def setUp(self):
        self.user = User.objects.create_user(
            email='test@example.com',
            password='testpass123'
        )
    
    def test_user_can_create_post(self):
        self.client.force_authenticate(user=self.user)
        
        data = {
            'title': 'Test Post',
            'content': 'Test content',
        }
        
        response = self.client.post('/api/posts/', data, format='json')
        
        self.assertEqual(response.status_code, status.HTTP_201_CREATED)
        self.assertEqual(Post.objects.count(), 1)
        self.assertEqual(Post.objects.first().title, 'Test Post')
        self.assertEqual(Post.objects.first().user, self.user)
    
    def test_unauthenticated_user_cannot_create_post(self):
        data = {
            'title': 'Test Post',
            'content': 'Test content',
        }
        
        response = self.client.post('/api/posts/', data, format='json')
        
        self.assertEqual(response.status_code, status.HTTP_401_UNAUTHORIZED)
```

---

## Migration Checklist

### Pre-Migration
- [ ] Complete code inventory
- [ ] Map dependencies
- [ ] Set up target environment
- [ ] Create backup strategy
- [ ] Define success criteria

### During Migration
- [ ] Set up project structure
- [ ] Migrate database models
- [ ] Migrate business logic
- [ ] Migrate API endpoints
- [ ] Migrate frontend components
- [ ] Migrate tests
- [ ] Update documentation

### Post-Migration
- [ ] Run all tests
- [ ] Performance testing
- [ ] Security audit
- [ ] User acceptance testing
- [ ] Deploy to staging
- [ ] Monitor for issues
- [ ] Deploy to production

---

## Risk Mitigation

| Risk | Impact | Mitigation |
|------|--------|------------|
| Data loss | Critical | Backup before migration |
| Feature regression | High | Comprehensive testing |
| Performance degradation | Medium | Performance testing |
| Security vulnerabilities | Critical | Security audit |
| Timeline overrun | Medium | Phased migration |

---

## Rollback Plan

### Rollback Strategy
1. Keep original code in separate branch
2. Database backup before migration
3. Feature flags for gradual rollout
4. Blue-green deployment for instant rollback

### Rollback Commands
```bash
# Database rollback
php artisan migrate:rollback
# or
python manage.py migrate [previous_migration]

# Code rollback
git checkout main
git merge --rollback-branch
```
