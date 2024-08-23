```bash

# to check connectivity with database use this command

#
mysql -u root -p'deployment' -h 10.43.117.171 -P 9088

# when you exec inside pod backend
mysql -h 10.43.117.171 -u root -p 9088

# run container to check connectivity with database
sudo kubectl run -it --rm --image=busybox:1.
31.1 telnet-test -- sh

# and run this in another terminal to with logs

sudo kubectl logs -f telnet-test
```
