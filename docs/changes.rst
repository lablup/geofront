Geofront Changelog
==================

Version 0.5.0
-------------

To be released.

- Geofront now supports Docker-based deployment.

  - Added shortcut factory functions for classes in
    :mod:`geofront.backends.cloud` for use in environment-variable
    configuration.
  - Allow :class:`DatabaseKeyStore <geofront.backends.dbapi.DatabaseKeyStore>`
    to accept string database module names for use in environment-variable
    configuration.
  - Added example configurations for nginx and docker-compose.

Version 0.4.1
-------------

To be released.

- Geofront now supports Ed25519 key.
  Under the hood, :func:`geofront.keystore.parse_openssh_pubkey()` function
  becomes to be possible to parse Ed25519 public keys.
- Fixed :exc:`TypeError`
  (``__init__() got an unexpected keyword argument 'filename'``)
  from :meth:`FileSystemMasterKeyStore.load()
  <geofront.masterkey.FileSystemMasterKeyStore.load>` method.
  [:issue:`7`]
- Since Werkzeug 0.14 changed the way to handle exceptions and it's no more
  compatible with the current use of Geofront, Geofront explicitly scoped
  the version of Werzkeug to be less than 0.14.


Version 0.4.0
-------------

Released on March 30, 2017.

- Geofront now supports ECDSA key.
  Under the hood, :func:`geofront.keystore.parse_openssh_pubkey()` function
  becomes to be possible to parse ECDSA public keys.  [:issue:`14`]
- Geofront now supports Atlassian Bitbucket Cloud.
  See also :mod:`geofront.backends.bitbucket` module.  [:issue:`4`]
- Geofront now can generate other than RSA master key (e.g. ECDSA).
  The new configuration :data:`~config.MASTER_KEY_TYPE` is added to
  choose the type of the master key Geofront will generate.
  It's :class:`~paramiko.rsakey.RSAKey` by default.
- Added :mod:`geofront.backends.oauth` module which provides
  base team implementations for OAuth.
- Added ``key_type`` optional parameter to
  :func:`~geofront.masterkey.renew_master_key()` function,
  :class:`~geofront.masterkey.PeriodicalRenewal` class constructor, and
  :func:`~geofront.regen.regenerate()` function.
- Since ``key_type`` now can be other than :class:`~paramiko.rsakey.RSAKey`,
  the ``bits`` optional parameter of
  :func:`~geofront.masterkey.renew_master_key()` function,
  :class:`~geofront.masterkey.PeriodicalRenewal` class constructor, and
  :func:`~geofront.regen.regenerate()` function now have the default value
  :const:`None` instead of 2048.  :const:`None` automatically choose
  the appropriate bits of the ``key_type``.
- Added :func:`~geofront.masterkey.generate_key()` function and
  :exc:`~geofront.masterkey.KeyGenerationError` that it raises.
- Added ``alias_namer`` option to :class:`CloudRemoteSet
  <geofront.backends.cloud.CloudRemoteSet>` constructor.
- Added :const:`geofront.team.GroupSet` type which is alias of
  :class:`~typing.AbstractSet`\ [:class:`~typing.Hashable`].
- Now master key can be found without token through
  :http:get:`/masterkey/` API.  The server root :http:get:`/` also became
  to contain a :http:header:`Link` header to it and ``"master_key_url"`` field.
- Deprecated :http:get:`/tokens/(token_id:token_id)/masterkey/`.
  Use :http:get:`/masterkey/` instead.
  The existing url redirects to the new url :http:get:`/masterkey/` with
  :http:statuscode:`301`.
- Fixed bug that :meth:`CloudKeyStore.list_keys()
  <~geofront.backends.cloud.CloudKeyStore>` sometimes returned an empty set
  even if there were stored keys.
- Geofront becomes to require Paramiko 2.0.1 or higher.
- Geofront becomes to require Werkzeug 0.11 or higher.
- Geofront becomes to require Flask 0.10.1 or higher.
- Geofront becomes to require Apache Libcloud 1.1.0 or higher.
- Geofront becomes to require OAuthLib 1.1.1 or higher.
- Geofront becomes to require Waitress 1.0.2 or higher.
- Goefront becomes to require typeguard_ 2.1.1 or higher.
- :mod:`geofront.util` is gone now.  Geofront instead became to require
  :mod:`typing` and typeguard_.

.. _typeguard: https://github.com/agronholm/typeguard


Version 0.3.2
-------------

Released on March 7, 2016.

- Added :class:`~geofront.remote.RemoteSetUnion` to make union view of
  multiple remote sets.
- Fixed :exc:`AttributeError` on :meth:`StashKeyStore.register()
  <geofront.backends.stash.StashKeyStore.register>` or
  :meth:`StashKeyStore.deregister()
  <geofront.backends.stash.StashKeyStore.deregister>` being called.


Version 0.3.1
-------------

Released on January 19, 2016.

- Added :class:`~geofront.remote.RemoteSetFilter` to dynamically filter
  set of remotes.
- Fixed a regression bug introduced since 0.3.0
  (:commit:`9db44659c423ed33a89de712fb645186b7c722cc`) that
  :class:`~geofront.backends.github.GitHubOrganization` fails to authenticate.
  [:issue:`12`]


Version 0.3.0
-------------

Released on January 15, 2016.

