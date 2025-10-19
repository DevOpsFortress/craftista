# Build stage
FROM golang:1.20-alpine AS builder

WORKDIR /app

# Copy go mod files
COPY go.mod go.sum ./
RUN go mod download

# Copy source code
COPY . .

# Build the application
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o recommendation .

# Runtime stage
FROM gcr.io/distroless/static-debian12

WORKDIR /app

# Copy binary from builder
COPY --from=builder /app/recommendation .

# Copy static files and templates
COPY --from=builder /app/templates ./templates
COPY --from=builder /app/static ./static
COPY --from=builder /app/data ./data
COPY --from=builder /app/config.json .

# Expose port
EXPOSE 8080

# Run the application
CMD ["./recommendation"]
