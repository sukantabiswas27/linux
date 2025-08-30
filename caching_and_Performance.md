
# Performance: systemctl Tuning & Caching

## 1. systemctl Tuning

### What is `systemctl`?

`systemctl` is a command used to manage **systemd**, which is the init system and service manager for Linux distributions. **systemd** is responsible for starting and managing system services (daemons), handling system boot processes, and more.

### Tuning `systemd` Services for Performance

1. **Understanding `systemd` Units**:
   - A **unit** is a configuration file that describes a system service or resource.
   - Types of units:
     - **Service** units (`*.service`): Define services that are run on startup or manually.
     - **Socket** units (`*.socket`): Manage network sockets.
     - **Target** units (`*.target`): Represent groups of services to be started together.
   
2. **Tuning Services**:
   You can optimize your system by disabling or modifying system services that are unnecessary or resource-hungry.

   - **List all services**:
     ```bash
     systemctl list-units --type=service
     ```

   - **Disable unnecessary services** (e.g., Bluetooth if not used):
     ```bash
     sudo systemctl disable bluetooth.service
     ```

   - **Enable critical services**:
     ```bash
     sudo systemctl enable apache2.service
     ```

   - **Check the status of a service**:
     ```bash
     systemctl status apache2.service
     ```

   - **Start/Stop services**:
     - Start a service:
       ```bash
       sudo systemctl start nginx.service
       ```
     - Stop a service:
       ```bash
       sudo systemctl stop nginx.service
       ```

   - **Optimize boot services**:
     Services that are needed for booting the system (e.g., `network.target`, `multi-user.target`) should be optimized for performance. You can remove unnecessary services from starting automatically by using:
     ```bash
     sudo systemctl disable <service-name>.service
     ```

3. **Performance Tweaks Using `systemd`**:
   - **Adjusting the CPU scheduling**:
     Modify `systemd` to use CPU-affinity for certain services. For example, limiting a service to use only specific CPUs (e.g., for CPU-intensive services).
   
     Add this to a service's override configuration:
     ```bash
     sudo systemctl edit <service-name>
     ```
     Then add:
     ```ini
     [Service]
     CPUSchedulingPolicy=other
     ```

   - **Tuning memory limits**:
     For memory-intensive applications, you can limit the memory available to them.
     Example:
     ```ini
     [Service]
     MemoryLimit=512M
     ```

4. **Auto-tuning on Boot**:
   Modify `systemd` to allow certain services to only start when needed, instead of starting on boot.
   - Modify the `systemd` unit file:
     ```bash
     sudo systemctl edit <service-name>
     ```
     Add or modify the following:
     ```ini
     [Service]
     Restart=on-failure
     ```

---

## 2. Caching

### What is Caching?

**Caching** is the process of storing data temporarily to reduce access times, improve speed, and reduce the load on underlying data sources. It’s used at various levels (e.g., application, database, filesystem) to enhance performance.

### Types of Caching

1. **Disk Caching**:
   - **Linux Page Cache**: Linux uses a **page cache** to store file data in memory. This speeds up read and write operations, as frequently accessed files are stored in RAM.
   - **How to monitor page cache**:
     ```bash
     free -m
     ```

2. **Web Caching**:
   - **Proxy caches**: Web proxies can cache web pages for faster access.
   - **Browser caching**: Modern web browsers cache images, scripts, and web pages to speed up load times on subsequent visits.
   - **Cache control headers**: Websites can control caching behavior using HTTP headers like `Cache-Control`.

3. **Application Caching**:
   - **Memory-based caches**: Tools like **Memcached** or **Redis** store data in memory to provide faster access.
   - **Common use cases**:
     - Caching API responses
     - Storing session data
     - Database query result caching

4. **Database Caching**:
   - **Database query caching**: Databases like MySQL and PostgreSQL have caching mechanisms to store query results.
     - MySQL:
       - Query cache (deprecated in MySQL 8.0)
       - InnoDB buffer pool caches data in memory.
     - PostgreSQL:
       - Shared buffers store cached data.

---

### Implementing System-Level Caching

1. **Optimize Linux Disk Cache**:
   You can control how the Linux page cache behaves using `vm` settings.

   Example: **Change the swappiness** (how often the system swaps memory pages to disk):
   ```bash
   sudo sysctl vm.swappiness=10
   ```
   This lowers the likelihood of the system swapping memory pages to disk.

2. **System Cache Management with `vmtouch`**:
   The `vmtouch` tool can be used to lock files into memory to prevent them from being swapped out.
   - Install it:
     ```bash
     sudo apt install vmtouch
     ```

   - Lock a file into memory:
     ```bash
     sudo vmtouch -t /path/to/file
     ```

3. **Web Caching with `nginx` or `Apache`**:
   - **nginx**:
     To cache static files using `nginx`:
     ```nginx
     location /static/ {
         expires 30d;
         add_header Cache-Control "public";
     }
     ```

   - **Apache**:
     Enable caching modules:
     ```bash
     sudo a2enmod cache
     sudo a2enmod cache_disk
     ```

     Add caching configuration in the `apache2.conf` or specific site configuration:
     ```apache
     CacheEnable disk /static/
     CacheHeader on
     ```

---

### Optimizing Application-Level Caching

1. **Redis Caching**:
   Redis is an in-memory key-value store, often used for caching.
   - Install Redis:
     ```bash
     sudo apt install redis-server
     ```

   - Basic Redis configuration in your application (e.g., Python, Node.js):
     ```python
     import redis
     r = redis.Redis(host='localhost', port=6379, db=0)
     r.set('mykey', 'Hello, World!')
     print(r.get('mykey'))
     ```

2. **Memcached**:
   - Memcached is another in-memory caching system.
   - Install Memcached:
     ```bash
     sudo apt install memcached
     ```

   - Example of using Memcached in a Python app:
     ```python
     import memcache
     mc = memcache.Client(['127.0.0.1:11211'], debug=0)
     mc.set("some_key", "some_value")
     print(mc.get("some_key"))
     ```

---

### File System Caching: Tune `sysctl` for I/O Performance

- **Increase file system cache size**:
   You can increase the **vm.dirty_ratio** setting to give the system more memory for file system cache.
   ```bash
   sudo sysctl -w vm.dirty_ratio=60
   ```

   This will allow up to 60% of the system's memory to be used for file system cache.

---

### Monitoring and Testing Cache Performance

1. **Check cache usage**:
   Use `free` and `vmstat` to monitor system cache.
   ```bash
   free -h
   vmstat -s
   ```

2. **Test Cache Hit Rate**:
   Measure the cache hit rate in your application (especially for databases and caching systems like Redis). Low cache hit rates can indicate that your cache size or strategy needs adjustment.

---

### Conclusion

- **`systemctl` Tuning**: Optimize system services by managing the units, enabling only necessary services, and fine-tuning parameters like CPU and memory usage.
- **Caching**: Improve performance by storing frequently accessed data in memory (file system cache, web, application, and database caching). Tools like Redis, Memcached, and `nginx` caching help reduce latency and server load.

---
