Production Deployment
=====================

Example how to deploy Tanner for single-instance production environment.

Clone repositories
------------------

.. code-block:: bash

    mkdir ~/extra2000
    cd ~/extra2000
    git clone https://github.com/extra2000/mushorg-tanner-podman.git
    git clone https://github.com/extra2000/mushorg-tanner.git mushorg-tanner-podman/src/tanner

Then, ``cd`` into project root directory:

.. code-block:: bash

    cd mushorg-tanner-podman

Build tanner Image
------------------

From the project root directory, ``cd`` into ``src/tanner/`` and then build:

.. code-block:: bash

    cd src/
    podman build -t extra2000/mushorg/tanner .

Deploy tanner-api
-----------------

From the project root directory, ``cd`` into ``deployment/production/tanner-api``:

.. code-block:: bash

    cd deployment/production/tanner-api

Create config files and fix permissions:

.. code-block:: bash

    cp -v configmaps/tanner-api.yaml{.example,}
    cp -v configs/config.yaml{.example,}
    chmod og+r ./configs/*

Create pod file:

.. code-block:: bash

    cp -v tanner-api-pod.yaml{.example,}

For SELinux platform, label the following files to allow to be mounted into container:

.. code-block:: bash

    chcon -R -v -t container_file_t ./configs

Create SELinux security policy:

.. code-block:: bash

    cp -v selinux/tanner_api_podman.cil{.example,}

Load SELinux security policy:

.. code-block:: bash

    sudo semodule -i selinux/tanner_api_podman.cil /usr/share/udica/templates/base_container.cil

Verify that the SELinux module exists:

.. code-block:: bash

    sudo semodule --list | grep -e "tanner_api_podman"

Deploy tanner:

.. code-block:: bash

    podman play kube --configmap configmaps/tanner-api.yaml --seccomp-profile-root ./seccomp tanner-api-pod.yaml

Create systemd files to run at startup:

.. code-block:: bash

    mkdir -pv ~/.config/systemd/user
    cd ~/.config/systemd/user
    podman generate systemd --files --name tanner-api-pod-srv01
    systemctl --user enable container-tanner-api-pod-srv01.service

Deploy tanner-web
-----------------

From the project root directory, ``cd`` into ``deployment/production/tanner-web``:

.. code-block:: bash

    cd deployment/production/tanner-web

Create config files and fix permissions:

.. code-block:: bash

    cp -v configmaps/tanner-web.yaml{.example,}
    cp -v configs/config.yaml{.example,}
    chmod og+r ./configs/*

Create pod file:

.. code-block:: bash

    cp -v tanner-web-pod.yaml{.example,}

For SELinux platform, label the following files to allow to be mounted into container:

.. code-block:: bash

    chcon -R -v -t container_file_t ./configs

Create SELinux security policy:

.. code-block:: bash

    cp -v selinux/tanner_web_podman.cil{.example,}

Load SELinux security policy:

.. code-block:: bash

    sudo semodule -i selinux/tanner_web_podman.cil /usr/share/udica/templates/base_container.cil

Verify that the SELinux module exists:

.. code-block:: bash

    sudo semodule --list | grep -e "tanner_web_podman"

Deploy tanner:

.. code-block:: bash

    podman play kube --configmap configmaps/tanner-web.yaml --seccomp-profile-root ./seccomp tanner-web-pod.yaml

Create systemd files to run at startup:

.. code-block:: bash

    mkdir -pv ~/.config/systemd/user
    cd ~/.config/systemd/user
    podman generate systemd --files --name tanner-api-pod-srv01
    systemctl --user enable container-tanner-api-pod-srv01.service

Deploy tanner
-------------

From the project root directory, ``cd`` into ``deployment/production/tanner``:

.. code-block:: bash

    cd deployment/production/tanner

Create config files and fix permissions:

.. code-block:: bash

    cp -v configmaps/tanner.yaml{.example,}
    cp -v configs/config.yaml{.example,}
    chmod og+r ./configs/*

Create pod file:

.. code-block:: bash

    cp -v tanner-pod.yaml{.example,}

For SELinux platform, label the following files to allow to be mounted into container:

.. code-block:: bash

    chcon -R -v -t container_file_t ./configs

Create SELinux security policy:

.. code-block:: bash

    cp -v selinux/tanner_podman.cil{.example,}

Load SELinux security policy:

.. code-block:: bash

    sudo semodule -i selinux/tanner_podman.cil /usr/share/udica/templates/base_container.cil

Verify that the SELinux module exists:

.. code-block:: bash

    sudo semodule --list | grep -e "tanner_podman"

Deploy tanner:

.. code-block:: bash

    podman play kube --configmap configmaps/tanner.yaml --seccomp-profile-root ./seccomp tanner-pod.yaml

Create systemd files to run at startup:

.. code-block:: bash

    mkdir -pv ~/.config/systemd/user
    cd ~/.config/systemd/user
    podman generate systemd --files --name tanner-pod-srv01
    systemctl --user enable container-tanner-pod-srv01.service

Testing
-------

Open web-browser and go to http://127.0.0.1:8091/
