# vxxx-cache
## Cache Service
A robust, production-ready Redis caching service with automatic fallback to in-memory
cache. Designed for high-performance applications requiring reliable caching with graceful
degradation.
Features
• Automatic Fallback: Seamlessly falls back to in-memory cache when Redis is
unavailable
• Production Ready: Comprehensive error handling, logging, and connection
management
• Type Safe: Full type hints for better IDE support and code reliability
• Flexible: Support for any pickleable Python object
• Configurable: Customizable expiration times, key prefixes, and connection settings
• Thread Safe: Safe for use in multi-threaded applications
• Health Monitoring: Built-in health checks and statistics
Installation
Bash
pip install redis-cache-service
For development dependencies:
Bash
pip install redis-cache-service[dev]
Quick Start
ppyytthhoonn 33..77+
LLiicceennssee MIITT
Python
from redis_cache_service import RedisCacheService
# Initialize the cache service
cache = RedisCacheService(host='localhost', port=6379)
# Store data
user_data = {'name': 'John Doe', 'email': 'john@example.com', 'age': 30}
cache.set('users', 'user_123', user_data, expire=3600) # 1 hour expiration
# Retrieve data
retrieved_user = cache.get('users', 'user_123')
print(retrieved_user) # {'name': 'John Doe', 'email': 'john@example.com',
'age': 30}
# Delete data
cache.delete('users', 'user_123')
# Clear entire category
cache.clear_category('users')
Advanced Usage
Using with Redis URL
Python
from redis_cache_service import create_cache_service
# Create cache service from Redis URL
cache = create_cache_service('redis://localhost:6379/0')
Health Monitoring
Python
# Check cache health
if cache.health_check():
print("Cache is healthy!")
# Get detailed statistics
stats = cache.get_stats()
print(f"Redis available: {stats['redis_available']}")
print(f"Fallback entries: {stats['fallback_entries']}")
Error Handling
The cache service is designed to be resilient. If Redis becomes unavailable, it automatically
falls back to an in-memory cache:
Python
# This will work even if Redis is down
cache.set('temp', 'key1', 'value1')
value = cache.get('temp', 'key1') # Returns 'value1'
Configuration
Basic Configuration
Python
cache = RedisCacheService(
host='localhost', # Redis host
port=6379, # Redis port
db=0, # Redis database number
prefix='myapp:', # Key prefix for organization
connect_timeout=3 # Connection timeout in seconds
)
Environment Variables
You can also configure using environment variables:
Bash
export REDIS_HOST=localhost
export REDIS_PORT=6379
export REDIS_DB=0
export CACHE_PREFIX=myapp:
API Reference
RedisCacheService
Methods
• set(category: str, key: str, value: Any, expire: int = 3600) -> bool
• Store a value in the cache
• Returns True if successful
• get(category: str, key: str) -> Optional[Any]
• Retrieve a value from the cache
• Returns the cached value or None if not found
• delete(category: str, key: str) -> bool
• Delete a value from the cache
• Returns True if successful
• clear_category(category: str) -> int
• Clear all entries in a specific category
• Returns the number of entries deleted
• health_check() -> bool
• Perform a health check on the cache service
• Returns True if healthy
• get_stats() -> Dict[str, Any]
• Get cache statistics and health information
Testing
Run the test suite:
Bash
pytest tests/
Run with coverage:
Bash
pytest --cov=redis_cache_service tests/
Examples
Caching API Responses
Python
import requests
from redis_cache_service import RedisCacheService
cache = RedisCacheService()
def get_user_data(user_id):
# Try to get from cache first
cached_data = cache.get('api_responses', f'user_{user_id}')
if cached_data:
return cached_data
# Fetch from API if not cached
response = requests.get(f'https://api.example.com/users/{user_id}')
user_data = response.json()
# Cache for 30 minutes
cache.set('api_responses', f'user_{user_id}', user_data, expire=1800)
return user_data
Caching Expensive Computations
Python
import time
from redis_cache_service import RedisCacheService
cache = RedisCacheService()
def expensive_computation(n):
# Check cache first
result = cache.get('computations', f'fib_{n}')
if result is not None:
return result
# Perform expensive computation
if n <= 1:
result = n
else:
result = expensive_computation(n-1) + expensive_computation(n-2)
# Cache the result for 1 hour
cache.set('computations', f'fib_{n}', result, expire=3600)
return result
Contributing
Contributions are welcome! Please feel free to submit a Pull Request. For major changes,
please open an issue first to discuss what you would like to change.
1. Fork the repository
2. Create your feature branch ( git checkout -b feature/AmazingFeature )
3. Commit your changes ( git commit -m 'Add some AmazingFeature' )
4. Push to the branch ( git push origin feature/AmazingFeature )
5. Open a Pull Request
License
This project is licensed under the MIT License - see the LICENSE file for details.
Changelog
v1.0.0
• Initial release
• Redis caching with automatic fallback
• Comprehensive error handling
• Health monitoring and statistics
• Full type hints support
Support
If you encounter any issues or have questions, please open an issue on GitHub.
