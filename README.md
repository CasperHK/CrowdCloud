# CrowdCloud

**CrowdCloud** is a lightweight, high-performance self-hosted cloud storage and sync platform built with the **Grit Framework** in Go. Designed as a modern, efficient alternative to monolithic PHP-based cloud solutions like Nextcloud, CrowdCloud offers fast file management, modular microservices/plugins, multi-tenant workspace isolation, and native S3/local storage integration.

---

## 🌟 Key Features

* **High-Performance Core**: Built on top of Go and Grit Framework for ultra-fast response times and minimal system footprint.
* **Storage Provider Agnostic**: Native support for Local Filesystem, AWS S3, MinIO, and POSIX-compliant storage backends.
* **Modular Plugin Architecture**: Easily extend functionality with custom Grit middleware and plugins (e.g., Office Preview, Media Transcoding, WebDAV).
* **Multi-Tenant & RBAC Isolation**: Fine-grained role-based access control, workspace sharing, and link creation with security expiry.
* **Real-time Sync & Events**: Event-driven sync pipeline using WebSocket and background job workers for concurrent file processing.
* **REST & WebDAV APIs**: Standardized RESTful endpoints alongside WebDAV support for desktop and mobile clients.

---

## 🏗️ Architecture Overview



```
                      ┌───────────────────────────┐
                      │   Web UI / Mobile Apps    │
                      └─────────────┬─────────────┘
                                    │ (HTTP / WebDAV / WS)
                                    ▼
                      ┌───────────────────────────┐
                      │    Caddy / Nginx Proxy    │
                      └─────────────┬─────────────┘
                                    │
                                    ▼
                      ┌───────────────────────────┐
                      │   CrowdCloud (Grit App)   │
                      │ ┌───────────────────────┐ │
                      │ │   Auth & RBAC Middleware│ │
                      │ ├───────────────────────┤ │
                      │ │   VFS & File Router   │ │
                      │ ├───────────────────────┤ │
                      │ │ Plugin System (Grit)  │ │
                      │ └───────────────────────┘ │
                      └──────┬────────────┬───────┘
                             │            │
        ┌────────────────────┘            └────────────────────┐
        ▼                                                      ▼



┌───────────────────────┐                              ┌───────────────────────┐
│ PostgreSQL / SQLite   │                              │ Local / S3 / MinIO    │
│ (Metadata & DB)       │                              │ (Object Storage)      │
└───────────────────────┘                              └───────────────────────┘

```

---

## 🚀 Quick Start

### Prerequisites

* **Go**: `1.22` or higher
* **Database**: PostgreSQL `14+` (or SQLite for development)
* **Storage**: Local directory or S3-compatible bucket

### Installation & Run

1. **Clone the repository**:
   ```bash
   git clone [https://github.com/your-org/crowdcloud.git](https://github.com/your-org/crowdcloud.git)
   cd crowdcloud
   ```

2. **Configure environment**:
   Copy `.env.example` to `.env` and set your credentials:
   ```bash
   cp .env.example .env
   ```


3. **Run database migrations**:
   ```bash
   go run main.go migrate
   
   ```


4. **Start the server**:
   ```bash
   go run main.go serve
   ```



The CrowdCloud service will start on `http://localhost:8080`.

---

## ⚙️ Configuration (`.env`)

```env
APP_NAME=CrowdCloud
APP_ENV=production
APP_PORT=8080
APP_SECRET=your-32-byte-secret-key

# Database
DB_DRIVER=postgres
DB_DSN="host=localhost user=crowdcloud password=secret dbname=crowdcloud_db port=5432 sslmode=disable"

# Storage Configuration
STORAGE_DRIVER=local # Options: local, s3, minio
STORAGE_LOCAL_PATH=./data/storage

# S3 / MinIO (Optional)
S3_ENDPOINT=[http://127.0.0.1:9000](http://127.0.0.1:9000)
S3_BUCKET=crowdcloud-data
S3_ACCESS_KEY=minioadmin
S3_SECRET_KEY=minioadmin
S3_REGION=us-east-1

```

---

## 🔌 Plugin System

CrowdCloud leverages Grit's modular framework to enable easy expansion. Official and community plugins can be placed under `/plugins`:

* `plugins/preview`: Image thumbnailing, PDF generation, and audio/video preview support.
* `plugins/webdav`: WebDAV gateway for mounting cloud drives on Windows/macOS/Linux natively.
* `plugins/activity`: Audit logging, user history, and storage quota monitoring.

---

## 🛠️ Development

```bash
# Install dependencies
go mod download

# Run unit tests
go test ./... -v

# Build binary
go build -o crowdcloud .

```

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](https://www.google.com/search?q=LICENSE) file for details.

