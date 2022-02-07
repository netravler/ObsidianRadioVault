# [How to build docker image from github repository](https://stackoverflow.com/questions/26753030/how-to-build-docker-image-from-github-repository)


[](https://stackoverflow.com/posts/26753030/timeline)

In official docs we can see:

```
# docker build github.com/creack/docker-firefox
```

It just works fine to me. `docker-firefox` is a repository and has `Dockerfile` within root dir.  
Then I want to buid redis image and exact version 2.8.10 :

```
# docker build github.com/docker-library/redis/tree/99c172e82ed81af441e13dd48dda2729e19493bc/2.8.10
2014/11/05 16:20:32 Error trying to use git: exit status 128 (Initialized empty Git repository in /tmp/docker-build-git067001920/.git/
error: The requested URL returned error: 403 while accessing https://github.com/docker-library/redis/tree/99c172e82ed81af441e13dd48dda2729e19493bc/2.8.10/info/refs

fatal: HTTP request failed
)
```

I got error above. What's the right format with build docker image from github repos?

[redis](https://stackoverflow.com/questions/tagged/redis "show questions tagged 'redis'")[docker](https://stackoverflow.com/questions/tagged/docker "show questions tagged 'docker'")

[Share](https://stackoverflow.com/q/26753030/14867493 "Short permalink to this question")

[Edit](https://stackoverflow.com/posts/26753030/edit "Revise and improve this post")

Follow

asked Nov 5 '14 at 8:57

[

![](https://lh6.googleusercontent.com/-_9Xa479Vabs/AAAAAAAAAAI/AAAAAAAAABE/cIrae5Xtcss/photo.jpg?sz=64)

](https://stackoverflow.com/users/4162734/seanlook)

[seanlook](https://stackoverflow.com/users/4162734/seanlook)

83722 gold badges99 silver badges1313 bronze badges

-   Docs: [docs.docker.com/engine/reference/commandline/build/…](https://docs.docker.com/engine/reference/commandline/build/#git-repositories) 
    
    – [Dilhan Jayathilake](https://stackoverflow.com/users/1891699/dilhan-jayathilake "1,662 reputation")
    
     [Apr 25 '18 at 23:29](https://stackoverflow.com/questions/26753030/how-to-build-docker-image-from-github-repository/39194765#comment87081506_26753030)
    

[Add a comment](https://stackoverflow.com/questions/26753030/how-to-build-docker-image-from-github-repository/39194765# "Use comments to ask for more information or suggest improvements. Avoid answering questions in comments.")

## 3 Answers

[Active](https://stackoverflow.com/questions/26753030/how-to-build-docker-image-from-github-repository?answertab=active#tab-top "Answers with the latest activity first")[Oldest](https://stackoverflow.com/questions/26753030/how-to-build-docker-image-from-github-repository?answertab=oldest#tab-top "Answers in the order they were provided")[Score](https://stackoverflow.com/questions/26753030/how-to-build-docker-image-from-github-repository?answertab=votes#tab-top "Answers with the highest score first")

45

[](https://stackoverflow.com/posts/39194765/timeline)

`docker build url#ref:dir`

> Git URLs accept context configuration in their fragment section, separated by a colon :. The first part represents the reference that Git will check out, this can be either a branch, a tag, or a commit SHA. The second part represents a subdirectory inside the repository that will be used as a build context.
> 
> For example, run this command to use a directory called docker in the branch container:

```
docker build https://github.com/docker/rootfs.git#container:docker
```

[https://docs.docker.com/engine/reference/commandline/build/](https://docs.docker.com/engine/reference/commandline/build/)

[Share](https://stackoverflow.com/a/39194765/14867493 "Short permalink to this answer")

[Edit](https://stackoverflow.com/posts/39194765/edit "Revise and improve this post")

Follow

[edited Nov 20 '20 at 10:12](https://stackoverflow.com/posts/39194765/revisions "show all edits to this post")

answered Aug 28 '16 at 18:59

[

![](https://www.gravatar.com/avatar/216ea4da5cae7be4030b6242ab90631c?s=64&d=identicon&r=PG)

](https://stackoverflow.com/users/588759/rofrol)

[rofrol](https://stackoverflow.com/users/588759/rofrol)

12.8k77 gold badges7272 silver badges6666 bronze badges

[Add a comment](https://stackoverflow.com/questions/26753030/how-to-build-docker-image-from-github-repository/39194765# "Use comments to ask for more information or suggest improvements. Avoid comments like “+1” or “thanks”.")

Report this ad

8

[](https://stackoverflow.com/posts/26753490/timeline)

The thing you specified as repo URL is not a valid git repository. You will get error when you will try

> `git clone github.com/docker-library/redis/tree/99c172e82ed81af441e13dd48dda2729e19493bc/2.8.10`

Valid URL for this repo is `github.com/docker-library/redis`. So you may want to try following:

> `docker build github.com/docker-library/redis`

But this will not work too. To build from github, docker requires `Dockerfile` in repository root, howerer, this repo doesn't provide this one. So, I suggest, you only have to clone this repo and build image using local Dockerfile.

[Share](https://stackoverflow.com/a/26753490/14867493 "Short permalink to this answer")

[Edit](https://stackoverflow.com/posts/26753490/edit "Revise and improve this post")

Follow

answered Nov 5 '14 at 9:24

[

![](https://i.stack.imgur.com/O8rJM.jpg?s=64&g=1)

](https://stackoverflow.com/users/1691583/viacheslav-kovalev)

[Viacheslav Kovalev](https://stackoverflow.com/users/1691583/viacheslav-kovalev)

1,7051111 silver badges1616 bronze badges

-   3
    
    In fact `docker build https://raw.githubusercontent.com/docker-library/redis/master/2.8.10/Dockerfile` works, but not like the [official example](http://docs.docker.com/v1.1/reference/commandline/cli/#build). Thanks for your answer. 
    
    – [seanlook](https://stackoverflow.com/users/4162734/seanlook "837 reputation")
    
     [Nov 5 '14 at 12:58](https://stackoverflow.com/questions/26753030/how-to-build-docker-image-from-github-repository/39194765#comment42097230_26753490)
    

[Add a comment](https://stackoverflow.com/questions/26753030/how-to-build-docker-image-from-github-repository/39194765# "Use comments to ask for more information or suggest improvements. Avoid comments like “+1” or “thanks”.")

4

[](https://stackoverflow.com/posts/48235957/timeline)

One can use the following example which sets up a Centos 7 container for testing ORC file format. Make sure to escape the `#` sign:

`$ docker build https://github.com/apache/orc.git\#:docker/centos7 -t orc-centos7`

[Share](https://stackoverflow.com/a/48235957/14867493 "Short permalink to this answer")

[Edit](https://stackoverflow.com/posts/48235957/edit "Revise and improve this post")

Follow

answered Jan 13 '18 at 0:55

[

![](https://www.gravatar.com/avatar/d32f25ab2112110cebb2ed4b86e87920?s=64&d=identicon&r=PG)

](https://stackoverflow.com/users/238880/pratik-khadloya)

[Pratik Khadloya](https://stackoverflow.com/users/238880/pratik-khadloya)

11.9k1111 gold badges7575 silver badges100100 bronze badges

-   1
    
    The "-t" flag and name is important in order the image gets a name set. Otherwise you'll end up with <none> images. 
    
    – [david](https://stackoverflow.com/users/120296/david "2,362 reputation")
    
     [Jan 15 '20 at 16:30](https://stackoverflow.com/questions/26753030/how-to-build-docker-image-from-github-repository/39194765#comment105657501_48235957)
    
-   I don't know why it fails on my computer. The error says "failed to solve with frontend dockerfile.v0: failed to read dockerfile: failed to load cache key: subdir not supported yet". I'm using Docker-CE-20.10 
    
    – [runzhi xiao](https://stackoverflow.com/users/9467045/runzhi-xiao "140 reputation")
    
     [Dec 7 '21 at 8:01](https://stackoverflow.com/questions/26753030/how-to-build-docker-image-from-github-repository/39194765#comment124194455_48235957)