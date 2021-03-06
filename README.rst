===============================
manila-ui
===============================

Manila Management Dashboard

* Free software: Apache license
* Documentation: http://docs.openstack.org/developer/manila-ui
* Source: http://git.openstack.org/cgit/openstack/manila-ui
* Bugs: http://bugs.launchpad.net/manila-ui

Installation instructions
-------------------------

Begin by cloning the Horizon and Manila UI repositories::

    git clone https://github.com/openstack/horizon
    git clone https://github.com/hp-storage/manila-ui

Apply a patch to horizon since it currently fails to load plugins correctly::

    cd horizon
    git review -d 128133

Create a virtual environment and install Horizon dependencies::

    ./run_tests.sh -8

Set up your ``local_settings.py`` file::

    cp openstack_dashboard/local/local_settings.py.example openstack_dashboard/local/local_settings.py

Open up the copied ``local_settings.py`` file in your preferred text
editor. You will want to customize several settings:

-  ``HORIZON_CONFIG`` requires, an entry, ``customization_module``,
   that refers to ``manila_ui.overrides``::

    HORIZON_CONFIG = {
        ...
        'js_spec_files': [],
        'customization_module': 'manila_ui.overrides',
    }

-  ``OPENSTACK_HOST`` should be configured with the hostname of your
   OpenStack server. Verify that the ``OPENSTACK_KEYSTONE_URL`` and
   ``OPENSTACK_KEYSTONE_DEFAULT_ROLE`` settings are correct for your
   environment. (They should be correct unless you modified your
   OpenStack server to change them.)


Install Manila UI with all dependencies in your virtual environment::

    tools/with_venv.sh pip install -e ../manila-ui/

And enable it in Horizon::

    cp ../manila-ui/_90_manila_admin_shares.py openstack_dashboard/local/enabled
    cp ../manila-ui/_90_manila_project_shares.py openstack_dashboard/local/enabled


Starting the app
----------------

If everything has gone according to plan, you should be able to run::

    ./run_tests.sh --runserver 0.0.0.0:8080

and have the application start on port 8080. The horizon dashboard will
be located at http://localhost:8080/
