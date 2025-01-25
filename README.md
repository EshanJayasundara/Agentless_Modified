# 😺 Agentless: Errors I had in 2025 January and Solutions
## My System Info
```
System
------
Operating System: Ubuntu 22.04.4 LTS                       
          Kernel: Linux 6.8.0-51-generic
    Architecture: x86-64
 Hardware Vendor: ASUSTeK COMPUTER INC.
  Hardware Model: Vivobook_ASUSLaptop X1504VA_X1504VA
             Mem: 15Gi


CPU
---
    Architecture: x86_64
  CPU op-mode(s): 32-bit, 64-bit
   Address sizes: 39 bits physical, 48 bits virtual
      Byte Order: Little Endian
          CPU(s): 12
On-line CPU(s) list: 0-11
       Vendor ID: GenuineIntel
      Model name: 13th Gen Intel(R) Core(TM) i5-1335U
      CPU family: 6
           Model: 186
Thread(s) per core: 2
Core(s) per socket: 10
       Socket(s): 1
        Stepping: 3
     CPU max MHz: 4600.0000
     CPU min MHz: 400.0000
        BogoMIPS: 4992.00

Virtualization features:  
  Virtualization: VT-x

Caches (sum of all):      
             L1d: 352 KiB (10 instances)
             L1i: 576 KiB (10 instances)
              L2: 6.5 MiB (4 instances)
              L3: 12 MiB (1 instance)


Wireless interface(which caused the third error)
------------------------------------------------
     physical id: a
        bus info: usb@1:1
    logical name: wlx1ca770b35e91
          serial: 1c:a7:70:b3:5e:91
    capabilities: ethernet physical wireless
   configuration: broadcast=yes
                  driver=rtl8xxxu
                  driverversion=6.8.0-51-generic
                  firmware=N/A
                  ip=192.168.1.198
                  link=yes
                  multicast=yes
                  wireless=IEEE 802.11

```

## First error: SWE-Bench latest version is used in Agentless
### Command:
```bash
pip install -r requirements.txt
```

### Fix:
#### In `Agentless -> requirement.txt` instructed to use *latest SWE-Bench* but it’s not compatible with Agentless.
```
Agentless/requirements.txt
```
```diff
anthropic
openai
pre-commit
datasets
tiktoken
libcst
llama-index
jsonlines
- swebench @ git+https://github.com/princeton-nlp/SWE-bench:latest
+ swebench @ git+https://github.com/princeton-nlp/SWE-bench@5f5a7df799663adba4b191eca3d675faf3621fe2
```


## Second error: Conda 'testbed' environment is not created in new docker imege before activationg it

### Command:
```bash
(agentless) eshan@eshan-Vivobook-ASUSLaptop-X1504VA-X1504VA:~/FYP/Agentless$ python agentless/test/run_regression_tests.py \
        --run_id generate_regression_tests \
        --output_file results/swe-bench-lite/passing_tests.jsonl \
        --instance_ids sympy__sympy-13480 
```

