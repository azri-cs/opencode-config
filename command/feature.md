---
description: Implement a new feature from requirements
agent: build
temperature: 0.3
---

Implement a new feature based on the requirements provided as $ARGUMENTS. Use workflow-oriented execution and load the relevant stack skill when the task spans a specific technology domain.

**Project Analysis:**
!`find . -name "*.php" -o -name "*.py" -o -name "*.tsx" -o -name "*.jsx" ! -path "./vendor/*" ! -path "./node_modules/*" ! -path "./.git/*" | head -10`

!`ls -la app/Http/Controllers 2>/dev/null || ls -la api/ 2>/dev/null || echo "Checking project structure..."`

!`cat package.json 2>/dev/null | grep -E "dependencies|devDependencies" | head -5`

## Feature Implementation Plan

### Feature Requirements
- **Feature**: $ARGUMENTS
- **Type**: [New Feature / Enhancement / Refactor]
- **Priority**: [High/Medium/Low]
- **Estimated Effort**: [X hours/days]

### Scope Definition
**In Scope**:
- [Component/functionality 1]
- [Component/functionality 2]
- [Component/functionality 3]

**Out of Scope**:
- [Feature 1]
- [Feature 2]

---

## Architecture & Design

### Technology Stack
- **Backend**: [Laravel/Django/Node]
- **Frontend**: [React/Vue/HTML]
- **Database**: [MySQL/PostgreSQL/MongoDB]
- **API Style**: [REST/GraphQL/gRPC]

### Data Model Design

**New Models/Tables**:
```[language]
// Model definition
class [ModelName] extends Model
{
    protected $fillable = [
        'field1',
        'field2',
        'field3',
    ];
    
    protected $casts = [
        'field1' => 'datetime',
        'field2' => 'integer',
    ];
    
    // Relationships
    public function user()
    {
        return $this->belongsTo(User::class);
    }
    
    public function items()
    {
        return $this->hasMany(Item::class);
    }
}
```

**Database Schema**:
```sql
CREATE TABLE [table_name] (
    id BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    field1 VARCHAR(255) NOT NULL,
    field2 TEXT,
    field3 INT DEFAULT 0,
    user_id BIGINT UNSIGNED,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    INDEX idx_field1 (field1),
    INDEX idx_user_id (user_id),
    
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

### API Design

**Endpoints**:
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | /api/[resource] | List [resources] |
| POST | /api/[resource] | Create [resource] |
| GET | /api/[resource]/{id} | Get [resource] |
| PUT | /api/[resource]/{id} | Update [resource] |
| DELETE | /api/[resource]/{id} | Delete [resource] |

**Request/Response Formats**:
```json
// POST /api/[resource]
{
  "field1": "value1",
  "field2": "value2",
  "field3": 123
}

// Response
{
  "success": true,
  "data": {
    "id": 1,
    "field1": "value1",
    "field2": "value2",
    "field3": 123,
    "created_at": "2024-01-01T00:00:00Z",
    "updated_at": "2024-01-01T00:00:00Z"
  },
  "message": "[Resource] created successfully"
}
```

### Frontend Components

**New Components**:
```
src/
└── components/
    └── [FeatureName]/
        ├── [FeatureName].tsx
        ├── [FeatureName].css
        ├── [FeatureName].test.tsx
        └── index.ts
```

---

## Implementation Steps

### Phase 1: Database & Backend

#### 1.1 Create Database Migration
```php
// database/migrations/xxxx_create_[table]_table.php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class Create[Table]Table extends Migration
{
    public function up()
    {
        Schema::create('[table]', function (Blueprint $table) {
            $table->id();
            $table->string('field1');
            $table->text('field2')->nullable();
            $table->integer('field3')->default(0);
            $table->foreignId('user_id')->constrained()->onDelete('cascade');
            $table->timestamps();
            
            $table->index('field1');
        });
    }

    public function down()
    {
        Schema::dropIfExists('[table]');
    }
}
```

#### 1.2 Create Model
```php
// app/Models/[ModelName].php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class [ModelName] extends Model
{
    protected $fillable = [
        'field1',
        'field2',
        'field3',
        'user_id',
    ];
    
    protected $casts = [
        'field3' => 'integer',
    ];
    
    public function user()
    {
        return $this->belongsTo(User::class);
    }
}
```

#### 1.3 Create Controller
```php
// app/Http/Controllers/Api/[FeatureName]Controller.php

