---
title: "How to make new user in Go CD"
date: 2022-04-24T09:24:00+09:00
categories:
  - Memo
tags:
  - GoCD
# header:
#   teaser: /assets/images/code.jpg
---
### What is GoCD ecosystem
In the GoCD ecosystem, the server is the one that controls everything. It provides the user interface to users of the system and provides work for the agents to do. The agents are the ones that do any work (run commands, do deployments, etc) that is configured by the users or administrators of the system.

The server does not do any user-specified "work" on its own. It will not run any commands or do deployments. That is the reason you need a GoCD Server and at least one GoCD Agent installed before you proceed.

Once you have them installed and running, you can navigate to the home page in a web browser (defaults to: http://localhost:8153).

If you have installed the GoCD Agent properly and click on the "Agents" link in the header, you should see an idle GoCD agent waiting (as shown below). If you click "Pipelines", you'll get back to the "Add pipeline" screen you saw earlier.

### Creating Pipeline
A pipeline, in GoCD, is a representation of workflow or a part of a workflow. For instance, if you are trying to automatically run tests, build an installer and then deploy an application to a test environment, then those steps can be modeled as a pipeline. GoCD provides different modeling constructs within a pipeline, such as stages, jobs and tasks.


### Authentication
GoCD has two methods of authentication built into it: Password-file based authentication and LDAP/Active Directory authentication. You can also choose from a collection of community-maintained plugins for other methods of authentication, such as using Google or GitHub OAuth.

You can even write your own plugins for authentication (as in username/password combination) or authorization (as in deciding which GoCD roles a user should be allowed into) by using the authorization plugin endpoint.

“Authorization configuration” is the term used in GoCD for the configuration which allows a GoCD administrator to configure the kind of authentication and authorization used by it. GoCD can be setup to use multiple authorization configurations at the same time.