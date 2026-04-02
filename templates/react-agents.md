# React Project Guidelines

**For React/Node.js Frontend Projects**

---

## Project Structure

```
project/
├── src/
│   ├── components/
│   │   ├── common/
│   │   │   ├── Button/
│   │   │   ├── Input/
│   │   │   ├── Modal/
│   │   │   └── Card/
│   │   ├── features/
│   │   │   ├── auth/
│   │   │   ├── dashboard/
│   │   │   └── users/
│   │   └── layout/
│   │       ├── Header/
│   │       ├── Sidebar/
│   │       └── Footer/
│   ├── pages/
│   │   ├── Home/
│   │   ├── Login/
│   │   ├── Dashboard/
│   │   └── (feature pages)
│   ├── hooks/
│   │   ├── useAuth.ts
│   │   ├── useFetch.ts
│   │   └── useLocalStorage.ts
│   ├── context/
│   │   ├── AuthContext.tsx
│   │   └── ThemeContext.tsx
│   ├── services/
│   │   ├── api.ts
│   │   ├── auth.ts
│   │   └── users.ts
│   ├── utils/
│   │   ├── formatters.ts
│   │   ├── validators.ts
│   │   └── helpers.ts
│   ├── types/
│   │   ├── user.ts
│   │   └── api.ts
│   ├── styles/
│   │   ├── global.css
│   │   └── variables.css
│   ├── assets/
│   └── App.tsx
├── public/
├── tests/
├── package.json
├── tsconfig.json
├── .eslintrc
├── .prettierrc
└── README.md
```

## Component Architecture

### Functional Components with Hooks
```tsx
// components/common/Button/Button.tsx
import React from 'react';
import './Button.css';

interface ButtonProps {
  children: React.ReactNode;
  variant?: 'primary' | 'secondary' | 'danger';
  size?: 'small' | 'medium' | 'large';
  disabled?: boolean;
  onClick?: () => void;
  type?: 'button' | 'submit' | 'reset';
  className?: string;
}

export const Button: React.FC<ButtonProps> = ({
  children,
  variant = 'primary',
  size = 'medium',
  disabled = false,
  onClick,
  type = 'button',
  className = '',
}) => {
  const classNames = [
    'btn',
    `btn-${variant}`,
    `btn-${size}`,
    className,
  ].filter(Boolean).join(' ');

  return (
    <button
      type={type}
      className={classNames}
      disabled={disabled}
      onClick={onClick}
    >
      {children}
    </button>
  );
};
```

### Custom Hooks
```tsx
// hooks/useFetch.ts
import { useState, useEffect, useCallback } from 'react';

interface UseFetchOptions<T> {
  immediate?: boolean;
  onError?: (error: Error) => void;
}

interface UseFetchResult<T> {
  data: T | null;
  loading: boolean;
  error: Error | null;
  refetch: () => Promise<void>;
}

export function useFetch<T>(
  url: string,
  options: UseFetchOptions<T> = {}
): UseFetchResult<T> {
  const { immediate = true, onError } = options;
  
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState<Error | null>(null);

  const fetchData = useCallback(async () => {
    setLoading(true);
    setError(null);
    
    try {
      const response = await fetch(url);
      
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      
      const data = await response.json();
      setData(data);
    } catch (err) {
      const error = err instanceof Error ? err : new Error('Unknown error');
      setError(error);
      onError?.(error);
    } finally {
      setLoading(false);
    }
  }, [url, onError]);

  useEffect(() => {
    if (immediate) {
      fetchData();
    }
  }, [fetchData, immediate]);

  return { data, loading, error, refetch: fetchData };
}
```