namespace App\Http\Controllers\Api;

use App\Models\[ModelName];
use Illuminate\Http\Request;
use App\Http\Resources\[ModelName]Resource;
use App\Services\[FeatureName]Service;

class [FeatureName]Controller extends Controller
{
    protected $service;
    
    public function __construct([FeatureName]Service $service)
    {
        $this->service = $service;
    }
    
    public function index(Request $request)
    {
        $[plural] = $this->service->list($request->all());
        return [ModelName]Resource::collection($[plural]);
    }
    
    public function store(Request $request)
    {
        $[model] = $this->service->create($request->all());
        return new [ModelName]Resource($[model]);
    }
    
    public function show($id)
    {
        $[model] = $this->service->findOrFail($id);
        return new [ModelName]Resource($[model]);
    }
    
    public function update(Request $request, $id)
    {
        $[model] = $this->service->update($id, $request->all());
        return new [ModelName]Resource($[model]);
    }
    
    public function destroy($id)
    {
        $this->service->delete($id);
        return response()->json(['message' => '[Model] deleted successfully']);
    }
}
```

#### 1.4 Create Service Layer
```php
// app/Services/[FeatureName]Service.php

namespace App\Services;

use App\Models\[ModelName];
use App\Repositories\[FeatureName]Repository;
use Illuminate\Support\Facades\Log;

class [FeatureName]Service
{
    protected $repository;
    
    public function __construct([FeatureName]Repository $repository)
    {
        $this->repository = $repository;
    }
    
    public function list(array $filters = [])
    {
        return $this->repository->all($filters);
    }
    
    public function create(array $data): [ModelName]
    {
        $this->validate($data);
        
        $[model] = $this->repository->create($data);
        
        Log::info('[FeatureName] created', [
            'id' => $[model]->id,
            'user_id' => auth()->id(),
        ]);
        
        return $[model];
    }
    
    public function findOrFail($id): [ModelName]
    {
        $[model] = $this->repository->find($id);
        
        if (!$[model]) {
            throw new \Exception('[ModelName] not found', 404);
        }
        
        return $[model];
    }
    
    public function update($id, array $data): [ModelName]
    {
        $[model] = $this->findOrFail($id);
        
        $this->validate($data, $[model]->id);
        $[model] = $this->repository->update($[model], $data);
        
        Log::info('[FeatureName] updated', [
            'id' => $[model]->id,
            'user_id' => auth()->id(),
        ]);
        
        return $[model];
    }
    
    public function delete($id): void
    {
        $[model] = $this->findOrFail($id);
        $this->repository->delete($[model]);
        
        Log::info('[FeatureName] deleted', [
            'id' => $id,
            'user_id' => auth()->id(),
        ]);
    }
    
    protected function validate(array $data, $id = null): void
    {
        // Validation logic
    }
}
```

#### 1.5 Create Routes
```php
// routes/api.php

Route::prefix('api')->middleware(['auth:api'])->group(function () {
    Route::resource('[plural]', [FeatureName]Controller::class);
});
```

### Phase 2: Frontend Implementation

#### 2.1 Create TypeScript Types
```typescript
// types/[feature-name].ts

export interface [FeatureName] {
  id: string;
  field1: string;
  field2: string | null;
  field3: number;
  userId: string;
  createdAt: string;
  updatedAt: string;
}

export interface Create[FeatureName]Request {
  field1: string;
  field2?: string;
  field3: number;
}

export interface Update[FeatureName]Request extends Partial<Create[FeatureName]Request> {}
```

#### 2.2 Create API Service
```typescript
// services/[feature-name].ts

