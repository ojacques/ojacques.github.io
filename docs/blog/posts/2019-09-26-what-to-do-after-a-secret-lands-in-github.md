---
date: 2019-09-26
authors:
  - ojacques
---

# Oops. I checked-in a secret key

OK, you checked in a secret, a password, a key to your GIT repository. That's not good. Now what?

<!-- more -->

## That moment

We all had that "oops" moment when we realized that we checked-in a password, a
private SSH key or a token in Github. 

You may think that is OK, that you can just remove it, and it will be gone. But
that's the thing: once it is out there, you never can be sure it was not
captured. Also, GIT keeps history. So, unless you rewrite GIT's history - the
key is still there.

## What to do next?

[GitHub](https://github.com/) has a good article on [removing sensitive data
from a
repository](https://help.github.com/articles/removing-sensitive-data-from-a-repository/).
This is a good start: you stop the leak. 

But, as GitHub's article points out, you need now to consider this data to have
been compromised. The next thing to do is to change that password or key **right
away**, deploy the changes through your automated continuous delivery pipeline and
you're done.

You deploy manually? Well, get ready for long hours spent searching for that key
or password and to change it. There is no way around.

## How can I prevent this?

### `git-secrets`

Good news, we can prevent this from ever happening. It requires *you* to do something.

Amazon released in Open Source
[git-secrets](https://github.com/awslabs/git-secrets), which "Prevents you from
committing passwords and other sensitive information to a git repository.".
Sounds like this is exactly what we need!

`git-secrets` installs as a GIT hook, so that it can detect the issue right away
on your PC, before it hits the repository on GitHub or GitHub enterprise.

You may want to also add `git-secrets` in a [Jenkins
pipeline](https://jenkins.io/doc/book/pipeline/jenkinsfile/). 
This is a bit late, as the secret will hit the server. But it makes you aware
of the situation and you can act on it.

### How do I store secrets securely?

OK, you know you do not want the secrets in the GIT repository, next to the
code. But you still need to store secrets somewhere. 

If we consider secrets to be "configuration", then the [12 factor
app](https://12factor.net/config) recommends to use environment variables, which
you can protect, and source as needed. It's [not
perfect](http://movingfast.io/articles/environment-variables-considered-harmful/),
but definitely a good step forward.

For better security and complete secret management, a dedicated solution like 
 [Hashicorp's Vault](https://github.com/hashicorp/vault) will do wonders.
