---
layout: post
title: How I Built This
---

I've recently seen a decent amount of posts generally in the theme of "Why you shouldn't use Medium to blog". I also like learning new frameworks and tools, so I took a look around to find some good alternatives to Medium. The option I decided to go with was a Jekyll site hosted on a DigitalOcean droplet. I more or less followed Josh Habdas' writeup [here](http://habd.as/simple-websites-jekyll-docker/). There were a couple small issues with the newer version of Jekyll. I also wasted some time thinking that docker wasn't working.

In my DigitalOcean droplet set up, Docker commands issued without `sudo` result in messages leading the user to believe Docker isn't running:

```
deployer@askkaz:~$ docker ps
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
```

It took me a little bit to realize I just needed `sudo`. D'oh.

```
deployer@askkaz:~$ sudo docker ps
[sudo] password for deployer:
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                         NAMES
66944bb2c7d6        webapp              "/sbin/my_init"     2 weeks ago         Up 2 weeks
```

This was a great place to use the new trick that I learned from [Learn Enough Command Line to Be Dangerous](http://www.learnenough.com/command-line-tutorial):

<pre>
deployer@askkaz:~$ docker ps
Cannot connect to the Docker daemon. Is the docker daemon running on this host?
deployer@askkaz:~$ <b>sudo !!</b>
sudo docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                         NAMES
66944bb2c7d6        webapp              "/sbin/my_init"     2 weeks ago         Up 2 weeks
</pre>

One thing I wish that Josh had written more about is the day-to-day usage - the process to add posts, etc. I find myself doing a lot of steps to publish every post:
<ol>
<li>Write post (obvs)</li>
<li>zip my local repository
<pre>
git archive -o app.tar.gz --prefix=app/ master
</pre>
</li>
<li>SCP the file to my droplet
<pre>
scp app.tar.gz deployer@HOST:
</pre>
</li>
<li>SSH into the droplet and expand the repository
<pre>
ssh deployer@HOST
tar zxvf app.tar.gz
</pre>
</li>
<li>Build a docker image with the new files
<pre>
sudo docker build -t webapp app/
</pre>
</li>
<li>Stop the currently running docker image and start it using the new docker image (results in momentary downtime - not optimal, but I think I'm the only one reading this site for now)
<pre>
sudo docker stop $(docker ps -lq)
sudo docker run -d -p 80:80 webapp
</pre>
</li>
<li>Cleanup unused docker images and containers
<pre>
sudo docker ps -q -a | xargs sudo docker rm
sudo docker images --no-trunc -aqf "dangling=true" | xargs sudo docker rmi
</pre>
</li>
</ol>

I'm assuming that I might be doing something wrong, but I haven't found a better solution to this yet.

I'll look into some automated solutions for this and/or work on an automated script that will do all this for me. Ideally, it would do both a local and a remote option. This will make previewing posts and publishing them much easier.

If you're looking to set up a DigitalOcean account, you can use my [link](https://m.do.co/c/c3dfe2ee61f5) and you'll get a $10 credit.