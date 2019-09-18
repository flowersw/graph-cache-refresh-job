Graph cache refresh CronJob
---------------------------

This simple CronJob is part of Thoth deployment. It has a one simple goal -
trigger re-builds of `adviser-cached` in OpenShift cluster. These builds are
responsible for filling cache for adviser. See the following links for more
info:

* https://github.com/thoth-station/storages#graph-database-cache
* https://github.com/thoth-station/adviser


Implementation
==============

The implementation of this CronJob is very simple - it just triggers
OpenShift's generic webhook from within the cluster - everything else is done
automatically based on the adjusted adviser s2i build - see adviser repo for
more info:

* https://github.com/thoth-station/adviser/tree/master/.s2i/bin
* https://github.com/thoth-station/adviser/blob/master/openshift/buildConfig-cached-template.yaml

Prerequisites
=============

It is required to have `s2i-thoth-ubi-8-py36
<https://quay.io/repository/thoth-station/s2i-thoth-ubi8-py36>`_ image imported
in OpenShift's container image registry. If you haven't done so already, import
it manually by running:

.. code-block:: console

  oc import-image s2i-thoth-ubi8-py36:latest

Or, you can also provide another container image (check OpenShift template
parameters for `IMAGE_STREAM` and `IMAGE_STREAM_TAG`). The only requirement for
the container image is cURL presence.

On provisioning, there is required to pass `generic webhook URL
<https://docs.openshift.com/container-platform/3.11/dev_guide/builds/triggering_builds.html>`_
which is triggered by this CronJob. You can obtain it by running:

.. code-block:: console

  oc describe bc adviser-cached