import api from './api';
import { [FeatureName], Create[FeatureName]Request, Update[FeatureName]Request } from '../types/[feature-name]';

export const [featureName]API = {
  list: (params?: Record<string, unknown>) => 
    api.get<[FeatureName][]>('/[plural]', { params }),
    
  get: (id: string) => 
    api.get<[FeatureName]>(`/${[plural]}/${id}`),
    
  create: (data: Create[FeatureName]Request) => 
    api.post<[FeatureName]>(`/${[plural]}`, data),
    
  update: (id: string, data: Update[FeatureName]Request) => 
    api.put<[FeatureName]>(`/${[plural]}/${id}`, data),
    
  delete: (id: string) => 
    api.delete(`/${[plural]}/${id}`),
};
```

#### 2.3 Create React Component
```tsx
// components/[FeatureName]/[FeatureName].tsx

import React, { useState, useEffect } from 'react';
import { use[FeatureName] } from '../hooks/use[FeatureName]';
import { [FeatureName]Form } from './[FeatureName]Form';
import { [FeatureName]List } from './[FeatureName]List';

export const [FeatureName]: React.FC = () => {
  const { 
    [plural], 
    loading, 
    error, 
    fetch[FeatureName]s,
    create[FeatureName],
    update[FeatureName],
    delete[FeatureName],
  } = use[FeatureName]();
  
  const [isFormOpen, setIsFormOpen] = useState(false);
  const [editing[FeatureName], setEditing[FeatureName] ] = useState<[FeatureName] | null>(null);
  
  useEffect(() => {
    fetch[FeatureName]s();
  }, [fetch[FeatureName]s]);
  
  const handleCreate = async (data: Create[FeatureName]Request) => {
    await create[FeatureName](data);
    setIsFormOpen(false);
  };
  
  const handleUpdate = async (id: string, data: Update[FeatureName]Request) => {
    await update[FeatureName](id, data);
    setEditing[FeatureName](null);
  };
  
  const handleEdit = ([featureName]: [FeatureName]) => {
    setEditing[FeatureName]([featureName]);
    setIsFormOpen(true);
  };
  
  const handleDelete = async (id: string) => {
    if (window.confirm('Are you sure you want to delete this [feature name]?')) {
      await delete[FeatureName](id);
    }
  };
  
  if (loading) return <LoadingSpinner />;
  if (error) return <ErrorMessage error={error} />;
  
  return (
    <div className="[feature-name]-container">
      <header className="[feature-name]-header">
        <h1>[Feature Name]</h1>
        <button 
          onClick={() => {
            setEditing[FeatureName](null);
            setIsFormOpen(true);
          }}
        >
          Add New
        </button>
      </header>
      
      {(isFormOpen || editing[FeatureName]) && (
        <[FeatureName]Form
          [featureName]={editing[FeatureName]}
          onSubmit={(data) => {
            if (editing[FeatureName]) {
              handleUpdate(editing[FeatureName].id, data);
            } else {
              handleCreate(data);
            }
          }}
          onCancel={() => {
            setIsFormOpen(false);
            setEditing[FeatureName](null);
          }}
        />
      )}
      
      <[FeatureName]List
        [plural]={[plural]}
        onEdit={handleEdit}
        onDelete={handleDelete}
      />
    </div>
  );
};
```

### Phase 3: Testing

#### 3.1 Unit Tests
```typescript
// __tests__/[feature-name].test.ts

