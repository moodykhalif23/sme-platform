# Build stage
FROM golang:1.24.5 AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY internal/ ./internal/
COPY internal/pkg/services/users/ ./users/
WORKDIR /app/users
RUN CGO_ENABLED=0 GOOS=linux go build -o user-service main.go

# Run stage
FROM debian:bullseye-slim
WORKDIR /app
COPY --from=builder /app/users/user-service .
EXPOSE 8080
ENTRYPOINT ["/app/user-service"]
