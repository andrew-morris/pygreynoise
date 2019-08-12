Tutorial
========

Create client object
--------------------

To interact with the API, a client object needs to be created::

   >>> from greynoise import GreyNoise
   >>> client = GreyNoise(<api_key>)

where *api_key* is the key you have been given to use the API.


Check specific IPs
------------------

Once the client object has been created, it's possible to check if a given IP
is considered internet noise as follows::

   >>> client.get_noise_status('0.0.0.0')
   {'ip': '0.0.0.0', 'noise': True, 'code': '0x02', 'code_message': 'IP has been observed scanning the GreyNoise sensor network, but has not completed a full connection, meaning this can be spoofed'}

or if a list of IP addresses needs to be checked instead::

   >>> client.get_noise_status_bulk(['0.0.0.0', '0.0.0.1'])
   [{'ip': '0.0.0.0', 'noise': True, 'code': '0x02', 'code_message': 'IP has been observed scanning the GreyNoise sensor network, but has not completed a full connection, meaning this can be spoofed'}, {'ip': '0.0.0.1', 'noise': False, 'code': '0x00', 'code_message': 'IP has never been observed scanning the Internet'}]

Detailed information for any given IP address is also available::

   >>> client.get_context('0.0.0.0')
   {'ip': '0.0.0.0', 'seen': True, 'classification': 'unknown', 'first_seen': '2019-01-29', 'last_seen': '2019-08-09', 'actor': 'unknown', 'tags': ['ZMap Client'], 'metadata': {'country': '', 'country_code': '', 'city': '', 'organization': '', 'asn': '', 'tor': False, 'os': 'unknown', 'category': ''}, 'raw_data': {'scan': [{'port': 67, 'protocol': 'UDP'}], 'web': {'paths': [], 'useragents': []}, 'ja3': []}}


Get noisy IP addresses
----------------------

A list of IP addresses that made up internet noise can also be retrieved if
needed for any given date::

   >>> import datetime
   >>> noise_ips = client.get_noise(date=datetime.date(2019, 1, 1))
   >>> len(noise_ips)
   105003

Note that:

- if *date* is not passed, the current list of noisy IP addresses will
  be returned.
- this is an expensive operation and it can take a few seconds to complete.