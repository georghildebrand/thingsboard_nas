# deploying thingsboard on nas

```bash
sudo mkdir -p ~/.mytb-data && sudo chown -R 799:799 ~/.mytb-data
sudo mkdir -p ~/.mytb-logs && sudo chown -R 799:799 ~/.mytb-logs
```

# run on docker enabled NAS

```bash
sudo docker stop /mytb && sudo docker rm /mytb && sudo docker run -it -p 8080:9090 -p 1883:1883 -p 5683:5683/udp -v ~/.mytb-data:/data -v ~/.mytb-logs:/var/log/thingsboard --name mytb --restart always thingsboard/tb-postgres
```

```yaml
version: '2.2'
services:
  mytb:
    restart: always
    image: "thingsboard/tb-postgres"
    ports:
      - "8080:9090"
      - "1883:1883"
      - "5683:5683/udp"
    environment:
      TB_QUEUE_TYPE: in-memory
    volumes:
      - ~/.mytb-data:/data
      - ~/.mytb-logs:/var/log/thingsboard
#     networks:
#       eth0:
#         priority: 1000
# networks:
#   eth0
```

Test:
Get hostname and token from the thingsboard gui.

```bash
curl -v -X POST -d "{\"temperature\": 25}" $HOST_NAME/api/v1/$ACCESS_TOKEN/telemetry --header "Content-Type:application/json"
```