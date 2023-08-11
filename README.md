# Hosted documents for metarex.media

This repo contains documents, specifications and schema used at [metarex.media].
Each folder corresponds to a `metarexId` used to register a particular class of
metadata. Inside the folder will be documents that relate to the metadata class.

The naming of files is not enforced, but the following guide might help for
consistency for contributors:

* `register.json` - copy of the register entry (probably out of date)
* `readme.md` - copy of the register's markdown formatted readme (no
  frontmatter & possibly out of date).
* `specification.md` - the human readable specification(s) of this class of
  metadata.
* `schema.xxx` - the machine readable schema(s) for validating instance documents
  of this class of metadata (such as `.json` or `.xsd` or `.dtd` etc.)

The url prefix for human access to a document is

```handlebars
url = https://github.com/metarex-media/mrx-hosted/blob/main/{{folder}}/{{doc}}
```

The url prefix for machine access to a document is

```handlebars
https://raw.githubusercontent.com/metarex-media/mrx-hosted/main/{{folder}}/{{doc}}
```

[metarex.media]: https://metarex.media
