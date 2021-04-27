# APIM-Values
# Default values for gravitee.
# This is a YAML-formatted file.
# Declare variables to be passed into your templates.
chaos:
  enabled: false

graviteeRepoAuth:
  enabled: true

inMemoryAuth:
  enabled: true
  allowEmailInSearchResults: false

# Default password "admin", use bcrypt ($2a$ version) to generate a new one
adminPasswordBcrypt: $2a$10$Ihk05VSds5rUSgMdsMVi9OKMIx2yUvMz7y9VP3rJmQeizZLrhLMyq
adminEmail:
adminFirstName:
adminLastName:

jwtSecret: myJWT4Gr4v1t33_S3cr3t

# Define extra inMemory users here or disable the default ones here
extraInMemoryUsers: |
  - user:
    username: user
    # Password value: password
    password: $2a$10$9kjw/SH9gucCId3Lnt6EmuFreUAcXSZgpvAYuW2ISv7hSOhHRH1AO
    roles: ORGANIZATION:USER, ENVIRONMENT:USER
    # Useful to receive notifications
    #email:
    #firstName:
    #lastName:
  - user:
    username: api1
    # Password value: api1
    password: $2a$10$iXdXO4wAYdhx2LOwijsp7.PsoAZQ05zEdHxbriIYCbtyo.y32LTji
    # You can declare multiple roles using comma separator
    roles: ORGANIZATION:USER, ENVIRONMENT:API_PUBLISHER
    #email:
    #firstName:
    #lastName:
  - user:
    username: application1
    # Password value: application1
    password: $2a$10$2gtKPYRB9zaVaPcn5RBx/.3T.7SeZoDGs9GKqbo9G64fKyXFR1He.
    roles: ORGANIZATION:USER, ENVIRONMENT:USER
    #email:
    #firstName:
    #lastName:
ldap:
  enabled: false
  context:
    # User to bind the LDAP
    user: user@example.com
    # Password to bind the LDAP
    password: pass@12345
    # URL to LDAP
    url: ldap://ldap.example.com
    # Bind base to be used in authentication and lookup sections
    base: dc=example,dc=com
  authentication:
    user:
      # Base to search users, must be relative to the context.base
      base: ou=users
      # Use sAMAccountName if you are in AD
      # Use uid if you are in a native LDAP
      # The {0} will be replaced by user typed to authenticate
      filter: sAMAccountName={0}
      # If you have an attribute with the user photo, you can set it here
      photo: "thumbnailPhoto"
    group:
      # Base to search groups, must be relative to the context.base
      # There an issue here, until fixed only oneleve search is supported
      base: ou=gravitee,ou=groups
      # The {0} will be replaced by DN of the user
      filter: member={0}
      role:
        # The attribute that define your group names on your AD/LDAP
        # You can use sAMAccountName if you're in AD or cn if you're in native LDAP
        attribute: sAMAccountName
        consumer: LDAP_GROUP_CONSUMER
        publisher: LDAP_GROUP_PUBLISHER
        admin: LDAP_GROUP_ADMIN
        user: LDAP_GROUP_USER
  lookup:
    allowEmailInSearchResults: false
    # Note that personal information can be exposed without user consentment
    user:
      # Base to lookup user, must be relative to context.base
      base: ou=users
      # The filter can be any type of complex LDAP query
      filter: (&(objectClass=person)(|(cn=*{0}*)(sAMAccountName={0})))
security:
  trustAll: false
  providers: []
oidcAuth:
  enabled: false
