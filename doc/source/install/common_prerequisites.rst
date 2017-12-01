Prerequisites
-------------

Before you install and configure the replace with the service it implements service,
you must create a database, service credentials, and API endpoints.

#. To create the database, complete these steps:

   * Use the database access client to connect to the database
     server as the ``root`` user:

     .. code-block:: console

        $ mysql -u root -p

   * Create the ``tiny_kubernetes`` database:

     .. code-block:: none

        CREATE DATABASE tiny_kubernetes;

   * Grant proper access to the ``tiny_kubernetes`` database:

     .. code-block:: none

        GRANT ALL PRIVILEGES ON tiny_kubernetes.* TO 'tiny_kubernetes'@'localhost' \
          IDENTIFIED BY 'TINY_KUBERNETES_DBPASS';
        GRANT ALL PRIVILEGES ON tiny_kubernetes.* TO 'tiny_kubernetes'@'%' \
          IDENTIFIED BY 'TINY_KUBERNETES_DBPASS';

     Replace ``TINY_KUBERNETES_DBPASS`` with a suitable password.

   * Exit the database access client.

     .. code-block:: none

        exit;

#. Source the ``admin`` credentials to gain access to
   admin-only CLI commands:

   .. code-block:: console

      $ . admin-openrc

#. To create the service credentials, complete these steps:

   * Create the ``tiny_kubernetes`` user:

     .. code-block:: console

        $ openstack user create --domain default --password-prompt tiny_kubernetes

   * Add the ``admin`` role to the ``tiny_kubernetes`` user:

     .. code-block:: console

        $ openstack role add --project service --user tiny_kubernetes admin

   * Create the tiny_kubernetes service entities:

     .. code-block:: console

        $ openstack service create --name tiny_kubernetes --description "replace with the service it implements" replace with the service it implements

#. Create the replace with the service it implements service API endpoints:

   .. code-block:: console

      $ openstack endpoint create --region RegionOne \
        replace with the service it implements public http://controller:XXXX/vY/%\(tenant_id\)s
      $ openstack endpoint create --region RegionOne \
        replace with the service it implements internal http://controller:XXXX/vY/%\(tenant_id\)s
      $ openstack endpoint create --region RegionOne \
        replace with the service it implements admin http://controller:XXXX/vY/%\(tenant_id\)s
