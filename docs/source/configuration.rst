
=============
Configuration
=============

.. contents:: Table of Contents
   :depth: 2
   :local:

The included minified JS and CSS files can be used for demoing or testing, but
you'll want to configure *Converse.js* to suit your needs before you deploy it
on your website.

*Converse.js* is passed its configuration settings when you call its *initialize* method.

You'll most likely want to call the *initialize* method in your HTML page. For
an example of how this is done, please see the bottom of the *./index.html* page.

Please refer to the `Configuration variables`_ section below for info on
all the available configuration settings.

After you have configured *Converse.js*, you'll have to regenerate the minified
JS file so that it will include the new settings. Please refer to the
:ref:`minification` section for more info on how to do this.

.. _`configuration-variables`:

Configuration variables
=======================

allow_contact_requests
----------------------

Default:  ``true``

Allow users to add one another as contacts. If this is set to false, the
**Add a contact** widget, **Contact Requests** and **Pending Contacts** roster
sections will all not appear. Additionally, all incoming contact requests will be
ignored.

allow_muc
---------

Default:  ``true``

Allow multi-user chat (muc) in chatrooms. Setting this to ``false`` will remove
the ``Chatrooms`` tab from the control box.

allow_otr
---------

Default:  ``true``

Allow Off-the-record encryption of single-user chat messages.

allow_registration
------------------

Default:  ``true``

Support for `XEP-0077: In band registration <http://xmpp.org/extensions/xep-0077.html>`_

Allow XMPP account registration showing the corresponding UI register form interface.

animate
-------

Default:  ``true``

Show animations, for example when opening and closing chat boxes.

auto_list_rooms
---------------

Default:  ``false``

If true, and the XMPP server on which the current user is logged in supports
multi-user chat, then a list of rooms on that server will be fetched.

Not recommended for servers with lots of chat rooms.

For each room on the server a query is made to fetch further details (e.g.
features, number of occupants etc.), so on servers with many rooms this
option will create lots of extra connection traffic.

auto_reconnect
--------------

Default:  ``true``

Automatically reconnect to the XMPP server if the connection drops
unexpectedly.

auto_subscribe
--------------

Default:  ``false``

If true, the user will automatically subscribe back to any contact requests.

.. _`bosh-service-url`:

bosh_service_url
----------------

Default: ``undefined``

To connect to an XMPP server over HTTP you need a `BOSH <https://en.wikipedia.org/wiki/BOSH>`_
connection manager which acts as a middle man between the HTTP and XMPP
protocols.

The bosh_service_url setting takes the URL of a BOSH connection manager.

Please refer to your XMPP server's documentation on how to enable BOSH.
For more information, read this blog post: `Which BOSH server do you need? <http://metajack.im/2008/09/08/which-bosh-server-do-you-need>`_

A more modern alternative to BOSH is to use `websockets <https://developer.mozilla.org/en/docs/WebSockets>`_.
Please see the :ref:`websocket-url` configuration setting.

cache_otr_key
-------------

Default:  ``false``

Let the `OTR (Off-the-record encryption) <https://otr.cypherpunks.ca>`_ private
key be cached in your browser's session storage.

The browser's session storage persists across page loads but is deleted once
the tab or window is closed.

If this option is set to ``false``, a new OTR private key will be generated
for each page load. While more inconvenient, this is a much more secure option.

This setting can only be used together with ``allow_otr = true``.

.. note::
    A browser window's session storage is accessible by all javascript that
    is served from the same domain. So if there is malicious javascript served by
    the same server (or somehow injected via an attacker), then they will be able
    to retrieve your private key and read your all the chat messages in your
    current session. Previous sessions however cannot be decrypted.

debug
-----

Default:  ``false``

If set to true, debugging output will be logged to the browser console.

domain_placeholder
------------------

Default: ``e.g. conversejs.org``

The placeholder text shown in the domain input on the registration form.

.. _`keepalive`:

keepalive
---------

Default:    ``true``

Determines whether Converse.js will maintain the chat session across page
loads.

See also:

* :ref:`session-support`
* `Using prebind in connection with keepalive`_

.. note::
    Currently the "keepalive" setting only works with BOSH and not with
    websockets. This is because XMPP over websocket does not use the same
    session token as with BOSH. A possible solution for this is to implement
    `XEP-0198 <http://xmpp.org/extensions/xep-0198.html>`_, specifically
    with regards to "stream resumption".

message_carbons
---------------

Default:  ``false``

Support for `XEP-0280: Message Carbons <https://xmpp.org/extensions/xep-0280.html>`_

In order to keep all IM clients for a user engaged in a conversation,
outbound messages are carbon-copied to all interested resources.

This is especially important in webchat, like converse.js, where each browser
tab serves as a separate IM client.

Both message_carbons and `forward_messages`_ try to solve the same problem
(showing sent messages in all connected chat clients aka resources), but go about it
in two different ways.

Message carbons is the XEP (Jabber protocol extension) specifically drafted to
solve this problem, while `forward_messages`_ uses
`stanza forwarding <http://www.xmpp.org/extensions/xep-0297.html>`_

