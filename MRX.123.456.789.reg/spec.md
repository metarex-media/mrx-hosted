# MRX.123.456.789.reg - MetaRex register entry

* STATUS: **draft**
* DATE: 2023-08-08
* Comments & suggestions should be raised as [issues on the repo](https://github.com/metarex-media/mrx-hosted/issues)

This document specifies the [MetaRex] register entry format. Documents may be
registered in either [json] or [yaml] formats. The MetaRex register parses the
document and validates the resulting object with the schema prior to storing it.

**Caution to users**: The MetaRex register will respond to queries with the data
in the document format that it was originally registered in. Don't change from
[json] to [yaml] when your system is in production. The register doesn't care
but your users might.

## High level design

The register has 3 high level object properties and few mandatory properties.
The goal is to provide enough information for an automation system to process
the metadata without needing any other information. It will be necessary for the
system to have some knowledge of the MetaRex semantics. This will usually be
obtained by using one of the planned SDKs.

## Properties

### `metarexId` - required

This is the canonical identifier of a class of metadata. There are two types of
ID accepted by the register:

* MRX identifier. A 19 character string starting with `MRX` followed by 4 groups
  of 3 characters from a controlled vocabulary (see [spec]). This document has a
  `metarexId` in its title. This type of registration is considered permanent
  and is generally used for metadata that will persist for a long time and
  therefore may need to be resolved at some time in the future.
* [UUID] -  A **U**niversally **U**nique **ID**entifier (UUID) URN Namespace
  RFC4122. This class of registration is considered temporary. Its lifetime in
  the register is subject to the rules imposed by the operator of the register.

### `name` - required

The `name` field is intended for display (as a label) to an end user in a MetaRex
system. It should therefore be as short and descriptive as possible and it should
be in the language of the intended audience. If internationalization is required
then please join the [MetaRex] project and help us define a mechanism for this.

* **Example**: `MRX.123.456.789.reg`  **name**="_MetaRex Register Entry_" (data defined by the
  document you're reading now)
* **Example**: `383bdadd-173f-48ef-8eda-39463dccc103` **name**="_Acteurs à l'écran_"
  temporary registration of actors on screen while logging a French movie in
  France.
* **Example**: `0c3c284b-1a93-4cfe-b5b6-11fdfb584bf7` **name**="_露出計の測定値_" Light meter
  readings taken on-set in Japan for a Japanese production.

### `description` - required

The `description` field is intended to give more detail to an end user about the
`name` field. It should be no longer than a paragraph of text that could be
easily read on a mobile device. It should be in the intended language of the
audience. If internationalisation is required, then please join the [MetaRex]
project and help us design a lightweight mechanism for this.

### `mediaType` - required

An application in a [MetaRex] system may choose to use one of the services
published in the register to process the metadata. The `mediaType` field is the
[Content-Type] of the unwrapped payload if it were placed in an [HTTP] response
or an [HTTP] [POST] request.

### `timingIs` (optional)

This field is a _hint_ for timeline processing by automation. If present, the
`timingIs` field shall have be one of the values below:

* `clocked` when the timing of the metadata is controlled by the metarex
  wrapper. This is typically one document per frame (MXF frame wrapping).
  This includes the case where the metadata needs to be applied to every sample
  in a sequence (e.g. static lighting position in a scene)
* `embedded` when the timing is inside the metadata document or binary blob.
  This includes the case where a document with no timing generically applies to
  an entire composition / scene / title etc (e.g. serial number of a camera)

### 'treatAs' (optional)

This field is a _hint_ for metadata processing by automation. If present, the
`treatAs` field shall have be one of the values below:

* `text` when the metadata is treated as plain text in a text processor
  * **Examples**:  JSON, YAML, XML, csv, txt
* `binary` when the metadata needs to be processed by some service or
  application. Dumb automation systems usually treat this type of metadata as an
  opaque binary blob. examples:
  * **Examples**: machine data,  word documents, excel spreadsheets, jpeg
    images, png images, pdf documents

### `expiresOn` (conditional)

The UTC date/time in ISO 8601 format when the entry becomes deprecated.

Any registration may have a `replacedBy` field that indicates a permanent
replacement is available.

If the `expiresOn` field is absent then the entry is considered permanent and
never expires.

The entity managing the register may remove all expired entries
at any time after the `expiresOn` date. The persistance of any entry after this
date cannot be guaranteed.

### `replacedBy` (optional)

The `metarexId` that replaces this one. The `metarexId` must exist.

This field is ignored if there is no `expiresOn` field.

### `replaces` (optional)

The `metarexId` that this entry replaces. The replaced `metarexId` may have
been deleted from the register.

### `registerEntrySemVer`

A [semver](https://semver.org/) string representing the version of this
document. This version of the document is:

```json
  "registerEntrySemVer": "0.2.0",
```

***

### `mrx` - extensions controlled by [MetaRex]

#### Overview

All `mrx` extensions are considered optional for [MetaRex] to work. Application
Use-Case specifications may specify other requirements as needed. It is
recommended that an extension of the base [MetaRex] [schema] is created for
Use-Cases to specify these requirements rather than relying on the good behavior
of humans typing into the register.

#### `sample`

A url string or array of url strings that resolve to samples of the metadata
defined by this [metaRex] entry.

#### `schema`

A url string or array of url strings that resolve to machine processable
schema(s) that can be used to validate the data. If there is no such schema
possible (e.g. because the metadata is binary and not text) then a validation
service may be published.

#### `services` - format TBD

An object that defines services available to process metadata defined by a
[MetaRex] register entry. If you're interested in monetizing metadata generated
by your products, then please join the [MetaRex] project and help us define this
section to allow for low-effort interoperability for complex metadata.

#### `specification`

A url string or array of url strings that resolve to human readable
specification(s) of the data.

***

### `extra` - extensions controlled by the registrant

If custom elements are required then they should be put in the `extra` object.

[content-type]: https://datatracker.ietf.org/doc/html/rfc7231#section-3.1.1.5
[http]:         https://datatracker.ietf.org/doc/html/rfc7231
[json]:         https://www.json.org
[metarex]:      https://metarex.media
[post]:         https://datatracker.ietf.org/doc/html/rfc7231#section-4.3.3
[schema]:       https://github.com/metarex.media/mrx-hosted/MRX.123.456.789.reg/schema.json
[UUID]:         https://datatracker.ietf.org/doc/html/rfc4122
[yaml]:         https://yaml.org
