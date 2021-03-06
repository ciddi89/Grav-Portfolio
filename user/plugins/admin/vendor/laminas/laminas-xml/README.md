# laminas-xml

[![Build Status](https://travis-ci.com/laminas/laminas-xml.svg?branch=master)](https://travis-ci.com/laminas/laminas-xml)
[![Coverage Status](https://coveralls.io/repos/github/laminas/laminas-xml/badge.svg?branch=master)](https://coveralls.io/github/laminas/laminas-xml?branch=master)

An utility component for XML usage and best practices in PHP

## Installation

You can install using:

```
curl -s https://getcomposer.org/installer | php
php composer.phar install
```

Notice that this library doesn't have any external dependencies, the usage of composer is for autoloading and standard purpose. 


## Laminas\Xml\Security

This is a security component to prevent [XML eXternal Entity](https://www.owasp.org/index.php/XML_External_Entity_%28XXE%29_Processing) (XXE) and [XML Entity Expansion](http://projects.webappsec.org/w/page/13247002/XML%20Entity%20Expansion) (XEE) attacks on XML documents.

The XXE attack is prevented disabling the load of external entities in the libxml library used by PHP, using the function [libxml_disable_entity_loader](http://www.php.net/manual/en/function.libxml-disable-entity-loader.php).

The XEE attack is prevented looking inside the XML document for ENTITY usage. If the XML document uses ENTITY the library throw an Exception.

We have two static methods to scan and load XML document from a string (scan) and from a file (scanFile). You can decide to get a SimpleXMLElement or DOMDocument as result, using the following use cases:

```php
use Laminas\Xml\Security as XmlSecurity;

$xml = <<<XML
<?xml version="1.0"?>
<results>
    <result>test</result>
</results>
XML;

// SimpleXML use case
$simplexml = XmlSecurity::scan($xml);
printf ("SimpleXMLElement: %s\n", ($simplexml instanceof \SimpleXMLElement) ? 'yes' : 'no');

// DOMDocument use case
$dom = new \DOMDocument('1.0');
$dom = XmlSecurity::scan($xml, $dom);
printf ("DOMDocument: %s\n", ($dom instanceof \DOMDocument) ? 'yes' : 'no');
```
