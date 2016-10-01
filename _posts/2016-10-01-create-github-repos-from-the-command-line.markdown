---
layout: post
title: "Create Github repos from the command line"
date: 2016-10-01T03:55:31-05:00
---

### Because why not

First make sure to [create personal API tokens](//github.com/blog/1509-personal-api-tokens) with appropriate scopes to make authenticated requests to your [GitHub](//github.com) account.

{% highlight bash %}
# /.bash_profile

github-create() {
 repo_name=$1

 dir_name=`basename $(pwd)`

 if [ "$repo_name" = "" ]; then
 echo "Repo name (hit enter to use '$dir_name')?"
 read repo_name
 fi

 if [ "$repo_name" = "" ]; then
 repo_name=$dir_name
 fi
{% endhighlight %}


{% highlight bash %}
 username=`git config github.user`
 if [ "$username" = "" ]; then
 echo "Could not find username, run 'git config --global github.user <username>'"
 invalid_credentials=1
 fi

 token=`git config github.token`
 if [ "$token" = "" ]; then
 echo "Could not find token, run 'git config --global github.token <token>'"
 invalid_credentials=1
 fi

 if [ "$invalid_credentials" == "1" ]; then
 return 1
 fi
{% endhighlight %}


{% highlight bash %}
 echo -n "Creating Github repository '$repo_name' ..."
 curl -u "$username:$token" https://api.github.com/user/repos -d '{"name":"'$repo_name'"}' > /dev/null 2>&1

 echo " done."
{% endhighlight %}

{% highlight bash %}
 remote=`git remote -v`
 if [ "$remote" = "" ]; then
 echo "Add remote as origin (y/n)?"
 read add_remote
 fi

 if [ "$add_remote" = "y" ]; then
 url="https://github.com/$username/$repo_name"
 `git remote add origin $url`
 echo "Added '$url' to remote as 'origin'."
 fi
}
{% endhighlight %}