expose_rid_and_sid
------------------

Default:  ``false``

Allow the prebind tokens, RID (request ID) and SID (session ID), to be exposed
globally via the API. This allows other scripts served on the same page to use
these values.

*Beware*: a malicious script could use these tokens to assume your identity
and inject fake chat messages.

forward_messages
----------------

Default:  ``false``

If set to ``true``, sent messages will also be forwarded to the sending user's
bare JID (their Jabber ID independent of any chat clients aka resources).

This means that sent messages are visible from all the user's chat clients,
and not just the one from which it was actually sent.

This is especially important for web chat, such as converse.js, where each
browser tab functions as a separate chat client, with its own resource.

This feature uses Stanza forwarding, see also `XEP 0297: Stanza Forwarding <http://www.xmpp.org/extensions/xep-0297.html>`_

For an alternative approach, see also `message_carbons`_.

fullname
--------

If you are using prebinding, can specify the fullname of the currently
logged in user, otherwise the user's vCard will be fetched.

hide_muc_server
---------------

Default:  ``false``

Hide the ``server`` input field of the form inside the ``Room`` panel of the
controlbox. Useful if you want to restrict users to a specific XMPP server of
your choosing.

hide_offline_users
------------------

Default:  ``false``

If set to ``true``, then don't show offline users.

i18n
----

Specify the locale/language. The language must be in the ``locales`` object. Refer to
``./locale/locales.js`` to see which locales are supported.

.. _`play-sounds`:

play_sounds
-----------

Default:  ``false``

Plays a notification sound when you receive a personal message or when your
nickname is mentioned in a chat room.

Inside the ``./sounds`` directory of the Converse.js repo, you'll see MP3 and Ogg
formatted sound files. We need both, because neither format is supported by all browsers.

For now, sound files are looked up by convention, not configuration. So to have
a sound play when a message is received, make sure that your webserver serves
it in both formats as ``http://yoursite.com/sounds/msg_received.mp3`` and
``http://yoursite.com/sounds/msg_received.ogg``.

``http://yoursite.com`` should of course be your site's URL.

.. _`prebind`:

prebind
--------

Default:  ``false``

See also: :ref:`session-support`

Use this option when you want to attach to an existing XMPP
`BOSH <https://en.wikipedia.org/wiki/BOSH>`_ session.

Usually a BOSH session is set up server-side in your web app.

Attaching to an existing BOSH session that was set up server-side is useful
when you want to maintain a persistent single session for your users instead of
requiring them to log in manually.

When a BOSH session is initially created, you'll receive three tokens.
A JID (jabber ID), SID (session ID) and RID (Request ID).

Converse.js needs these tokens in order to attach to that same session.

There are two complementary configuration settings to ``prebind``.
They are :ref:`keepalive` and :ref:`prebind_url`.

``keepalive`` can be used keep the session alive without having to pass in
new tokens to ``converse.initialize`` every time you reload the page. This
removes the need to set up a new BOSH session every time a page loads.

``prebind_url`` lets you specify a URL which converse.js will call whenever a
new BOSH session needs to be set up.


Here's an example of converse.js being initialized with these three options:

.. code-block:: javascript

    converse.initialize({
        bosh_service_url: 'https://bind.example.com',
        keepalive: true,
        prebind: true,
        prebind_url: 'http://example.com/api/prebind',
        allow_logout: false
    });

.. note:: The ``prebind_url`` configuration setting is new in version 0.9 and
    simplifies the code needed to set up and maintain prebinded sessions.

    When using ``prebind_url`` and ``keepalive``, you don't need to manually pass in
    the RID, SID and JID tokens anymore.


.. _`prebind_url`:

prebind_url
-----------

* Default:  ``null``
* Type:  URL

See also: :ref:`session-support`

This setting should be used in conjunction with :ref:`prebind` and :ref:`keepalive`.

It allows you to specify a URL which converse.js will call when it needs to get
the RID and SID (Request ID and Session ID) tokens of a BOSH connection, which
converse.js will then attach to.

The server behind ``prebind_url`` should return a JSON encoded object with the
three tokens::

    {
        "jid": "me@example.com/resource",
        "sid": "346234623462",
        "rid": "876987608760"
    }

providers_link
--------------

Default:  ``https://xmpp.net/directory.php``

The hyperlink on the registration form which points to a directory of public
XMPP servers.


roster_groups
-------------

Default:  ``false``

If set to ``true``, converse.js will show any roster groups you might have
configured.

.. note::
    It's currently not possible to use converse.js to assign contacts to groups.
    Converse.js can only show users and groups that were previously configured
    elsewhere.

show_controlbox_by_default
--------------------------

Default:  ``false``

The "controlbox" refers to the special chatbox containing your contacts roster,
status widget, chatrooms and other controls.

By default this box is hidden and can be toggled by clicking on any element in
the page with class *toggle-controlbox*.

If this options is set to true, the controlbox will by default be shown upon
page load.