describe('[FeatureName] Service', () => {
  it('should create [feature name]', async () => {
    const [featureName]Data = {
      field1: 'value1',
      field2: 'value2',
      field3: 123,
    };
    
    const created[FeatureName] = {
      id: '1',
      ...[featureName]Data,
      user_id: 'user-1',
      created_at: new Date().toISOString(),
      updated_at: new Date().toISOString(),
    };
    
    jest.spyOn(api, 'post').mockResolvedValue({ data: created[FeatureName] });
    
    const result = await [featureName]API.create([featureName]Data);
    
    expect(result).toEqual(created[FeatureName]);
    expect(api.post).toHaveBeenCalledWith('/[plural]', [featureName]Data);
  });
  
  it('should list [feature names]', async () => {
    const [featureName]s = [
      { id: '1', field1: 'value1' },
      { id: '2', field1: 'value2' },
    ];
    
    jest.spyOn(api, 'get').mockResolvedValue({ data: [featureName]s });
    
    const result = await [featureName]API.list();
    
    expect(result).toEqual([featureName]s);
  });
});
```

#### 3.2 Component Tests
```typescript
// components/[FeatureName]/__tests__/[FeatureName].test.tsx

import React from 'react';
import { render, screen, fireEvent, waitFor } from '@testing-library/react';
import { [FeatureName] } from '../[FeatureName]';
import { use[FeatureName] } from '../hooks/use[FeatureName];

jest.mock('../hooks/use[FeatureName]');

describe('[FeatureName] Component', () => {
  beforeEach(() => {
    jest.clearAllMocks();
  });
  
  it('should render loading state', () => {
    (use[FeatureName] as jest.Mock).mockReturnValue({
      [plural]: [],
      loading: true,
      error: null,
      fetch[FeatureName]s: jest.fn(),
      create[FeatureName]: jest.fn(),
      update[FeatureName]: jest.fn(),
      delete[FeatureName]: jest.fn(),
    });
    
    render(<[FeatureName] />);
    expect(screen.getByRole('status')).toHaveTextContent(/loading/i);
  });
  
  it('should render list of [feature names]', () => {
    const mock[FeatureName]s = [
      { id: '1', field1: 'Value 1' },
      { id: '2', field1: 'Value 2' },
    ];
    
    (use[FeatureName] as jest.Mock).mockReturnValue({
      [plural]: mock[FeatureName]s,
      loading: false,
      error: null,
      fetch[FeatureName]s: jest.fn(),
      create[FeatureName]: jest.fn(),
      update[FeatureName]: jest.fn(),
      delete[FeatureName]: jest.fn(),
    });
    
    render(<[FeatureName] />);
    
    expect(screen.getByText('Value 1')).toBeInTheDocument();
    expect(screen.getByText('Value 2')).toBeInTheDocument();
  });
});
```

---

## Implementation Checklist

### Backend
- [ ] Database migration created
- [ ] Model created with relationships
- [ ] Repository pattern implemented
- [ ] Service layer created
- [ ] Controller created
- [ ] Routes defined
- [ ] Request validation
- [ ] API documentation updated

### Frontend
- [ ] TypeScript interfaces defined
- [ ] API service created
- [ ] React component created
- [ ] Form component created
- [ ] List component created
- [ ] Error handling implemented
- [ ] Loading states implemented

### Testing
- [ ] Unit tests for service layer
- [ ] Unit tests for components
- [ ] Integration tests for API
- [ ] E2E tests for user flows

### Documentation
- [ ] API documentation updated
- [ ] README updated
- [ ] User guide updated
- [ ] Code comments added

---

## Quality Standards

### Code Review Requirements
- [ ] Follows project conventions
- [ ] Type safety maintained
- [ ] Error handling comprehensive
- [ ] Performance considered
- [ ] Security best practices followed
- [ ] Tests are comprehensive

### Acceptance Criteria
- [ ] Feature works as specified
- [ ] All edge cases handled
- [ ] No breaking changes
- [ ] Performance acceptable
- [ ] Accessibility requirements met
- [ ] Mobile responsive (if applicable)

---

## Estimated Timeline

### Phase 1: Backend (2 days)
- Database migration: 2 hours
- Model & Repository: 2 hours
- Service layer: 4 hours
- Controller & Routes: 2 hours
- Testing: 4 hours

### Phase 2: Frontend (2 days)
- Types & API service: 2 hours
- Main component: 4 hours
- Form & List: 4 hours
- Testing: 4 hours

### Phase 3: Integration (1 day)
- Integration testing: 4 hours
- Bug fixes: 4 hours

### Total: 5 days
