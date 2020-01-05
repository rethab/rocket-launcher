[![Build Status](https://travis-ci.org/rethab/rocket-launcher.svg?branch=master)](https://travis-ci.org/rethab/rocket-launcher)

# rocket-launcher
Dynamic configuration is a difficult endavour with [rocket](https://rocket.rs/). You can either hard-code all your values in your Rocket.toml or override certain values with `ROCKET_{PARAM}` (see [guide](https://rocket.rs/v0.4/guide/configuration/#environment-variables)). However both of these options can be quite cumbersome. What this script allows you is to define variables in your Rocket.toml which are replaced when the application is started.

The variables are replaced by prefix: This script basically takes all your environment variables that start with `ROCKET_REPLACE` and tries to replace them in your rocket file. If after this operation any `ROCKET_REPLACE` variables are left in your Rocket.toml, the script exits with an error.

## Setup
Rocket.toml:
```toml
[global.databases]
my-db-db = "ROCKET_REPLACE_DB_PW"
```

Launch app:
```bash
$ ROCKET_REPLACE_DB_PW="my-secret-pw" ./launch-rocket.sh --app my-rocket-app
```

## Docker
This sript is made for use in Docker. Add these lines to your Dockerfile:
```Dockerfile
RUN curl -o /launch-rocket.sh https://raw.githubusercontent.com/rethab/rocket-launcher/master/launch-rocket.sh
CMD /launch-rocket.sh --app my-rocket-app
```

## Options
- `--no-replace` start app without replacing any variables
- `--insecure` prints database (and other) credentials during startup


## Heroku
This script transparently works with heroku by taking the `$PORT` variable (which is set by heroku) and setting its value to `$ROCKET_PORT` (which is the one used by rocket). When deploying to heroku, don't forget to also define `$ROCKET_ENV`, because otherwise rocket binds to `localhost` which won't work.

# Contributions
Contributions are very welcome. When making a pull request, please make sure the script passes all checks from [shellcheck](https://github.com/koalaman/shellcheck).