### Error:
```bash
Using run_id: generate_regression_tests
Running 1 unevaluated instances...
Base image sweb.base.x86_64:latest already exists, skipping build.
Base images built successfully.
No environment images need to be built.
Found 0 existing instance images. Will reuse them.
#extract_resolved_info
#finished: {}
#1
#2: ['sympy__sympy-13480']
#3
  0%|                                                                                                                                                              | 0/1 [00:00<?, ?it/s]#4
#5: sympy__sympy-13480
# try to build container
# Image build at: logs/build_images/instances/sweb.eval.x86_64.sympy__sympy-13480__latest
#image_name: sweb.eval.x86_64.sympy__sympy-13480:latest
#env_image_name: sweb.env.x86_64.c795f4b88616b8462021ed:latest

# ok: client.images.get(env_image_name)
# Image not found
# Image sweb.eval.x86_64.sympy__sympy-13480:latest does not exist: Building wtf!
Building docker image sweb.eval.x86_64.sympy__sympy-13480:latest in logs/build_images/instances/sweb.eval.x86_64.sympy__sympy-13480__latest with platform linux/x86_64
Step 1/5 : FROM --platform=linux/x86_64 sweb.env.x86_64.c795f4b88616b8462021ed:latest
 ---> 2816ad873f77
Step 2/5 : COPY ./setup_repo.sh /root/
 ---> Using cache
 ---> 9f26704dcc76
Step 3/5 : RUN sed -i -e 's/\r$//' /root/setup_repo.sh
 ---> Using cache
 ---> df8603f5a259
Step 4/5 : RUN /bin/bash /root/setup_repo.sh
 ---> Running in 6271f3a3f129
+ git clone -o origin https://github.com/sympy/sympy /testbed
Cloning into '/testbed'...
+ chmod -R 777 /testbed
+ cd /testbed
+ git reset --hard f57fe3f4b3f2cab225749e1b3b38ae1bf80b62f0
HEAD is now at f57fe3f4b3 Merge pull request #13193 from segevfiner/setuptools-console-scripts
+ git remote remove origin
+ source /opt/miniconda3/bin/activate
++ _CONDA_ROOT=/opt/miniconda3
++ . /opt/miniconda3/etc/profile.d/conda.sh
+++ export CONDA_EXE=/opt/miniconda3/bin/conda
+++ CONDA_EXE=/opt/miniconda3/bin/conda
+++ export _CE_M=
+++ _CE_M=
+++ export _CE_CONDA=
+++ _CE_CONDA=
+++ export CONDA_PYTHON_EXE=/opt/miniconda3/bin/python
+++ CONDA_PYTHON_EXE=/opt/miniconda3/bin/python
+++ '[' -z '' ']'
+++ export CONDA_SHLVL=0
+++ CONDA_SHLVL=0
+++ '[' -n '' ']'
+++++ dirname /opt/miniconda3/bin/conda
++++ dirname /opt/miniconda3/bin
+++ PATH=/opt/miniconda3/condabin:/opt/miniconda3/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
+++ export PATH
+++ '[' -z '' ']'
+++ PS1=
++ conda activate
++ local cmd=activate
++ case "$cmd" in
++ __conda_activate activate
++ '[' -n '' ']'
++ local ask_conda
+++ PS1=
+++ __conda_exe shell.posix activate
+++ /opt/miniconda3/bin/conda shell.posix activate
++ ask_conda='PS1='\''(base) '\''
export PATH='\''/opt/miniconda3/bin:/opt/miniconda3/condabin:/opt/miniconda3/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin'\''
export CONDA_PREFIX='\''/opt/miniconda3'\''
export CONDA_SHLVL='\''1'\''
export CONDA_DEFAULT_ENV='\''base'\''
export CONDA_PROMPT_MODIFIER='\''(base) '\''
export CONDA_EXE='\''/opt/miniconda3/bin/conda'\''
export _CE_M='\'''\''
export _CE_CONDA='\'''\''
export CONDA_PYTHON_EXE='\''/opt/miniconda3/bin/python'\'''
++ eval 'PS1='\''(base) '\''
export PATH='\''/opt/miniconda3/bin:/opt/miniconda3/condabin:/opt/miniconda3/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin'\''
export CONDA_PREFIX='\''/opt/miniconda3'\''
export CONDA_SHLVL='\''1'\''
export CONDA_DEFAULT_ENV='\''base'\''
export CONDA_PROMPT_MODIFIER='\''(base) '\''
export CONDA_EXE='\''/opt/miniconda3/bin/conda'\''
export _CE_M='\'''\''
export _CE_CONDA='\'''\''
export CONDA_PYTHON_EXE='\''/opt/miniconda3/bin/python'\'''
+++ PS1='(base) '
+++ export PATH=/opt/miniconda3/bin:/opt/miniconda3/condabin:/opt/miniconda3/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
+++ PATH=/opt/miniconda3/bin:/opt/miniconda3/condabin:/opt/miniconda3/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
+++ export CONDA_PREFIX=/opt/miniconda3
+++ CONDA_PREFIX=/opt/miniconda3
+++ export CONDA_SHLVL=1
+++ CONDA_SHLVL=1
+++ export CONDA_DEFAULT_ENV=base
+++ CONDA_DEFAULT_ENV=base
+++ export 'CONDA_PROMPT_MODIFIER=(base) '
+++ CONDA_PROMPT_MODIFIER='(base) '
+++ export CONDA_EXE=/opt/miniconda3/bin/conda
+++ CONDA_EXE=/opt/miniconda3/bin/conda
+++ export _CE_M=
+++ _CE_M=
+++ export _CE_CONDA=
+++ _CE_CONDA=
+++ export CONDA_PYTHON_EXE=/opt/miniconda3/bin/python
+++ CONDA_PYTHON_EXE=/opt/miniconda3/bin/python
++ __conda_hashr
++ '[' -n '' ']'
++ '[' -n '' ']'
++ hash -r
+ conda activate testbed
+ local cmd=activate
+ case "$cmd" in
+ __conda_activate activate testbed
+ '[' -n '' ']'
+ local ask_conda
++ PS1='(base) '
++ __conda_exe shell.posix activate testbed
++ /opt/miniconda3/bin/conda shell.posix activate testbed

EnvironmentNameNotFound: Could not find conda environment: testbed
You can list all discoverable environments with `conda info --envs`.


+ ask_conda=
+ return
 ---> Removed intermediate container 6271f3a3f129
Error: The command '/bin/sh -c /bin/bash /root/setup_repo.sh' returned a non-zero code: 1
Error building image sweb.eval.x86_64.sympy__sympy-13480:latest: The command '/bin/sh -c /bin/bash /root/setup_repo.sh' returned a non-zero code: 1
Check (logs/build_images/instances/sweb.eval.x86_64.sympy__sympy-13480__latest/build_image.log) for more information.
```

