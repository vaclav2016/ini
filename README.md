# Configuration file parser

Simple read-only config parser. 

Support sections. Syntax like:

    key = value
    [section1]
    key1 = value
    key2 = value

    [section2]
    key3 = 3
    key4 = value

## Example

    char strValue[256];
    uint64_t intValue;

    void *conf = ini_load("example.conf");

    if (conf == NULL) {
          error(1, errno, "ini_load fail");
    }

    ini_getstr(conf, "section1", "key2", strValue, sizeof(strValue));
    ini_getint(conf, "section2", "key3", &intValue);

    ini_free(conf);

## Build

    $ cmake ./
    $ make
    $ sudo make install

## Dependencies

None

## Licensing

(c) 2016 Copyright Vaclav2016 https://github.com/vaclav2016, jabber id vaclav2016@jabber.cz

BOOST License, <http://www.boost.org/LICENSE_1_0.txt>
