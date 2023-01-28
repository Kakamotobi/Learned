# XML and HTML

## Table of Contents
- [What is Extensive Markup Language(XML)?](#what-is-extensive-markup-languagexml)
  - [XML Structure](#xml-structure)
  - [Conditions for Well-formed and Valid XML](#conditions-for-well-formed-and-valid-xml)
  - [XML vs JSON](#xml-json)
- [What is HyperText Markup Langauge(HTML)?](#what-is-hypertext-markup-languagehtml)

## What is Extensive Markup Language(XML)?
> XML (Extensible Markup Language) is a markup language similar to HTML, but without predefined tags to use. Instead, you define your own tags designed specifically for your needs. | MDN

- XML is designed to store and transfer data.
  - i.e. focused on data transfer.
- XML is used for various purposes.
  - Simply transferring data (like JSON).
  - Publishing web pages (like HTML).
- Despite custom tags/elements, the fundamental format/syntax of XML is standardized. Therefore, recipients of XML are still able to correctly parse the XML data.
### XML Structure
- XML Declaration
  - The XML declaration is at the top of the document.
  - It is not a tag. It is meta data about the document itself.
  - Ex: `<?xml version="1.0" encoding="UTF-8"?>`
- Tags
  - An XML document is built up on user-defined tags.
  - The top-level element is called the root.
  - Tags are case sensitive.
### Conditions for Well-formed and Valid XML
- A well-formed XML document adheres to the XML standard (W3C requirements).
  - i.e. correct XML syntax.
- A valid XML document is a well-formed XML that additionally adheres to an XML schema (definition) or DTD.
  - i.e. validated against a schema or DTD that defines the legal elements and attributes that can be used to build the XML document.
### XML vs JSON
- Both XML and JSON can be used as a data format to be transferred.
#### XML Example
```xml
<users>
  <user>
    <name>Kakamotobi</name>
    <age>3</age>
  </user>
  <user>
    <name>Tom</name>
    <age>5</age>
  </user>
</users>
```
#### JSON Example
```json
{
  "users": [
    {
      "name": "Kakamotobi",
      "age": 3
    },
    {
      "name": "Tom",
      "age": 5
    }
  ]
}
```

## What is HyperText Markup Langauge(HTML)?
> HTML (HyperText Markup Language) is the most basic building block of the Web. It defines the meaning and structure of web content. | MDN

- HTML is designed to describe the structure of a webpage.
  - i.e. focused on data presentation.
- "HyperText": links to other web pages.
- "Markup": tags/elements are used to annotate the contents.
  - Tags are case insensitive.

## Reference
[XML introduction - XML: Extensible Markup Language | MDN](https://developer.mozilla.org/en-US/docs/Web/XML/XML_introduction)  
[HTML: HyperText Markup Language | MDN](https://developer.mozilla.org/en-US/docs/Web/HTML)  
[XML Vs. HTML: What is Difference Between XML and HTML?](https://www.guru99.com/xml-vs-html-difference.html)
[How web browsers work - parsing the HTML (part 3, with illustrations)📜🔥 - DEV Community 👩‍💻👨‍💻](https://dev.to/arikaturika/how-web-browsers-work-parsing-the-html-part-3-with-illustrations-45fi)  