### Fix: 

#### 1. Partially created docker image is the predisisor of the above error.
Since incompleted previous build of the environment image exists, `build_env_images` function at `/home/eshan/miniconda3/envs/agentless/lib/python3.11/site-packages/swebench/harness/docker_build.py` returns without doing nothing, causing the error EnvNotFound in conda environment. Because the partially completed installation inside that container which did not completed the command `conda create -n {env_name} python={specs['python']} -y` can't activate a conda environment with `onda activate {env_name}`.
That's why the error:
```bash
+ __conda_activate activate testbed
+ '[' -n '' ']'
+ local ask_conda
++ PS1='(base) '
++ __conda_exe shell.posix activate testbed
++ /opt/miniconda3/bin/conda shell.posix activate testbed

EnvironmentNameNotFound: Could not find conda environment: testbed
```
occurs.

**To fix this run:**
```
docker image prune -f
```
to remove the images that were created partially.

**`Don't do the followings:`**

#### [x] Add following command to make_repo_script_list function's setup_commands list (Don't do this)
```
miniconda3/envs/agentless/lib/python3.11/site-packages/swebench/harness/test_spec.py
```
```diff
+ # Ensure the environment exists; create if it doesn't
+ f"if ! conda env list | grep -q '{env_name}'; then conda create -n {env_name} python=3.8 -y; fi",
```

```python
def make_repo_script_list(specs, repo, repo_directory, base_commit, env_name):
    """
    Create a list of bash commands to set up the repository for testing.
    This is the setup script for the instance image.
    """
    setup_commands = [
        f"git clone -o origin https://github.com/{repo} {repo_directory}",
        f"chmod -R 777 {repo_directory}",  # So nonroot user can run tests
        f"cd {repo_directory}",
        f"git reset --hard {base_commit}",
        # Remove the remote so the agent won't see newer commits.
        "git remote remove origin",
        # Make sure conda is available for later use
        "source /opt/miniconda3/bin/activate",
        # Ensure the environment exists; create if it doesn't
        f"if ! conda env list | grep -q '{env_name}'; then conda create -n {env_name} python=3.8 -y; fi",
        f"conda activate {env_name}",
        'echo "Current environment: $CONDA_DEFAULT_ENV"',
    ]
    if repo in MAP_REPO_TO_INSTALL:
        setup_commands.append(MAP_REPO_TO_INSTALL[repo])

    # Run pre-install set up if provided
    if "pre_install" in specs:
        for pre_install in specs["pre_install"]:
            setup_commands.append(pre_install)

    if "install" in specs:
        setup_commands.append(specs["install"])
    return setup_commands
```

