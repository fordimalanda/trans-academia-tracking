FROM golang:1.22-alpine AS builder

WORKDIR /app

# Téléchargement des dépendances
COPY go.mod go.sum ./
RUN go mod download

# Copie du code source et compilation
COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o /ingestion-app ./cmd/main.go

# --- Stage de Production ---
FROM alpine:latest

WORKDIR /app

COPY --from=builder /ingestion-app .

EXPOSE 8080

CMD ["./ingestion-app"]