### Context API
```tsx
// context/AuthContext.tsx
import React, { createContext, useContext, useState, useCallback, ReactNode } from 'react';
import { authAPI } from '../services/auth';

interface User {
  id: string;
  email: string;
  name: string;
  role: 'admin' | 'user';
}

interface AuthContextType {
  user: User | null;
  isAuthenticated: boolean;
  isLoading: boolean;
  login: (email: string, password: string) => Promise<void>;
  logout: () => void;
}

const AuthContext = createContext<AuthContextType | undefined>(undefined);

export const AuthProvider: React.FC<{ children: ReactNode }> = ({ children }) => {
  const [user, setUser] = useState<User | null>(null);
  const [isLoading, setIsLoading] = useState(false);

  const login = useCallback(async (email: string, password: string) => {
    setIsLoading(true);
    try {
      const response = await authAPI.login({ email, password });
      setUser(response.user);
    } finally {
      setIsLoading(false);
    }
  }, []);

  const logout = useCallback(() => {
    setUser(null);
    authAPI.logout();
  }, []);

  const value = {
    user,
    isAuthenticated: !!user,
    isLoading,
    login,
    logout,
  };

  return <AuthContext.Provider value={value}>{children}</AuthContext.Provider>;
};

export const useAuth = (): AuthContextType => {
  const context = useContext(AuthContext);
  if (context === undefined) {
    throw new Error('useAuth must be used within an AuthProvider');
  }
  return context;
};
```

## State Management

### React Query (Server State)
```tsx
// hooks/useUsers.ts
import { useQuery, useMutation, useQueryClient } from '@tanstack/react-query';
import { usersAPI } from '../services/users';
import { User, CreateUserRequest } from '../types/user';

export function useUsers(page = 1, limit = 10) {
  return useQuery({
    queryKey: ['users', page, limit],
    queryFn: () => usersAPI.getUsers(page, limit),
    staleTime: 5 * 60 * 1000, // 5 minutes
  });
}

export function useCreateUser() {
  const queryClient = useQueryClient();
  
  return useMutation({
    mutationFn: (data: CreateUserRequest) => usersAPI.createUser(data),
    onSuccess: () => {
      queryClient.invalidateQueries({ queryKey: ['users'] });
    },
  });
}
```

### Zustand (Client State)
```tsx
// store/useUIStore.ts
import { create } from 'zustand';

interface UIState {
  sidebarOpen: boolean;
  theme: 'light' | 'dark';
  notifications: Notification[];
  toggleSidebar: () => void;
  setTheme: (theme: 'light' | 'dark') => void;
  addNotification: (notification: Omit<Notification, 'id'>) => void;
  removeNotification: (id: string) => void;
}

interface Notification {
  id: string;
  type: 'success' | 'error' | 'warning' | 'info';
  message: string;
}

export const useUIStore = create<UIState>((set) => ({
  sidebarOpen: true,
  theme: 'light',
  notifications: [],
  
  toggleSidebar: () => set((state) => ({ 
    sidebarOpen: !state.sidebarOpen 
  })),
  
  setTheme: (theme) => set({ theme }),
  
  addNotification: (notification) => set((state) => ({
    notifications: [
      ...state.notifications,
      { ...notification, id: crypto.randomUUID() },
    ],
  })),
  
  removeNotification: (id) => set((state) => ({
    notifications: state.notifications.filter((n) => n.id !== id),
  })),
}));
```

## TypeScript Best Practices

### Type Definitions
```typescript
// types/user.ts
export interface User {
  id: string;
  email: string;
  name: string;
  role: 'admin' | 'user';
  avatar?: string;
  createdAt: string;
  updatedAt: string;
}

export interface CreateUserRequest {
  email: string;
  name: string;
  password: string;
  role?: 'admin' | 'user';
}

export interface UpdateUserRequest extends Partial<CreateUserRequest> {
  id: string;
}

export interface PaginatedResponse<T> {
  data: T[];
  pagination: {
    page: number;
    limit: number;
    total: number;
    totalPages: number;
  };
}

export interface ApiError {
  code: string;
  message: string;
  details?: Record<string, string[]>;
}
```

### Generic Components
```tsx
// components/common/DataTable/DataTable.tsx
interface DataTableProps<T> {
  data: T[];
  columns: Column<T>[];
  loading?: boolean;
  onRowClick?: (item: T) => void;
  emptyMessage?: string;
}

interface Column<T> {
  key: keyof T;
  header: string;
  render?: (item: T) => React.ReactNode;
  width?: string;
}

export function DataTable<T extends { id: string }>({
  data,
  columns,
  loading = false,
  onRowClick,
  emptyMessage = 'No data available',
}: DataTableProps<T>) {
  // Implementation
}
```

