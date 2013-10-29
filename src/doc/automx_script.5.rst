===============
 automx_script
===============

:Date: 02/08/2013
:Subtitle: automx script backend configuration parameters
:Manual Section: 5
:Manual Group: automx
:Copyright: This document has been placed in the public domain.


Description
'''''''''''

The automx_sript(5) man page specifies all parameters that control access from within automx to a script backend.

Parameters
''''''''''

result_attrs (default: none)
        Specifies a list of one or more variables. The output of the external script will assign values to these variables. Results will be assigned in the same order as variables have been listed::

                result_attrs = server, email, auth, socket

.. IMPORTANT::
        
        automx_script(5) does not check if the number of values returned matches the ones expected. Too many return values will be discarded. If the script returns too few automx_script(5) will use the corresponding result attribute name.

script (default: none)
        Specifies the absolute path to the script which should be run by the backend automx_script(5) backend::

                script = /usr/local/bin/example_com.sh "%s"

separator (default: whitespace)
        Specifies the character that separates values returned by the external script::

                separator = ,

Authors
'''''''

Christian Roessner <cr@sys4.de>
	Wrote the program.

Michael Menge
        Wrote and contributed this backend.

Patrick Ben Koetter <p@sys4.de>
	Wrote the documentation.

See also
''''''''

`automx(8)`_, `automx.conf(5)`_, `automx_ldap(5)`_, `automx_script(5)`_, `automx_sql(5)`_, `automx-test(1)`_

.. _automx(8): automx.8.html
.. _automx.conf(5): automx.conf.5.html
.. _automx_ldap(5): automx_ldap.5.html
.. _automx_sql(5): automx_sql.5.html
.. _automx_script(5): automx_script.5.html                                                                                                             
.. _automx-test(1): automx-test.1.html
