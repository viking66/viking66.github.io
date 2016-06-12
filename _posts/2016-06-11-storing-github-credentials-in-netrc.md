---
layout: post
title: Storing GitHub Credentials in netrc
date: 2016-06-11 23:27:49
categories: git github netrc gpg
---

In order to avoid having to authenticate each time I want to push to github I decided to set up an encrypted netrc file. The steps were:

1. Create a personal access token in github
2. Add the following in ~/.netrc

    ```
    machine github.com
    login yourusername
    password <token>
    protocol https

    machine gist.github.com
    login yourusername
    password <token>
    protocol https
    ```

3. Generate a gpg key

    ```
    gpg --gen-key
    ```

  * Initially I got the following error: "gpg: agent_genkey failed: No pinentry". pinentry is just a symlink that points to one of the many pinentry binaries depending on the type you want to use. I'm running a headless server and the default pinentry expects a gui. To fix this I changed the pinentry symlink to point to pinentry-curses. I'm sure there's a better way to do this but the few options I came across didn't seem to work.
4. blah blah blah
