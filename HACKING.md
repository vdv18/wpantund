Hacking wpantund
================

## Prerequisite Knowledge ##

The wpantund project makes heavy use of the following:

* [Protothreads](http://dunkels.com/adam/pt/)
* [`boost::any`](http://www.boost.org/doc/libs/1_57_0/doc/html/any.html)
* [`boost::signals2`](http://www.boost.org/doc/libs/1_51_0/doc/html/signals2.html)

Understanding what these are and how they work are critical to being able to
successfully work on WPAN Tunnel Driver.


## General Project Structure ##

There are give types of code in this project:

1.  Third-party code - Always contained in the `third_party`
    directory.
2.  Separable Utilities - There are stand-alone utilities that are
    generally useful and may be easily used by other projects. These
    are in the `src/utils` directory.
3.  Fundamental Utilities - These are utilitity classes and functions
    which are specific to WPAN Tunnel Driver, but used by both
    application-specific and modem-specific classes. These can be
    found in `src/utils` and `src/wpantund`.
4.  Application Specific - There are classes which define the basic
    structure of the application. These are found in `src/wpantund`.
5.  Modem Specific - These are classes that are specific to individual
    NCP implementations. These are found in specific subdirectories.
    For example, `src/ncp-spinel`.

There are three primary application-level virtual base classes in
wpantund:

 *  `NCPControlInterface`
 *  `NCPInstance`
 *  `IPCServer`

The `NCPControlInterface` is the interface through which all
operations on an NCP are preformed, with the exception of sending and
receiving packets on the network. For example, if you want to form or
join a network, you do so via the virtual methods of this class.

The `NCPInstance` class represents a NCP. The methods of this class
are use by WPAN Tunnel Driver for low-level configuration and setup of
the NCP. The `NCPInstance` class offers the `NCPControlInterface`
object which is used to interact with this specific NCP. Generally,
NCP implementations use this class for setting up the `TUN` interface,
framing/unframing data and management packets, etc.

The `IPCServer` class exposes the methods from the
`NCPControlInterface` class via an unspecified IPC mechanism (DBus is
currently the only sublass implemented).
