# Go redis-sessions

This is a plugin for [github.com/RichardKnop/go-oauth2-server](https://github.com/RichardKnop/go-oauth2-server) that provides redis storage for sessions

## Installation
~~~go
go get https://github.com/adam-hanna/redis-sessions
~~~

## Usage
Within `github.com/RichardKnop/go-oauth2-server/cmd/run_server.go`

~~~go
// RunServer runs the app
func RunServer(configBackend string) error {
    ...

    // configure redis for session store
    sessionSecrets := make([][]byte, 1)
    sessionSecrets[0] = []byte(cnf.Session.Secret)
    redisConfig := redis.ConfigType{
        Size:           10,
        Network:        "tcp",
        Address:        ":6379",
        Password:       "",
        SessionSecrets: sessionSecrets,
    }

    // start the services
    services.UseSessionService(redis.NewService(cnf, redisConfig))
    if err := services.InitServices(cnf, db); err != nil {
        return err
    }
    defer services.CloseServices()

    ...
}
~~~