#  id: keycloak
#  clientId:
#  clientSecret:
#  tokenIntrospectionEndpoint:
#  tokenEndpoint:
#  authorizeEndpoint:
#  userInfoEndpoint:
#  userLogoutEndpoint:
#  color:
#  syncMappings:
#  scopes:
#    - openid
#    - profile
#  userMapping:
#    id: sub
#    email: email
#    lastname: family_name
#    firstname: given_name
#    picture: picture
#  groupMapping:
#    - condition: "{#jsonPath(#profile, '$.realm_roles').contains('group1')}"
#      groups:
#        - Group 1
#        - Group 2
#  roleMapping:
#    - condition: "{#jsonPath(#profile, '$.realm_roles').contains('admin')}"
#      roles:
#        - "ENVIRONMENT:ADMIN"
#        - "ORGANIZATION:ADMIN"
smtp:
  enabled: true
  host: smtp.example.com
  port: 25
  from: info@example.com
  username: info@example.com
  password: example.com
  subject: "[gravitee] %s"
  properties:
    auth: true
    starttlsEnable: false
    #localhost: apim.zdralic.com

notifiers:
  smtp:
    enabled: true
    host: ${email.host}
    subject: ${email.subject}
    port: ${email.port}
    from: ${email.from}
    username: ${email.username}
    password: ${email.password}
    # starttlsEnabled: false
    # ssl:
    #   trustAll: false
    #   keyStore:
    #   keyStorePassword:

mongo:
  # uri: mongodb://mongo-mongodb-replicaset:27017/gravitee?connectTimeoutMS=30000
  # servers: |
  #   - host: mongo1
  #     port: 27017
  #   - host: mongo2
  #     port: 27017
  sslEnabled: false
  socketKeepAlive: false
  rs: rs0
  rsEnabled: true
  dbhost: gravitee-apim-mongodb-replicaset
  dbname: gravitee
  dbport: 27017
  connectTimeoutMS: 30000
  auth:
    enabled: false
    source: admin
    username:
    password:

jdbc:
  driver: mysql
  host: localhost
  port: 3306
  database: gravitee
  username:
  password:
  # URLs to download the drivers
  drivers:
    - https://repo1.maven.org/maven2/mysql/mysql-connector-java/8.0.22/mysql-connector-java-8.0.22.jar
    - https://repo1.maven.org/maven2/dev/miku/r2dbc-mysql/0.8.2.RELEASE/r2dbc-mysql-0.8.2.RELEASE.jar

redis:
  host: 'redis.mycompany'
  port: 6379
#  password: 'mysecretpassword'
#  timeout: 2000
#  repositoryVersion: 3.3.0

mongodb-replicaset:
  enabled: true
  replicas: 2
  replicaSetName: rs0
  auth:
    enabled: false
    adminUser: username
    adminPassword: password
    metricsUser: metrics
    metricsPassword: password
    key: keycontent
    # existingKeySecret:
    # existingAdminSecret:
    # existingMetricsSecret:
  image:
    repository: mongo
    tag: 3.6
  persistentVolume:
    enabled: true
    # storageClass: "-"
    accessModes:
      - ReadWriteOnce
    size: 1Gi
  configmap: {}

es:
  enabled: true
  cluster: elasticsearch
  index: gravitee
  # If the details for security are entered
  # authentication will be provided for the
  # elastic search cluster
  # https://docs.gravitee.io/apim_installguide_repositories_elasticsearch.html#management_api_configuration
  security:
    enabled: false
    username: example
    password: example
  lifecyle:
    enabled: false
    policyPropertyName: index.lifecycle.name   #for openDistro, use 'opendistro.index_state_management.policy_id' instead of 'index.lifecycle.name'
    policies:
      monitor: my_policy ## ILM policy for the gravitee-monitor-* indexes
      request: my_policy ## ILM policy for the gravitee-request-* indexes
      health: my_policy ## ILM policy for the gravitee-health-* indexes
      log: my_policy ## ILM policy for the gravitee-log-* indexes
  ssl:
    enabled: false
    # keystore:
    #   type: jks
    #   path: path/to/jks
    #   password: example
    #   certs:
    #     - /path/to/cert1
    #     - /path/to/cert2
    #   keys:
    #     - /path/to/key
    #     - /path/to/key2
  endpoints:
    - http://gravitee-apim-elasticsearch-client.default.svc.cluster.local:9200

