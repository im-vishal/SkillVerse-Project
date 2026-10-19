# Stage 1: Build
FROM golang:1.26-alpine as builder

WORKDIR /app

COPY . .
RUN go mod tidy && CGO_ENABLED=0 GOOS=linux go build -o skillverse .

# Stage 2: Run
FROM alpine:3.23

RUN apk --no-cache add ca-certificates

WORKDIR /app

COPY --from=builder /app/skillverse .

EXPOSE 8080

CMD ["./skillverse"]
