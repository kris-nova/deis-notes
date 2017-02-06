# Makefile

 - So the first thing I did with workflow:
 
```bash
 git clone git@github.com:kris-nova/workflow.git
 cd workflow
 make
```
 - I know probably not the best (or safest) practice, but the following error could probably be a bit more intuitive/graceful
 
```bash
kris-nova:workflow kris$ make
mkdocs build --clean --strict --verbose --site-dir _build/html && echo && echo "Build finished. The HTML pages are in _build/html."
/bin/sh: mkdocs: command not found
```

-  After a quick google and `pip install mkdocs` I was able to get a little further in the `make` progress

```bash
WARNING -  Template variable warning: 'content' is being deprecated and will not be available in a future version. Use 'page.content' instead. 
WARNING -  Template variable warning: 'google_analytics' is being deprecated and will not be available in a future version. Use 'config.google_analytics' instead. 
WARNING -  Template variable warning: 'meta' is being deprecated and will not be available in a future version. Use 'page.meta' instead. 
WARNING -  Template variable warning: 'site_name' is being deprecated and will not be available in a future version. Use 'config.site_name' instead. 
WARNING -  Template variable warning: 'page_title' is being deprecated and will not be available in a future version. Use 'page.title' instead. 
WARNING -  Template variable warning: 'page_description' is being deprecated and will not be available in a future version. Use 'config.site_description' instead. 
WARNING -  Template variable warning: 'canonical_url' is being deprecated and will not be available in a future version. Use 'page.canonical_url' instead. 
WARNING -  Template variable warning: 'site_url' is being deprecated and will not be available in a future version. Use 'config.site_url' instead. 
DEBUG   -  Building extra_templates page 
DEBUG   -  Building page index.md 
ERROR   -  Error building page index.md 
Traceback (most recent call last):
  File "/usr/local/bin/mkdocs", line 11, in <module>
    sys.exit(cli())
  File "/Library/Python/2.7/site-packages/click/core.py", line 722, in __call__
    return self.main(*args, **kwargs)
  File "/Library/Python/2.7/site-packages/click/core.py", line 697, in main
    rv = self.invoke(ctx)
  File "/Library/Python/2.7/site-packages/click/core.py", line 1066, in invoke
    return _process_result(sub_ctx.command.invoke(sub_ctx))
  File "/Library/Python/2.7/site-packages/click/core.py", line 895, in invoke
    return ctx.invoke(self.callback, **ctx.params)
  File "/Library/Python/2.7/site-packages/click/core.py", line 535, in invoke
    return callback(*args, **kwargs)
  File "/Library/Python/2.7/site-packages/mkdocs/__main__.py", line 156, in build_command
    ), dirty=not clean)
  File "/Library/Python/2.7/site-packages/mkdocs/commands/build.py", line 379, in build
    build_pages(config, dirty=dirty)
  File "/Library/Python/2.7/site-packages/mkdocs/commands/build.py", line 332, in build_pages
    dump_json)
  File "/Library/Python/2.7/site-packages/mkdocs/commands/build.py", line 188, in _build_page
    site_navigation=site_navigation
  File "/Library/Python/2.7/site-packages/mkdocs/commands/build.py", line 59, in convert_markdown
    extension_configs=config['mdx_configs']
  File "/Library/Python/2.7/site-packages/mkdocs/utils/__init__.py", line 364, in convert_markdown
    extension_configs=extension_configs or {}
  File "/Library/Python/2.7/site-packages/markdown/__init__.py", line 159, in __init__
    configs=kwargs.get('extension_configs', {}))
  File "/Library/Python/2.7/site-packages/markdown/__init__.py", line 185, in registerExtensions
    ext = self.build_extension(ext, configs.get(ext, {}))
  File "/Library/Python/2.7/site-packages/markdown/__init__.py", line 264, in build_extension
    module = importlib.import_module(module_name_old_style)
  File "/System/Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/importlib/__init__.py", line 37, in import_module
    __import__(name)
ImportError: Failed loading extension 'markdown_checklist.extension' from 'markdown_checklist.extension', 'markdown.extensions.markdown_checklist.extension' or 'mdx_markdown_checklist.extension'
make: *** [build] Error 1
```

 - Another quick google and `pip install markdown-checklist` seemed to do the trick
  
  
