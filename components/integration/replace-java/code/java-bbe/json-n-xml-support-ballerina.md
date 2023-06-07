---
title: 'Ballerina'
description: null
---
```
import ballerina/io;
import ballerina/xmldata;

type Address record {|
    string street;
    int city;
|};

public function main() returns error? {
    json jsonValue = {
        "Store": {
            "@id": "AST",
            "name": "Anne",
            "Address": {
                "street": "Main",
                "city": "94"
            },
            "codes": ["4", "8"]
        }
    };

    xml? xmlValue = check xmldata:fromJson(jsonValue);

    if xmlValue is () {
        return;
    }

    string name = (xmlValue/**/<name>).data();
    Address address = check xmldata:fromXml(xmlValue/<Address>);
   
    io:println(string `${name} lives in ${address.street} street, ${address.city}`);
}
```