# 📘 Advanced API Development with FastAPI

## 📋 Table of Contents
- [Project Structure](#project-structure)
- [API Routes Design](#api-routes-design)
- [Authentication & Authorization](#authentication--authorization)
- [Middleware Implementation](#middleware-implementation)
- [CORS Handling](#cors-handling)
- [Request & Response Handling](#request--response-handling)
- [Cookie Management](#cookie-management)
- [Headers Management](#headers-management)
- [Database Integration](#database-integration)
- [Caching with Redis](#caching-with-redis)
- [Background Tasks](#background-tasks)
- [API Security Best Practices](#api-security-best-practices)

## 📁 Project Structure

### Recommended Structure
```
project_root/
├── app/
│   ├── api/               # API routes organized by version
│   ├── core/              # Core logic, security, config
│   ├── db/                # Database models and session
│   ├── middleware/        # Custom middleware
│   ├── schemas/           # Pydantic models
│   └── services/          # Business logic
├── tests/                 # Test modules
└── main.py                # App entry point
```

### Key Points:
- Separate concerns with clear directory structure
- Version your API endpoints
- Keep configuration separate from code
- Organize routes by resource type

## 🛣️ API Routes Design

### Best Practices:
- Use resource-based URLs (`/users` instead of `/get-users`)
- Apply proper HTTP methods (GET, POST, PUT, DELETE)
- Version your API (`/api/v1/...`)
- Use query parameters for filtering, sorting, pagination

### Example Router Setup:
```python
# routes organization
from fastapi import APIRouter

# Main router
api_router = APIRouter()

# Include sub-routers
api_router.include_router(
    users_router, 
    prefix="/users", 
    tags=["users"]
)
api_router.include_router(
    items_router, 
    prefix="/items", 
    tags=["items"]
)

# In main.py
app = FastAPI()
app.include_router(api_router, prefix="/api/v1")
```

### Example Endpoint:
```python
@router.get("/", response_model=List[UserSchema])
async def get_users(
    skip: int = 0, 
    limit: int = 100,
    current_user = Depends(get_current_user)
):
    """Get list of users with pagination"""
    users = await user_service.get_users(skip=skip, limit=limit)
    return users
```

## 🔐 Authentication & Authorization

### JWT Authentication:
```python
# Generate JWT token
def create_access_token(data: dict, expires_delta: timedelta = None):
    to_encode = data.copy()
    expire = datetime.utcnow() + (expires_delta or timedelta(minutes=15))
    to_encode.update({"exp": expire})
    return jwt.encode(to_encode, SECRET_KEY, algorithm=ALGORITHM)

# Verify token and get current user
async def get_current_user(token: str = Depends(oauth2_scheme)):
    credentials_exception = HTTPException(
        status_code=401,
        detail="Invalid authentication credentials",
        headers={"WWW-Authenticate": "Bearer"},
    )
    try:
        payload = jwt.decode(token, SECRET_KEY, algorithms=[ALGORITHM])
        username: str = payload.get("sub")
        if username is None:
            raise credentials_exception
    except JWTError:
        raise credentials_exception
    
    user = await get_user(username=username)
    if user is None:
        raise credentials_exception
    return user
```

### Role-Based Access Control:
```python
# Define roles
class UserRole(str, Enum):
    ADMIN = "admin"
    MANAGER = "manager"
    USER = "user"

# Check user roles
def require_roles(allowed_roles: List[UserRole]):
    def check_role(current_user = Depends(get_current_user)):
        if current_user.role not in allowed_roles:
            raise HTTPException(status_code=403, detail="Permission denied")
        return current_user
    return check_role

# Usage in endpoint
@router.post("/items/")
async def create_item(
    item: ItemCreate,
    current_user = Depends(require_roles([UserRole.ADMIN, UserRole.MANAGER]))
):
    return await item_service.create_item(item, current_user.id)
```

## 🔄 Middleware Implementation

### Types of Middleware:
1. Authentication middleware
2. Logging middleware
3. CORS middleware
4. Rate limiting middleware
5. Compression middleware
6. Request ID middleware

### Basic Middleware Structure:
```python
class CustomMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        # Pre-processing
        # ...
        
        # Call the next middleware or endpoint
        response = await call_next(request)
        
        # Post-processing
        # ...
        
        return response

# Register middleware in main.py
app.add_middleware(CustomMiddleware)
```

### Example: Logging Middleware
```python
class RequestLoggingMiddleware(BaseHTTPMiddleware):
    async def dispatch(self, request: Request, call_next):
        start_time = time.time()
        
        # Process request
        response = await call_next(request)
        
        # Log after processing
        process_time = time.time() - start_time
        logger.info(
            f"{request.method} {request.url.path} "
            f"Status: {response.status_code} "
            f"Time: {process_time:.3f}s"
        )
        
        return response
```

### Example: Rate Limiting
```python
class RateLimitMiddleware(BaseHTTPMiddleware):
    def __init__(self, app, max_requests=100, window_seconds=60):
        super().__init__(app)
        self.max_requests = max_requests
        self.window = window_seconds
        self.clients = {}
        
    async def dispatch(self, request: Request, call_next):
        client_ip = request.client.host
        current_time = time.time()
        
        if client_ip in self.clients:
            requests, window_start = self.clients[client_ip]
            
            # Reset if window expired
            if current_time - window_start > self.window:
                self.clients[client_ip] = [1, current_time]
            elif requests >= self.max_requests:
                raise HTTPException(status_code=429, detail="Too many requests")
            else:
                self.clients[client_ip][0] += 1
        else:
            self.clients[client_ip] = [1, current_time]
            
        return await call_next(request)
```

## 🌐 CORS Handling

### CORS Configuration:
```python
app.add_middleware(
    CORSMiddleware,
    allow_origins=["http://localhost:3000", "https://yourfrontend.com"],
    allow_credentials=True,  # Important for cookies/auth
    allow_methods=["*"],     # Or specify: ["GET", "POST", "PUT", "DELETE"]
    allow_headers=["*"],     # Or specify required headers
)
```

### When to Use:
- Always needed for browser-based clients on different domains
- Configure more strictly in production:
  - Limit `allow_origins` to specific domains
  - Specify exact `allow_methods` and `allow_headers`
  - Set `allow_credentials=True` only if needed

## 📨 Request & Response Handling

### Pydantic Models for Validation:
```python
class UserCreate(BaseModel):
    email: EmailStr
    password: str = Field(..., min_length=8)
    full_name: str = Field(..., max_length=100)
    
    @validator("password")
    def password_strength(cls, v):
        if not re.search(r"[A-Z]", v):
            raise ValueError("Password must contain uppercase letter")
        if not re.search(r"[a-z]", v):
            raise ValueError("Password must contain lowercase letter")
        if not re.search(r"\d", v):
            raise ValueError("Password must contain digit")
        return v

class UserResponse(BaseModel):
    id: int
    email: EmailStr
    full_name: str
    is_active: bool
    
    class Config:
        orm_mode = True  # Convert from ORM objects
```

### Standard API Response Format:
```python
class APIResponse(GenericModel, Generic[T]):
    success: bool
    message: Optional[str] = None
    data: Optional[T] = None
    errors: Optional[List[str]] = None

# Usage
@router.get("/users/{user_id}", response_model=APIResponse[UserResponse])
async def get_user(user_id: int):
    user = await user_service.get_user(user_id)
    if not user:
        return APIResponse(
            success=False,
            message="User not found",
            errors=["User with specified ID does not exist"]
        )
    
    return APIResponse(
        success=True,
        message="User retrieved successfully",
        data=user
    )
```

## 🍪 Cookie Management

### Setting Cookies:
```python
@router.post("/login")
async def login(response: Response, form_data: OAuth2PasswordRequestForm = Depends()):
    user = authenticate_user(form_data.username, form_data.password)
    if not user:
        raise HTTPException(status_code=401, detail="Invalid credentials")
        
    access_token = create_access_token(data={"sub": user.email})
    
    # Set cookie
    response.set_cookie(
        key="access_token",
        value=f"Bearer {access_token}",
        httponly=True,     # Prevents JavaScript access
        secure=True,       # HTTPS only
        samesite="lax",    # Protection against CSRF
        max_age=1800       # 30 minutes
    )
    
    return {"message": "Login successful"}
```

### Reading Cookies:
```python
@router.get("/me")
async def get_me(access_token: str = Cookie(None)):
    if not access_token or not access_token.startswith("Bearer "):
        raise HTTPException(status_code=401, detail="Not authenticated")
        
    token = access_token.replace("Bearer ", "")
    # Verify token and get user...
    return {"user": user}
```

### When to Use Cookies:
- Web applications where users interact via browsers
- When you need to persist auth between page refreshes
- When you want protection against XSS (with HttpOnly)
- For session management

## 📋 Headers Management

### Custom Headers:
```python
@router.get("/download")
async def download_file():
    content = generate_file_content()
    
    response = Response(content=content)
    response.headers["Content-Disposition"] = "attachment; filename=report.pdf"
    response.headers["X-Custom-Header"] = "custom-value"
    
    return response
```

### WWW-Authenticate Header:
```python
@app.exception_handler(HTTPException)
async def http_exception_handler(request, exc):
    headers = exc.headers or {}
    
    # Add WWW-Authenticate header for auth failures
    if exc.status_code == 401:
        headers["WWW-Authenticate"] = "Bearer"
    
    return JSONResponse(
        status_code=exc.status_code,
        content={"detail": exc.detail},
        headers=headers
    )
```

### When to Use:
- `WWW-Authenticate`: For authentication failures (401)
- `Content-Disposition`: For file downloads
- `Cache-Control`: For controlling browser caching
- Custom `X-` headers: For API-specific metadata

## 🗄️ Database Integration

### SQLAlchemy Async Setup:
```python
from sqlalchemy.ext.asyncio import create_async_engine, AsyncSession
from sqlalchemy.orm import sessionmaker

# Create engine
engine = create_async_engine(
    "postgresql+asyncpg://user:password@localhost/dbname",
    echo=True  # Set to False in production
)

# Create session factory
AsyncSessionLocal = sessionmaker(
    engine, class_=AsyncSession, expire_on_commit=False
)

# Dependency for database access
async def get_db():
    async with AsyncSessionLocal() as session:
        try:
            yield session
            await session.commit()
        except Exception:
            await session.rollback()
            raise
```

### Model Definition:
```python
class User(Base):
    __tablename__ = "users"

    id = Column(Integer, primary_key=True, index=True)
    email = Column(String, unique=True, index=True)
    hashed_password = Column(String)
    is_active = Column(Boolean, default=True)
    created_at = Column(DateTime, default=datetime.utcnow)
```

### Repository Pattern:
```python
class UserRepository:
    def __init__(self, db: AsyncSession):
        self.db = db
        
    async def get_by_id(self, user_id: int):
        query = select(User).where(User.id == user_id)
        result = await self.db.execute(query)
        return result.scalars().first()
        
    async def create(self, user_data: UserCreate):
        user = User(
            email=user_data.email,
            hashed_password=get_password_hash(user_data.password),
            full_name=user_data.full_name
        )
        self.db.add(user)
        await self.db.commit()
        await self.db.refresh(user)
        return user
```

## 🚀 Caching with Redis

### Redis Setup:
```python
import redis.asyncio as redis

# Connect to Redis
redis_client = redis.Redis.from_url(
    "redis://localhost:6379/0",
    encoding="utf-8", 
    decode_responses=True
)
```

### Basic Caching Functions:
```python
import json
from typing import Any, Optional

async def cache_get(key: str) -> Optional[Any]:
    """Get value from cache"""
    data = await redis_client.get(key)
    if data:
        return json.loads(data)
    return None

async def cache_set(key: str, value: Any, expire: int = 300) -> None:
    """Set value in cache with expiration (seconds)"""
    serialized = json.dumps(value)
    await redis_client.set(key, serialized, ex=expire)

async def cache_delete(key: str) -> None:
    """Delete value from cache"""
    await redis_client.delete(key)
```

### Caching Decorator:
```python
def cache_response(expire: int = 300):
    def decorator(func):
        @wraps(func)
        async def wrapper(*args, **kwargs):
            # Generate cache key from function name and arguments
            key_parts = [func.__name__]
            key_parts.extend(str(arg) for arg in args)
            key_parts.extend(f"{k}:{v}" for k, v in sorted(kwargs.items()))
            cache_key = f"cache:{'_'.join(key_parts)}"
            
            # Try to get from cache
            cached_result = await cache_get(cache_key)
            if cached_result is not None:
                return cached_result
                
            # Call original function
            result = await func(*args, **kwargs)
            
            # Cache result
            await cache_set(cache_key, result, expire)
            
            return result
        return wrapper
    return decorator

# Usage
@router.get("/items/")
@cache_response(expire=60)  # Cache for 60 seconds
async def get_items(skip: int = 0, limit: int = 100):
    items = await item_service.get_items(skip, limit)
    return items
```

## 🔄 Background Tasks

### Single Background Task:
```python
from fastapi import BackgroundTasks

@router.post("/send-email")
async def send_email(
    email: EmailSchema, 
    background_tasks: BackgroundTasks
):
    # Add task to background
    background_tasks.add_task(
        send_email_async, 
        recipient=email.recipient,
        subject=email.subject,
        body=email.body
    )
    
    return {"message": "Email will be sent in the background"}
```

### Using Celery for Complex Tasks:
```python
from celery import Celery

# Configure Celery
celery_app = Celery(
    "app",
    broker="redis://localhost:6379/1",
    backend="redis://localhost:6379/2"
)

# Define task
@celery_app.task
def process_large_data(data_id: int):
    # Time-consuming processing
    # ...
    return {"status": "completed", "data_id": data_id}

# FastAPI endpoint
@router.post("/process")
async def start_processing(data_id: int):
    # Start async task
    task = process_large_data.delay(data_id)
    return {"task_id": task.id}

@router.get("/task/{task_id}")
async def get_task_status(task_id: str):
    task = process_large_data.AsyncResult(task_id)
    if task.ready():
        return {"status": "completed", "result": task.result}
    return {"status": "processing"}
```

## 🔒 API Security Best Practices

### Key Security Measures:
1. **Rate Limiting**
   - Prevent abuse and DoS attacks
   - Implement per-user or per-IP limits

2. **Input Validation**
   - Use Pydantic for strict validation
   - Validate and sanitize all inputs

3. **Authentication & Authorization**
   - Use JWT with short expiration
   - Implement role-based access
   - Store tokens securely (HttpOnly cookies)

4. **HTTP Security Headers**
```python
@app.middleware("http")
async def add_security_headers(request: Request, call_next):
    response = await call_next(request)
    
    # Add security headers
    response.headers["X-Content-Type-Options"] = "nosniff"
    response.headers["X-Frame-Options"] = "DENY"
    response.headers["X-XSS-Protection"] = "1; mode=block"
    response.headers["Strict-Transport-Security"] = "max-age=31536000; includeSubDomains"
    response.headers["Content-Security-Policy"] = "default-src 'self'"
    
    return response
```

5. **Environment Variables for Secrets**
```python
# settings.py
from pydantic import BaseSettings

class Settings(BaseSettings):
    SECRET_KEY: str
    DATABASE_URL: str
    ALGORITHM: str = "HS256"
    ACCESS_TOKEN_EXPIRE_MINUTES: int = 30
    
    class Config:
        env_file = ".env"
        
settings = Settings()
```

6. **HTTPS Only**
   - Enable HTTPS in production
   - Use secure cookies 
   - Redirect HTTP to HTTPS

7. **Logging and Monitoring**
   - Log authentication events
   - Monitor for suspicious activity
   - Use structured logging
