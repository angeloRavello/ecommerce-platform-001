# ecommerce-platform-001

Reactive e-commerce platform built as a microservices monorepo. Both services use **Spring Boot 3.5 / Java 17 / Spring WebFlux** (non-blocking end-to-end) with **R2DBC + H2 in-memory** databases.

## Services

| Service | Port | Architecture | Description |
|---|---|---|---|
| [`product-service`](product-service/README.md) | 8081 | Layered | Product catalog and stock management |
| [`order-service`](order-service/README.md) | 8080 | Hexagonal | Order creation and listing |

`order-service` depends on `product-service` — start `product-service` first.

## Cloning

This repository uses git submodules. Use the following command to clone everything in one step:

```bash
git clone --recurse-submodules <repo-url>
```

If you already cloned without submodules, initialize them with:

```bash
git submodule update --init --recursive
```

## Quick start

```bash
# Terminal 1 — product-service
cd product-service
./mvnw spring-boot:run

# Terminal 2 — order-service
cd order-service
./mvnw spring-boot:run
```

## API overview

### product-service (`http://localhost:8081`)

| Method | Path | Description |
|---|---|---|
| `GET` | `/api/products` | List all products |
| `GET` | `/api/products/{id}` | Get product by ID |
| `POST` | `/api/products/reserve` | Reserve stock (used by order-service) |

### order-service (`http://localhost:8080`)

| Method | Path | Description |
|---|---|---|
| `POST` | `/api/orders` | Create an order |
| `GET` | `/api/orders` | List all orders |

## Bruno collection

API requests are provided as a [Bruno](https://www.usebruno.com/) collection under [`ecommerce-platform-001/`](ecommerce-platform-001/).

```
ecommerce-platform-001/
├── opencollection.yml
├── environments/
│   └── local.yml              # product_service_url, order_service_url
├── product-service/
│   ├── findAll.yml            # GET  /api/products
│   ├── findById.yml           # GET  /api/products/:productId
│   └── reserve.yml            # POST /api/products/reserve
└── order-service/
    ├── createOrder.yml        # POST /api/orders
    └── findAll.yml            # GET  /api/orders
```

To use: open Bruno, click **Open Collection**, select the `ecommerce-platform-001/` folder. Switch to the **local** environment before running requests.

The `local` environment pre-configures:

| Variable | Default |
|---|---|
| `product_service_url` | `http://localhost:8081` |
| `order_service_url` | `http://localhost:8080` |

## Repository structure

```
ecommerce-platform-001/
├── order-service/              # git submodule
├── product-service/            # git submodule
└── ecommerce-platform-001/     # Bruno API collection
```

Both services are git submodules with their own repositories. All Maven commands must be run from within each service directory.