elasticsearch:
  enabled: true
  cluster:
    name: "elasticsearch"
    env:
      MINIMUM_MASTER_NODES: "1"
    plugins: []

  image:
    repository: "docker.elastic.co/elasticsearch/elasticsearch-oss"
    tag: "6.7.0"
  client:
    name: client
    replicas: 1
    heapSize: "512m"
    # additionalJavaOpts: "-XX:MaxRAM=512m"
    
  master:
    name: master
    replicas: 2
    heapSize: "512m"
    # additionalJavaOpts: "-XX:MaxRAM=512m"
    persistence:
      enabled: true
      accessMode: ReadWriteOnce
      name: data
      size: "4Gi"
      # storageClass: "ssd"

  data:
    name: data
    exposeHttp: false
    replicas: 1
    heapSize: "512m"
    # additionalJavaOpts: "-XX:MaxRAM=512m"
    persistence:
      enabled: true
      accessMode: ReadWriteOnce
      name: data
      size: "30Gi"
      # storageClass: "ssd"

alerts:
  enabled: false
  endpoints:
    - http://localhost:8072/
  security:
    enabled: false
    username: admin
    password: adminadmin

management:
  type: mongodb

ratelimit:
  type: mongodb

api:
  enabled: true
  name: api
  logging:
    debug: false
    stdout:
      encoderPattern: "%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n"
    file:
      enabled: true
      rollingPolicy: |
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- daily rollover -->
            <fileNamePattern>${gravitee.management.log.dir}/gravitee_%d{yyyy-MM-dd}.log</fileNamePattern>
            <!-- keep 30 days' worth of history -->
            <maxHistory>30</maxHistory>
        </rollingPolicy>
      encoderPattern: "%d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n%n"
    graviteeLevel: DEBUG
    jettyLevel: INFO
  restartPolicy: OnFailure
  reloadOnConfigChange: true
  updateStrategy:
    rollingUpdate:
      maxUnavailable: 1
    type: RollingUpdate
  replicaCount: 1
  image:
    repository: graviteeio/apim-management-api
    # tag: 3.0.2
    pullPolicy: Always
    # pullSecrets:
    #   - name: gravitee_secrets
  # env:
  #   - name: ENV_VARIABLE
  #     value: ENV_VARIABLE_VALUE
  #   - name: ENV_VARIABLE_WITH_FROM
  #     valueFrom:
  #       configMapKeyRef:
  #         name: special-config
  #         key: SPECIAL_LEVEL
  additionalPlugins:
#    - https://path_to_plugin
  ssl:
    enabled: false
  #  keystore:
  #    type: jks # Supports jks, pkcs12
  #    path: ${gravitee.home}/security/keystore.jks
  #    password: secret
  #  truststore:
  #    type: jks # Supports jks, pkcs12
  #    path: ${gravitee.home}/security/truststore.jks
  #    password: secret
  http:
    services:
      core:
        http:
          enabled: false
          port: 18083
          host: localhost
          authentication:
            password: adminadmin
        ingress:
          enabled: true
          path: /management/_(.*)
          hosts:
            - apim.zdralic.com
          annotations:
            kubernetes.io/ingress.class: nginx
            nginx.ingress.kubernetes.io/rewrite-target: /_$1
        service: 
#       If you choose to enable this service, you'll need to expose the technical api
#       on an accessible host outside of the pod: api.http.services.core.http.host
          enabled: false
