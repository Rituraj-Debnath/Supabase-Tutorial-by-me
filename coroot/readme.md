######## Before creating coroot as a native linux system ########

so to make coroot as a native linux systemd service the documentation proivides some commands such as : 

# To install coroot & coroot node cluster 
curl -sfL https://raw.githubusercontent.com/coroot/coroot/main/deploy/install.sh | \
  BOOTSTRAP_PROMETHEUS_URL="http://127.0.0.1:9090" \
  BOOTSTRAP_REFRESH_INTERVAL=15s \
  BOOTSTRAP_CLICKHOUSE_ADDRESS=127.0.0.1:9000 \
  sh -


# To install coroot-node agent
curl -sfL https://raw.githubusercontent.com/coroot/coroot-node-agent/main/install.sh | \
  COLLECTOR_ENDPOINT=http://127.0.0.1:8080 \
  SCRAPE_INTERVAL=15s \
  sh -


## We cannot create coroot services from scratch just by downloading their binaries and writing their .services files inside /etc/systemd/system, becuase coroot publish their binaries in 
https://github.com/coroot/coroot/releases  

# but they are not made in that way that we can only install coroot and its agent,,,cluster agent will come as a bundle with coroot ( in the first command ). After that the script command will run suapase services as a native linux service, now here comes the main changes,  i moved to /etc/systemd/system  edit the both .env files and .service files for coroot and coroot node agent and pasted my updated code ( accoording to my technology stack). After that needs to run these commands to see the changes : 

sudo systemctl daemon-reload
sudo systemctl restart coroot coroot-node-agent 

# Both of them will start as your native linux service under systemd.

##### The ports used ##########
To check the ports used by these services run : 
sudo ss -tulnp | grep -E 'coroot|node-agent'

Output will be like : 

tcp   LISTEN 0      4096            127.0.0.1:10301      0.0.0.0:*    users:(("coroot-cluster-",pid=630384,fd=8))
tcp   LISTEN 0      4096            127.0.0.1:10300      0.0.0.0:*    users:(("coroot-node-age",pid=1094400,fd=32))
tcp   LISTEN 0      4096                    *:4317             *:*    users:(("coroot",pid=1134973,fd=20))
tcp   LISTEN 0      4096                    *:8080             *:*    users:(("coroot",pid=1134973,fd=21))

------ While the main noticeable thing is that coroot have a internal Otel collector using the port 4317 ,, so if someone is using a OTEL collector outside of coroot ,, a port clashing will be there , i tried hard to change its internal OTLP port but ,, its not changeable nor supports disable.


!!! Now your coroot and coroot node agent is live !!! 


!!!!!!!---- coroot just takes hardly 2 mins to show the service map , application  or node details,,so wait after the services restart and if you can't see any of them after a couple of mins , there is some issue like the node agent keeps restarting ,to see the logs run : 

sudo journalctl -u coroot-node-agent.service -n 50 --no-pager

There might be a "not enough memory issue" if you've wrote limits for the service , while 1gb RAM is fair enough for the service to run.



##### The things written here is my complete experience i've faced nothing fake ,, nothing AI 

At last the main very important thing in my case was this : GLOBAL_PROMETHEUS_REMOTE_WRITE_URL='http://127.0.0.1:4004/v1/prometheus/write?db=public'

as im using another db instead of prometheus ,, so have to update the endpoint as requirement.