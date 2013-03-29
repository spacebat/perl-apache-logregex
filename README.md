# NAME

Apache::LogRegex - Parse a line from an Apache logfile into a hash

# VERSION

This document refers to version 1.5 of Apache::LogRegex, released November 20th 2008

# SYNOPSIS

    use Apache::LogRegex;

    my $lr;

    eval { $lr = Apache::LogRegex->new($log_format) };
    die "Unable to parse log line: $@" if ($@);

    my %data;

    while ( my $line_from_logfile = <> ) {
        eval { %data = $lr->parse($line_from_logfile); };
        if (%data) {
            # We have data to process
        } else {
            # We could not parse this line
        }
    }

# DESCRIPTION

## Overview

Designed as a simple class to parse Apache log files. It will construct
a regex that will parse the given log file format and can then parse
lines from the log file line by line returning a hash of each line.

The field names of the hash are derived from the log file format. Thus if
the format is '%a %t \\"%r\\" %s %b %T \\"%{Referer}i\\" ...' then the keys of
the hash will be %a, %t, %r, %s, %b, %T and %{Referer}i.

Should these key names be unusable, as I guess they probably are, then subclass
and provide an override rename\_this\_name() method that can rename the keys
before they are added in the array of field names.

# SUBROUTINES/METHODS

## Constructor

- Apache::LogRegex->new( FORMAT )

    Returns a Apache::LogRegex object that can parse a line from an Apache
    logfile that was written to with the FORMAT string. The FORMAT
    string is the CustomLog string from the httpd.conf file.

## Class and object methods

- parse( LINE )

    Given a LINE from an Apache logfile it will parse the line and
    return a hash of all the elements of the line indexed by their
    format. If the line cannot be parsed an empty hash will be
    returned.

- names()

    Returns a list of field names that were extracted from the data. Such as
    '%a', '%t' and '%r' from the above example.

- regex()

    Returns a copy of the regex that will be used to parse the log file.

- rename\_this\_name( NAME )

    Use this method to rename the keys that will be used in the returned hash.
    The initial NAME is passed in and the method should return the new name.

# CONFIGURATION AND ENVIRONMENT

Perl 5

# DIAGNOSTICS

The various custom time formats could be problematic but providing that
they are encased in '\[' and '\]' all should be fine.

- Apache::LogRegex->new() takes 1 argument

    When the constructor is called it requires one argument. This message is
    given if more or less arguments were supplied.

- Apache::LogRegex->new() argument 1 (FORMAT) is undefined

    The correct number of arguments were supplied with the constructor call,
    however the first argument, FORMAT, was undefined.

- Apache::LogRegex->parse() takes 1 argument

    When the method is called it requires one argument. This message is
    given if more or less arguments were supplied.

- Apache::LogRegex->parse() argument 1 (LINE) is undefined

    The correct number of arguments were supplied with the method call,
    however the first argument, LINE, was undefined.

- Apache::LogRegex->names() takes no argument

    When the method is called it requires no arguments. This message is
    given if some arguments were supplied.

- Apache::LogRegex->regex() takes no argument

    When the method is called it requires no arguments. This message is
    given if some arguments were supplied.

# BUGS

None so far

# FILES

None

# SEE ALSO

mod\_log\_config for a description of the Apache format commands

# THANKS

Peter Hickman wrote the original module and maintained it for
several years. He kindly passed maintainership on just prior to
the 1.6 release. Most of the features of this module are the fruits of
his work. If you find any bugs they are my doing.

# AUTHOR

Original code by Peter Hickman <peterhi@ntlworld.com>

Additional code by Andrew Kirkpatrick <ubermonk@gmail.com>

LICENSE AND COPYRIGHT
\---------------------

Original code copyright (c) 2004-2006 Peter Hickman. All rights reserved.

Additional code copyright (c) 2013 Andrew Kirkpatrick. All rights reserved.

This module is free software. It may be used, redistributed and/or
modified under the same terms as Perl itself.