#         type: ClusterIP
#         externalPort: 18083
    api:
      entrypoint: /
    client:
      timeout: 10000
      # proxy:
      #   type: HTTP
      #   http:
      #     host: localhost
      #     port: 3128
      #     username:
      #     password:
      #   https:
      #     host: localhost
      #     port: 3128
      #     username:
      #     password:
  user:
    login:
      defaultApplication: true
    anynomizeOnDelete: false
  supportEnabled: true
  ratingEnabled: true
  service:
    type: ClusterIP
    externalPort: 83
    internalPort: 8083
    internalPortName: http
  # annotations:
  securityContext:
    runAsUser: 1001
    runAsNonRoot: true
  autoscaling:
    enabled: true
    minReplicas: 1
    maxReplicas: 2
    targetAverageUtilization: 50
    targetMemoryAverageUtilization: 80
  ingress:
    management:
      enabled: true
      path: /management
      # Used to create an Ingress record.
      hosts:
        - apim.zdralic.com
      annotations:
        kubernetes.io/ingress.class: nginx
        nginx.ingress.kubernetes.io/configuration-snippet: "etag on;\nproxy_pass_header ETag;\nproxy_set_header if-match \"\";\n"
        # kubernetes.io/tls-acme: "true"
      #tls:
        # Secrets must be manually created in the namespace.
        # - secretName: chart-example-tls
        #   hosts:
        #     - chart-example.local
      tls:
        - hosts:
            - apim.zdralic.com
          secretName: api-custom-cert
    portal:
      enabled: true
      path: /portal
      # Used to create an Ingress record.
      hosts:
        - apim.zdralic.com
      annotations:
        kubernetes.io/ingress.class: nginx
        nginx.ingress.kubernetes.io/configuration-snippet: "etag on;\nproxy_pass_header ETag;\nproxy_set_header if-match \"\";\n"
        # kubernetes.io/tls-acme: "true"
      #tls:
        # Secrets must be manually created in the namespace.
        # - secretName: chart-example-tls
        #   hosts:
        #     - chart-example.local
      tls:
        - hosts:
            - apim.zdralic.com
          secretName: api-custom-cert

gateway:
  enabled: true
  type: Deployment
  name: gateway
  logging:
    debug: false
    stdout:
      encoderPattern: "%d{HH:mm:ss.SSS} [%thread] [%X{api}] %-5level %logger{36} - %msg%n"
    file:
      enabled: true
      rollingPolicy: |
        <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
            <!-- daily rollover -->
            <fileNamePattern>${gravitee.home}/logs/gravitee_%d{yyyy-MM-dd}.log</fileNamePattern>
            <!-- keep 30 days' worth of history -->
            <maxHistory>30</maxHistory>
        </rollingPolicy>
      encoderPattern: "%d{HH:mm:ss.SSS} [%thread] [%X{api}] %-5level %logger{36} - %msg%n"
    graviteeLevel: DEBUG
    jettyLevel: WARN
  reloadOnConfigChange: true
  additionalPlugins:
#    - https://path_to_plugin
  ssl:
    enabled: false
  #  keystore:
  #    type: jks # Supports jks, pem, pkcs12
  #    path: ${gravitee.home}/security/keystore.jks
  #    password: secret
    clientAuth: false # Supports false/none, request, true/requires
  #  truststore:
  #    type: jks # Supports jks, pem, pkcs12
  #    path: ${gravitee.home}/security/truststore.jks
  #    password: secret
  replicaCount: 1
  # sharding_tags:
  # tenant:
  websocket: false
  ratelimit:
    redis:
      # host:
      # port:
      # password:
  management: 
    http:
      # url: 
      # username: 
      # pssword: 
      # ssl:
      #   trustall: true
      #   verifyHostname: true
      #   keystore:
      #     type: jks # Supports jks, pem, pkcs12
      #     path: ${gravitee.home}/security/keystore.jks
      #     password: secret
      #   truststore:
      #     type: jks # Supports jks, pem, pkcs12
      #     path: ${gravitee.home}/security/truststore.jks
      #     password: secret
      # proxy:
      #   host:
      #   port:
      #   type: http
      #   username:
      #   password:
  services:
    core:
      http:
        enabled: false
        port: 18082
        host: localhost
        authentication:
          type: basic
          password: adminadmin
        secured: false
        ssl:
          keystore:
            type: "PKCS12"
            path: "/p12/keystore"
      ingress:
        enabled: true
        path: /management/_(.*)
        hosts:
          - apim.zdralic.com
        annotations:
          kubernetes.io/ingress.class: nginx
          nginx.ingress.kubernetes.io/rewrite-target: /_$1
      service: 
