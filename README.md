# Nginx Load Balancer

![Nginx](https://img.shields.io/badge/nginx-%23009639.svg?style=for-the-badge&logo=nginx&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)
![Linux](https://img.shields.io/badge/Linux-FCC624?style=for-the-badge&logo=linux&logoColor=black)

## Overview

High-performance Nginx load balancer configuration for distributing traffic across multiple backend servers with session persistence using IP hash algorithm.

## Features

- **IP Hash Load Balancing**: Ensures client session persistence
- **Multiple Algorithm Support**: Easy switching between load balancing methods
- **Weighted Distribution**: Configurable server weights for traffic distribution
- **Health Monitoring**: Automatic failover for unhealthy servers
- **AWS EC2 Integration**: Optimized for AWS infrastructure

## Load Balancing Algorithms

| Algorithm | Description | Use Case |
|-----------|-------------|----------|
| `round_robin` | Default, distributes requests evenly | General purpose |
| `least_conn` | Routes to server with fewest connections | Long-running requests |
| `ip_hash` | Routes based on client IP hash | Session persistence |
| `least_time` | Routes to fastest responding server | Performance critical |
| `random` | Random server selection | Simple distribution |
| `hash` | Custom hash-based routing | Custom routing logic |

## Configuration

### Backend Servers
- **172.31.0.122** (Weight: 1)
- **172.31.6.104** (Weight: 2) 
- **172.31.15.218** (Weight: 3)
- **172.31.0.81** (Weight: 4)

### Current Settings
- **Load Balancer**: IP Hash (active)
- **Port**: 80
- **Domain**: ec2-3-139-108-253.us-east-2.compute.amazonaws.com

## Quick Start

1. **Deploy Configuration**
   ```bash
   sudo cp nginx-loadbalancer /etc/nginx/sites-available/
   sudo ln -s /etc/nginx/sites-available/nginx-loadbalancer /etc/nginx/sites-enabled/
   ```

2. **Test Configuration**
   ```bash
   sudo nginx -t
   ```

3. **Reload Nginx**
   ```bash
   sudo systemctl reload nginx
   ```

## Switching Load Balancing Methods

Edit the `nginx-loadbalancer` file and uncomment desired algorithm:

```nginx
upstream backend_servers {
    # Uncomment one method:
    # least_conn;      # For connection-based balancing
    ip_hash;           # For session persistence (current)
    # random;          # For random distribution
}
```

## Monitoring

Check load balancer status:
```bash
sudo nginx -s reload
curl -I http://your-domain.com
```

## Architecture

```
Internet → Nginx Load Balancer → Backend Servers
                ↓
        [172.31.0.122:weight=1]
        [172.31.6.104:weight=2]
        [172.31.15.218:weight=3]
        [172.31.0.81:weight=4]
```

## Tech Stack

- **Web Server**: Nginx
- **Cloud Platform**: AWS EC2
- **Operating System**: Linux
- **Load Balancing**: IP Hash Algorithm
- **Protocol**: HTTP/HTTPS

## Performance Benefits

- **High Availability**: Automatic failover
- **Session Persistence**: IP-based routing
- **Scalability**: Easy server addition/removal
- **Low Latency**: Optimized request routing