# Fullstack Implementation Reference

## Laravel 12 Patterns

### Invokable Controllers
```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Inertia\Inertia;
use Inertia\Response;

class ShowDashboardController
{
    public function __invoke(Request $request): Response
    {
        return Inertia::render('Dashboard', [
            'stats' => $this->getStats(),
        ]);
    }
}
```

### Form Requests
```php
<?php

namespace App\Http\Requests;

use Illuminate\Foundation\Http\FormRequest;

class StorePostRequest extends FormRequest
{
    public function authorize(): bool
    {
        return $this->user()->can('create', Post::class);
    }

    public function rules(): array
    {
        return [
            'title' => ['required', 'string', 'max:255'],
            'content' => ['required', 'string'],
            'published_at' => ['nullable', 'date'],
        ];
    }
}
```

### Eloquent Models
```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Illuminate\Database\Eloquent\Relations\BelongsTo;
use Illuminate\Database\Eloquent\Relations\HasMany;

class Post extends Model
{
    protected $fillable = [
        'title',
        'content',
        'user_id',
        'published_at',
    ];

    protected $casts = [
        'published_at' => 'datetime',
    ];

    public function user(): BelongsTo
    {
        return $this->belongsTo(User::class);
    }

    public function comments(): HasMany
    {
        return $this->hasMany(Comment::class);
    }
}
```

## React 19 + TypeScript Patterns

### Inertia Page Component
```tsx
import { Head, useForm } from '@inertiajs/react';
import { FormEvent } from 'react';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';

interface Props {
    post?: {
        id: number;
        title: string;
        content: string;
    };
}

export default function PostForm({ post }: Props) {
    const { data, setData, post: submit, processing, errors } = useForm({
        title: post?.title ?? '',
        content: post?.content ?? '',
    });

    const handleSubmit = (e: FormEvent) => {
        e.preventDefault();
        submit(route('posts.store'));
    };

    return (
        <>
            <Head title="Create Post" />
            <form onSubmit={handleSubmit} className="space-y-4">
                <div>
                    <Label htmlFor="title">Title</Label>
                    <Input
                        id="title"
                        value={data.title}
                        onChange={(e) => setData('title', e.target.value)}
                        aria-invalid={!!errors.title}
                    />
                    {errors.title && (
                        <p className="text-sm text-destructive">{errors.title}</p>
                    )}
                </div>
                <Button type="submit" disabled={processing}>
                    {processing ? 'Saving...' : 'Save'}
                </Button>
            </form>
        </>
    );
}
```

### Custom Hook
```tsx
import { useState, useCallback } from 'react';

export function useToggle(initial = false) {
    const [value, setValue] = useState(initial);
    
    const toggle = useCallback(() => setValue((v) => !v), []);
    const setTrue = useCallback(() => setValue(true), []);
    const setFalse = useCallback(() => setValue(false), []);
    
    return { value, toggle, setTrue, setFalse };
}
```

## File Structure Reference

```
app/
├── Http/
│   ├── Controllers/
│   │   └── PostController.php
│   └── Requests/
│       └── StorePostRequest.php
├── Models/
│   └── Post.php
└── Policies/
    └── PostPolicy.php

src/
├── api/
│   └── users.py              # APIRouter with endpoints
├── schemas/
│   └── users.py              # Pydantic models
├── models/
│   └── user.py               # SQLAlchemy/SQLModel models
├── services/
│   └── user_service.py       # Business logic
└── main.py                   # FastAPI app entrypoint

resources/js/
├── pages/
│   └── Posts/
│       ├── Index.tsx
│       ├── Create.tsx
│       └── Edit.tsx
├── components/
│   └── posts/
│       └── PostForm.tsx
├── hooks/
│   └── useToggle.ts
└── types/
    └── post.d.ts
```

## FastAPI Patterns

### Router
```python
from fastapi import APIRouter, Depends, HTTPException
from src.schemas.users import UserCreate, UserRead
from src.services.user_service import UserService

router = APIRouter(prefix="/users", tags=["users"])

@router.post("/", response_model=UserRead)
async def create_user(payload: UserCreate, svc: UserService = Depends()):
    return await svc.create(payload)
```

### Pydantic Schema
```python
from pydantic import BaseModel, EmailStr

class UserCreate(BaseModel):
    email: EmailStr
    password: str

class UserRead(BaseModel):
    id: int
    email: EmailStr
```

### SQLModel Example
```python
from sqlmodel import SQLModel, Field

class User(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    email: str = Field(index=True)
    password_hash: str
```