```bash
DEBUG   -  Building page index.md 
WARNING -  Your theme does not appear to contain a 'main.html' template. The 'base.html' template was used instead, which is deprecated. Update your theme so that the primary entry point is 'main.html'. 

Build finished. The HTML pages are in _build/html.
```

- So yes I know I didn't read the docs, but I think it's a common enough practice that we could handle the install a little more gracefully.
- Ah yes.. so looking at the README.md it looks like I missed the critical step `make deps`.. Would be nice if `make` told me to do that ;)
- And that of course fixed the problem.. 
- Ah, looks like there is a documentation server (thanks to the readme).. spinning up the server with `make serve` and going through the 3 steps to getting workflow up and running
- Installing the deis client

```bash
curl -sSL http://deis.io/deis-cli/install-v2.sh | bash
```
- Wondering why we are piping to bash here.. what's wrong with `sh`? What if our user doesn't use `bash`?
- Looks like that dropped something off in my local path.. 

```bash
kris-nova:workflow kris$ curl -sSL http://deis.io/deis-cli/install-v2.sh | bash
Downloading deis-stable-darwin-amd64 From Google Cloud Storage...

deis is now available in your current directory.

To learn more about deis, execute:

    $ ./deis --help

kris-nova:workflow kris$ ./deis 
Usage: deis <command> [<args>...]
kris-nova:workflow kris$ file deis
deis: Mach-O 64-bit executable x86_64
```

 - Just an executable with no real help off the cuff.. I wonder what `./deis help` will do.. also I really want this thing in `$PATH` already.. (I went ahead and dropped the bin off in `/usr/local/bin`)
 - I really should just read these docs.. ofc the next step suggest dropping it off in `$PATH`
 - Installing helm (why does it untar to `darwin-amd64` that seems odd)
 
 ```bash
kris-nova:workflow kris$ wget https://kubernetes-helm.storage.googleapis.com/helm-v2.1.3-darwin-amd64.tar.gz
--2017-02-06 09:49:28--  https://kubernetes-helm.storage.googleapis.com/helm-v2.1.3-darwin-amd64.tar.gz
Resolving kubernetes-helm.storage.googleapis.com... 216.58.194.144, 2607:f8b0:4000:80d::2010
Connecting to kubernetes-helm.storage.googleapis.com|216.58.194.144|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 11581520 (11M) [application/x-tar]
Saving to: ‘helm-v2.1.3-darwin-amd64.tar.gz’

helm-v2.1.3-darwin-amd64.tar.gz               100%[================================================================================================>]  11.04M  11.2MB/s    in 1.0s    

2017-02-06 09:49:30 (11.2 MB/s) - ‘helm-v2.1.3-darwin-amd64.tar.gz’ saved [11581520/11581520]

kris-nova:workflow kris$ ls
CONTRIBUTING.md			LICENSE				_build				helm-v2.1.3-darwin-amd64.tar.gz	src
DCO				Makefile			_scripts			mkdocs.yml			themes
Dockerfile			README.md			charts				requirements.txt
kris-nova:workflow kris$ chmod +x helm-v2.1.3-darwin-amd64.tar.gz 
kris-nova:workflow kris$ tar -xzf helm-v2.1.3-darwin-amd64.tar.gz 
kris-nova:workflow kris$ cd helm-v2.1.3-darwin-amd64.tar.gz 
kris-nova:workflow kris$ cd 
.dockerignore                    .travis.yml                      Makefile                         darwin-amd64/                    themes/
.editorconfig                    CONTRIBUTING.md                  README.md                        helm-v2.1.3-darwin-amd64.tar.gz  
.git/                            DCO                              _build/                          mkdocs.yml                       
.gitignore                       Dockerfile                       _scripts/                        requirements.txt                 
.idea/                           LICENSE                          charts/                          src/                             
kris-nova:workflow kris$ cd darwin-amd64/
LICENSE    README.md  helm       
kris-nova:workflow kris$ cd darwin-amd64/
kris-nova:darwin-amd64 kris$ chmod +x helm 
kris-nova:darwin-amd64 kris$ mv helm /usr/local/bin/
 ```
 
 - So (still several pages deep into the docs/quickstart/cli tools/installing on aws).. I am going to (for grins) try to deploy a cluster in AWS according to our docs
 - `You need AWS API keys with full access` <-- That seems scary
 
 - Just kidding.. I was able to get GCP creds.. so I am going to try there.. Creating a cluster in `Deis Sandbox` in GCP
 - Looks like the image for GCP is out of date.. there is no `Turn on Cloud Logging` checkbox anymore.. "
 - Went ahead and created the cluster.. why not
 - So I deployed the cluster but my `kubectl` config was still pointing at one of my old clusters. Was able to update my config with :
 