## Performance Optimization

### Code Splitting
```tsx
// App.tsx
import React, { Suspense, lazy } from 'react';
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import { Layout } from './components/layout/Layout';
import { LoadingSpinner } from './components/common/LoadingSpinner';

const HomePage = lazy(() => import('./pages/Home'));
const DashboardPage = lazy(() => import('./pages/Dashboard'));
const UserPage = lazy(() => import('./pages/User'));

export const App: React.FC = () => {
  return (
    <BrowserRouter>
      <Layout>
        <Suspense fallback={<LoadingSpinner />}>
          <Routes>
            <Route path="/" element={<HomePage />} />
            <Route path="/dashboard" element={<DashboardPage />} />
            <Route path="/users" element={<UserPage />} />
          </Routes>
        </Suspense>
      </Layout>
    </BrowserRouter>
  );
};
```

### Memoization
```tsx
// components/UserList.tsx
import React, { memo, useMemo } from 'react';
import { useQuery } from '@tanstack/react-query';

interface UserListProps {
  onSelect: (userId: string) => void;
  filter: string;
}

export const UserList: React.FC<UserListProps> = memo(({ onSelect, filter }) => {
  const { data: users, isLoading } = useQuery({
    queryKey: ['users'],
    queryFn: () => fetchUsers(),
  });

  const filteredUsers = useMemo(() => {
    if (!users) return [];
    return users.filter(user => 
      user.name.toLowerCase().includes(filter.toLowerCase())
    );
  }, [users, filter]);

  if (isLoading) return <LoadingSpinner />;

  return (
    <div className="user-list">
      {filteredUsers.map(user => (
        <UserCard 
          key={user.id} 
          user={user}
          onClick={() => onSelect(user.id)}
        />
      ))}
    </div>
  );
});
```

## Testing

### Jest + React Testing Library
```tsx
// components/Button/Button.test.tsx
import React from 'react';
import { render, screen, fireEvent } from '@testing-library/react';
import { Button } from './Button';

describe('Button', () => {
  it('renders children correctly', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByRole('button', { name: /click me/i })).toBeInTheDocument();
  });

  it('calls onClick when clicked', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);
    
    fireEvent.click(screen.getByRole('button'));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });

  it('is disabled when disabled prop is true', () => {
    render(<Button disabled>Click me</Button>);
    expect(screen.getByRole('button')).toBeDisabled();
  });

  it('applies variant classes', () => {
    render(<Button variant="danger">Click me</Button>);
    expect(screen.getByRole('button')).toHaveClass('btn-danger');
  });
});
```

### Vitest for Hooks
```tsx
// hooks/useCounter.test.ts
import { renderHook, act } from '@testing-library/react';
import { useCounter } from './useCounter';

describe('useCounter', () => {
  it('initializes with default value', () => {
    const { result } = renderHook(() => useCounter());
    expect(result.current.count).toBe(0);
  });

  it('initializes with custom value', () => {
    const { result } = renderHook(() => useCounter(10));
    expect(result.current.count).toBe(10);
  });

  it('increments count', () => {
    const { result } = renderHook(() => useCounter());
    
    act(() => {
      result.current.increment();
    });
    
    expect(result.current.count).toBe(1);
  });

  it('decrements count', () => {
    const { result } = renderHook(() => useCounter());
    
    act(() => {
      result.current.decrement();
    });
    
    expect(result.current.count).toBe(-1);
  });
});
```

## API Integration

