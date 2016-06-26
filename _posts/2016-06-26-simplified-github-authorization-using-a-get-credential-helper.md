---
layout: post
title: Simplified GitHub Authorization Using a Git Credential Helper
date: 2016-06-26 15:45:00
categories: git github netrc gpg git_credential_helper
---

In order to avoid having to authenticate each time I want to push to github I decided to set up a get credential helper that parses an encrypted credential file. The steps were:

1. Create a personal access token in github
2. Add the following in ~/.gitauth

   ```
   host=github.com protocol=https username=<user> password=<token>

   host=gist.github.com protocol=https username=<user> password=<token>
   ```

3. Generate a gpg key

   ```
   gpg --gen-key
   ```

    * Initially I got the following error: "gpg: agent_genkey failed: No pinentry". pinentry is just a symlink that points to one of the many pinentry binaries depending on the type you want to use. I'm running a headless server and the default pinentry expects a gui. To fix this I changed the pinentry symlink to point to pinentry-curses. I'm sure there's a better way to do this but the few options I came across didn't seem to work.
4. Encrypt netrc

   ```
   gpg -e -r email@example.com ~/.gitauth
   ```
5. You should know have ~/.gitauth.gpg and can remove the original ~/.gitauth
6. Get the credential helper:

   ```bash
   #!/usr/bin/env sh

   CREDENTIAL_FILE=$1

   TARGET=$(cat)
   HOST=$(echo $TARGET | grep -Po 'host=[^ ]+')
   PROTOCOL=$(echo $TARGET | grep -Po 'protocol=[^ ]+')

   AUTH=$(gpg -q --decrypt $CREDENTIAL_FILE | grep "$HOST" | grep "$PROTOCOL")

   echo "$(echo $AUTH | grep -Po 'username=[^ ]+')"
   echo "$(echo $AUTH | grep -Po 'password=[^ ]+')"
   ```

7. Update git config to use the credential helper:

   ```
   git config --global credential.helper "/home/viking66/bin/git-credential-helper ~/.gitauth.gpg"
   ```
8. Now when you push to github, your credential helper should kick in and eliminate the need to provide your username and password.