#### [x] Also updated following function as well (Don't do this)
```
Agentless/agentless/test/run_tests.py
```
```diff
-  eval_commands = [
-      "source /opt/miniconda3/bin/activate",
-      f"conda activate {env_name}",
-      f"cd {repo_directory}",
-  ]
+  eval_commands = [
+     "source /opt/miniconda3/bin/activate",
+     # Ensure the environment exists; create if it doesn't
+     f"if ! conda env list | grep -q '{env_name}'; then conda create -n {env_name} python=3.8 + -y; fi",
+     f"conda activate {env_name}",
+     f"cd {repo_directory}",
+]
```

```python
def make_regression_script_list(instance, specs, env_name, repo_directory, base_commit):
    """
    Applies the test patch and runs the tests.
    """
    # Reset test files to the state they should be in before the patch.
    reset_tests_command = f"git checkout {base_commit}"

    HEREDOC_DELIMITER = "EOF_114329324912"
    fake_apply_test_patch_command = (
        f"git apply -v - <<'{HEREDOC_DELIMITER}'\n{NOOP_PATCH_2}\n{HEREDOC_DELIMITER}"
    )

    test_command = " ".join(
        [
            MAP_REPO_VERSION_TO_SPECS[instance["repo"]][instance["version"]][
                "test_cmd"
            ],
            *get_test_directives(instance),
        ]
    )
    # eval_commands = [
    #     "source /opt/miniconda3/bin/activate",
    #     f"conda activate {env_name}",
    #     f"cd {repo_directory}",
    # ]
    eval_commands = [
        "source /opt/miniconda3/bin/activate",
        # Ensure the environment exists; create if it doesn't
        f"if ! conda env list | grep -q '{env_name}'; then conda create -n {env_name} python=3.8 -y; fi",
        f"conda activate {env_name}",
        f"cd {repo_directory}",
    ]
    if "eval_commands" in specs:
        eval_commands += specs["eval_commands"]
    eval_commands += [
        f"git config --global --add safe.directory {repo_directory}",  # for nonroot user
        f"cd {repo_directory}",
        # This is just informational, so we have a record
        "git status",
        "git show",
        f"git diff {base_commit}",
        "source /opt/miniconda3/bin/activate",
        f"conda activate {env_name}",
    ]
    if "install" in specs:
        eval_commands.append(specs["install"])
    eval_commands += [
        reset_tests_command,
        fake_apply_test_patch_command,  # If we don't apply some sort of patch the harness won't return the tests which passed
        test_command,
        reset_tests_command,
    ]
    return eval_commands
```

## Third error: Connection issue but url exists

### Command:
```bash
(agentless) eshan@eshan-Vivobook-ASUSLaptop-X1504VA-X1504VA:~/FYP/Agentless$ python agentless/test/run_regression_tests.py \
        --run_id generate_regression_tests \
        --output_file results/swe-bench-lite/passing_tests.jsonl \
        --instance_ids sympy__sympy-13480 
```

### Error:
```bash
CondaHTTPError: HTTP 000 CONNECTION FAILED for url <https://repo.anaconda.com/pkgs/main/noarch/repodata.json>
Elapsed: -

An HTTP error occurred when trying to retrieve this URL.
HTTP errors are often intermittent, and a simple retry will get you on your way.

If your current network has https://repo.anaconda.com blocked, please file
a support request with your network engineering team.

'https//repo.anaconda.com/pkgs/main/noarch'
```

### Fix:

#### This error was a serious one not because of the internet connection issue.
This occcured due to the **`old network-card`** which is however not compatible with new `conda` versions.

**What Happens During `conda create`:**

**Package Dependency Resolution:** When you run conda create, Conda resolves dependencies for the specified packages.
To do this, Conda fetches metadata from the repositories (channels) configured in your Conda environment.
The repodata.json file at URLs like https://repo.anaconda.com/pkgs/main/noarch/repodata.json contains metadata about available packages, including:
Package names and versions.
Dependency information.
Build numbers.

`RIP`: Data from https://repo.anaconda.com/pkgs/main cannot accessed with old `network-cards` because Wi-Fi 4 may experience interference or reduced speeds on congested networks.