#       If you choose to enable this service, you'll need to expose the technical api
#       on an accessible host outside of the pod: api.http.services.core.http.host
        enabled: false
#         type: ClusterIP
#         externalPort: 18082
    kubeController:
      enabled: false
    bridge:
      enabled: false
      # version: 3.3.1
      # host: localhost
      # username:
      # password:
      # ssl:
      #  enabled: false
      #  keystore:
      #    type: jks # Supports jks, pem, pkcs12
      #    path: ${gravitee.home}/security/keystore.jks
      #    password: secret
      #  clientAuth: false
      #  truststore:
      #    type: jks # Supports jks, pem, pkcs12
      #    path: ${gravitee.home}/security/truststore.jks
      #    password: secret
      # service:
      #   externalPort: 92
      #   internalPort: 18092
      # ingress:
      #   enabled: false
      #   path: /gateway/_bridge
      #   # Used to create an Ingress record.
      #   hosts:
      #     - apim.zdralic.com
      #   annotations:
      #     kubernetes.io/ingress.class: nginx
      #     nginx.ingress.kubernetes.io/ssl-redirect: "false"
      #     nginx.ingress.kubernetes.io/enable-rewrite-log: "true"
      #     kubernetes.io/app-root: /gateway
      #     kubernetes.io/rewrite-target: /gateway
      #     nginx.ingress.kubernetes.io/configuration-snippet: "etag on;\nproxy_pass_header ETag;\nproxy_set_header if-match \"\";\n"
      #     kubernetes.io/tls-acme: "true"
      #   tls:
      #     # Secrets must be manually created in the namespace.
      #     - secretName: chart-example-tls
      #       hosts:
      #         - chart-example.local

  # handlers:
  #   request:
  #     transaction:
  #       header: X-Gravitee-Transaction-Id
  #     request:
  #       header: X-Gravitee-Request-Id
  reporters:
    elasticsearch:
      enabled: true
#    tcp:
#      enabled: true
#      host: localhost
#      port: 8379
#    file:

  # WARNING: This part will be removed shortly in favor of gateway.policy (see below)
  apiKey:
    header: X-Gravitee-Api-Key
    param: api-key

  #policy:
  #  api-key:
  #    header: X-Gravitee-Api-Key
  #    param: api-key

  image:
    repository: graviteeio/apim-gateway
    # tag: 3.0.2
    pullPolicy: Always
    # pullSecrets:
    #   - name: gravitee_secrets
  # env:
  #   - name: ENV_VARIABLE
  #     value: ENV_VARIABLE_VALUE
  #   - name: ENV_VARIABLE_WITH_FROM
  #     valueFrom:
  #       configMapKeyRef:
  #         name: special-config
  #         key: SPECIAL_LEVEL
  service:
    type: ClusterIP
    externalPort: 82
    internalPort: 8082
    internalPortName: http
  # annotations:
  securityContext:
    runAsUser: 1001
    runAsNonRoot: true
  autoscaling:
    enabled: true
    minReplicas: 1
    maxReplicas: 2
    targetAverageUtilization: 50
    targetMemoryAverageUtilization: 80
  ingress:
    enabled: true
    path: /gateway
    # Used to create an Ingress record.
    hosts:
      - apim.zdralic.com
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/ssl-redirect: "false"
      # nginx.ingress.kubernetes.io/configuration-snippet: "etag on;\nproxy_pass_header ETag;\nproxy_set_header if-match \"\";\n"
      # kubernetes.io/tls-acme: "true"
    #tls:
      # Secrets must be manually created in the namespace.
      # - secretName: chart-example-tls
      #   hosts:
      #     - chart-example.local
    tls:
      - hosts:
          - apim.zdralic.com
        secretName: api-custom-cert

