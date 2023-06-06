## Setup

1. git clone repository
```
$ git clone git@github.com:takato-noguchi/vue-todomvc.git
```

1. docker image build

```
$ docker-compose build
```

2. docker cotainer up
```
$ docker-compose up -d 
```

3. enter the container
```
$ docker-compose exec client /bin/sh
```

4. set up server
```
$ npm run serve
```

5. access localhost
```
$ http://localhost:18080
```