---
layout: post
title: "Creating XML Test Data with PowerShell"
tags: [XML, PowerShell, Testing]
author: Andreas Bjørn Hassing
---

To kick off my latest project (this blog) I'll start with something simple I had to do recently.

At work I am sometimes exposed to a special language called X++. The code looks like, from a developer perspective, it's stored in .xpp files, but in fact it is stored in XML files. These XML files contain the code and metadata for that particular artifact. Tables, for instance, can have both code and metadata about which fields and what data types those fields have. Without going too much into detail on the whole X++ story: I wanted to take an existing XML file, and then append many elements to it.

I used a number of great PowerShell building blocks to solve this simple problem:

#### Reading an XML file

```powershell
# The `[xml]` part is doing the magic!
[xml]$myXml = Get-Content -Raw "~/path/to/file.xml"
```

#### Reading XML element values

```powershell
[xml]$myXml = "<outer><inner>Hello, Xml!</inner></outer>"

$myXml.outer.inner # => "Hello, Xml!"

# Also works when there are more children in an element:
[xml]$myXml = `
"<outer>
    <inner>hey</inner>
    <inner>there</inner>
</outer>"

$myXml.outer.inner
# => "hey"
#    "there"
```

#### Append an XML element as a child of another element

```powershell
[xml]$middleEarth = `
"<denizens>
    <person>
        <name>Gandalf the White</name>
        <age>2 days</age>
    </person>
</denizens>"

# We want to add a hobbit here..
$hobbit = $middleEarth.CreateElement("person")
$hobbitName = $middleEarth.CreateElement("name")
$hobbitAge = $middleEarth.CreateElement("age")

$hobbit.AppendChild($hobbitName)
$hobbit.AppendChild($hobbitAge)

# at this point, `$hobbit` contains an XML element like:
# <person><name></name><age></age></person>

$hobbitName.InnerText = "Frodo Underhill"
$hobbitAge.InnerText = "Not the same as Frodo Baggins"

$middleEarth.denizens.AppendChild($hobbit)

$middleEarth.denizens.person
# => name              age
#    ----              ---
#    Gandalf the White 2 days
#    Frodo Underhill   Not the same as Frodo Baggins

# Disclaimer: the middle earth denizen-archive may be
#             partially incomplete and/or completely
#             incorrect.
```

#### Loop over integer ranges [a..b]

```powershell
1..5 | % {
    Write-Host "I'm dealing with the number $_!"
}
# => "I'm dealing with the number 1!"
#    "I'm dealing with the number 2!"
#    "I'm dealing with the number 3!"
#    "I'm dealing with the number 4!"
#    "I'm dealing with the number 5!"
```

### All together now

This is the real world scenario that I had, where these building blocks were extremely useful:

```powershell
[xml]$testEnumXml = Get-Content -Raw `
    -Path "X:\Path\To\X++\Code\BigTestEnumeration.xml"

# Create 249 enumeration values on an existing
# base enumeration artifact, with the shape:
# <AxEnumValue>
#   <Name>BaseEnumValue{N}</Name>
#   <Value>{N-1}</Value>
# </AxEnumValue>
2..250 | % {
    $enum = $testEnumXml.CreateElement("AxEnumValue")
    $enumName = $testEnumXml.CreateElement("Name")
    $enumValue = $testEnumXml.CreateElement("Value")

    $enumName.InnerText = "BaseEnumValue$_"
    $enumValue.InnerText = $_ - 1

    $enum.AppendChild($enumName)
    $enum.AppendChild($enumValue)

    $testEnumXml.AxEnum.EnumValues.AppendChild($enum)
}
```

## Summary

PowerShell has great support for loading and reading XML files, but appending elements to existing documents is a bit of a hassle; I have to create elements in reverse (`Name` and `Value`, then `AxEnumValue` and append the two former to the latter as children).
