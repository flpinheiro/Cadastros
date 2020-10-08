version: '3.5'

networks:
localdev:
name: localdev

services:
main-api:
build: ws-cadastros/
restart: always
ports:
- "5053:5083" #primeira é no localhost e o segundo é o docker
depends_on:
- sql-server
networks:
- localdev

sql-server:
image: danrleywillyan/sqlserver-compulog:version1.0
container_name: sql-server
environment:
- ACCEPT_EULA=Y
- MSSQL_SA_PASSWORD=<DW@letsCod3H4rd>
- MSSQL_TCP_PORT=1433
ports:
- "1433:1433" #primeira é onde estamos expondo e a segunda é onde está no docker
networks:
- localdev

# react-ui:
# container_name: react-ui
# build: web-cadastro/
# ports:
# - "3001:3000"
# networks:
# - localdev
# volumes:
# - /usr/src/app/node_modules
# - ./react-ui:/usr/src/app

docker run -it --rm -v ${PWD}:/app -v /app/node_modules -p 3001:3000 -e CHOKIDAR_USEPOLLING=true sample:dev