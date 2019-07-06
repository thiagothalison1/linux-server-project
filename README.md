# Linux Server Configuration

This document describes the Linux Server Configuration project.

## Overview

The project consists of an Amazon Lightsail server running the [Amazing Catalog Application](https://github.com/thiagothalison1/amazing-catalog-project) with a PostgresSQL database. The following sections describes:

1. [The project stack](#project-stack)
2. [The configuration overview](#configuration-overview)
3. [The server access guidelines](#server-access)
4. [The application access guidelines](#application-access)
5. [Third-party resources](#third-party-resources)

## Project Stack

| Name        | Description           | Version  |
| ------------- |:-------------:| -----|
| Ubuntu | Operational System | 16.04.6 LTS |
| PostgreSQL | Database Management System | 9.5.17 |
| Python | python interpreter | 2.7.12 |
| Apache | web application server | 2.4.18 |
| WSGI | Python application executor on apache server | 4.3.0-1

## Configuration Overview

### Server Configuration

1. Created user `grader` and added the user on `sudoers` group

2. Created `catalog` database and granted access to the user `grader`

3. Setup the firewall to only accept requests on ports: `80 (http)`, `2200 (ssh)` and `123 (ntp)`

4. PostgresSQL running on port `5432`, but is not visible on the internet.

5. Create a key-pair for user `grader` authentication.

### Application Configuration

1. Executed the `project_config.sh` script to install dependencies on the server.

```
Located at: ./var/www/flask/project_config.sh
```

2. Changed the database connection url from the sqlite to postgres driver.

```
postgresql://grader:grader@localhost:5432/catalog
```

3. Moved the app secret key from within the __main__ function to a global scope because the WSGI does not call the __main__ function.

4. Created a special Client ID for this application to enable the Google Oauth service.


## Server Access

1. SSH Connection:

```
ssh grader@3.81.23.35 -p 2200 -i <GRADER-PRIVATE-KEY>
```

2. The application is located at:

```
/var/www/flask/catalog
```

3. The Application WSGI config file is located at:

```
/var/www/flask/catalog/catalog_app.wsgi
```

4. The WSGI plugin config for apache is located at:

```
/etc/apache2/sites-available/000-default.conf
```

## Application Access

1. Application URL

```
http://3.81.23.35.xip.io:80
```

**Note**: It is important to use the `xip.io` domain for Google Auth2 to trust the origin in case the Google Auth2 is used for login.

2. More information regarding the application can be found [here](https://github.com/thiagothalison1/amazing-catalog-project/blob/master/README.md)

## Third-Party Resources

* [Google Oauth2](https://developers.google.com/identity/protocols/OAuth2)


## License
This project is distributed under [MIT License](https://opensource.org/licenses/MIT)
