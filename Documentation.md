# Kubernetes Vite Portfolio

[![Docker Hub](https://img.shields.io/badge/Docker%20Hub-bunna44%2Fk8s--portfolio-blue?logo=docker)](https://hub.docker.com/r/bunna44/k8s-portfolio)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-Deployed-326CE5?logo=kubernetes)](https://kubernetes.io/)
[![React](https://img.shields.io/badge/React-18-61DAFB?logo=react)](https://reactjs.org/)
[![Vite](https://img.shields.io/badge/Vite-4.4-646CFF?logo=vite)](https://vitejs.dev/)

A modern DevOps portfolio application demonstrating containerization, orchestration, and automated deployment practices. This project showcases a React Vite frontend deployed on Kubernetes with production-ready configurations.

## 🎯 Project Overview

This portfolio demonstrates real-world DevOps practices by deploying a React application to a Kubernetes homelab environment. The project emphasizes container orchestration, multi-architecture builds, and infrastructure automation.

### Key Features

- ⚛️ **Modern Frontend**: React 18 with Vite for lightning-fast development
- 🐳 **Containerization**: Multi-stage Docker builds for optimized production images
- ☸️ **Kubernetes Native**: Deployed with proper resource management and health checks
- 🌐 **External Access**: NodePort service for network accessibility
- 🏗️ **Multi-Architecture**: ARM64/AMD64 support for diverse deployment environments
- 📊 **Resource Optimized**: Configured with appropriate CPU and memory limits
- 🔄 **Production Ready**: Health checks, readiness probes, and restart policies

## 🏗️ Architecture

### System Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   Development   │    │   Container     │    │   Kubernetes    │
│   Environment   │───▶│   Registry      │───▶│   Cluster       │
│                 │    │                 │    │                 │
│ MacBook M1/M2   │    │ Docker Hub      │    │ Ubuntu/Proxmox  │
│ (ARM64)         │    │ Multi-arch      │    │ (AMD64)         │
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

### Container Architecture

```
┌─────────────────────────────────────┐
│              Docker Image           │
│  ┌─────────────────────────────────┐│
│  │         Build Stage             ││
│  │  ┌─────────────────────────────┐││
│  │  │     Node.js 18 Alpine       │││
│  │  │   - Install dependencies    │││
│  │  │   - Build React app         │││
│  │  │   - Generate optimized dist │││
│  │  └─────────────────────────────┘││
│  └─────────────────────────────────┘│
│  ┌─────────────────────────────────┐│
│  │       Production Stage          ││
│  │  ┌─────────────────────────────┐││
│  │  │      Nginx Alpine           │││
│  │  │   - Serve static files      │││
│  │  │   - Handle client routing   │││
│  │  │   - Health check endpoint   │││
│  │  └─────────────────────────────┘││
│  └─────────────────────────────────┘│
└─────────────────────────────────────┘
```

### Kubernetes Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        my-portfolio Namespace                    │
│                                                                 │
│  ┌─────────────────┐                 ┌─────────────────────────┐│
│  │   NodePort      │                 │      Deployment         ││
│  │   Service       │                 │                         ││
│  │                 │                 │  ┌─────────────────────┐││
│  │ External:30081 ◄┼─────────────────┼─▶│      Pod 1          │││
│  │ Internal:80     │                 │  │ bunna44/k8s-portf.. │││
│  │                 │                 │  │ Resources: 64Mi/50m │││
│  └─────────────────┘                 │  └─────────────────────┘││
│                                      │                         ││
│                                      │  ┌─────────────────────┐││
│                                      │  │      Pod 2          │││
│                                      │  │ bunna44/k8s-portf.. │││
│                                      │  │ Resources: 64Mi/50m │││
│                                      │  └─────────────────────┘││
│                                      └─────────────────────────┘│
└─────────────────────────────────────────────────────────────────┘
```

## 🛠️ Technology Stack

### Frontend
- **React 18**: Modern component-based UI framework
- **Vite 4.4**: Next-generation frontend build tool
- **CSS3**: Custom styling with responsive design
- **JavaScript ES6+**: Modern JavaScript features

### DevOps & Infrastructure
- **Docker**: Containerization platform
- **Kubernetes**: Container orchestration
- **Docker Hub**: Container registry
- **Nginx**: Production web server
- **Alpine Linux**: Lightweight base images

### Development Tools
- **Node.js 18**: JavaScript runtime
- **npm**: Package manager
- **Docker Buildx**: Multi-platform builds

## 📋 Prerequisites

### Local Development
- **Node.js** 18 or higher
- **npm** or **yarn**
- **Docker** with buildx support
- **Docker Hub** account

### Kubernetes Deployment
- **Kubernetes cluster** (v1.20+)
- **kubectl** configured and authenticated
- **Internet access** for image pulling

### Network Requirements
- Access to Docker Hub (`docker.io`)
- NodePort range (30000-32767) available
- Cluster nodes accessible from your network

## 🚀 Quick Start

### 1. Clone and Setup

```bash
# Clone the repository
git clone https://github.com/yourusername/k8s-vite-portfolio.git
cd k8s-vite-portfolio

# Install dependencies
npm install

# Start development server
npm run dev
```

Visit `http://localhost:3000` to see the application.

### 2. Build and Deploy

```bash
# Build multi-architecture Docker image
docker buildx build --platform linux/amd64,linux/arm64 -t bunna44/k8s-portfolio:latest . --push

# Deploy to Kubernetes
kubectl apply -f k8s/

# Get external access URL
NODE_IP=$(kubectl get nodes -o jsonpath='{.items[0].status.addresses[?(@.type=="InternalIP")].address}')
NODE_PORT=$(kubectl get svc portfolio-nodeport -n my-portfolio -o jsonpath='{.spec.ports[0].nodePort}')
echo "Access your app at: http://$NODE_IP:$NODE_PORT"
```

## 📁 Project Structure

```
k8s-vite-portfolio/
├── 📄 README.md                      # Project documentation
├── 📄 package.json                   # Node.js dependencies and scripts
├── 📄 vite.config.js                 # Vite configuration
├── 📄 Dockerfile                     # Multi-stage container build
├── 📄 .dockerignore                  # Docker build exclusions
├── 📄 nginx.conf                     # Nginx web server configuration
├── 📄 .gitignore                     # Git exclusions
│
├── 📂 src/                           # React application source
│   ├── 📄 main.jsx                   # Application entry point
│   ├── 📄 App.jsx                    # Main React component
│   └── 📄 App.css                    # Application styles
│
├── 📂 public/                        # Static assets
│   └── 📄 index.html                 # HTML template
│
├── 📂 k8s/                          # Kubernetes manifests
│   ├── 📄 namespace.yaml             # Namespace definition
│   ├── 📄 deployment.yaml            # Application deployment
│   ├── 📄 service.yaml               # ClusterIP service
│   └── 📄 service-nodeport.yaml      # External access service
│
└── 📂 scripts/                      # Automation scripts
    ├── 📄 build.sh                   # Build Docker image
    ├── 📄 deploy.sh                  # Deploy to Kubernetes
    └── 📄 start.sh                   # Port-forward for local access
```

## 🔧 Development Workflow

### Local Development

```bash
# Start development server with hot reload
npm run dev

# Build for production
npm run build

# Preview production build
npm run preview
```

### Container Development

```bash
# Build and test locally
docker build -t k8s-portfolio-local .
docker run -p 8080:80 k8s-portfolio-local

# Build for production (multi-architecture)
docker buildx build --platform linux/amd64,linux/arm64 -t bunna44/k8s-portfolio:latest . --push
```

### Kubernetes Development

```bash
# Apply all manifests
kubectl apply -f k8s/

# Watch deployment status
kubectl get pods -n my-portfolio -w

# Check logs
kubectl logs -n my-portfolio -l app=portfolio

# Update deployment with new image
kubectl set image deployment/portfolio-app portfolio=bunna44/k8s-portfolio:latest -n my-portfolio
```

## 🚀 Deployment Guide

### Step 1: Build and Push Image

```bash
# Build for your target architecture
docker buildx build --platform linux/amd64 -t bunna44/k8s-portfolio:latest . --push
```

### Step 2: Deploy to Kubernetes

```bash
# Create namespace and deploy application
kubectl apply -f k8s/namespace.yaml
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
kubectl apply -f k8s/service-nodeport.yaml
```

### Step 3: Verify Deployment

```bash
# Check all resources
kubectl get all -n my-portfolio

# Verify pods are running
kubectl get pods -n my-portfolio

# Check service endpoints
kubectl get endpoints -n my-portfolio
```

### Step 4: Access Application

```bash
# Get access information
NODE_IP=$(kubectl get nodes -o jsonpath='{.items[0].status.addresses[?(@.type=="InternalIP")].address}')
NODE_PORT=$(kubectl get svc portfolio-nodeport -n my-portfolio -o jsonpath='{.spec.ports[0].nodePort}')

echo "🌐 Application URL: http://$NODE_IP:$NODE_PORT"
```

## 🔍 Monitoring and Maintenance

### Health Checks

The application includes built-in health monitoring:

```bash
# Check application health
curl http://[NODE-IP]:[NODE-PORT]/health

# Monitor pod health
kubectl describe pods -n my-portfolio

# View resource usage
kubectl top pods -n my-portfolio
```

### Scaling

```bash
# Scale deployment
kubectl scale deployment portfolio-app --replicas=3 -n my-portfolio

# Verify scaling
kubectl get pods -n my-portfolio
```

### Updates

```bash
# Rolling update with new image
kubectl set image deployment/portfolio-app portfolio=bunna44/k8s-portfolio:v2.0 -n my-portfolio

# Check rollout status
kubectl rollout status deployment/portfolio-app -n my-portfolio

# Rollback if needed
kubectl rollout undo deployment/portfolio-app -n my-portfolio
```

## 🛠️ Configuration

### Resource Limits

The deployment is configured with conservative resource limits suitable for homelab environments:

```yaml
resources:
  requests:
    memory: "64Mi"
    cpu: "50m"
  limits:
    memory: "128Mi"
    cpu: "100m"
```

### Environment Variables

The application supports these environment variables:

- `NODE_ENV`: Runtime environment (development/production)
- `POD_NAME`: Kubernetes pod name (auto-injected)
- `POD_IP`: Kubernetes pod IP (auto-injected)

### Network Configuration

- **Internal Port**: 80 (nginx)
- **External Port**: Auto-assigned (30000-32767 range)
- **Service Type**: NodePort for external access
- **Protocol**: HTTP (HTTPS can be added via Ingress)

## 🔧 Troubleshooting

### Common Issues

#### 1. ImagePullBackOff
```bash
# Check image name and availability
kubectl describe pod [POD-NAME] -n my-portfolio

# Verify image exists on Docker Hub
docker pull bunna44/k8s-portfolio:latest
```

#### 2. CrashLoopBackOff
```bash
# Check container logs
kubectl logs [POD-NAME] -n my-portfolio --previous

# Common causes: Architecture mismatch, nginx config errors
```

#### 3. Architecture Mismatch
```bash
# Build for correct architecture
docker buildx build --platform linux/amd64 -t bunna44/k8s-portfolio:latest . --push

# Restart deployment
kubectl rollout restart deployment/portfolio-app -n my-portfolio
```

#### 4. Service Not Accessible
```bash
# Check service endpoints
kubectl get endpoints -n my-portfolio

# Verify NodePort is accessible
telnet [NODE-IP] [NODE-PORT]

# Check firewall rules on cluster nodes
```

### Debug Commands

```bash
# Get comprehensive cluster info
kubectl cluster-info

# Describe problematic resources
kubectl describe deployment portfolio-app -n my-portfolio
kubectl describe service portfolio-nodeport -n my-portfolio

# Check events
kubectl get events -n my-portfolio --sort-by='.lastTimestamp'

# Access pod for debugging
kubectl exec -it [POD-NAME] -n my-portfolio -- /bin/sh
```

## 🔐 Security Considerations

### Container Security
- ✅ **Non-root user**: Nginx runs as non-privileged user
- ✅ **Minimal base image**: Alpine Linux reduces attack surface
- ✅ **Multi-stage build**: Production image excludes build tools
- ✅ **Regular updates**: Base images updated regularly

### Kubernetes Security
- ✅ **Namespace isolation**: Application runs in dedicated namespace
- ✅ **Resource limits**: CPU and memory constraints prevent resource exhaustion
- ✅ **Health checks**: Automatic restart of unhealthy containers
- ✅ **Network policies**: Can be added for additional network security

### Recommendations
- 🔒 **Add TLS**: Implement HTTPS via Ingress controller
- 🔒 **Network policies**: Restrict inter-pod communication
- 🔒 **RBAC**: Implement role-based access control
- 🔒 **Pod security**: Add Pod Security Standards

## 📈 Performance Optimization

### Container Optimization
- **Multi-stage builds**: Separate build and runtime environments
- **Layer caching**: Optimized Dockerfile layer ordering
- **Gzip compression**: Nginx configured for asset compression
- **Static asset caching**: Long-term caching for immutable assets

### Kubernetes Optimization
- **Resource requests**: Appropriate CPU/memory allocation
- **Replica count**: Multiple pods for high availability
- **Readiness probes**: Traffic only to ready pods
- **Rolling updates**: Zero-downtime deployments

## 🚀 Future Enhancements

### Planned Features
- [ ] **Ingress Controller**: Custom domain support with SSL/TLS
- [ ] **Horizontal Pod Autoscaler**: Automatic scaling based on metrics
- [ ] **Monitoring Stack**: Prometheus and Grafana integration
- [ ] **CI/CD Pipeline**: GitHub Actions for automated deployment
- [ ] **Helm Chart**: Package management for easier deployment
- [ ] **Service Mesh**: Istio integration for advanced traffic management

### Infrastructure Improvements
- [ ] **GitOps**: ArgoCD for declarative deployment
- [ ] **Secrets Management**: External secrets operator
- [ ] **Backup Strategy**: Persistent volume backup solutions
- [ ] **Multi-environment**: Development, staging, production environments

## 🤝 Contributing

### Development Setup

1. **Fork the repository**
2. **Create feature branch**: `git checkout -b feature/amazing-feature`
3. **Make changes** and test locally
4. **Commit changes**: `git commit -m 'Add amazing feature'`
5. **Push to branch**: `git push origin feature/amazing-feature`
6. **Open Pull Request**

### Guidelines
- Follow existing code style
- Add appropriate documentation
- Test changes locally before submitting
- Update README if adding new features

## 📞 Support

### Getting Help
- **Issues**: Report bugs via GitHub Issues
- **Discussions**: Ask questions in GitHub Discussions
- **Documentation**: Check this README for common solutions

### Useful Resources
- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Docker Documentation](https://docs.docker.com/)
- [React Documentation](https://reactjs.org/docs/)
- [Vite Documentation](https://vitejs.dev/guide/)

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- **Kubernetes Community** for excellent documentation and tools
- **React Team** for the amazing frontend framework
- **Vite Team** for the lightning-fast build tool
- **Docker Team** for containerization technology
- **Nginx Team** for the robust web server

---

## 📊 Project Metrics

- **Container Size**: ~23MB (production image)
- **Build Time**: ~2-3 minutes (multi-architecture)
- **Memory Usage**: ~64MB (typical runtime)
- **CPU Usage**: ~50m (under normal load)
- **Startup Time**: ~10-15 seconds (including health checks)

---

**Built with ❤️ for DevOps learning and demonstration**

*This project showcases modern containerization and orchestration practices suitable for portfolio presentation and technical interviews.*