show_only_online_users
----------------------

Default:  ``false``

If set to ``true``, only online users will be shown in the contacts roster.
Users with any other status (e.g. away, busy etc.) will not be shown.

storage
-------

Default: ``session``

Valid options: ``session``, ``local``.

This option determines the type of `storage <https://developer.mozilla.org/en-US/docs/Web/Guide/API/DOM/Storage>`_
(``localStorage`` or ``sessionStorage``) used by converse.js to cache user data.

Originally converse.js used only localStorage, however sessionStorage is from a
privacy perspective a better choice.

The main difference between the two is that sessionStorage only persists while
the current tab or window containing a converse.js instance is open. As soon as
it's closed, the data is cleared.

Data in localStorage on the other hand is kept indefinitely.

.. note::
    Since version 0.8.0, the use of local storage is not recommended. The
    statuses (online, away, busy etc.) of your roster contacts are cached in
    the browser storage. If you use local storage, these values are stored for
    multiple sessions, and they will likely become out of sync with your contacts'
    actual statuses. The session storage doesn't have this problem, because
    roster contact statuses will not become out of sync in a single session,
    only across more than one session.


use_otr_by_default
------------------

Default:  ``false``

If set to ``true``, Converse.js will automatically try to initiate an OTR (off-the-record)
encrypted chat session every time you open a chat box.

use_vcards
----------

Default:  ``true``

Determines whether the XMPP server will be queried for roster contacts' VCards
or not. VCards contain extra personal information such as your fullname and
avatar image.

visible_toolbar_buttons
-----------------------

Default:

.. code-block:: javascript

    {
        call: false,
        clear: true,
        emoticons: true,
        toggle_participants: true
    }

Allows you to show or hide buttons on the chat boxes' toolbars.

* *call*:
    Provides a button with a picture of a telephone on it.
    When the call button is pressed, it will emit an event that can be used by a third-party library to initiate a call.::

        converse.on('callButtonClicked', function(event, data) {
            console.log('Strophe connection is', data.connection);
            console.log('Bare buddy JID is', data.model.get('jid'));
            // ... Third-party library code ...
        });
* *clear*:
    Provides a button for clearing messages from a chat box.
* *emoticons*:
    Enables rendering of emoticons and provides a toolbar button for choosing them.
* toggle_participants:
    Shows a button for toggling (i.e. showing/hiding) the list of participants in a chat room.

.. _`websocket-url`:

websocket_url
-------------

Default: ``undefined``

This option is used to specify a 
`websocket <https://developer.mozilla.org/en/docs/WebSockets>`_ URI to which
converse.js can connect to.

Websockets provide a more modern and effective two-way communication protocol
between the browser and a server, effectively emulating TCP at the application
layer and therefore overcoming many of the problems with existing long-polling
techniques for bidirectional HTTP (such as `BOSH <https://en.wikipedia.org/wiki/BOSH>`_).

Please refer to your XMPP server's documentation on how to enable websocket
support.

.. note::
    Please note that not older browsers do not support websockets. For older
    browsers you'll want to specify a BOSH URL. See the :ref:`bosh-service-url`
    configuration setting).
    
.. note::
    Converse.js does not yet support "keepalive" with websockets.

xhr_custom_status
-----------------

Default:  ``false``

.. note::
    XHR stands for XMLHTTPRequest, and is meant here in the AJAX sense (Asynchronous Javascript and XML).

This option will let converse.js make an AJAX POST with your changed custom chat status to a
remote server.

xhr_custom_status_url
---------------------

.. note::
    XHR stands for XMLHTTPRequest, and is meant here in the AJAX sense (Asynchronous Javascript and XML).

Default:  Empty string

Used only in conjunction with ``xhr_custom_status``.

This is the URL to which the AJAX POST request to set the user's custom status
message will be made.

The message itself is sent in the request under the key ``msg``.

xhr_user_search
---------------

Default:  ``false``

.. note::
    XHR stands for XMLHTTPRequest, and is meant here in the AJAX sense (Asynchronous Javascript and XML).

There are two ways to add users.

* The user inputs a valid JID (Jabber ID), and the user is added as a pending contact.
* The user inputs some text (for example part of a firstname or lastname), an XHR (Ajax Request) will be made to a remote server, and a list of matches are returned. The user can then choose one of the matches to add as a contact.

This setting enables the second mechanism, otherwise by default the first will be used.

*What is expected from the remote server?*

A default JSON encoded list of objects must be returned. Each object
corresponds to a matched user and needs the keys ``id`` and ``fullname``.

xhr_user_search_url
-------------------

.. note::
    XHR stands for XMLHTTPRequest, and is meant here in the AJAX sense (Asynchronous Javascript and XML).

Default:  Empty string

Used only in conjunction with ``xhr_user_search``.

This is the URL to which an AJAX GET request will be made to fetch user data from your remote server.
The query string will be included in the request with ``q`` as its key.

The calendar can be configured through a `data-pat-calendar` attribute.
The available options are:
