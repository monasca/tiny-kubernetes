2. Edit the ``/etc/tiny_kubernetes/tiny_kubernetes.conf`` file and complete the following
   actions:

   * In the ``[database]`` section, configure database access:

     .. code-block:: ini

        [database]
        ...
        connection = mysql+pymysql://tiny_kubernetes:TINY_KUBERNETES_DBPASS@controller/tiny_kubernetes