- Geofront becomes to require Paramiko 1.15.0 or higher.
- Added save check for :class:`~geofront.remote.AuthorizedKeyList`.
  [:issue:`5`]
- :meth:`Team.request_authentication()
  <geofront.team.Team.request_authentication>` method becomes to no more take
  ``auth_nonce`` and return :class:`~geofront.team.AuthenticationContinuation`
  value instead of simple url :class:`str`, so that arbitrary value more
  general than simple nonce :class:`str` can be shared between
  :meth:`~geofront.team.Team.request_authentication()` and
  :meth:`Team.authenticate() <geofront.team.Team.authenticate>`.  If arbitrary
  nonce is needed, :meth:`~geofront.team.Team.request_authentication()`
  method has to generate one by itself.
- Geofront now supports Atlassian Bitbucket Server (which was Stash).
  See also :mod:`geofront.backends.stash` module.
- :class:`~geofront.masterkey.TwoPhaseRenewal` became to raise
  :exc:`ConnectionError` with attempted remote address instead of
  :exc:`socket.gaierror` which is hard to troubleshoot.
- Fixed signature mismatch errors of
  :class:`~geofront.backends.cloud.CloudMasterKeyStore` when it's used with
  AWS S3.


Version 0.2.2
-------------

Released on July 8, 2014.

- Became to depend on apache-libcloud 0.15.0 or later.
- Added HSTS_ support:

  - Added :data:`~config.ENABLE_HSTS` configuration.
  - Added :option:`--force-https <geofront-server --force-https>` option
    to :program:`geofront-server` command.

- Fixed a bug of :meth:`KeyPairDoesNotExistError.save()
  <geofront.backends.cloud.KeyPairDoesNotExistError.save>` method that
  leaks :exc:`~libcloud.common.types.MalformedResponseError` raised by
  :class:`~libcloud.compute.drivers.ec2.EC2NodeDriver` which ought to
  raise proper :exc:`libcloud.compute.types.KeyPairDoesNotExistError`.

.. _HSTS: https://developer.mozilla.org/en-US/docs/Web/Security/HTTP_strict_transport_security


Version 0.2.1
-------------

Released on June 16, 2014.

- Fixed an authentication bug of :class:`~geofront.masterkey.TwoPhaseRenewal`
  raised due to not specify login username.
- More detailed message logging of exceptions that rise during master key
  renewal.


Version 0.2.0
-------------

Released on May 3, 2014.

- Added :meth:`~geofront.team.Team.list_groups()` method to
  :class:`~geofront.team.Team` interface.
- Added :meth:`~geofront.backends.github.GitHubOrganization.list_groups()`
  method to :class:`~geofront.backends.github.GitHubOrganization` class.
- Removed an unnecessary dependency to enum34_ on Python 3.3.
- Added :mod:`geofront.backends.cloud` module.

  - ``geofront.masterkey.CloudMasterKeyStore`` is moved to
    :class:`geofront.backends.cloud.CloudMasterKeyStore`.
  - ``geofront.remote.CloudRemoteSet`` is moved to
    :class:`geofront.backends.cloud.CloudRemoteSet`.

- :class:`~geofront.remote.Remote` now has
  :attr:`~geofront.remote.Remote.metadata` attribute.
- :class:`~geofront.backends.cloud.CloudRemoteSet` fills
  :attr:`~geofront.remote.Remote.metadata` of the resulted
  :class:`~geofront.remote.Remote` objects if the given driver supports.
- Now depends on singledispatch_ if Python is older than 3.4.
- Added :class:`~geofront.remote.PermissionPolicy` interface.
- Added :class:`~geofront.remote.DefaultPermissionPolicy` class.
- Added :class:`~geofront.remote.GroupMetadataPermissionPolicity` class.
- Added new ``PERMISSION_POLICY`` configuration.
- Added :mod:`geofront.backends.dbapi` module.
- Added :program:`geofront-key-regen` command.
- HTTP APIs became more RESTful.  Now it has the root endpoint which provides
  the link to create a new token, and the token API provides several
  links to subresources as well.
- Added new ``MASTER_KEY_BITS`` configuration.
- Added new ``bits`` optional parameters to :func:`renew_master_key()
  <geofront.masterkey.renew_master_key>`, :class:`PeriodicalRenewal
  <geofront.masterkey.PeriodicalRenewal>`, and :func:`regenerate()
  <geofront.regen.regenerate>`.
- Added :class:`~geofront.backends.cloud.CloudKeyStore`.  [:issue:`2`]
- Added :class:`~geofront.backends.cloud.CloudMasterPublicKeyStore`.
  [:issue:`2`]

.. _enum34: https://pypi.python.org/pypi/enum34
.. _singledispatch: https://pypi.python.org/pypi/singledispatch


Version 0.1.1
-------------

Released on April 22, 2014.

- Fixed :exc:`TypeError` that rises when :class:`CloudMasterKeyStore
  <geofront.backends.cloud.CloudMasterKeyStore>` is used with AWS S3 driver.
- Added :option:`--trusted-proxy <geofront-server --trusted-proxy>` option
  to :program:`geofront-server` command.  It's useful when the server is
  run behind a reverse proxy.
- Added token no-op API: :http:get:`/tokens/(token_id:token_id)/`.


Version 0.1.0
-------------

First alpha release.  Released on April 21, 2014.