### Axios Configuration
```typescript
// services/api.ts
import axios, { AxiosError, InternalAxiosRequestConfig } from 'axios';
import { useAuth } from '../contexts/AuthContext';

const api = axios.create({
  baseURL: import.meta.env.VITE_API_URL || '/api',
  timeout: 10000,
  headers: {
    'Content-Type': 'application/json',
  },
});

// Request interceptor
api.interceptors.request.use(
  (config: InternalAxiosRequestConfig) => {
    const token = localStorage.getItem('auth_token');
    if (token && config.headers) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error: AxiosError) => {
    return Promise.reject(error);
  }
);

// Response interceptor
api.interceptors.response.use(
  (response) => response,
  async (error: AxiosError) => {
    const originalRequest = error.config as InternalAxiosRequestConfig & { _retry?: boolean };
    
    if (error.response?.status === 401 && !originalRequest._retry) {
      originalRequest._retry = true;
      
      try {
        const refreshToken = localStorage.getItem('refresh_token');
        const response = await axios.post('/api/auth/refresh', { refreshToken });
        
        const newToken = response.data.token;
        localStorage.setItem('auth_token', newToken);
        
        originalRequest.headers.Authorization = `Bearer ${newToken}`;
        return api(originalRequest);
      } catch {
        // Redirect to login
        window.location.href = '/login';
      }
    }
    
    return Promise.reject(error);
  }
);

export default api;
```

## Accessibility (WCAG)

### ARIA Labels
```tsx
// components/common/Modal/Modal.tsx
import React, { useEffect, useRef } from 'react';

interface ModalProps {
  isOpen: boolean;
  onClose: () => void;
  title: string;
  children: React.ReactNode;
}

export const Modal: React.FC<ModalProps> = ({
  isOpen,
  onClose,
  title,
  children,
}) => {
  const modalRef = useRef<HTMLDivElement>(null);
  const previousActiveElement = useRef<HTMLElement | null>(null);

  useEffect(() => {
    if (isOpen) {
      previousActiveElement.current = document.activeElement as HTMLElement;
      modalRef.current?.focus();
      document.body.style.overflow = 'hidden';
    } else {
      previousActiveElement.current?.focus();
      document.body.style.overflow = '';
    }

    return () => {
      document.body.style.overflow = '';
    };
  }, [isOpen]);

  const handleKeyDown = (e: React.KeyboardEvent) => {
    if (e.key === 'Escape') {
      onClose();
    }
    
    if (e.key === 'Tab' && modalRef.current) {
      const focusableElements = modalRef.current.querySelectorAll(
        'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
      );
      
      const firstElement = focusableElements[0] as HTMLElement;
      const lastElement = focusableElements[focusableElements.length - 1] as HTMLElement;
      
      if (e.shiftKey && document.activeElement === firstElement) {
        e.preventDefault();
        lastElement.focus();
      } else if (!e.shiftKey && document.activeElement === lastElement) {
        e.preventDefault();
        firstElement.focus();
      }
    }
  };

  if (!isOpen) return null;

  return (
    <div
      className="modal-overlay"
      onClick={onClose}
      role="presentation"
    >
      <div
        ref={modalRef}
        className="modal"
        role="dialog"
        aria-modal="true"
        aria-labelledby="modal-title"
        tabIndex={-1}
        onKeyDown={handleKeyDown}
      >
        <header className="modal-header">
          <h2 id="modal-title">{title}</h2>
          <button
            type="button"
            className="modal-close"
            onClick={onClose}
            aria-label="Close modal"
          >
            ×
          </button>
        </header>
        <div className="modal-content">{children}</div>
      </div>
    </div>
  );
};
```

## Styling

### Tailwind CSS Classes
```tsx
// component with Tailwind
export const Card: React.FC<CardProps> = ({ children, className = '' }) => {
  return (
    <div className={`
      bg-white
      rounded-lg
      shadow-md
      p-6
      hover:shadow-lg
      transition-shadow
      ${className}
    `}>
      {children}
    </div>
  );
};

// Responsive design
<div className="
  grid
  grid-cols-1
  md:grid-cols-2
  lg:grid-cols-3
  xl:grid-cols-4
  gap-4
">
  {/* Content */}
</div>
```

## Additional Resources

- [React Documentation](https://react.dev/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [React Query](https://tanstack.com/query/latest)
- [Zustand](https://github.com/pmndrs/zustand)
- [Testing Library](https://testing-library.com/)
- [WCAG Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [Tailwind CSS](https://tailwindcss.com/docs)
