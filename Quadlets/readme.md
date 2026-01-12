Before moving into creating quadlets------------

for services like supabase and greptimedb ,,if they need root access then i've to place the podmans .container quadlet files into this location : /etc/containers/systemd/  
as this is used when i want ec2 starts and the services starts with it..

But if i want rootless then i've to create a location : mkdir -p ~/.config/containers/systemd/

so inside this location if i place my quadlets they wont have root permision . only have ubuntu permissins means when ubuntu user will login then only the services will start.


*********** very imp ques *************
why i cant use any other path instead of both /etc/containers/systemd  and  .config/containers/systemd/  ?

Ans: Becuae the podman and systemd generator only looks for these two paths for the config file (.container file ) ,so they are not gonna search the entire disk for the files that could be slow & insecure.



Also for storing the data we gonna use the same vol as : greptimedb_data:/tmp/greptimedb

---- After creating the greptimedb service in the location etc/containers/systemd  , we reload systemd with : sudo systemctl daemon-reload   ,  and then sudo systemctl start greptimedb  ,,,like we start a service now and for auto start we do : systemctl enable greptimedb

