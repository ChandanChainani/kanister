.. _install:

Installation
************

.. contents:: Installation
  :local:

Kanister can be easily installed and managed with `Helm <https://helm.sh>`_. You
will need to configure your ``kubectl`` CLI tool to target the Kubernetes
cluster you want to install Kanister on.

Start by adding the Kanister repository to your local setup:

.. substitution-code-block:: bash

  helm repo add kanister https://charts.kanister.io/

Use the ``helm install`` command to install Kanister in the ``kanister``
namespace:

.. substitution-code-block:: bash

  helm -n kanister upgrade --install kanister --create-namespace kanister/kanister-operator

Confirm that the Kanister workloads are ready:

.. substitution-code-block:: bash

  kubectl -n kanister get po

You should see the operator pod in the ``Running`` state:

.. substitution-code-block:: bash

  NAME                                          READY   STATUS    RESTARTS        AGE
  kanister-kanister-operator-85c747bfb8-dmqnj   1/1     Running   0               15s

.. note::
  Kanister is guaranteed to work with the 3 most recent versions of Kubernetes.
  For example, if the latest version of Kubernetes is 1.24, Kanister will work
  with 1.24, 1.23, and 1.22. Support for older versions is provided on a
  best-effort basis. If you are using an older version of Kubernetes, please
  consider upgrading to a newer version.

Configuring Kanister
====================

Use the ``helm show values`` command to list the configurable options:

.. substitution-code-block:: bash

  helm show values kanister/kanister-operator

For example, you can use the ``image.tag`` value to specify the Kanister version
to install.

The source of the ``values.yaml`` file can be found on
`GitHub <https://github.com/kanisterio/kanister/blob/master/helm/kanister-operator/values.yaml>`_.


Managing Custom Resource Definitions (CRDs)
===========================================

The default RBAC settings in the Helm chart permit Kanister to manage and
auto-update its own custom resource definitions, to ease the user's operation
burden. If your setup requires the removal of these settings, you will have to
install Kanister with the ``--set controller.updateCRDs=false`` option:

.. substitution-code-block:: bash

  helm -n kanister upgade --install kanister --create-namespace kanister/kanister-operator \
    --set controller.updateCRDs=false

This option lets Helm manage the CRD resources.

Building and Deploying from Source
==================================

Follow the instructions in the ``BUILD.md`` file in the
`Kanister GitHub repository <https://github.com/kanisterio/kanister/blob/master/BUILD.md>`_
to build Kanister from source code.
