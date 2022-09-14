# ROS package with array messages for event based cameras

This package has definitions for messages created by event based vision
sensors. The events are kept in a compact binary format to avoid slow
serialization and deserialization.

## Encoding schemes

- ``mono``: messages from monochrome cameras such as the DVS and
	Prophesee Metavision cameras with ON and OFF events on
	Encodes on 64 bit boundaries as follows:

    | bits  | interpretation                         |
    |-------|----------------------------------------|
    | 63    | polarity: ON event = 1, OFF event = 0  |
    | 48-62 | y (15 bits)                            |
    | 32-48 | x (16 bits)                            |
    | 0-32  | dt (32 bits)                           |

    To recover the original sensor time, add the delta ``dt`` to the
	message ``time_base`` field.
	To recover the best estimate ROS sensor time stamp, add ``dt`` to the
	header stamp.

- ``trigger``: external trigger messages from e.g. the
    Prophesee Metavision cameras.

    | bits  | interpretation                         |
    |-------|----------------------------------------|
    | 63    | polarity: ON event = 1, OFF event = 0  |
    | 33-62 | unused (31 bits)                       |
    | 0-32  | dt (32 bits)                           |

    To recover the original sensor time, add the delta ``dt`` to the
	message ``time_base`` field.
	To recover the best estimate ROS sensor time stamp, add ``dt`` to the
	header stamp.

- ``evt3``: raw Metavision evt3 data as it comes from the SDK.
    For decoding refer to Prophesee Metavision documents.

	The content of the ``time_base`` field is undefined. Recovery of
	sensor time requires decoding the data packets.
	
For encoding and decoding see the examples in the ``python`` directory and the header files
in ``include/event_array_msgs``.

## License
This package is released under the [Apache-2 license](LICENSE).