portal:
  enabled: true
  name: portal
  replicaCount: 1
  image:
    repository: graviteeio/apim-portal-ui
    # tag: 3.0.2
    pullPolicy: Always
  # pullSecrets:
  #   - name: gravitee_secrets
  # env:
  #   - name: ENV_VARIABLE
  #     value: ENV_VARIABLE_VALUE
  #   - name: ENV_VARIABLE_WITH_FROM
  #     valueFrom:
  #       configMapKeyRef:
  #         name: special-config
  #         key: SPECIAL_LEVEL
  autoscaling:
    enabled: true
    minReplicas: 1
    maxReplicas: 2
    targetAverageUtilization: 50
    targetMemoryAverageUtilization: 80
  service:
    name: nginx
    type: ClusterIP
    externalPort: 8003
    internalPort: 8080
    internalPortName: http
  # annotations:
  securityContext:
    runAsUser: 101
    runAsGroup: 101
    runAsNonRoot: true
  ingress:
    enabled: true
    path: /
    # Used to create an Ingress record.
    hosts:
      - apim.zdralic.com
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/configuration-snippet: "etag on;\nproxy_pass_header ETag;\n"
      #tls:
      # Secrets must be manually created in the namespace.
      # - secretName: chart-example-tls
      #   hosts:
      #     - chart-example.local
    tls:
      - hosts:
          - apim.zdralic.com
        secretName: api-custom-cert

ui:
  enabled: true
  name: ui
  companyName: Gravitee.io
  title: Management UI
  managementTitle: API Management
  documentationLink: http://docs.gravitee.io/
  scheduler:
    tasks: 10
  theme:
    name: "default"
    logo: "themes/assets/GRAVITEE_LOGO1-01.png"
    loader: "assets/gravitee_logo_anim.gif"
  portal:
    apikeyHeader: "X-Gravitee-Api-Key"
    userCreation:
      enabled: false
    support:
      enabled: true
    rating:
      enabled: false
    analytics:
      enabled: false
      trackingId: ""
  replicaCount: 1
  image:
    repository: graviteeio/apim-management-ui
    # tag: 3.0.2
    pullPolicy: Always
  # pullSecrets:
  #   - name: gravitee_secrets
  # env:
  #   - name: ENV_VARIABLE
  #     value: ENV_VARIABLE_VALUE
  #   - name: ENV_VARIABLE_WITH_FROM
  #     valueFrom:
  #       configMapKeyRef:
  #         name: special-config
  #         key: SPECIAL_LEVEL
  autoscaling:
    enabled: true
    minReplicas: 1
    maxReplicas: 2
    targetAverageUtilization: 50
    targetMemoryAverageUtilization: 80
  service:
    name: nginx
    type: ClusterIP
    externalPort: 8002
    internalPort: 8080
    internalPortName: http
  # annotations:
  securityContext:
    runAsUser: 101
    runAsGroup: 101
    runAsNonRoot: true
  ingress:
    enabled: true
    path: /console(/|$)(.*)
    # Used to create an Ingress record.
    hosts:
      - apim.zdralic.com
    annotations:
      kubernetes.io/ingress.class: nginx
      nginx.ingress.kubernetes.io/rewrite-target: /$1$2
      nginx.ingress.kubernetes.io/configuration-snippet: "etag on;\nproxy_pass_header ETag;\n"
      #tls:
      # Secrets must be manually created in the namespace.
      # - secretName: chart-example-tls
      #   hosts:
      #     - chart-example.local
    tls:
      - hosts:
          - apim.zdralic.com
        secretName: api-custom-cert
