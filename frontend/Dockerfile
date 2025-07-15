# SuiteCRM application container
FROM bitnami/suitecrm:latest

# ----------  DB connection details ----------
ENV \
    SUITECRM_DATABASE_HOST=mariadb \
    SUITECRM_DATABASE_PORT_NUMBER=3306 \
    SUITECRM_DATABASE_NAME=appdb \
    SUITECRM_DATABASE_USER=appuser \
    SUITECRM_DATABASE_PASSWORD=apppassword \
    \
    # (optional) first-login admin credentials
    SUITECRM_USERNAME=admin \
    SUITECRM_PASSWORD=adminpassword \
    SUITECRM_EMAIL=admin@example.com

EXPOSE 8080 8443
