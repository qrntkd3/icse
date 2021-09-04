## How to run EMB

First, you need to download and run our docker image. The Docker socket should be bind-mounted to run Docker inside Docker.

```
docker pull qrntkd3/icse
docker run -v {your docker.sock location}:/run/docker.sock --network="host" -it qrntkd3/icse /bin/bash
```
Then, please set environment variables and run the tool and service.

```
cd /rest
. setenv
python3 run.py {tool_name} {service_name} {port_number}
```

- Available tool_name: bboxrt, dredd, evomaster, restest, restler, resttestgen, schemathesis, and tcases
- Available service_name: catwatch, features-service, languagetool, ncs, news, ocvn, proxyprint, restcountries, scout-api, and scs

If you want to use tool Dredd with catwatch using port 10300, the command is "python3 run.py dredd catwatch 10300".
You can get the report using get_html_report.py or get_csv_report.py.
```
python3 get_html_report.py {target_dir} {port_number}
```
for example, python3 get_html_report.py /rest/benchmarks/em/embedded/rest/catwatch 10300

## How to run Gitlab

```
cd /gitlab-ee-8.15
sh setup1.sh
echo "sudo docker cp gitlab.rb gitlab:/etc/gitlab/gitlab.rb"
echo "sudo docker exec gitlab gitlab-ctl reconfigure"
echo "sudo docker exec -it gitlab bash"
echo "gitlab-rails console"
echo "user = User.find_by_username('root')"
```
Terminal will print the root information which include authentication token. Please copy it and replace the token to /gitlab-ee-8.15/run_dredd.sh and /gitlab-ee-8.15/token.py.

```
sh setup2.sh
```
and wait 1 minute.

In the /gitlab-ee-8.15 directory, you can run the tools to test Gitlab.
```
tmux new -d -s dredd 'sh run_dredd.sh "5 hours"'
tmux new -d -s bboxrt 'sh run_bboxrt.sh "5 hours"'
tmux new -d -s evomaster 'sh run_evomaster.sh'
tmux new -d -s restest 'sh run_restest.sh "5 hours"'
tmux new -d -s restler 'sh run_restler.sh'
tmux new -d -s resttestgen 'sh run_resttestgen.sh "5 hours"'
tmux new -d -s schemathesis 'sh run_schemathesis.sh "5 hours"'
tmux new -d -s tcases 'sh run_tcases.sh "5 hours"'
```

After 5 hours, you can get the result by typing python3 main.py. 1.csv and bug1.txt are the results.
