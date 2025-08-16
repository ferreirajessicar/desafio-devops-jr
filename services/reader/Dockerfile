FROM golang:1.23.0
WORKDIR /app
ADD . /app
RUN [ -f go.mod ] || (go mod init reader) && go mod tidy
EXPOSE 8080
CMD ["go","run","main.go"]