```bash
gcloud container clusters get-credentials kris     --zone us-west1-a --project deis-sandbox
```
 - Now `kubectl` seems to be working and talking to my GCE cluster
 - Installing helm... 
 - The install link totally skips how to install tiller.. had to Google that as well
 
```bash
kris-nova:workflow kris$ helm version
Client: &version.Version{SemVer:"v2.1.3", GitCommit:"5cbc48fb305ca4bf68c26eb8d2a7eb363227e973", GitTreeState:"clean"}
Error: cannot connect to Tiller
kris-nova:workflow kris$ helm init
Creating /Users/kris/.helm 
Creating /Users/kris/.helm/repository 
Creating /Users/kris/.helm/repository/cache 
Creating /Users/kris/.helm/repository/local 
Creating /Users/kris/.helm/plugins 
Creating /Users/kris/.helm/starters 
Creating /Users/kris/.helm/repository/repositories.yaml 
Creating /Users/kris/.helm/repository/local/index.yaml 
$HELM_HOME has been configured at $HOME/.helm.

Tiller (the helm server side component) has been installed into your Kubernetes Cluster.
Happy Helming!
kris-nova:workflow kris$

```
 - I wish these github links would open in a new tab.. keeping losing my doc server `localhost:8000`
 - Okay now this command `helm repo add deis https://charts.deis.com/workflow` is working
 - Okay installing workflow
 
 ```bash
 ==> v1/ServiceAccount
 NAME           SECRETS   AGE
 deis-builder   1         4s
 deis-nsqd   1         4s
 deis-monitor-telegraf   1         4s
 deis-minio   1         4s
 deis-logger   1         4s
 deis-registry   1         4s
 deis-router   1         4s
 deis-workflow-manager   1         4s
 deis-database   1         4s
 deis-logger-fluentd   1         4s
 deis-controller   1         4s
 ```
 - So that formatting is a bit annoying.. why don't we dynamically build a table in the terminal?
 - There is talk of `router` and `edge router` I am assuming those are the same things..
 - The first user to register against Deis Workflow will automatically be given administrative privileges... interesting
 - So this is the most confusing thing in the universe http://localhost:8000/quickstart/deploy-an-app/#register-an-admin-user
     - I think we are calling the router the "controller" here.. and the docs made me think we had a DIFFERENT public IP for ingress.. This just doesn't seem linear to me
 - As with the deis hostname, any HTTP traffic to proper-barbecue will be automatically routed to your application pods by the edge router. Vernacular people! So I think edge router here is router.. and I think the DNS docs are suggesting `proper-babecue.$ip` and don't actually say that
 - Ah.. but it looks like it hints at it later in the docs
 - Okay.. application is built and scaled. Nice 
