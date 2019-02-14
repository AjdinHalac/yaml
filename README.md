# YAML support for the Go language

Modified version of https://github.com/go-yaml/yaml

This version of yaml parser will also achieve Java Spring functionality where 
values that look like ${VALUE} will get replaced with values from environment variables.

##Usage example:

type Config struct {
    Port     string   `yaml:"port"`
    RabbitMQ RabbitMQ `yaml:"rabbitmq"`
}

type RabbitMQ struct {
    Host     string `yaml:"host"`
    Port     string `yaml:"port"`
    Username string `yaml:"username"`
    Password string `yaml:"password"`
    Vhost    string `yaml:"vhost"`
}

func main() {
    var config Config
    file, err := ioutil.ReadFile("application.yaml")
    if err != nil {
        panic(err)
    }
    yaml.Unmarshal(file, &config)
    spew.Dump(config)
}
This is how application.yaml looks like:

port: ${SERVER_PORT}
rabbitmq:
  host: ${RMQ_HOST}
  port: ${RMQ_PORT}
  username: ${RMQ_USERNAME}
  password: ${RMQ_PASSWORD}
  vhost: test

vhost value will get parsed as usual, while everything surrounded with "${" and "}" will get replaced with environment variables.
